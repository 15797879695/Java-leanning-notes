集合

1. set  list   map 区别

   set  无序，不允许重复，Hashset  无序，不重复

   ​                                          Linkedset 按插入排序，不重复

   ​                                          Shortedset 可排序，不重复

   List  有序，可以重复， ArrayList 可重复，按下标排序，线程不安全，LinkedList 可重复，按指针排序，线性不安全

   map 键值对，键唯一，值不唯一，其实key就是set， 值就是list， Hashmap， 线程不安全，hashtable 线程安全

2. Hashmap 跟Hashtable 区别

   - 线程安全性不同。HashMap线程不安全；Hashtable 中的方法是Synchronize的。

   - key、value是否允许null。HashMap的key和value都是可以是null，key只允许一个null；Hashtable的key和value都不可为null。

   - 迭代器不同。HashMap的Iterator是fail-fast迭代器；Hashtable还使用了enumerator迭代器。

      enumerator 迭代器可以同步，fail-fast 不可以同步，enumerator  不可以修改数据，fail-fast 可以修改数据    enumerator  ：翻译

   - hash的计算方式不同。HashMap计算了hash值；Hashtable使用了key的hashCode方法。

   - 默认初始大小和扩容方式不同。HashMap默认初始大小16，容量必须是2的整数次幂，扩容时将容量变为原来的2倍；Hashtable默认初始大小11，扩容时将容量变为原来的2倍加1。

   - 是否有contains方法。HashMap没有contains方法；Hashtable包含contains方法，类似于containsValue。

   - 父类不同。HashMap继承自AbstractMap；Hashtable继承自Dictionary。

3. ArrayList跟Linkedlist 区别

   - ArrayList 基于动态数组实现的非线程安全的集合；LinkedList 基于双向链表实现的非线程安全的集合。
   - 扩容问题：ArrayList 使用数组实现，无参构造函数默认初始化长度为 10，数组扩容是会将原数组中的元素重新拷贝到新数组中，长度为原来的 1.5 倍(扩容代价高)；LinkedList 不存在扩容问题，新增元素放到集合尾部，修改相应的指针节点即可。
   - LinkedList 比 ArrayList 更占内存，因为 LinkedList 为每一个节点存储了两个引用节点，一个指向前一个元素，一个指向下一个元素。
   - 对于随机 index 访问的 get 和 set 方法，一般 ArrayList 的速度要优于 LinkedList。因为 ArrayList 直接通过数组下标直接找到元素；LinkedList 要移动指针遍历每个元素直到找到为止。
   - 新增和删除元素，一般 LinkedList 的速度要优于 ArrayList。因为 ArrayList 在新增和删除元素时，可能扩容和复制数组；LinkedList 实例化对象需要时间外，只需要修改节点指针即可。
   - LinkedList 集合不支持高效的随机访问（RandomAccess）
   - ArrayList 的空间浪费主要体现在在list列表的结尾预留一定的容量空间；LinkedList 的空间花费则体现在它的每一个元素都需要消耗存储指针节点对象的空间。

   都是非线程安全，允许存放 null

4. 使用Hashmap  还是Treemap 情况

     都是非线程安全。

   - HashMap基于散列桶（数组和链表）实现；TreeMap基于红黑树实现。
   - HashMap不支持排序；TreeMap默认是按照Key值升序排序的，可指定排序的比较器，主要用于存入元素时对元素进行自动排序。
   - HashMap大多数情况下有更好的性能，尤其是读数据。在没有排序要求的情况下，使用HashMap。

5. 数组和List入户互转

   数组转换List 时使用Arrays类的asList（）方法

   eg：String  【】str ={“aaa”,"bbb","cc"}

   ​        List<String> list=Arrarys.asList(str);

   ​        for(String s：list){

   ​         system.out.printin(s)}

   List 转换数组使用List 的toArrays()方法

   List<String> list=Arrarys.asList("aaa","bbb","ccc");

   String  []str =list.toArrays(new String[list.size()])

   for(String s: str){

   system.out.printin(s)

   }

