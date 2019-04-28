###  Unsafe 与 CAS(比较交换)

Java无法直接访问底层操作系统，是通过本地（native）方法来访问的。不过在JDk中还是有一个特例，Unsafe，它提供了硬件级别的原子操作。

这个类尽管里面的方法都是public的，但是并没有办法使用它们，JDK API文档也没有提供任何关于这个类的方法的解释。Unsafe类的使用都是受限制的，只有授信的代码才能获得该类的实例，当然JDK库里面的类是可以随意使用的。

CAS，Compare and Swap即比较并交换，设计并发算法时常用到的一种技术。

CAS有三个操作数：内存值V、旧的预期值A、要修改的值B，当且仅当预期值A和内存值V相同时，将内存值修改为B并返回true，否则什么都不做并返回false。

CAS也是通过Unsafe实现的。

    public final native boolean compareAndSwapObject(Object var1, long var2, Object var4, Object var5);

    public final native boolean compareAndSwapInt(Object var1, long var2, int var4, int var5);

    public final native boolean compareAndSwapLong(Object var1, long var2, long var4, long var6);


CAS都是硬件级别的操作，因此效率会高一些


java.util.concurrent.atomic包下的原子操作类都是基于CAS实现的

CAS操作的"ABA"问题。java.util.concurrent包为了解决这个问题，提供了一个带有标记的原子引用类"AtomicStampedReference"。</br>
通过控制变量的版本来保证CAS的正确性。大部分情况下ABA问题并不会影响程序并发的正确性，如果需要解决ABA问题，使用传统的互斥同步可能会比原子类更加高效。