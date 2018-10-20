JAVA面试题（二）——线程
===

1. 线程状态

   - 创建状态：在程序中用构造方法创建了一个线程对象后，新的线程对象就处于创建状态。

   - 就绪状态：线程创建之后，就可以通过调用start()方法启动线程，即进入就绪状态。

   - 运行状态：当就绪状态的线程获得CPU资源是，即可进入运行状态，执行run()方法。

   - 阻塞状态：一个正在运行的线程因某种原因不能继续运行时，进入阻塞状态。

     - 由于线程优先级比较低，因此它不能获得CPU资源。
     - 使用sleep()方法使线程休眠。
     - 通过调用wait()方法，使线程等待。
     - 通过调用yield(),线程显式出让CPU控制权。
     - 线程由于等待一个文件，I/O事件被阻塞。

   - 死亡状态：一个线程的run()方法运行完毕，则进入死亡状态。

     ![线程状态转换图](https://images2015.cnblogs.com/blog/1135816/201705/1135816-20170511195200801-2134368699.png)

   

2. 创建线程

   - 继承Thread类：直接实例化Thread类或者定义一个继承Thread类，重写run()方法，调用start()方法启动线程。

     ```java
     /**
      * 继承Thread类
      */
     public class TestThread extends Thread{
     
         public TestThread(String name){
             super(name);
         }
     
         /**重写run()方法**/
         @Override
         public void run() {
             System.out.println("当前线程名称为："+Thread.currentThread().getName());
         }
     
         public static void main(String[] args) {
             Thread testThread=new TestThread("new Thread");//实例化Thread类
             //启动线程   注：一个Thread类的start()方法不能让一个实例进行多次调用，否则会出现：IllegalThreadStateException异常
             testThread.start(); 
         }
     
     }
     ```

     

   - 实现Runnable接口：实例化实现Runnable接口的类，使用Thread构造方法对Thread对象进行创建并实例化，最后调用Thread实例中的start()方法启动线程。

     ```java
     /**
      * 实现Runnable接口
      */
     public class TestRunnable implements Runnable{
     
         /**重写run()方法**/
         @Override
         public void run() {
             System.out.println("当前线程名称为："+Thread.currentThread().getName());
         }
     
         public static void main(String[] args) {
             TestRunnable testRunnable=new TestRunnable();//实例化实现Runnable接口的TestRunnable类
             Thread thread = new Thread(testRunnable,"newRunnable");//使用构造方法对Thread进行创建并实例化 
             //启动线程   注：一个Thread类的start()方法不能让一个实例进行多次调用，否则会出现：IllegalThreadStateException异常
             thread.start();
             System.out.println("主线程名称为："+Thread.currentThread().getName());
         }
     
     }
     ```

     

   - 两种方法的区别：

     - 如果一个类继承Thread，则不适合资源共享。但是如果实现了Runable接口的话，则很容易的实现资源共享。
     - 实现Runnable接口比继承Thread类所具有的优势：
       1. 适合多个相同的程序代码的线程去处理同一个资源
       2. 可以避免java中的单继承的限制
       3. 增加程序的健壮性，代码可以被多个线程共享，代码和数据独立
       4. 线程池只能放入实现Runable或callable类线程，不能直接放入继承Thread的类

3. sleep、wait、yield、join的用法区别

   - start：start()方法的调用后并不是立即执行线程代码，而是使得该线程变为可运行态（Runnable），什么时候运行是由操作系统决定的。

   - sleep： 在指定的毫秒数内让当前正在执行的线程休眠（暂停执行）。
   - yield（让步）：让当前运行线程回到可运行状态，以允许具有相同优先级的其他线程获得运行机会。
   - join：等待该线程终止。
     - 在子线程调用了join()方法后面的代码，只有等到子线程结束了才能执行。
     - 在很多情况下，主线程生成并启动了子线程，如果子线程里要进行大量的耗时的运算，主线程往往将于子线程之前结束，但是如果主线程处理完其他的事务后，需要用到子线程的处理结果，也就是主线程需要等待子线程执行完成之后再结束，这个时候就要用到join()方法了。
   - wait（继承自Object类的方法）：导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 唤醒方法。notify() 和 notifyAll() 这个两个唤醒方法也是Object类中的方法。

4. 线程池

   - 为什么需要线程池：主要用来解决线程生命周期开销问题和资源不足问题。
   - Executor接口：提供四种线程池
     - CachedThreadPool()：可缓存线程池，线程数无限制
     - FixedThreadPool()：定长线程池，可控制线程最大并发数（同时执行的线程数），超出的线程会在队列中等待
     - ScheduledThreadPool()：定长线程池，支持定时及周期性任务执行。
     - SingleThreadExecutor()：单线程化的线程池，有且仅有一个工作线程执行任务，所有任务按照指定顺序执行，即遵循队列的入队出队规则。

5. 多线程

   - 为什么需要多线程：解决多任务同时执行的需求，提高程序的执行效率

   - 多线程容易引发线程安全问题（即多个线程同时访问一个资源时，会导致程序运行结果并不是想看到的结果）。

     - synchronized
       1. 使用synchronized关键字来标记一个方法或者代码块
       2. 当某个线程调用该对象的synchronized方法或者访问synchronized代码块时，这个线程便获得了该对象的锁
       3. 其他线程暂时无法访问这个方法，只有等待这个方法执行完毕或者代码块执行完毕，这个线程才会释放该对象的锁，其他线程才能执行这个方法或者代码块。
     - Lock:使用Lock必须在try{}catch{}块中进行，并且将释放锁的操作放在finally块中进行，以保证锁一定被被释放，防止死锁的发生。

   - 多线程并发三个核心

     - 原子性：一个操作（有可能包含有多个子操作）要么全部执行（生效），要么全部都不执行（都不生效）。
     - 可见性：当多个线程并发访问共享变量时，一个线程对共享变量的修改，其它线程能够立即看到。
       - volatile:使变量在多个线程间可见，不保证线程安全，只修饰变量，只能保证数据的可见性，不用来同步。主要用在多个线程感知实例变量被更改了的场合，从而使得各个线程获得最新的值。
     - 有序性：程序执行的顺序按照代码的先后顺序执行

   - 线程间通信：共享变量、Message消息机制

     