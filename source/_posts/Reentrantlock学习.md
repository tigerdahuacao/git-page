---
title: Reentrantlock学习
date: 2022-06-25 20:51:45
categories:
- java
- 教程
tags: JUC
---

log4j.properties
```properties
# Global logging configuration 开发时候建议使用 debug
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p <%d{yyy-MM-dd HH:mm:ss }> [%t] - %m%n
```
POM
```xml
 <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.10</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.26</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.26</version>
        </dependency>
    </dependencies>
```

sleep工具类

```java
public class Common {

    public static void sleep(Integer t){
        try {
            Thread.sleep(t*1000);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

主文件
```java
import lombok.extern.java.Log;
import lombok.extern.slf4j.Slf4j;

import java.util.Random;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

@Slf4j(topic = "ReentrantLock-Test")
public class Demo01_Reentrantlock {
    //可中断
    //可以设置超时时间
    //可以设置为公平锁,排队, 不随机争抢锁, 先进先出
    //支持多个条件变量 类似于synchronized中的 waitSet, 不过这里可以有多个waitSet

    //与synchronized一样, 都是可重入的(对一个线程重复获取)
    //可重入: 一个线程如果首次获得了这把锁, 有权利再次获取这把锁; 否则称为不可重入
    //synchronized是在关键字级别, ReentrantLock是对象级
    static ReentrantLock lock = new ReentrantLock();


    //可重入性test:
   public static void main(String[] args) {
       lock.lock();
       try {
           log.debug("enter main");
           method1();
       } finally {
           log.debug(" main unlock");
           lock.unlock();
       }
   }

   public static void method1() {
       lock.lock();
       try {
           log.debug("enter method 1");
           method2();
       } finally {
           log.debug(" method1 unlock");
           lock.unlock();
       }
   }

   public static void method2() {
       lock.lock();
       try {
           log.debug("enter method 2");
       } finally {
           log.debug(" method2 unlock");
           lock.unlock();
       }
   }
    //==========================================
    //可打断性:
   public static void main(String[] args) {
       Thread t1 = new Thread(() -> {
           try {
               //如果没有竞争, 那么此方法就会获取lock对象锁
               //如果有竞争,进入阻塞队列, 可以被其他线程 interrupt方法打断

               log.debug("尝试获得锁");
               lock.lockInterruptibly();

               //如果不可打断, 用了lock方法, 那会导致没有catch块, 一直会等待下去, 造成死锁
           } catch (Exception e) {
               e.printStackTrace();
               log.debug("没有获得锁");
               return;
           }

           try {
               log.debug("获取到锁");
           } finally {
               lock.unlock();
           }
       }, "t1");

       lock.lock();
       t1.start();

       Common.sleep(1);
       log.debug("打断t1");
       t1.interrupt();
   }
    //==========================================
    //锁超时, 主动打断锁
   public static void main(String[] args) {
       Thread t1 = new Thread(() -> {
           log.debug("尝试获得锁");
           try {
               if(!lock.tryLock(2, TimeUnit.SECONDS)){
                   log.debug("获取不到锁");
                   return;
               }
           } catch (InterruptedException e) {
               e.printStackTrace();
               log.debug("获取不到锁");
               return;
           }

           try {
               log.debug("临界区:获取到锁");
           } finally {
               lock.unlock();
           }
       }, "t1");

       lock.lock();
       log.debug("主线程get lock");
       t1.start();

       Common.sleep(1);
       log.debug("release lock");
       lock.unlock();
   }

    //==========================================
    //哲学家吃饭问题
   static class ChopStick extends ReentrantLock {
       String name;

       public ChopStick(String name) {
           this.name = name;
       }

       @Override
       public String toString() {
           return "ChopStick{" +
                   "name='" + name + '\'' +
                   '}';
       }
   }

   @Slf4j(topic = "Philosopher")
   static class Philosopher extends Thread {
       ChopStick right;
       ChopStick left;

       public Philosopher(String name, ChopStick right, ChopStick left) {
           super(name);
           this.left = left;
           this.right = right;
       }

       @Override
       public void run() {
           while (true) {

               //try get left chopstick
               if (left.tryLock()) {
                   try {
                       //try get right chopstick
                       if (right.tryLock()) {
                           try {
                               eat();
                           } finally {
                               right.unlock();
                           }
                       }
                   } finally {
                       left.unlock();
                       //if left chopstick not get, release this chopstick
                   }
               }
           }
       }

       Random random = new Random();

       private void eat() {
           log.debug("eating---");
           Common.sleep(1);
       }
   }

   public static void main(String[] args) {
       ChopStick c1 = new ChopStick("1");
       ChopStick c2 = new ChopStick("2");
       ChopStick c3 = new ChopStick("3");
       ChopStick c4 = new ChopStick("4");
       ChopStick c5 = new ChopStick("5");

       new Philosopher("苏格拉底", c1, c2).start();
       new Philosopher("亚里士多德", c2, c3).start();
       new Philosopher("柏拉图", c3, c4).start();
       new Philosopher("第欧根尼", c4, c5).start();
       new Philosopher("赫拉克利特", c5, c1).start();
   }

    //==========================================
    //公平锁 reentrantlock默认也是不公平的 通过构造方法, 但是底层实现是链表, 所以用公平锁会降低并发量


//    条件变量
   public static void main(String[] args) {
//    创建一个新的条件变量
       Condition condition1 = lock.newCondition();
       Condition condition2 = lock.newCondition();

       lock.lock();
//    await前得获取锁,执行后会释放锁
//    await进行等待

//    await 执行后, 会释放锁, 进入conditionObject等待
       condition1.await();
//     await的线程被唤醒后, 重新竞争lock锁
       condition1.signal();
       condition1.signalAll();
   }

    static final Object room = new Object();
    static Boolean hasCigarette = false;
    static Boolean hasTakeOut = false;
    static ReentrantLock ROOM = new ReentrantLock();

    static Condition waitCigaretteSet = ROOM.newCondition();
    static Condition waitTakeoutSet = ROOM.newCondition();

    //没烟没外卖不能干活的例子
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            ROOM.lock();
            try {
                log.debug("有烟么?[{}]", hasCigarette);
                //防止虚假唤醒
                while (!hasCigarette) {
                    log.debug("没烟抽,先歇会儿!");
                    try {
                        waitCigaretteSet.await();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                        log.debug("可以开始干活了");
                }
            } finally {
                ROOM.unlock();
            }

        }, "t1");
        t1.start();

        Thread t2 = new Thread(() -> {
            ROOM.lock();
            try {
                log.debug("外卖送到了?[{}]", hasCigarette);
                //防止虚假唤醒
                while (!hasCigarette) {
                    log.debug("没外卖,先歇会儿!");
                    try {
                        waitTakeoutSet.await();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    log.debug("可以开始干活了");
                }
            } finally {
                ROOM.unlock();
            }
        },"t2");
        t2.start();

        Common.sleep(1);
        new Thread(()->{
            ROOM.lock();
            try {
                hasTakeOut = true;
                waitTakeoutSet.signal();
            }finally {
                ROOM.unlock();
            }
        },"送外卖线程").start();

        Common.sleep(1);
        new Thread(()->{
            ROOM.lock();
            try {
                hasCigarette = true;
                waitCigaretteSet.signal();
            }finally {
                ROOM.unlock();
            }
        },"送烟线程").start();
    }
}


```