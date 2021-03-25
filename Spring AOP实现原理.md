# Spring AOP实现原理

总结： aop两种实现方式，一种是接口的JDK 动态代理实现，通过反射的机制来接收代理的类，要求代理类实现某个接口，被代理类需要实现InvocationHander 接口，重写invoke（） 方法，通过Proxy 类的 newProxyInstance（）方法实现增强目标对象，参数为（增强类的类加载器，增强类的实现所有接口，被增强类） 



还有一种就是继承的CHLIB，要求增强类不能被final修饰，是一个代码生成的类库，可以在运行时动态生成某一个类的子类。

## 一、AOP是什么？

AOP（Aspect Orient Programming），我们一般称为面向方面（切面）编程，作为面向对象的一种补充，用于处理系统中分布于各个模块的横切关注点，比如事务管理、日志、缓存等等。AOP实现的关键在于AOP框架自动创建的AOP代理，AOP代理主要分为静态代理和动态代理，静态代理的代表为AspectJ；而动态代理则以Spring AOP为代表。本文会分别对AspectJ和Spring AOP的实现进行分析和介绍。

## 二、springaop如何实现的？

Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理。JDK动态代理通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口。JDK动态代理的核心是`InvocationHandler`接口和`[Proxy](http://www.liuhaihua.cn/archives/tag/proxy "View all posts in Proxy")`类。

如果目标类没有实现接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意，CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为`final`，那么它是无法使用CGLIB做动态代理的。

## 2.1、JDK动态代理

定义一个接口类

```
package org.java.base.springaop;
public interface Service {
 /**
 * add方法
 */
 public void add();

 /**
 * update方法
 */
 public void update();
}
复制代码
```

实现一个接口类

```
package org.java.base.springaop;
public class ServiceImpl implements Service {
 
 public void add() {
 System.out.println("AService add>>>>>>>>>>>>>>>>>>");
 }


 public void update() {
 System.out.println("AService update>>>>>>>>>>>>>>>");
 }
}
复制代码
```

实现动态代理类MyInvocationHandler，实现InvocationHandler接口，并且实现接口中的invoke方法。仔细看invoke方法，就是在该方法中加入切面逻辑的。目标类方法的执行是由mehod.invoke(target,args)这条语句完成。

```
package org.java.base.springaop;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
public class MyInvocationHandler implements InvocationHandler {
 private Object target;
 /**
 * 构造函数
 * @param target
 */
 MyInvocationHandler(Object target) {
 super();
 this.target = target;
 }
 public Object invoke(Object proxy, Method method, Object[] args)
 throws Throwable {
 // 程序执行前加入逻辑，MethodBeforeAdviceInterceptor
 System.out.println("before-----------------------------");
 // 程序执行
 Object result = method.invoke(target, args);
 // 程序执行后加入逻辑，MethodAfterAdviceInterceptor
 System.out.println("after------------------------------");
 return result;
 }

}
复制代码
```

测试类，其中增强的目标对象是由Proxy.newProxyInstance(aService.getClass().getClassLoader(),aService.getClass().getInterfaces(), handler);来生成的。

```
package org.java.base.springaop;

import java.lang.reflect.Proxy;
public class Test {
 public static void main(String[] args) {
 Service aService = new ServiceImpl();
 MyInvocationHandler handler = new MyInvocationHandler(aService);
 // Proxy为InvocationHandler实现类动态创建一个符合某一接口的代理实例
 Service aServiceProxy = (Service) Proxy.newProxyInstance(aService
 .getClass().getClassLoader(), aService.getClass()
 .getInterfaces(), handler);
 // 由动态生成的代理对象来aServiceProxy 代理执行程序，其中aServiceProxy 符合Service接口
 aServiceProxy.add();
 System.out.println();
 aServiceProxy.update();
 
 }
}
复制代码
```

[![1525849940(1)](https://user-gold-cdn.xitu.io/2018/5/10/163481c1df852830?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)](http://www.liuhaihua.cn/wp-content/uploads/2018/05/15258499401.png)

可以看到，在目标类ServiceImpl的add和update方法前后已经加入了自定义的切面逻辑，AOP拦截机制生效了。

## 2.2、CGLIB动态代理

目标类，[cglib](http://www.liuhaihua.cn/archives/tag/cglib)不需要定义目标类的统一接口

```
package org.java.base.springaop;

public class Base {
 /**
 * 一个模拟的add方法
 */
 public void add() {
 System.out.println("add ------------");
 }
}
复制代码
```

实现动态代理类CglibProxy，需要实现MethodInterceptor接口，实现intercept方法。该代理中在add方法前后加入了自定义的切面逻辑，目标类add方法执行语句为proxy.invokeSuper(object, args);

```
package org.java.base.springaop;
import java.lang.reflect.Method;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

public class CglibProxy implements MethodInterceptor {

 public Object intercept(Object object, Method method, Object[] args,
 MethodProxy proxy) throws Throwable {
 // 添加切面逻辑（advise），此处是在目标类代码执行之前，即为MethodBeforeAdviceInterceptor。
 System.out.println("before-------------");
 // 执行目标类add方法
 proxy.invokeSuper(object, args);
 // 添加切面逻辑（advise），此处是在目标类代码执行之后，即为MethodAfterAdviceInterceptor。
 System.out.println("after--------------");
 return null;
 }

}
复制代码
```

获取增强的目标类的工厂Factory，其中增强的方法类对象是有Enhancer来实现的，代码如下所示：

```
 package org.java.base.springaop;

import org.springframework.cglib.proxy.Enhancer;

public class Factory {
 /**
 * 获得增强之后的目标类，即添加了切入逻辑advice之后的目标类
 * 
 * @param proxy
 * @return
 */
 public static Base getInstance(CglibProxy proxy) {
 Enhancer enhancer = new Enhancer();
 enhancer.setSuperclass(Base.class);
 //回调方法的参数为代理类对象CglibProxy，最后增强目标类调用的是代理类对象CglibProxy中的intercept方法
 enhancer.setCallback(proxy);
 // 此刻，base不是单纯的目标类，而是增强过的目标类
 Base base = (Base) enhancer.create();
 return base;
 }
}
复制代码
```

测试类

```
package org.java.base.springaop;

public class Testcglib {
 public static void main(String[] args) {
 CglibProxy proxy = new CglibProxy();
 // base为生成的增强过的目标类
 Base base = Factory.getInstance(proxy);
 base.add();
 }
}
复制代码
```

自此，cglib动态代理实现的AOP拦截机制已经基本实现，下面我们来看一下拦截的效果如何，程序执行结果如下：

> before————-
>
> add ————
>
> after————–

可以看到，在目标类Base的add方法前后已经加入了自定义的切面逻辑，AOP拦截机制生效了。

## 三、关于动态代理和CGLIB这两种方式的简要总结如下：

- JDK动态代理(Dynamic Proxy)
  - 基于标准JDK的动态代理功能
  - 只针对实现了接口的业务对象
- CGLIB
  - 通过动态地对目标对象进行子类化来实现AOP代理，
  - 需要指定`@EnableAspectJAutoProxy(proxyTargetClass = true)`来强制使用
  - 当业务对象没有实现任何接口的时候默认会选择CGLIB