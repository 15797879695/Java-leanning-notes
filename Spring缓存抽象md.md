## Spring缓存抽象概述

Spring框架自身并没有实现缓存解决方案，但是从3.1开始定义了org.springframework.cache.Cache和org.springframework.cache.CacheManager接口，提供对缓存功能的声明，能够与多种流行的缓存实现集成。

Cache接口为缓存的组件规范定义，包含缓存的各种操作集合；

Cache接口下Spring提供了各种xxxCache的实现：如RedisCache，EhCacheCache , ConcurrentMapCache等；

CacheManager接口为缓存管理器规范，简单来说就是用于存放cache，Spring默认也提供了一些列管理器的实现。

Spring缓存抽象提供了5个注解用来声明缓存规则：

@Cacheable：能够根据方法的请求参数对其结果进行缓存，多用于查询，查询时先查询缓存是否有，没有再执行方法，结果缓存

```
 @Cacheable(cacheNames = "product")
```

@CachePut: 执行方法，并缓存结果，查询时直接执行方法，并把结果缓存

```
@CachePut(value="product"
```

@Caching:能够同时应用多个缓存注解功能，同时有 Cacheable 和CachePut 时查询时不会先再缓存中查询，而是直接执行方法

@CacheConfig: 用于抽取缓存的公共配置（类级别）

以上5个注解除了@CacheConfig注解是类级别的注解，其余4个注解在类和方法上均可以使用，作用在类上表示对该类下所有方法生效，作用的方法上只对该方法生效，且只能用于public修饰的符方法，protected或者private修饰的方法不适用。

## @Cacheable注解

@Cacheable注解的作用是Spring在调用该方法之前，首先在缓存中查找方法的返回值，默认的key是根据参数值生成，如果存在，直接返回缓存中的值，否则执行该方法，并将返回值保存到缓存中

### @Cacheable运行流程：

 1.方法运行之前，先去查询Cache（缓存组件），按照cacheNames指定的名字获取；

​      （CacheManager先获取相应的缓存），第一次获取缓存如果没有Cache组件会自动创建。

 2.去Cache中查找缓存的内容，使用一个key，默认就是方法的参数值；

​      key是按照某种策略生成的；默认是使用keyGenerator生成的，        

　　  Spring默认加载的是SimpleCacheManage，SimpleKeyGenerator生成key的默认策略是：

​            如果没有参数；key=new SimpleKey()

​            如果有一个参数：key=参数的值

​            如果有多个参数：key=new SimpleKey(params)

 3.没有查到缓存就调用目标方法；

 4.将目标方法返回的结果，放进缓存中

### @Cacheable属性说明：

　　1.acheNames/value：该属性值必须提供，指定缓存组件的名字，将方法的返回结果放在哪个缓存中，是数组的方式，可以指定多个缓存；

 　　　 如：cacheNames = "product"或者cacheNames = {"product1","product2"}

　　2.key：缓存数据使用的key，不指定key则默认是使用方法参数的值该属性值支持SpEL表达式

​    3.cacheManager：指定缓存管理器；或者cacheResolver指定获取解析器

​    4.condition：指定符合条件的情况下才缓存

　　5.unless:否定缓存；当unless指定的条件为true，方法的返回值就不会被缓存；可以获取到结果进行判断

​          unless = "#result == null"

​          unless = "#a0==2":如果第一个参数的值是2，结果不缓存；

　　6.sync：是否使用异步模式

使用示例：

```
 1 @Cacheable(cacheNames = "product")// 默认key为参数，多个参数SimpleKey [arg1,arg2]
 2 //@Cacheable(cacheNames = "product",key = "#root.methodName+'['+#id+']'")
 3 //@Cacheable(cacheNames = "product",keyGenerator = "myKeyGenerator")
 4 //@Cacheable(cacheNames = "product",key = "#root.methodName+'['+#id+']'",condition="#a0>10",unless = "#a0==11") //带条件的缓存满足condition=true缓存，满足unless=true则不缓存 
 5 public Product getProductById(Long id){
 6    Product product =productMapper.getProductById(id);
 7    System.out.println(product);
 8    return product;
 9 }
10 //指定key属性值
11 @Cacheable(cacheNames ="product", key="#id")    //”#+参数名”的形式，直接使用参数名
12 //或者
13 //@Cacheable(cacheNames ="product", key="#a0")    //”#a+参数位置”的形式
14    public Product getProductById(long id) {
15      xxxx
16    }
17 @Cacheable(cacheNames ="product", key="# productcondition.productId")    
18 //或者
19 //@Cacheable(cacheNames ="product", key="#a0.productId")    
20    public Product getProduct (Product productcondition) {
21      xxxx
22    }复制代码
```

### 自定义Key生成器

