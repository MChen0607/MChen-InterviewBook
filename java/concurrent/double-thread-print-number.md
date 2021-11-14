# 创建两个线程输出1-100

创建两个线程使用多种办法输出1-100.

## 创建线程

> 使用Runnable方法来进行创建线程，这里创建两个线程，一个线程输出为偶数，一个线程输出为奇数。

```java
// 输出为奇数
Runnable odd = new Runnable() {
    @Override
    public void run() {
        for (int i = 1; i <= 100; i = i + 2) {
            System.out.println(i);
        }
    }
};
```

```java
# 输出为偶数
Runnable even = new Runnable() {
    @Override
    public void run() {
        for (int i = 2; i <= 100; i = i + 2) {
            System.out.println(i);
        }
    }
};
```

### 运行测试

#### 直接运行run方法

```java
even.run();
odd.run();
```

> 这个时候是没有涉及到多线程的，直接运行run方法，相当于什么一个Runnable对象，重写了run方法，直接运行 run方法，则就是运行一个方法吧。
>
> 会把 `run()` 方法当成一个 main 线程下的**普通方法**去执行。
>
> 上述结果固定为先输出奇数再输出整数。

#### 创建线程方法运行

```java
Thread oddThread = new Thread(odd);
Thread evenThread = new Thread(even);
oddThread.start();
evenThread.start();
```

> 通过创建Thread对象的方式进行运行对应的线程函数。由Runnable声明的odd、even对象作为参数传到Thread中，将作为线程函数。`start()`后,**可启动线程并使线程进入就绪状态.**
>
> 运行的结果不唯一，输出的顺序跟调度器有关。

## 同步+锁

```java
/**
 * notify():唤醒等待的线程后，需要继续执行完自己的东西
 * wait():等待，运行->waiting。让出CPU，让出锁。唤醒后又恢复锁。
 *
 * 同步+锁的方式
 */
public static void doSynchronized() {
    int TOTAL = 100;
    Thread odd = new Thread(() -> {
        while (i <= TOTAL) {
            synchronized (lock) {
                if (i % 2 == 1) {
                    System.out.println(i);
                    i++;
                    lock.notify();
                } else {
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    });
    Thread even = new Thread(() -> {
        while (i <= TOTAL) {
            synchronized (lock) {
                if (i % 2 == 0) {
                    System.out.println(i);
                    i++;
                    lock.notify();
                } else {
                    try {
                        lock.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    });
    odd.start();
    even.start();
}
```

## 显示锁

> await的时候会释放掉所有的锁。

```java
private static Lock lockR = new ReentrantLock();
private static Condition oddCondition = lockR.newCondition();
private static Condition evenCondition = lockR.newCondition();
/**
 * 使用显示锁进行操作，并使用条件
 */
public static void doReentrantLock() {
    Thread oddThread = new Thread(() -> {
        try {
            lockR.lock();
            // 只获得一次锁，再满足所有条件下才被唤醒，运行时，重新保持锁的所有权
            while (counter <= MAX_COUNTER) {
                if (counter % 2 != 0) {
                    System.out.println("Thread1 : " + counter);
                    counter = counter + 1;
                    evenCondition.signalAll();
                }
                else {
                    try {
                        oddCondition.await();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        } finally {
            lockR.unlock();
        }
    });

    Thread evenThread = new Thread(() -> {
        try {
            lockR.lock();
            // 只获得一次锁，再满足所有条件下才被唤醒，运行时，重新保持锁的所有权
            while (counter <= MAX_COUNTER) {
                if (counter % 2 == 0) {
                    System.out.println("Thread2 : " + counter);
                    counter = counter + 1;
                    oddCondition.signalAll();
                }
                else {
                    try {
                        evenCondition.await();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                }
            }
        } finally {
            lockR.unlock();
        }
    });
    oddThread.start();
    evenThread.start();
}
```

## 信号量

> 使用信号量的时候，需要注意输出为奇数时候，判断条件发生改变，只能为\<MAX\_COUNTER的时候才能进入循环。

```java
private static Semaphore oddSema = new Semaphore(1);
private static Semaphore evenSema = new Semaphore(0);
private static int counter = 1;
private static final int MAX_COUNTER = 100;
/**
 * 使用信号量进行
 */
public static void doSemaphore() {
    Thread oddThread = new Thread(() -> {
        while (counter < MAX_COUNTER) {
            try {
                oddSema.acquire();
                if (counter % 2 != 0) {
                    System.out.println("Thread1 : " + counter);
                    counter = counter + 1;
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                evenSema.release();
            }
        }
    });

    Thread evenThread = new Thread(() -> {
        while (counter < MAX_COUNTER) {
            try {
                // 98的时候进来等待
                evenSema.acquire();
                if (counter % 2 == 0) {
                    System.out.println("Thread2 : " + counter);
                    counter = counter + 1;
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                oddSema.release();
            }
        }
    });
    oddThread.start();
    evenThread.start();
}

```

## 注意

{% hint style="info" %}
实现的方式，以及在哪个地方获取锁/信号量
{% endhint %}
