#### java基础    

    https://juejin.im/post/5de8ff69e51d455828471a2c
##### 1.equal()和 hashcode()的区别?

    https://blog.csdn.net/wonad12/article/details/78958411
    https://www.cnblogs.com/skywang12345/p/3324958.html
总结 ：== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；而 equals 默认情况下是引用比较，只是很多类重新了 equals 方法，比如 String、Integer 等把它变成了值比较，所以一般情况下 equals 比较的是值是否相等

    equals()：
         作用：用来判断两个对象是否相等。
         1. 没有重写equals()方法，默认判断指针是否相等。
         2. 重写equals() 方法，判断对象内容是否相等。
         注意：String类，Integer包装类等已经重写了equals()方法，判断值是否相等。
    hashCode()：
        作用：确定该类的每一个对象在散列表中的位置。

##### 2.hashCode() 和 equals() 之间有什么联系？

    要说清楚两者关系，得从创建类说起。
    1. 不会创建该类的HashSet集合，就是比如不会存在map<Person>, set<Person>, 两者半毛钱关系都没有。
      equals() 用来比较该类的两个对象是否相等。而hashCode() 则根本没有任何作用。
    
    2.创建该类的HashSet集合
        1)、如果两个对象相等，那么它们的hashCode()值一定相同。
              这里的相等是指，通过equals()比较两个对象时返回true。
        2)、如果两个对象hashCode()相等，它们并不一定相等。
               因为在散列表中，hashCode()相等，即两个键值对的哈希值相等。然而哈希值相等，并不一定能得出键值对相等。
               补充说一句：“两个不同的键值对，哈希值相等”，这就是哈希冲突。
    
        此外，在这种情况下。若要判断两个对象是否相等，除了要覆盖equals()之外，也要覆盖hashCode()函数。否则，equals()无效。
         使用默认hashCode()的话，取到的是对象的地址呀，两个对象地址在内存中肯定不相等，没法继续比较内容了。
    
    注意：如果我们将对象的属性值参与了hashCode的运算中，在进行删除的时候，就不能对其属性值进行修改，否则会出现严重的问题。例如内存泄漏，无法删除原值。

##### 3.equals()有什么特性？        

        1. 对称性：如果x.equals(y)返回是"true"，那么y.equals(x)也应该返回是"true"。
        2. 反射性：x.equals(x)必须返回是"true"。
        3. 类推性：如果x.equals(y)返回是"true"，而且y.equals(z)返回是"true"，那么z.equals(x)也应该返回是"true"。
        4. 一致性：如果x.equals(y)返回是"true"，只要x和y内容一直不变，不管你重复x.equals(y)多少次，返回都是"true"。
        5. 非空性，x.equals(null)，永远返回是"false"；x.equals(和x不同类型的对象)永远返回是"false"。

##### 4.equals() 与 == 的区别是什么？

    “==”： 判断两个对象地址是否相等
    equals(): 判断对象是否相等。
        1.类没有覆盖equals()方法。则通过equals()比较该类的两个对象时，等价于通过“==”比较这两个对象。
        2.类覆盖了equals()方法。一般，我们都覆盖equals()方法来两个对象的内容相等；若它们的内容相等，则返回true(即，认为这两个对象相等)。

##### 5.说说面向对象的设计原则：

        https://juejin.im/post/5b9526c1e51d450e69731dc2
        1.单一职责原则（Single Responsibility Principle）
            每一个类(接口)应该专注于做一件事情。常常可见接口多继承现象。
        2.里氏替换原则（Liskov Substitution Principle）
            超类存在的地方，子类是可以替换的。
    
        3.依赖倒置原则（Dependence Inversion Principle）
            实现尽量依赖抽象(抽象类或者接口)，不依赖具体实现。  
    
        4.接口隔离原则（Interface Segregation Principle）
            每个接口中不存在子类用不到却必须实现的方法，如果不然，就要将接口拆分。使用多个隔离的接口，比使用单个接口（多个接口方法集合到一个的接口）要好。
        5.迪米特法则（Law Of Demeter）
            又叫最少知道原则，一个软件实体应当尽可能少的与其他实体发生相互作用。
        6.开闭原则（Open Close Principle）
            软件实体面向扩展开放，面向修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，而是要扩展原有代码，实现一个热插拔的效果。所以一句话概括就是：为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类等 
        7.组合/聚合复用原则（Composite/Aggregate Reuse Principle CARP）
            尽量使用合成/聚合达到复用，尽量少用继承。原则： 一个类中有另一个类的对象。即多用组合，少用继承。

##### 6.说说面向对象的特性？

        1.封装
            封装是把过程和数据包围起来，对数据的访问只能通过已定义的接口。（通俗点即隐藏信息 提供使用接口给别人使用，不让别人直接操作属性或方法。）
        2.继承
            从已有的类中派生出新的类，新的类能吸收已有类的数据属性和行为，并能扩展新的能力。
        3.多态
            同一个接口，使用不同的实例而执行不同操作。
        4.抽象
            把一个对象分析出各个属性,来替代表达的手法。

##### 7.说说jdk和jre的区别？

    - jdk:JDK：Java Development Kit 的简称，java 开发工具包，提供了 java 的开发环境和运行环境。
    - jre:Java Runtime Environment 的简称，java 运行环境，为 java 的运行提供了所需环境.
    
    具体来说 JDK 其实包含了 JRE，同时还包含了编译 java 源码的编译器 javac，还包含了很多 java 程序调试和分析的工具。简单来说：如果你需要运行 java 程序，只需安装 JRE 就可以了，如果你需要编写 java 程序，需要安装 JDK。