除了通过SPEL表达式之外，还可以通过自定义key生成器的方式，Spring缓存模块提供了org.springframework.cache.interceptor.KeyGenerator接口用于缓存key的生成声明，因此我们可以自定义一个MyKeyGenerator类并实现了KeyGenerator接口 ，使用如下：

```
 1 @Configuration
 2 public class MyCacheConfig {
 3 
 4     @Bean("myKeyGenerator")
 5     public KeyGenerator keyGenerator(){
 6         return new KeyGenerator(){
 7 
 8             @Override
 9             public Object generate(Object target, Method method, Object... params) {
10                 return method.getName()+"["+ Arrays.asList(params).toString()+"]";
11             }
12         };
13     }
14 }复制代码
```

该方法测试用，关于缓存key的生成方式，网上有很多种策略。

使用时只需要修改注解的key属性即可：

```
1 @Cacheable(cacheNames = "product",keyGenerator = "myKeyGenerator")复制代码
```

##  @CachePut

@CachePut注解的作用简单的说一句话：既调用方法，又缓存数据。@cachePut和@Cacheable两个注解都可以用于填充缓存，但使用上略有点差异，@Cacheable注解的执行流程是先在按key在缓存中查找，存在则返回，不存在则执行目标方法，并缓存目标方法的结果。而@CachePut并不会检查缓存，总是先执行目标方法，并将目标方法的结果保存到缓存中。实际中比如执行到更新操作时，则希望将最新的数据更新到缓存，如果该方法返回异常，将不再执行保存缓存的逻辑。

### @CachePut属性说明

@CachePut注解属性与@CachePut类似，并没有增加其他属性

使用示例：

```
 1 @CachePut(value="product",key = "#result.productId",condition = "#result!=null")
 2 public  Product updateProduct(Product product){
 3     int count = productMapper.updateProduct(product);
 4     System.out.println("影响行数："+count);
 5     if(count>0){
 6         return product;
 7     }else{
 8         return null;
 9     }
10 }复制代码
```

## @CacheEvict注解

该注解的作用根据指定的key或者是allEntries属性值移除缓存中特性的键值对。

### @CacheEvict属性说明

与@Cacheable相比@CacheEvict注解提供了另外两个属性：

1. allEntries：表示是否清空所有缓存内容，默认false，如果该值为true则清空指定cacheNames缓存块下所有内容，如果指定了allEntries为true，那么再zhidingkey值将没有意义
2. beforeInvocation：是否在执行方法前请空缓存，默认值为false,如果该值为true则在调用目标方法前执行清空缓存，为false的情况下，如果目标方法抛出异常，则不再执行清空缓存逻辑

示例：

```
1 //@CacheEvict(value="product",key="#id")
2 //@CacheEvict(value="product",allEntries = true) //清楚所有缓存
3 @CacheEvict(value="product",allEntries = true,beforeInvocation = true) //清楚所有缓存
4 public boolean deleteProductById(Long id) {
5     productMapper.deleteProductById(id);
6     return true;
7 }复制代码
```

## @Caching注解

该注解是一个分组注解，作用是可以同时应用多个其他注解，该注解提供了3个属性cacheable，put，evict分别用于组合@Cacheable、@CachePut、@CacheEvict三个注解

使用示例：

```
 1 @Caching(
 2         cacheable = {@Cacheable(value="product",key="#productName")},
 3         put = {
 4                 @CachePut(value="product",key="#result.productId"),
 5                 @CachePut(value="product",key="#result.productName")
 6         }
 7 )
 8 public  Product getProductByName(String productName){
 9 
10     Product product =productMapper.getProductByName(productName);
11 
12      return product;
13 }复制代码
```

当@Cacheing同时含有CachePut注解和Cacheable注解时，仍然会先执行目标方法。（并不是按@Cacheable的执行过程，先检查缓存，存在则返回）

## @CacheConfig

是一个类级别的注解，允许共享缓存的名称、KeyGenerator、CacheManager 和CacheResolver

示例：

```
1 @Service
2 @CacheConfig(cacheNames = "product")
3 public class ProductService {
4 }复制代码
```

在类上使用该注解，指定cacheNames属性值，则类中方法上的注解将默认继承了该属性值，如果方法上注解使用和了@CacheConfig向同的属性，则以方法上的为准。

```
 1 @Service
 2 @CacheConfig(cacheNames = "product")
 3 public class ProductService {
 4     @Autowired
 5     private ProductMapper productMapper;
 6 
 7     @Cacheable(cacheNames = "product1",key = "#root.methodName+'['+#id+']'")
 8     public Product getProductById(Long id){
 9        Product product =productMapper.getProductById(id);
10        System.out.println(product);
11        return product;
12     }
13 }复制代码
```

上面@Cacheable和@CacheConfig都指定了属性值cacaeNames，实际以方法上注解指定的为准。

 

Spring缓存抽象的关键原理就是使用spring AOP，通过切面实现了在方法调用前、调用后获取方法的入参和返回值，进而实现了缓存的逻辑。