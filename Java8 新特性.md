# Java8 新特性

> Java8的发布带来了很多新特性，特别是Lambda表达式的支持 ，使得函数传递变得很容易，另外StreamApi使得对容器内的数据处理变得异常容易和强大，大大增加了开发效率。

**总结： **新增 lambda 表达式，接口默认方法，静态方法，方法引用，可以直接引用类或对象的构造函数跟方法，使用规则 引用构造方法 Class ：：new ，引用方法  Class：： method ，静态方法引用 Class：： static_method  对象方法引用，Instance：： method ，方法参数名获取，参数添加到字节码文件中，可以通过反射API 与 Parameter.getName()方法获取，Stream API 的使用， 真正引入函数式编程式风格到java 8中。

## Lambda表达式

Java语言层面上最受期待的改变，Lambda允许把函数作为一个方法的参数（函数作为参数传递进方法中），代替了原先匿名内部类，使得将行为表达为数据变得很容易，在纯Java语言环境中提供一种优雅的方式来支持函数式编程，从而使开发具有更强表达能力。

Lambda表达式由用逗号分隔的**参数列表**、**–>符号**、**函数体**三部分表示 e -> System.out.println( e ) 举几个例子

```
Arrays.asList( "a", "b", "d" ).forEach( e -> System.out.println( e ) );
Arrays.asList( "a", "b", "d" ).forEach( ( String e ) -> System.out.println( e ) );
Arrays.asList( "a", "b", "d" ).forEach( e -> {
    System.out.print( e );
    System.out.print( e );
} );

String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) );
final String separator = ",";
Arrays.asList( "a", "b", "d" ).forEach( 
    ( String e ) -> System.out.print( e + separator ) ); 

Arrays.asList( "a", "b", "d" ).sort( ( e1, e2 ) -> {
    int result = e1.compareTo( e2 );
    return result;
} );

Arrays.asList( "a", "b", "d" ).sort( ( e1, e2 ) -> e1.compareTo( e2 ) );
复制代码
```

## 函数式接口

Java8引入了函数式接口FunctionalInterface概念，来使得现有的函数支持Lambda表达式。

函数式接口就是一个具有一个方法的普通接口，可以被隐式转换为lambda表达式。例如java.lang.Runnable与java.util.concurrent.Callable是函数式接口最典型的两个例子。

在实际使用过程中，函数式接口是容易出错的：如有某个人在接口定义中增加了另一个方法，这时，这个接口就不再是函数式的了，并且编译过程也会失败。为了克服函数式接口的这种脆弱性并且能够明确声明接口作为函数式接口的意图，Java 8增加了一种特殊的注解@FunctionalInterface

```
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}

Function<Integer, String> function = (e) -> {
     e++;
     return "y = " + e;
};
复制代码
```

### 内置的函数式接口

```
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}

@FunctionalInterface
public interface Consumer<T> {
	void accept(T t);
}

@FunctionalInterface
public interface Supplier<T> {
	T get();
}

@FunctionalInterface
public interface Predicate<T> {
	boolean test(T t);
}

public interface Iterable<T> {
	Iterator<T> iterator();
	
	default void forEach(Consumer<? super T> action) {
	   Objects.requireNonNull(action);
	   for (T t : this) {
	      action.accept(t);
	   }
	}
}
复制代码
```

## 接口的默认方法与静态方法

Java 8用默认方法与静态方法这两个新概念来扩展接口的声明，可以包含实现代码，允许在已有的接口中添加新方法，而同时又保持了与旧版本代码的兼容性。默认方法与抽象方法不同之处在于抽象方法必须要求实现，但是默认方法则没有这个要求。

- 默认方法：提供默认的方法实现，或者提供依赖当前实例的新的接口实例
- 静态方法：提供当前接口实例，且不依赖当前实例this

Java8通过在接口中提供默认方法的方式，增强了现有接口的功能，比如 java.util.Collection接口增加了stream()，parallelStream()，removeIf()；java.lang.Iterable接口增加forEach()

```
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
    
    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    static <T> Function<T, T> identity() {
        return t -> t;
    }
}
复制代码
```

## 方法引用

方法引用提供了非常有用的语法，可以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合使用，方法引用可以使语言的构造更紧凑简洁，减少冗余代码。

