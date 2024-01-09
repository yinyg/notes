1、谈谈你对Java平台的理解？

1. Java的两大特征：跨平台（编译成字节码、JVM解释执行、JIT及时编译）、垃圾收集（serial、parnew、parallel、CMS、G1、ZGC）；
2. JVM即Java运行环境和一些Java基础类库（集合、IO、网络、并发、安全等），JDK是JVM的超集，提供了编译器、诊断工具（jmap、jstack、jinfo、jstat）等更多工具。



2、Exception和Error有什么区别？

1. Exception和Error都继承自Throwable。Exception一般用于正常的、可预料的异常情况，程序可以恢复；Error一般用于非正常的，不可预料的异常情况，如OutOfMemoryError，程序无法恢复；
2. Exception分为检查型异常和不检查异常，检查型异常如IOException必须显式处理或者继续上抛给上层处理，不检查异常如RuntimeException及其子类可以不用显式处理；
3. 常见的Exception和Error：

​			Exception：

​				检查型：IOException、ClassNotFoundException（反射获取类的Class对象时）

​				不检查：RuntimeExeption、NullPointException、IllegalArgumentException、ClassCastException

​			Error：NoClassDefFoundError（new等操作时找不到类，可能是编译后文件丢失或被篡改）、OutOfMemoryError、                      						 StackOverflowError

4. 异常处理原则：

​			a. 尽量捕获特定的异常，而不是捕获Exception，一般也不去捕获Error或Throwable；

​			b. 不要生吞异常，不要catch块里忽略异常的处理；

​			c. 选择合适的选项输出异常信息，不要使用System.out.print()、printStackTrace()来输出异常信息；

​			d. Throw early, catch late原则。



3、谈谈final、finally、finalize有什么不同？

1. final用来修饰类、方法、变量，修饰类时，该类不可以被继承，修饰方法时，该方法不可以被重写，修饰变量时，该变量不可以被修改（基本类型的值不可以被修改，引用类型的引用不可以被修改，引用类型的行为不受影响）；
2. finally用来保证代码块一定会被执行，常用try-finally、try-catch-finally来对资源进行回收，也有一些特殊情况，如try块或catch块执行了System.exit时，finally代码块不会被执行；
3. finalize是Object类的一个方法，该方法会在对象被GC回收时执行一次，但是该方法什么时候执行，执行的是否符合预期，都是不确定的，因此不推荐使用finalize。



4、强引用、软引用、弱引用、幻象引用有什么区别？

1. 强引用指对象被直接引用，强引用对象处于强可达状态，不会被垃圾收集器回收；
2. 软引用指被SoftRefernece引用的对象，软引用对象处于软可达状态，如果垃圾收集器能回收足够的内存，则不会回收软引用对象；
3. 弱引用指被WeakReference引用的对象，弱引用对象处于弱可达状态，弱引用对象会直接被垃圾收集器回收；
4. 幻象引用或虚引用指被PhantomReference引用的对象，幻象引用对象处于幻象可达状态，幻象引用的get方法返回null，幻象引用对象已经执行过finalize()方法，会被垃圾收集器直接回收。



5、String、StringBuffer、StringBuilder有什么区别？

1. String是一个不可变类，String类被final修饰，其属性也被final修饰，因此一个String类型的对象一旦创建，就不可以被修改，只能为变量创建新的对象；
2. StringBuffer底层是一个可以修改的字符数组，可以动态的修改，可以调用toString()方法获得一个String对象，StringBuilder本质上跟StringBuffer没有区别，只是StringBuffer的方法被synchronize修饰，是线程安全的，而StringBuilder不是线程安全的，如果不存线程安全问题，推荐使用StringBuilder，因为保证线程安全会带来额外的开销。



6、动态代理是基于什么原理？

1. Java中有3中代理类的实现方式，分别是静态代理、jdk动态代理、cglib动态大理代理。静态代理需要开发人员自己实现代理类，jdk动态代理基于反射和接口，需要被代理的类必须实现接口，而cglib则没有这个限制；
2. 使用动态代理时，程序在运行时动态的创建代理类，并创建代理对象，对被代理对象的行为进行增强，程序实际上调用的是代理对象的方法，代理对象会调用被代理对象的方法。
3. 动态代理可以对某些行为解耦，比如日志、监控、安全等行为，被代理类无需关注这些行为，spring aop也是通过动态代理实现的；
4. jdk提供的动态代理无需引入额外的依赖，且更加可靠，但是被代理类必须实现接口；而cglib实现的动态代理没有实现接口这个限制，且性能更高。



7、int和Integer有什么区别？

1. int是是基本类型，Integer是引用类型，Java实现了自动拆箱和自动装箱，开发人员可以直接吧int赋值给Integer，也可以把Integer直接赋值给int；
2. Integer等包装类实现了缓存，自动装箱时，用到了Integre.valueOf()，这里Java实现了缓存，默认的缓存范围是-128刀127；
3. 无论是int还是Integer都不能保证线程安全，如果需要保证线程安全，Java提供了原子操作类AtomicInteger，对该类对象的操作是原子操作，从而能够保证线程安全；
4. int等基本类型有其局限性，不能用在泛型上，如果需要在泛型中表达该类型，只能使用对应的包装类。



8、对比Vector、ArrayList、LinkedList有何区别？

1. 它们都实现了List接口，都是有序的集合；

2. Vector 、ArrayList底层是用数组实现的，根据下标随机访问的时间复杂度是O(1)，在末尾插入或删除的均摊时间复杂度是O(1)，而在随机位置插入的时间复杂度是O(n)，都可以自动扩容，区别在于：

   ​	1）Vector默认扩容一倍，ArrayList扩容50%；

   ​	2）Vector和ArrayList支持的操作基本相似，不同的是Vector的操作都是线程安全的，都被synchronized修饰；

3. LinkedList底层是用双向链表实现的，根据下标随机访问，需要从链表头部开始遍历，时间复杂度是O(k)，插入删除的时间复杂度是O(1)，但是找到该节点需要遍历链表，总的时间复杂度是O(n)，LinkedList的操作不是线程安全的；

4. 根据需求，一般随机访问操作多的场景，选择使用ArrayList，如果需要线程安全，使用Vector，而插入、删除更多的场景，选择使用LinkedList。

