1. ==跟equals（）区别

· == 是关系运算符，equals() 是方法，结果都返回布尔值

· Object 的 == 和 equals() 比较的都是地址，作用相同

 

***\*== 作用：\****

· 基本类型，比较值是否相等

· 引用类型，比较内存地址值是否相等

· 不能比较没有父子关系的两个对象

 

***\*equals()方法的作用：\****

· JDK 中的类一般已经重写了 equals()，比较的是内容

· 自定义类如果没有重写 equals()，将调用父类（默认 Object 类）的 equals() 方法，Object 的 equals() 比较使用了 this == obj

· 可以按照需求逻辑，重写对象的 equals() 方法（重写 equals 方法，一般须重写 hashCode 方法）

总结：== 比较时 包装对象 ：包装对象 返回 flase 因为比较的是地址，但是JVM 缓存部分基本类型常用的包装类对象，如 Integer -128 ~ 127 是被缓存的）

Eg：integer a=100

Integer b=100

Print（a==b） 结果为true 

 

2. try 跟fally代码块至少有一个return时

  Fally 代码块会执行，fally没有return 变量不会重新赋值，fally和try 都有return时以finally代码块变量会赋值

3. String对象的replace（）和replaceall（）区别，replace代替的是第一次出现的字符串或字符，replaceall（）代替的是所有出现的字符串

4. Math 类ceil（）方法，向上取整，floor（）向下取整，round（）四舍五入，可以理解为正数+0.5向下取整

5. String 对象是fanal修饰，方法都是返回的new string ，所以对string操作都不会改变原对象的值，stringbuffer 是用synchronized修饰的，保证了线性安全，stringbuilder 不保证线性安全，修改是对原对象修改。

6. 字符串反转

   Stringbuilder stringbuffer 使用reverse（）方法，string

   Public string Reversestr（string str）{

   Int len=str.length();

   Char【】len = new char[len];

   For(int i=len-1;i>=0;i--){

    Char[len-1-i]=str.Atchar（i）；

   }

   }return new string（char）

 

7. a++跟++a的区别

  a++是先赋值，再+1，++a是先加1再把临时值赋值

  Eg：int a=3;int b=a++;

​    System.out.println(b)  //3

   int a=3;int b=++a;

​    System.out.println(b)  //4

8.包装类跟基本数据类型的转换

包装类——》基本类型  XXX.value()     基本类型——》包装类   new 类 （基本数据值）  

string ——》基本类型   parsexxx（string str） 基本类型 ——》string     stirng.valueOf( 基本类型)

 基本类型  对应的包装类   除了  int——》Intger  ，char ——》Character 其他都是首字母大写

9. JVM内存回收机制特点， 1.释放 堆的对象内存，不释放栈中的属性方法 。2 .垃圾回收调用finalize（） 方法，不要自己主动调用此方法，3.无法释放操作系统资源 如IO流，Socket连接

10. 基本类型跟包装类的区别

    泛型不支持基本类型，eg：ArraryList<Integer> 

    反射不支持 基本类型

11. Java.sql.Date 是java.util.Date  的子类

    java.util.Date 精确到时分秒，sql.Date 时分秒为0 ，数据库想要得到时分秒 用Timesstamp

12. 创建子类时先执行父类方法，再执行子类方法

13. 标识符 可以使用字母，数字，下划线，$， 不能数字开头，不能是关键字

14. 内存溢出：程序申请内存时，内存不够

    内存泄漏：程序申请的内存无法释放，占用内存

15. 创建对象的四种方法

    1：new 创建对象 

    2：使用反射机制，class类创建对象

    3：使用对象的clone（）方法

    4：使用 反序列化方法创建 ObjectInputStream类的readObject（） 方法

16. 同步方法：在方法前加synchronize 修饰

    同步代码块：在方法内的代码块加synchronize
    
17. string类为什么是final的，不能重写

    一个是因为安全性，string作为基础类频繁使用，final修饰就不能继承了，string作为基础类，是跟操作系统打交道的，这种类很多方法都不是用Java语言编写的，而是调用操作系统的api，如果重写的话就有很多安全隐患。

    而是效率，string作为频繁使用的类，使用final修饰降低了程序的效率，所以干脆使用final修饰，提高效率。
    
18. 日期如何标准化，跟字符串如何互转

    Dateformat  sj=new SimpleDateformat（“yyyy-yy-yy  hh：ff：mm”）；

    String str=sj.format(new Date())

    DateFormat  sj =new SimpleDateFormat(" hh:ff:mm  YYYY-MM-DD");

    String="2020-8-3116:39:22"；

    date=sj.parse(str)

19. switch 使用的数据类型有 int short  char byte    String

20. inner class 跟static  nested class 区别，

    inner class 可以访问外部类的成员变量，内部类在方法体外可以使用四种权限修饰语。方法体内不可以使用修饰语，但是可以是final abstrac t  

    static nested class 可以访问外部类的静态变量，不过加上外部类的名字，不可以访问外部类的非静态变量