6. Queue 队列添加元素方法使用 add（）和offer（）方法区别

   - Queue 中 add() 和 offer() 都是用来向队列添加一个元素。
   - 在容量已满的情况下，add() 方法会抛出IllegalStateException异常，offer() 方法只会返回 false 。

7. Queue  remove（）方法和poll（）方法区别

   - Queue 中 remove() 和 poll() 都是用来从队列头部删除一个元素。
   - 在队列元素为空的情况下，remove() 方法会抛出NoSuchElementException异常，poll() 方法只会返回 null 。

8. Queue element （）方法和peek（）方法区别

   - Queue 中 element() 和 peek() 都是用来返回队列的头元素，不删除。
   - 在队列元素为空的情况下，element() 方法会抛出NoSuchElementException异常，peek() 方法只会返回 null。

9. 哪些集合类是线程安全的

   Hashtable  vector  Stack 

    java.util.concurrent 包下所有的集合类 ArrayBlockingQueue、ConcurrentHashMap、ConcurrentLinkedQueue、ConcurrentLinkedDeque... 

10. 迭代器  iterator 是什么

    - 首先说一下迭代器模式，它是 Java 中常用的设计模式之一。用于顺序访问集合对象的元素，无需知道集合对象的底层实现。
    - Iterator 是可以遍历集合的对象，为各种容器提供了公共的操作接口，隔离对容器的遍历操作和底层实现，从而解耦。
    - 缺点是增加新的集合类需要对应增加新的迭代器类，迭代器类与集合类成对增加。

11. iterator 如何使用

    next（）返回下一个元素

    hashnext （）判断是否有下个元素

    remove（）删除元素

    eg：List list=new  ArrayList （）；

    ​         list.add("111");

    ​         list.add("111");

    ​           list.add("333");

    ​       iterator  it=list.iterator()

    ​       while(it.hashnext ){

    ​       if("222"equals(it.next())){

    ​     it.remove()

    }

    }

12. Iterator 跟ListIterator有什么区别

    - ListIterator 继承 Iterator
    - ListIterator 比 Iterator多方法

    1） add(E e)  将指定的元素插入列表，插入位置为迭代器当前位置之前
    2） set(E e)  迭代器返回的最后一个元素替换参数e
    3） hasPrevious()  迭代器当前位置，反向遍历集合是否含有元素
    4） previous()  迭代器当前位置，反向遍历集合，下一个元素
    5） previousIndex()  迭代器当前位置，反向遍历集合，返回下一个元素的下标
    6） nextIndex()  迭代器当前位置，返回下一个元素的下标

    - 使用范围不同，Iterator可以迭代所有集合；ListIterator 只能用于List及其子类
    - ListIterator 有 add 方法，可以向 List 中添加对象；Iterator 不能
    - ListIterator 有 hasPrevious() 和 previous() 方法，可以实现逆向遍历；Iterator不可以
    - ListIterator 有 nextIndex() 和previousIndex() 方法，可定位当前索引的位置；Iterator不可以
    - ListIterator 有 set()方法，可以实现对 List 的修改；Iterator 仅能遍历，不能修改

13. 怎么确保集合不被修改

    使用Connections 类的 unmodifiable ***（）方法赋值原集合

    eg：

    ```javascript
    	static List<String> list = new ArrayList<String>();
    	static Set<String> set = new HashSet<String>();
    	static Map<String, String> map = new HashMap<String, String>();
        	list = Collections.unmodifiableList(list);
    		set = Collections.unmodifiableSet(set);
    		map = Collections.unmodifiableMap(map);
     //修改集合时会报错
    	
    ```

14. 为什么基本数据类型不能作为Hashmap 键值对的类型

    - Java中是使用泛型来约束 HashMap 中的key和value的类型的，HashMap<K, V>
    - 泛型在Java的规定中必须是对象Object类型的，基本数据类型不是Object类型，不能作为键值
    - map.put(0, "ConstXiong")中编译器已将 key 值 0 进行了自动装箱，变为了 Integer 类型

