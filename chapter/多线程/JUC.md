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


所以在此采用JUC下的AtomicInteger类



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