21. abstract 方法不能使用static native synchronize 秀死，静态方法不可以重写，本地方法是本地动态库实现的，抽象方法没有实现，加锁需要方法体，抽象方法没有方法体

22. 反射主要用到的类

    - java.lang.Class ：一个类
    - java.lang.reflect.Field ：类的成员变量(属性)
    - java.lang.reflect.Method ：类的成员方法
    - java.lang.reflect.Constructor ：类的构造方法
    - java.lang.reflect.Array ：提供了静态方法动态创建数组，访问数组的元素

    
    
23. 重写equal 方法为什么需要重写还是从hashcode方法

    equal 默认比较的是对象的内存地址，hashcode方法时根据对象地址结合哈希算法得出的，如果不重写hashcode 得到的hash值是不相等的，对象相等hash值必须相等，所以需要重写hashcode方法

    1、对象相等，hash值必须相等

    2、对象不相等，hash值不一定不相等

    3、hash相等，对象不一定相等

    4、hash不相等，对象一定不相等

24. 抽象类跟接口的区别

    1、抽象类定义的是什么，接口定义的是能做什么，抽象公共属性方法，接口扩展功能，就像是usb可以接鼠标和键盘等外设

    2、abstract  只能修饰类和方法，不能修饰变量
    
25. POJO ，VO，DTO ，PO类的区别

    1、pojo类指的是简单java类，通常值实体类，没有继承别的类，接口，被框架侵入其他类

    2、vo 视图类，View object类

    3、dto数据传输类，data tanslate object 类

    4、po 持久化类，persistent object 类

26. 编码规范

    1、尽量使用英文命名，不能出现英文，拼音混用的情况

    2、类名使用大写命名，方法，参数，变量，构造方法使用小写命名。

    3、常量全部使用大写命名

    4、抽象类使用Abstract 或者Base 开头，异常类使用Exception结尾，测试类使用测试类名开头，Test结尾

    5、pojo 类的任何布尔变量不能使用is 开头，否则在框架解析会引起序列会错误

    eg：定义isDeleted 变量，方法也是isDaleted（） 解析会认定属性为deleted 找不到这个属性

    6、包名全部使用小写

    7、接口方法，变量都不用使用修饰符，使用默认

    8、接口和实现了之间的关系，实现类是接口类名加上impl

    9、枚举类加上后缀，Enum ，变量都使用大写

    枚举类就是常量类，构造方法默认是私有的

27. 枚举 Enum 就是有限个实例对象的类进行类内部创建对象列举出来，属性需要使用private final 修饰，创建对象使用static final 修饰

28. 创建线程的多种方式

    1、继承Thread 类

    2、实现 Runable 接口，重写run方法

    3、实现Callable接口，不需要重写run方法，可以有返回值，使用FetureTask 接收封装。

29. 线程终止方法

    1、调用Thread 类的interrupt （）方法中断

    2、使用Excutor 任务管理 shutdown（） 方法，但是是执行完再中断，调用shutdownNow （） 方法就是调用每个线程的interrupt（） 方法

    3、 使用Executor 中断一个线程使用commit（） ，用Feture 接收，调用这个对象的cancel（true ）方法即可

30. 内存模型三个特性

    1、原子性

    2、可见性，一个线程修改数据其他线程立即可以读取修改后数据，三个方法 ① volatile  ② synchronized ③ final

    3、有序性

31. CAS 比较并交换，（compare and swap ） CAS指令有三个操作数，V 是指内存地址，A是 预计旧值，B是新值，只有 比较V 与A相等时才交换B

32. TCP，UDP 代码实现

    TCP ： CLient  创建Socket 对象 传参（服务器地址，端口号），so 得到getOutputStream （） 输出流，os write（）写入数据

    ​      		 Server 创建ServerSocket 得到ss ，ss.axcept （） 对象， 得到 socket 对象 s，s.getIntputStream () 得到 IntputStream 对象

    UDP ：CLient 创建DataGramSocket 对象  ds ，创建DataGramePackage 对象构造函数传参（字节数组，服务器地址，端口号）得到dp对象

    ​          ds.send(dp)

    Server 端  创建DataGramSocket 构造函数（端口号）对象  ds，创建DataGramePackage 对象构造函数传参（接收字节数组），ds.recive(dp) 

33. Byte和byte[]数组，“表示一个 8 位无符号整数, 一般为8位二进制数”。

34. JVM类加载机制，有三种

    1、全盘负责： 需要加载当前类的所有关联类，引用类都全部加载

    2、双亲委派：优先加载当前类的父类，如果加载失败就加载父类的父类，期间如果成功加载某个父类就返回，如果都失败就加载当前类

    3、缓存机制： 先在缓存寻找是否有已经创建的对象，如果有就返回，如果没有就新创建一个对象

    

    

    