15. Hashmap 需要注意什么

    HashMap 的 key 相等的条件是，条件 1 必须满足，条件2和3必须满足一个。

    1. key 的 hash 值相等
    2. 内存中是同一个对象，即使用 == 判断 key 相等
    3. key 不为 null， 且使用 equals 判断 key 相等

    所以自定义类作为 HashMap 的 key，需要注意按照自己的设计逻辑，重写自定义类的 hashCode() 方法和 equals() 方法。

16. Treeset 原理是什么，需要注意什么

     TreeSet 基于 TreeMap 实现，TreeMap 基于红黑树实现 

    特点：

    - 有序
    - 无重复
    - 添加、删除元素、判断元素是否存在，效率比较高，时间复杂度为 O(log(N))

17. Hashset 原理，特点

    - HashSet 是基于 HashMap 实现的，查询速度特别快
    - HashMap 是支持 key 为 null 值的，所以 HashSet 支持添加 null 值
    - HashSet 存放自定义类时，自定义类需要重写 hashCode() 和 equals() 方法，确保集合对自定义类的对象的唯一性判断(具体判断逻辑，见 HashMap put() 方法，简单概括就是 key 进行 哈希。判断元素 hash 值是否相等、key 是否为同个对象、key 是否 equals。第 1 个条件为 true，2、3 有一个为 true，HashMap 即认为 key 相同)
    - 无序、不可重复

18. Hashmap跟hashset 区别

    **HashMap**

    - 实现 Map 接口
    - 键值对的方式存储
    - 新增元素使用 put(K key, V value) 方法
    - 底层通过对 key 进行 hash，使用数组 + 链表或红黑树对 key、value 存储

     

    **HashSet**

    - 实现 Set 接口
    - 存储元素对象
    - 新增元素使用 add(E e) 方法
    - 底层是采用 HashMap 实现，大部分方法都是通过调用 HashMap 的方法来实现

19.  Treemap和Treeset如何实现排序

    - TreeMap 会对 key 进行比较，有两种比较方式，第一种是构造方法指定 Comparator，使用 Comparator#compare() 方法进行比较；第二种是构造方法未指定 Comparator 接口，需要 key 对象的类实现 Comparable 接口，用 Comparable #compareTo() 方法进行比较
    - TreeSet 底层是使用 TreeMap 实现

20. List ，set   map  特点，常用类

    **List：**

    - 允许重复的对象
    - 可以插入多个 null 元素
    - 是有序容器，保持了每个元素的插入顺序
    - 常用的实现类有 ArrayList、LinkedList 和 Vector。ArrayList，它提供了使用索引的随意访问，LinkedList 更合适经常添加或删除元素的场景

     

    **Set：**

    - 不允许重复对象
    - 只允许一个 null 元素
    - Set 接口最常用的几个实现类是 HashSet、LinkedHashSet 以及 TreeSet。HashSet 基于 HashMap 实现；LinkedHashSet 按照插入排序；TreeSet 通过 Comparator 或 Comparable 接口实现排序

     

    **Map：**

    - 是单独的顶级接口，不是 Collection 的子接口
    - Map 的 每个 Entry 都持有两个对象，key 和 value，key 唯一，value 可为 null 或重复
    - Map 接口常用的实现类有 HashMap、LinkedHashMap、Hashtable 和 TreeMap
    - Hashtable 和 未指定 Comparator 的 TreeMap 不可为 null；HashMap、LinkedHashMap、指定 Comparator 的 TreeMap 的 key 可以为 null

21. Hashmap实现原理

    - HashMap 基于 Hash 算法实现，通过 put(key,value) 存储，get(key) 来获取 value
    - 当传入 key 时，HashMap 会根据 key，调用 hash(Object key) 方法，计算出 hash 值，根据 hash 值将 value 保存在 Node 对象里，Node 对象保存在数组里
    - 当计算出的 hash 值相同时，称之为 hash 冲突，HashMap 的做法是用链表和红黑树存储相同 hash 值的 value
    - 当 hash 冲突的个数：小于等于 8 使用链表；大于 8 时，使用红黑树解决链表查询慢的问题
    
