### 有T1、T2、T3三个线程，你怎样保证T2在T1执行完后执行，T3在T2执行完后执行？

用线程的join()方法，
	
	t1.start(); 
	t1.join();
	
	t2.start(); 
	t2.join();
	
	t3.start(); 
	t3.join();

就是阻塞线程（阻塞队列）


### 在Java中Lock接口比synchronized块的优势是什么？你需要实现一个高效的缓存，它允许多个用户读，但只允许一个用户写，以此来保持它的完整性，你会怎样去实现它？

能够显示地获取和释放锁，锁的运用更灵活，可以方便地实现公平锁，能够响应中断，非阻塞获取锁

java.util.concurrent.locks包下面ReadWriteLock接口,该接口下面的实现类ReentrantReadWriteLock维护了两个锁读锁和写锁，可用该类实现这个功能。

    //读写锁
	private static ReadWriteLock rwLock = new ReentrantReadWriteLock();
    ...
    rwLock.readLock().lock();//读锁开启，读进程均可进入
    ...
    rwLock.writeLock().lock();//写锁开启，这时只有一个写线程进入


### 在java中wait和sleep方法的不同

sleep()方法属于Thread类中的,wait()方法属于Object类中的。在等待时wait会释放锁，而sleep一直持有锁。Wait通常被用于线程间交互，sleep通常被用于暂停执行。

### 用Java实现阻塞队列

阻塞队列的实现原理，事实它和我们用 Object.wait()、Object.notify()和非阻塞队列实现生产者-消费者的思路类似，只不过它把这些工作一起集成到了阻塞队列中实现。


就是在队列中对 数据的操作加了等待唤醒操作，实现了线程间的通信。

### Java编程怎么解决一个会导致死锁的程序

不用显示的锁</br>
信号量可以控制资源能被多少线程访问，这里我们指定只能被一个线程访问，就做到了类似锁住。而信号量可以指定去获取的超时时间，我们可以根据这个超时时间，去做一个额外处理。

### 什么是原子操作，Java中的原子操作是什么

原子操作(atomic operation)是指不会被线程调度机制打断的操作,java中一般事务管理里面用到原子操作


###  Java中的volatile关键是什么作用？怎样使用它？在Java中它跟synchronized方法有什么不同？

volatile该关键字是主要使用的场合多线程读取共享变量的时候可以获取到最新的值使用。不能保障原子性

volatile不能保证操作的原子性。synchronized可以保证操作的原子性。

此外 volatile是变量修饰符，而synchronized是要修饰一段代码或者方法

### 为什么我们调用start()方法时会执行run()方法，为什么我们不能直接调用run()方法？

调用start()方法时你将创建新的线程，并且执行在run()方法里的代码

直接调用run() 方法相当于一个普通函数的调用


### Java中怎样唤醒一个阻塞的线程

线程遇到了IO阻塞，我并且不认为有一种方法可以中止线程。如果线程因为调用wait()、sleep()、或者join()方法而导致的阻塞，你可以中断线程，并且通过抛出InterruptedException来唤醒它

### 在Java中CycliBarriar和CountdownLatch有什么区别？

CyclicBarrier可以重复使用已经通过的障碍，而CountdownLatch不能重复使用。