##### 8.final 在 java 中有什么作用？    

    final修饰的类叫最终类，该类不能被继承。
    final修饰的方法不能被重写。
    final修饰的变量叫常量，常量必须初始化，初始化之后值就不能修改了。

 


##### 9.java 中的 Math.round(-1.5) 等于多少？

    等于 -1，因为在数轴上取值时，中间值（0.5）向右取整，所以正 0.5 是往上取整，负 0.5 是直接舍弃
10.String 属于基础的数据类型吗？

    String 不属于基础类型，基础类型有 8 种：byte、boolean、char、short、int、float、long、double，而 String 属于对象。
##### 11.java 中操作字符串都有哪些类？它们之间有什么区别？    

    操作字符串的类有：
        String、StringBuffer、StringBuilder
    区别在于 
        1.String 声明的是不可变的对象，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象。
        2.而 StringBuffer、StringBuilder可以在原有对象的基础上进行操作。
        所以在经常改变字符串内容的情况下最好不要使用 String。
        3.StringBuffer 是线程安全的，而 StringBuilder 是非线程安全的。
        4.StringBuilder 的性能却高于StringBuffer，所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

##### 12.String str="i"与 String str=new String("i")一样吗？    

    不一样，因为内存的分配方式不一样。String str="i"的方式，java 虚拟机会将其分配到常量池中；而 String str=new String("i") 则会被分到堆内存中。
##### 13.普通类和抽象类有哪些区别？    

    1.普通类不能包含抽象方法，抽象类可以包含抽象方法。
    2.抽象类不能直接实例化，普通类可以直接实例化。

##### 14.抽象类能使用 final 修饰吗？    

    不能，定义抽象类就是让其他类继承的，如果定义为 final 该类就不能被继承，这样彼此就会产生矛盾，所以 final 不能修饰抽象类。

##### 15.接口和抽象类有什么区别？    

    1.实现：抽象类的子类使用 extends 来继承；接口必须使用 implements 来实现接口。
    2.构造函数：抽象类可以有构造函数；接口不能有。
    3.main 方法：抽象类可以有 main 方法，并且我们能运行它；接口不能有 main 方法。
    4.实现数量：类可以实现很多个接口；但是只能继承一个抽象类。
    5.访问修饰符：接口中的方法默认使用 public 修饰；抽象类中的方法可以是任意访问修饰符。

##### 16.java 中 IO 流分为几种？    

    按功能来分：输入流（input）、输出流（output）。
    按类型来分：字节流和字符流。
    
    字节流和字符流的区别是：
    
        字节流按 8 位传输以字节为单位输入输出数据，
        字符流按 16 位传输以字符为单位输入输出数据。
##### 17.BIO、NIO、AIO 有什么区别？

    BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
    NIO：New IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
    AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。

##### 18.深拷贝和浅拷贝的区别？

​	浅拷贝：对基本数据类型进行值传递，对引用数据类型进行引用传递般的拷贝，此为浅拷贝。
​	深拷贝：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容，此为深拷贝。

	深度拷贝的方式：
		1.序列化，反序列化得到新对象
		2.继续利用clone()方法浅拷贝，实现Cloneable接口，内部进行浅拷贝，最终达到深度拷贝的效果。
##### 

##### 19.hashmap并发死锁原理

https://www.jianshu.com/p/619a8efcf589

JDK 7 rehash 会倒置链表元素，并发翻转插入到新数组，然后另一个线程又把它翻转回来，导致出现了循环链。

JDK 8 用 head 和 tail 来保证链表的顺序和之前一样；

但是还会有数据丢失等弊端，并发情况下建议使用ConcurrentHashMap。

在并发的情况，发生扩容时，可能会产生循环链表，在执行get的时候，会触发死循环，引起CPU的100%问题，所以一定要避免在并发环境下使用HashMap。



线程不安全：

1.并发时，put数据到数组的同一个位置，发生覆盖。

2.多个线程对node数组进行扩容，重新计算，复制数据到新table，最终只有一个线程修改成功，其他线程的put数据丢失。

##### 20.ConcurrentHashMap底层原理

ConcurrentHashMap所采用的"分段锁"思想。容器中有多把锁，每一把锁锁一段数据，这样在多线程访问时不同段的数据时，就不会存在锁竞争了。



get方法无需加锁，由于其中涉及到的共享变量都使用volatile修饰，volatile可以保证内存可见性，所以不会读取到过期数据。

concurrentHashMap代理到Segment上的put方法，Segment中的put方法是要加锁的。只不过是锁粒度细了而已。ReetranLock.

#####21.exception层次结构

Throwable

​		Error（不可查）

​			VirtualMachineError

​			AWTError

​		Exception

​			IOException（可查）

​				EOFException

​				FileNotFoundException

​			RuntimeException（运行时异常，不可检查，其他的异常都会在编译器可检查出）

​				空指针

​				数组下标越界

​				非法参数

​				。。。

#####22.collection下的直接子接口

Collection集合

​		**Collection接口**

​				**set接口**

​						**HashSet**（无序，不重复）

​				**list接口**

​						**LinkedList**（有序，重复，双向链表）

​						**ArrayList**（有序重复）

​		map接口

​				HashMap（散列表，无序，适合插入，删除，定位）

​				TreeMap(红黑树实现，适合顺序遍历，有序)

​		iterator接口（所有collection接口都有这个接口，可以访问集合中的各个元素，或者删除某个元素）

​				ListIterator

​		comparabel接口（实现了这个方法的类，都可以用comparato()方法，从而确定对象的排序）

​				