22. hashmap 实现原理

      1、Node[] table的初始化长度length(默认值是16)，Load factor为负载因子(默认值是0.75)，threshold  翻译：门槛 是HashMap所能容纳的最[大数据](http://lib.csdn.net/base/hadoop)量的Node(键值对)个数。threshold = length * Load factor。也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数越多。

      2、确定哈希桶数组（node 数组） 索引位置，Hash算法本质上就是三步：取key的hashCode值、高位运算、取模运算。JDK1.8的实现中，优化了高位运算的算法，通过hashCode()的高16位异或低16位实现的

      3、put 实现原理

      1. 对key的hashCode()做hash，然后再计算index;
      2. 如果没碰撞直接放到bucket里；
      3. 如果碰撞了，以链表的形式存在buckets后；
      4. 如果碰撞导致链表过长(大于等于TREEIFY_THRESHOLD 默认等于8 )，就把链表转换成红黑树；
      5. 如果节点已经存在就替换old value(保证key的唯一性)
      6. 如果bucket满了(超过load factor*current capacity)，就要resize。 

      1. 什么时候会使用HashMap？他有什么特点？

         > 是基于Map接口的实现，存储键值对时，它可以接收null的键值，是非同步的，HashMap存储着Entry(hash, key, value, next)对象。

      2. 你知道HashMap的工作原理吗？

         > 通过hash的方法，通过put和get存储和获取对象。存储对象时，我们将K/V传给put方法时，它调用hashCode计算hash从而得到bucket位置，进一步存储，HashMap会根据当前bucket的占用情况自动调整容量(超过Load Facotr则resize为原来的2倍)。获取对象时，我们将K传给get，它调用hashCode计算hash从而得到bucket位置，并进一步调用equals()方法确定键值对。如果发生碰撞的时候，Hashmap通过链表将产生碰撞冲突的元素组织起来，在[Java ](http://lib.csdn.net/base/java)8中，如果一个bucket中碰撞冲突的元素超过某个限制(默认是8)，则使用红黑树来替换链表，从而提高速度。

      3. 你知道get和put的原理吗？equals()和hashCode()的都有什么作用？

         > 通过对key的hashCode()进行hashing，并计算下标( n-1 & hash)，从而获得buckets的位置。如果产生碰撞，则利用key.equals()方法去链表或树中去查找对应的节点

      4. 你知道hash的实现吗？为什么要这样实现？

         > 在Java 1.8的实现中，是通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，主要是从速度、功效、质量来考虑的，这么做可以在bucket的n比较小的时候，也能保证考虑到高低bit都参与到hash的计算中，同时不会有太大的开销。

      5. 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？

         > 如果超过了负载因子(默认0.75)，则会重新resize一个原来长度两倍的HashMap，并且重新调用hash方法。 
         > 关于Java集合的小抄中是这样描述的： 
         > 以Entry[]数组实现的哈希桶数组，用Key的哈希值取模桶数组的大小可得到数组下标。 
         > 插入元素时，如果两条Key落在同一个桶(比如哈希值1和17取模16后都属于第一个哈希桶)，Entry用一个next属性实现多个Entry以单向链表存放，后入桶的Entry将next指向桶当前的Entry。 
         > 查找哈希值为17的key时，先定位到第一个哈希桶，然后以链表遍历桶里所有元素，逐个比较其key值。 
         > 当Entry数量达到桶数量的75%时(很多文章说使用的桶数量达到了75%，但看代码不是)，会成倍扩容桶数组，并重新分配所有原来的Entry，所以这里也最好有个预估值。 
         > 取模用位运算(hash & (arrayLength-1))会比较快，所以数组的大小永远是2的N次方， 你随便给一个初始值比如17会转为32。默认第一次放入元素时的初始值是16。 
         > iterator()时顺着哈希桶数组来遍历，看起来是个乱序。

      6. 当两个对象的hashcode相同会发生什么？

         > 因为hashcode相同，所以它们的bucket位置相同，‘碰撞’会发生。因为HashMap使用链表存储对象，这个Entry(包含有键值对的Map.Entry对象)会存储在链表中。

      7. 如果两个键的hashcode相同，你如何获取值对象？

         > 找到bucket位置之后，会调用keys.equals()方法去找到链表中正确的节点，最终找到要找的值对象。因此，设计HashMap的key类型时，如果使用不可变的、声明作final的对象，并且采用合适的equals()和hashCode()方法的话，将会减少碰撞的发生，提高效率。不可变性能够缓存不同键的hashcode，这将提高整个获取对象的速度，使用String，Interger这样的wrapper类作为键是非常好的选择

      8. 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？

         > 默认的负载因子大小为0.75，也就是说，当一个map填满了75%的bucket时候，和其它集合类(如ArrayList等)一样，将会创建原来HashMap大小的两倍的bucket数组，来重新调整map的大小，并将原来的对象放入新的bucket数组中。这个过程叫作rehashing，因为它调用hash方法找到新的bucket位置

      9. 你了解重新调整HashMap大小存在什么问题吗？

         > 当重新调整HashMap大小的时候，确实存在条件竞争，因为如果两个线程都发现HashMap需要重新调整大小了，它们会同时试着调整大小。在调整大小的过程中，存储在链表中的元素的次序会反过来，因为移动到新的bucket位置的时候，HashMap并不会将元素放在链表的尾部，而是放在头部，这是为了避免尾部遍历(tail traversing)。如果条件竞争发生了，那么就死循环了。因此在并发环境下，我们使用CurrentHashMap来替代HashMap

      10. 为什么String, Interger这样的wrapper类适合作为键？

          > 因为String是不可变的，也是final的，而且已经重写了equals()和hashCode()方法了。其他的wrapper类也有这个特点。不可变性是必要的，因为为了要计算hashCode()，就要防止键值改变，如果键值在放入时和获取时返回不同的hashcode的话，那么就不能从HashMap中找到你想要的对象。不可变性还有其他的优点如线程安全。如果你可以仅仅通过将某个field声明成final就能保证hashCode是不变的，那么请这么做吧。因为获取对象的时候要用到equals()和hashCode()方法，那么键对象正确的重写这两个方法是非常重要的。如果两个不相等的对象返回不同的hashcode的话，那么碰撞的几率就会小些，这样就能提高HashMap的性能

23. ConcurrentHashMap 问题

        1、ConcurrentHashMap有哪些构造函数？

        一共有五个，作用及代码如下：

        ![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

        [![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

        ```
            //无参构造函数
            public ConcurrentHashMap() {
            }
            //可传初始容器大小的构造函数
            public ConcurrentHashMap(int initialCapacity) {
                if (initialCapacity < 0)
                    throw new IllegalArgumentException();
                int cap = ((initialCapacity >= (MAXIMUM_CAPACITY >>> 1)) ?
                           MAXIMUM_CAPACITY :
                           tableSizeFor(initialCapacity + (initialCapacity >>> 1) + 1));
                this.sizeCtl = cap;
            }
            //可传入map的构造函数
            public ConcurrentHashMap(Map<? extends K, ? extends V> m) {
                this.sizeCtl = DEFAULT_CAPACITY;
                putAll(m);
            }
            //可设置阈值和初始容量
            public ConcurrentHashMap(int initialCapacity, float loadFactor) {
                this(initialCapacity, loadFactor, 1);
            }
        
            //可设置初始容量和阈值和并发级别
            public ConcurrentHashMap(int initialCapacity,
                                     float loadFactor, int concurrencyLevel) {
                if (!(loadFactor > 0.0f) || initialCapacity < 0 || concurrencyLevel <= 0)
                    throw new IllegalArgumentException();
                if (initialCapacity < concurrencyLevel)   // Use at least as many bins
                    initialCapacity = concurrencyLevel;   // as estimated threads
                long size = (long)(1.0 + (long)initialCapacity / loadFactor);
                int cap = (size >= (long)MAXIMUM_CAPACITY) ?
                    MAXIMUM_CAPACITY : tableSizeFor((int)size);
                this.sizeCtl = cap;
            }
        ```

        [![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

         

        2、ConcurrentHashMap使用什么技术来保证线程安全？

         

        jdk1.7：Segment+HashEntry来进行实现的；

        jdk1.8：放弃了Segment臃肿的设计，采用Node+CAS+Synchronized来保证线程安全；

         

        3、ConcurrentHashMap的get方法是否要加锁，为什么？

        不需要，get方法采用了unsafe方法，来保证线程安全。

         

        4、ConcurrentHashMap迭代器是强一致性还是弱一致性？HashMap呢？

        弱一致性，hashmap强一直性。

        ConcurrentHashMap可以支持在迭代过程中，向map添加新元素，而HashMap则抛出了ConcurrentModificationException，

        因为HashMap包含一个修改计数器，当你调用他的next()方法来获取下一个元素时，迭代器将会用到这个计数器。

         

        5、ConcurrentHashMap1.7和1.8的区别：

        jdk1.8的实现降低锁的粒度，jdk1.7锁的粒度是基于Segment的，包含多个HashEntry，而jdk1.8锁的粒度就是Node

        数据结构：jdk1.7 Segment+HashEntry；jdk1.8 数组+链表+红黑树+CAS+synchronized

      24、ConcurentHashmap put 方法

      **1、1.7之前**

      当执行put操作时，会经历两个步骤：

      1. 判断是否需要扩容
      2. 定位到添加元素的位置，将其放入 HashEntry 数组中

      插入过程会进行第一次 key 的 hash 来定位 Segment 的位置，如果该 Segment 还没有初始化，即通过 CAS 操作进行赋值，然后进行第二次 hash 操作，找到相应的 HashEntry 的位置，这里会利用继承过来的锁的特性，在将数据插入指定的 HashEntry 位置时（尾插法），会通过继承 ReentrantLock 的 `tryLock()` 方法尝试去获取锁，如果获取成功就直接插入相应的位置，如果已经有线程获取该Segment的锁，那当前线程会以自旋的方式去继续的调用 `tryLock()` 方法去获取锁，超过指定次数就挂起，等待唤醒。

      2、1.8之后

      **整体流程：**
       1）首先对于每一个放入的值，首先利用spread方法对key的hashcode进行一次hash计算，获取在table数组的索引下标地址
       2）如果当前table还没有初始化，先调用`initTable()`进行初始化
       3）如果该位置为null,说明还没有数据放入，则利用CAS操作直接放入
       4）如果不为空，说明存在哈希碰撞，当fh==MOED(-1),说明数组正在扩容
       5）当数组不在扩容状态，对该节点利用`sychronized`加锁，然后再进行一次判断当前节点是否发生变化，没有变化执行下面的方法；发生了变化直接跳转到第8步
       6）如果是链表节点（fh>0）,开始遍历链表节点，如果key相等，则进行值覆盖；如果都没有到节点尾部插入新节点
       7）如果这个节点类型是TreeBin，利用红黑树的方法插入新的节点。
       8）如果链表长度大于8，则利用`treeifyBin`把这个链表转化为红黑树，但是不是大于8就转化为红黑树，当数组长度小于`MIN_TREEIFY_CAPACITY`(默认是64)时，进行扩容操作。
       9）如果当前实际大小数量+1超过了临界值，就进行扩容
       **spread()方法** 计算hash,主要是将key的hashcode的低16位和高16位进行异或运算，0x7fffffff主要是 用于和负数hash值进行 & 运算，将其转化为正数（绝对值不相等）。