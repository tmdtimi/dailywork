##JUC

CAS（Compare And Swap）指比较并交换。CAS算法CAS(V, E, N)包含 3 个参数，V 表示要更新的变量，E 表示预期的值，N 表示新值。在且仅在 V 值等于 E值时，才会将 V 值设为 N，如果 V 值和 E 值不同，则说明已经有其他线程做了更新，当前线程什么都不做。最后，CAS 返回当前 V 的真实值。


Concurrent包下所有类底层都是依靠CAS操作来实现.

ABA问题 

自旋导致的问题： 单次CAS不一定能执行成功，所以一般是配合循环来实现的。有的时候甚至是死循环，不停地进行重试，直到线程竞争不激烈的时候，才能修改成功。

CPU 资源也是一直在被消耗的，这会对性能产生很大的影响。所以这就要求我们，要根据实际情况来选择是否使用 CAS

不能灵活控制线程安全的范围。只能针对某一个，而不是多个共享变量的，不能针对多个共享变量同时进行 CAS操作，因为这多个变量之间是独立的，简单的把原子操作组合到一起，并不具备原子性。


AQS： 抽象同步队列
 

volatile:保证可见性和有序性


**atomicInteger**

a++包含三个操作：从主存中读取a的值、对a进行+1操作、把a重新刷到主存。


	public class Atomic {

    private static volatile int a = 0;
    public static void main(String[] args) {
        Atomic test = new Atomic();
        Thread[] threads = new Thread[5];
        //定义5个线程，每个线程加10
        for (int i = 0; i < 5; i++) {
            threads[i] = new Thread(() -> {
                try {
                    for (int j = 0; j < 10; j++) {
                        System.out.println(a++);
                        Thread.sleep(500);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            });
            threads[i].start();
        }   
    }
	}

多个线程进行a++操作，有的线程已经+1了，但是没有刷进主存，其他线程就重新读取到了旧值，因此就造成了错误。


所以在此采用JUC下的AtomicInteger类  --cas



	package JUC;

import java.util.concurrent.atomic.AtomicInteger;

public class Atomic {


    static AtomicInteger a=new AtomicInteger();
    public static void main(String[] args) {
        Atomic test = new Atomic();
        Thread[] threads = new Thread[5];
        //定义5个线程，每个线程加10
        for (int i = 0; i < 5; i++) {
            threads[i] = new Thread(() -> {
                try {
                    for (int j = 0; j < 10; j++) {
                        System.out.println(a.incrementAndGet());
                        Thread.sleep(500);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            });
            threads[i].start();
        }
    }
	}

利用 a.incrementAndGet() 替换 a++

底层使用的是Unsafe.compareAndSwapInt

unsafe是sun.misc包下的一个类



**CountDownLatch**

使用场景 ：  让多个线程等待 / 让单个线程等待


场景一：类似于发令枪 ， 让多个线程等待，同时开始执行。

	 public static void main(String[] args) throws InterruptedException {
        Thread[] threads=new Thread[5];
        CountDownLatch countDownLatch = new CountDownLatch(1);
        for (int i = 0; i < 5 ; i++) {
            threads[i]=new Thread(()->{
                try {
                    countDownLatch.await();
                    String s=Thread.currentThread().getName();
                    System.out.println(s);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
            threads[i].start();
        }
        Thread.sleep(2000);
        countDownLatch.countDown();
    }

场景二：让单个线程等待，多个线程完成后，进行汇总合并。

countDownLatch.countDown() 让计数器减一

	public class Atomic {

    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch = new CountDownLatch(5);
        for (int i = 0; i < 5 ; i++) {
            final int index=i;
            new Thread(()->{
                try {
                    Thread.sleep(1000);
                    System.out.println("finish" + index + Thread.currentThread().getName());
                    String s=Thread.currentThread().getName();
                    System.out.println(s);
                    countDownLatch.countDown();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }).start();
    }

        countDownLatch.await();
        Thread.sleep(1000);
        System.out.println("主线程:在所有任务运行完成后，进行结果汇总");
	}
	}

CountDownLatch的构造函数中的count就是闭锁需要等待的线程数量。这个值只能被设置一次，而且不能重新设置；

await()：调用该方法的线程会被阻塞，直到构造方法传入的 N 减到 0 的时候，才能继续往下执行；

countDown()：使 CountDownLatch 计数值 减 1；

CountDownLatch是通过一个计数器来实现的，计数器的初始值为线程的数量；

调用await()方法的线程会被阻塞，直到计数器 减到 0 的时候，才能继续往下执行；

![](../pics/r1.png)

**CyclicBarrier** --AQS

![](../pics/r2.png)


CyclicBrrier：N 个线程相互等待，直到有足够数量的线程都到达屏障点之后，之前等待的线程就可以继续执行了。



	public class Cyclic {

    public static CyclicBarrier cyclicBarrier=new CyclicBarrier(5);

    public static void main(String[] args) throws InterruptedException {
        ExecutorService executorService = Executors.newCachedThreadPool();
        for (int i = 0; i < 10; i++) {
            final int index=i;
            Thread.sleep(1000);
            executorService.execute(()->{
                try {
                    Thread.sleep(1000);
                    System.out.println(Thread.currentThread().getName()+"ready");
                    cyclicBarrier.await();
                    System.out.println(Thread.currentThread().getName()+"go");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            });
        }
        executorService.shutdown();
    }
	}	

CyclicBarrier在释放等待线程后可以重用，所以称为循环barrier。  

CountDownLatch是让一个线程等多个线程，通过await 和 countDown方法，通过计数器实现。一次性使用。

CyclicBarrier实现多个线程等待，满足条件后才能一起继续。可以循环使用。



**Semaphore**

信号量。 一个是用于多个共享资源的互斥使用,另一个用于并发线程数的控制。

Semaphore的值被获取到后是可以释放的，并不像CountDownLatch那样一直减到底。它也被更多地用来限制流量，类似阀门的功能。

如果限定某些资源最多有N个线程可以访问，那么超过N个主不允许再有线程来访问，同时当现有线程结束后，就会释放，然后允许新的线程进来。有点类似于锁的lock与unlock过程。

相对来说他也有两个主要的方法：用于获取权限的acquire()，其底层实现与CountDownLatch.countdown()类似；

用于释放权限的release()，其底层实现与acquire()是一个互逆的过程。


	public class Sampho {

    private static Semaphore semaphore=new Semaphore(5);

    public static void main(String[] args) {
        ExecutorService executorService = Executors.newCachedThreadPool();
        for (int i = 0; i < 10; i++) {
            new Thread(()->{
                try {
                    Thread.sleep(1000);
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName()+"抢到车位");
                    TimeUnit.SECONDS.sleep(3);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }finally {
                    System.out.println(Thread.currentThread().getName()+"离开车位");
                    semaphore.release();
                }
            }).start();
        }
        executorService.shutdown();
    }
	}


**CopyOnWriteArrayList**  --ReetrantLock  volatile +数组拷贝

CopyOnWriteArrayList的get方法不会加锁，因为CopyOnWriteArrayList在add/remove时不会修改原数组，所以读操作时，不会产生线程安全问题。

只有写入的时候才加锁，复制副本来进行修改。 --写时复制容器

因为迭代的始终是原数组，而所有的变化都发生在原数组的副本上。所以对于迭代器来说，迭代的集合结构不会发生改变。数组的结构被改变也不会抛出ConcurrentModificationException异常

CopyOnWriteArrayList 线程安全，相对vector提高读操作的并发量

缺点： 每次写都会开辟新的数组空间，无法保证实时性，只能保证最终一致性。 add/remove效率低，既要加锁，还要拷贝数组。



**AtomicStampedReference**