- 构造器引用，它的语法是Class::new
- 静态方法引用，它的语法是Class::static_method
- 特定类的任意对象的实例方法引用，它的语法是Class::method
- 特定对象的方法引用，它的语法是instance::method

```
public static class Car {
	public Car() {
    }

    public Car(String name) {
        this.name = name;
    }

    public static Car create(Supplier<Car> supplier) {
        return supplier.get();
    }

    public static Car create(String name, Function<String, Car> supplier) {
        return supplier.apply(name);
    }

    public static void collide(final Car car) {
        System.out.println("Collided " + car.toString());
    }

    public void follow(final Car another) {
        System.out.println("Following the " + another.toString());
    }

    public void repair() {
        System.out.println("Repaired " + this.toString());
    }

    public static void main(String[] args) {
        // 构造器引用
        Car car = Car.create(Car::new);
        car = Car.create(() -> new Car());
        Car car3 = Car.create("test", Car::new);
        car3 = Car.create("test", (e) -> new Car(e));

		List<Car> cars = Arrays.asList(car);
        // 静态方法引用
        cars.forEach(Car::collide);
        cars.forEach(e -> Car.collide(e));

        // 类的任意对象的方法引用
        cars.forEach(Car::repair);
        cars.forEach(e -> e.repair());

        // 对象的实例方法引用
        Car police = Car.create(Car::new);
        cars.forEach(police::follow);
        cars.forEach(e -> police.follow(e));
    }
}
复制代码
```

## 方法参数名

Java8将方法的参数名添加到了字节码文件中（通过新版的javac的–parameters选项），通过反射API与Parameter.getName()方法来获取（接口和抽象方法也能获取）。

在maven项目中，可以通过maven-compiler-plugin的配置可以将-parameters参数添加到编译器中去

```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <compilerArgument>-parameters</compilerArgument>
        <source>1.8</source>
        <target>1.8</target>
    </configuration>
</plugin>
复制代码
```

## Optional类

到目前为止，臭名昭著的空指针异常是导致Java应用程序失败的最常见原因。以前，为了解决空指针异常，Google公司著名的Guava项目引入了Optional类，Guava通过使用检查空值的方式来防止代码污染，它鼓励程序员写更干净的代码。受到Google Guava的启发，Optional类已经成为Java 8类库的一部分。

Optional实际上是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。Optional一般只用作方法的返回值。

```
public static void main(String[] args) {
        Optional<String> fullName = Optional.ofNullable(null);
        System.out.println("Full Name is set? " + fullName.isPresent());
        System.out.println("Full Name: " + fullName.orElseGet(() -> "[none]"));
        System.out.println(fullName.map(s -> "Hey " + s + "!").orElse("Hey Stranger!"));

        Optional< String > firstName = Optional.of( "Tom" );
        System.out.println( "First Name is set? " + firstName.isPresent() );
        System.out.println( "First Name: " + firstName.orElseGet( () -> "[none]" ) );
        System.out.println( firstName.map( s -> "Hey " + s + "!" ).orElse( "Hey Stranger!" ) );
        System.out.println();
}
复制代码
```

## Stream API

Stream API（java.util.stream） 把真正的函数式编程风格引入到Java中，极大简化了集合框架的处理，可以极大提供Java程序员的生产力，让程序员写出高效率、干净、简洁的代码。

### Stream 使用示例

先来学习下stream api的使用：

```
public class SteamsDemo {
    private enum Status {
        OPEN, CLOSED
    }

    private static final class Task {
        private final Status status;
        private final Integer points;

        Task(final Status status, final Integer points) {
            this.status = status;
            this.points = points;
        }

        public Integer getPoints() {
            return points;
        }

        public Status getStatus() {
            return status;
        }

        @Override
        public String toString() {
            return String.format("[%s, %d]", status, points);
        }
    }

    public static void main(String[] args) {
        final Collection<Task> tasks = Arrays.asList(
                new Task(Status.OPEN, 5),
                new Task(Status.OPEN, 13),
                new Task(Status.CLOSED, 8)
        );

        // Calculate total points of all active tasks using sum()
        final long totalPointsOfOpenTasks = tasks
                .stream()
                .filter(task -> task.getStatus() == Status.OPEN)
                .mapToInt(Task::getPoints)
                .sum();
        System.out.println("Total points: " + totalPointsOfOpenTasks);
        
		final Collection<String> result = tasks
		      .stream()                                    // Stream< String >
		      .mapToInt(Task::getPoints)                   // IntStream
		      .asLongStream()                              // LongStream
		      .mapToDouble(points -> points / totalPoints) // DoubleStream
		      .boxed()                                     // Stream< Double >
		      .mapToLong(weight -> (long) (weight * 100))  // LongStream
		      .mapToObj(percentage -> percentage + "%")    // Stream< String>
		      .collect(Collectors.toList());               // List< String >
    }
}
复制代码
```

- 第一，task集合被转换化为其相应的stream表示。
- 然后，filter操作过滤掉状态为CLOSED的task。
- 下一步，mapToInt操作通过Task::getPoints这种方式调用每个task实例的getPoints方法把Task的stream转化为Integer的stream。
- 最后，用sum函数把所有的分数加起来，得到最终的结果。

stream另一个有价值的地方是能够原生支持并行处理：

```
// Calculate total points of all tasks
final double totalPoints = tasks
      .stream()
      .parallel()
      .map(Task::getPoints) 
      .reduce(0, Integer::sum);
复制代码
```

Stream API不仅仅处理Java集合框架。像从文本文件中逐行读取数据这样典型的I/O操作也很适合用Stream API来处理：

```
final Path path = new File( filename ).toPath();
try( Stream< String > lines = Files.lines( path, StandardCharsets.UTF_8 ) ) {
    lines.onClose(() -> System.out.println("Done!")).forEach( System.out::println);
}
复制代码
```

### Stream（流）是什么

Stream（流）是一个来自特定数据源的元素组成的队列，并支持各类计算操作

![img](https://user-gold-cdn.xitu.io/2019/12/21/16f28f0489a4113c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- 不提供存储空间。流不是存储元素的数据结构，相反，它将数组、生成器函数或I / O通道等源的数据元素传递到由各类计算操作组成的管道。
- 对流的操作会产生结果，但不会修改数据源。例如，流过滤会从现有Stream生成一个不包含过滤元素的新Stream，而不是从源集合中删除元素。
- 数据源可以是无限的。虽然集合的大小有限，但流不需要。诸如limit（n）或findFirst（）之类的短路操作可以允许无限流上的计算在有限时间内完成。
- 不可重用。stream对象仅可以遍历一次，重复使用会抛出IllegalStateException运行时异常
- 惰性求值。中间操作总是采用惰性求值方式，运行一个像filter这样的中间操作实际上没有进行任何过滤，相反它在遍历元素时会产生了一个新的stream对象，这个新的stream对象包含原始stream中符合给定谓词的所有元素。

![img](https://user-gold-cdn.xitu.io/2019/12/21/16f28f14708c6fc0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Stream和以前的Collection操作不同，Stream操作有两个基础的特征：

- Pipelining:中间操作都会返回流对象本身。这样多个操作可以串联成一个管道，如同流式风格（fluent style）。 这样做可以对操作进行优化，比如延迟执行(laziness)和短路( short-circuiting)。
- 内部迭代：以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代，这叫做外部迭代。 Stream提供了内部迭代的方式，通过访问者模式(Visitor)实现。

### Stream流操作API分类

在 Java 8 中, 集合接口有两个方法来生成流：

- stream() − 为集合创建串行流。
- parallelStream() − 为集合创建并行流。

![img](https://user-gold-cdn.xitu.io/2019/12/21/16f28f68530b20c3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

java.util.stream.Stream中定义了许多流操作的方法，可分为两类：中间操作和最终操作

- 中间操作：如filter、map等处理操作将Stream一层一层的进行抽离，返回一个流给下一层使用。
- 结束操作：从最后一次流中生成一个结果给调用方（foreach只做处理不做返回）

中间操作又可以分为无状态的(Stateless)和有状态的(Stateful)：

- 无状态中间操作是指元素的处理不受前面元素的影响；
- 有状态的中间操作必须等到所有元素处理之后才知道最终结果，比如排序是有状态操作，在读取所有元素之前并不能确定排序结果；

| Stream操作分类                     |                                                         |                                                              |
| ---------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| 中间操作 (Intermediate operations) | 无状态(Stateless)                                       | filter() map() mapToInt() mapToLong() mapToDouble() flatMap() flatMapToInt() flatMapToLong() flatMapToDouble() peek() |
| 有状态(Stateful)                   | distinct() sorted() limit() skip()                      |                                                              |
| 结束操作 (Terminal operations)     | 非短路操作                                              | forEach() forEachOrdered() toArray() reduce() collect() max() min() count() |
| 短路操作 (short-circuiting)        | anyMatch() allMatch() noneMatch() findFirst() findAny() |                                                              |

#### map

返回一个新的Stream，其中的元素由原有的Stream元素经由mapper函数转换而来（返回类型可以发生变化，元素个数不会发生变化）

```
<R> Stream<R> map(Function<? super T, ? extends R> mapper);

复制代码
```

#### flatMap

返回一个新的Stream，其中的元素由原有的Stream元素经由mapper函数转换生成的Stream平铺而来（返回类型可以发生变化，元素个数可以发生变化）

```
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
复制代码
```

#### reduce

返回一个非Stream类型的对象，其值为对Stream中的所有元素进行规约操作accumulator后生成的

```
<U> U reduce(U identity,
             BiFunction<U, ? super T, U> accumulator,
             BinaryOperator<U> combiner);

// 执行过程的伪代码
U result = identity;
for (T element : this stream)
     result = accumulator.apply(result, element)
return result;
// combiner用于合并多个U（并行执行时）

// 示例数据
haha:hehe:heihei
nihao:hello:words
// map与flatMap的差异
try (Stream<String> lines = Files.lines(path, StandardCharsets.UTF_8)) {
      List<String> wordsList1 = lines.flatMap(line -> Stream.of(line.split(":"))).collect(Collectors.toList());
      List<String[]> wordsList2 = lines.map(line -> line.split(":")).collect(Collectors.toList());
}
复制代码
```

#### collect

返回一个非Stream类型的对象，对象由supplier提供，对象中的元素是由对Stream中的所有元素进行规约操作accumulator后生成的

```
<R> R collect(Supplier<R> supplier,
		      BiConsumer<R, ? super T> accumulator,
		      BiConsumer<R, R> combiner);

// 执行过程的伪代码
R result = supplier.get();
for (T element : this stream)
     accumulator.accept(result, element);
return result;	 

// 示例
Stream<String> stringStream = Stream.of("3333", "dddd");
List<String> asList = stringStream.collect(ArrayList::new, ArrayList::add, ArrayList::addAll);

String concatStr = stringStream.collect(StringBuilder::new, StringBuilder::append, StringBuilder::append).toString();   
复制代码
```

### Stream内部原理

![img](https://user-gold-cdn.xitu.io/2019/12/21/16f28f6ff23c4dd1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

通过Collection接口的stream()方法，返回一个流对象（ReferencePipeline.Head），后续每调用一个中间操作都会生成一个流对象（StatelessOp/StatefulOp），并与之前的流对象（管道/stage）连接形成一个整体的管道，最后调用终止操作（TerminalOp），会触发真正的操作，将源数据流经管道，由管道中的各个操作Op处理，最后返回给调用方。

![img](https://user-gold-cdn.xitu.io/2019/12/21/16f28f7404cf8db2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**AbstractPipeline** 管道的基类，提供了Stream接口核心实现

- 主要属性
  - sourceStage
  - previousStage
  - nextStage
  - sourceSpliterator
- 主要方法
  - **wrapAndCopyInto** 将Spliterator中的元素应用到Sink链条
  - **evaluate** 触发终止操作，计算结果
  - **opWrapSink** 子类实现 创建Sink链条
  - **opEvaluateParallel** 子类实现 有状态操作并行操作时触发
  - opIsStateful 子类实现 当前操作是否有状态
- 子类
  - ReferencePipeline（Head/StatelessOp/StatefulOp）
  - IntPipeline（同上）
  - LongPipeline（同上）
  - DoublePipeline（同上）

**Spliterator 分裂器** 用于遍历和拆分源数据

- tryAdvance 尝试消费一个元素
- forEachRemaining 消费剩余的所有元素（循环调用tryAdvance）
- trySplit 并发流使用，拆分数据源数据，不可拆分则返回null，否则返回一个新的Spliterator（一般将剩余元素一分为二）
- estimateSize 估计剩余的元素数量

**Sink** 下沉接口，继承Consumer，用于访问元素

- begin 数据访问前操作
- accept 数据访问操作
- end 数据访问后操作

**TerminalOp** 定义结束操作的行为，继承Sink

- evaluateSequential 顺序执行，调用管道的wrapAndCopyInto方法，触发计算操作
- evaluateParallel 并行执行 （利用ForkJoinTask，对Spliterator拆分的数据，调用管道的wrapAndCopyInto方法进行计算）

![img](https://user-gold-cdn.xitu.io/2019/12/21/16f28f79e5dad8cc?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**示例的执行过程**

```
Arrays.asList("abc", "defg")
	  .stream()
      .filter(str -> str.length() <= 3)
      .map(String::toUpperCase)
      .forEach(System.out::println);
复制代码
```

1. 创建一个包含两个字符串的ArrayList对象
2. stream法内部创建Spliterator（IteratorSpliterator）以及Head管道A（ReferencePipeline.Head）
3. filter方法内部创建StatelessOp管道B并与管道A关联，并复写opWrapSink方法，用于生成Sink实例（主要实现accept方法，持有downstream引用，即下游管道的Sink实例）
4. map方法内部创建StatelessOp管道C并与管道B关联，并复写opWrapSink方法，用于生成Sink实例（同上）
5. forEach操作触发管道C的wrapAndCopyInto方法，其方法内部将管道C、管道B的opWrapSink返回的Sink实例 组成Sink链条，首先调用Sink链条的begin方法，接着调用Spliterator的forEachRemaining方法，遍历访问元素，最后调用Sink链条的end方法

**各类工具类**

- Spliterators 定义了各类Spliterator静态子类，以及创建Spliterator的静态方法
- XxxOps 定义了各类创建 中间操作 和 结束操作 的静态方法，对于中间操作分为StatelessOp和StatefulOp，结束操作即为TerminalOp

## CompletableFuture

CompletableFuture出现之前只能借助JDK提供的Future，但只能等待结果或轮询，google的Guava包扩展了Future提供ListenableFuture，允许你添加回调获取结果。但是在异步编程通过回调方式很容易出现回调地狱，而CompletableFuture的出现为Java异步编程提供了很大便利性，类似于JavaScript中的Promise，通过级联的方式而非回调。CompletableFuture内部使用ForkJoinPool线程池执行异步处理。

### 主要的API

> 不以Async结尾的方法，意味着Action使用相同的线程执行，而Async可能会使用其它的线程去执行(如果使用相同的线程池，也可能会被同一个线程选中执行)

#### 静态方法

```
// 执行的任务无返回值，可以指定executor，默认ForkJoinPool
public static CompletableFuture<Void> runAsync(Runnable runnable) {}
public static CompletableFuture<Void> runAsync(Runnable runnable, Executor executor) {}

// 执行的任务有返回值，可以指定executor，默认ForkJoinPool
public static CompletableFuture<U> supplyAsync(Supplier<U> supplier) {}
public static CompletableFuture<U> supplyAsync(Supplier<U> supplier, Executor executor) {}

// 获取一个已完成的CompletableFuture
public static <U> CompletableFuture<U> completedFuture(U value){}

// 获取一个CompletableFuture，当所有的cfs完成的时候则变为完成
public static CompletableFuture<Void> allOf(CompletableFuture<?>... cfs) {}
// 获取一个CompletableFuture，当cfs中任何一个完成的时候则变为完成
public static CompletableFuture<Object> anyOf(CompletableFuture<?>... cfs) {}
复制代码
```

#### Future方法

```
// 取消任务
boolean cancel(boolean mayInterruptIfRunning)
// 任务是否取消
boolean isCancelled();
// 任务是否完成
boolean isDone();
// 等待直到获取结果
V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;
V get() throws InterruptedException, ExecutionException;
复制代码
```

#### 主动完成任务

```
// 正常完成
public boolean complete(T value){}
// 异常完成
public boolean completeExceptionally(Throwable ex){}
复制代码
```

#### 任务完成处理

```
// CompletableFuture完成时触发action（正常结束或异常时），可以指定executor
public CompletableFuture<T> whenComplete(BiConsumer<? super T,? super Throwable> action){}
public CompletableFuture<T> whenCompleteAsync(BiConsumer<? super T,? super Throwable> action){}
public CompletableFuture<T> whenCompleteAsync(BiConsumer<? super T,? super Throwable> action, Executor executor){}
// CompletableFuture异常时触发
public CompletableFuture<T> exceptionally(Function<Throwable,? extends T> fn){}
复制代码
```

#### 任务正常完成处理

参考CompletionStage其他方法的定义