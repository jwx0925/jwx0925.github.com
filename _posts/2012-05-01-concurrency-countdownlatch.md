---
layout: post
title: 并发学习-CountDownLatch
description: 并发类CountDownLatch学习
keywords: concurrency, CountDownLatch
category : concurrency
tags : [concurrency]
---

CountDownLatch是一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待。

用给定的计数 初始化 CountDownLatch。由于调用了 countDown() 方法，所以在当前计数到达零之前，await方法会一直受阻塞。之后，会释放所有等待的线程，await 的所有后续调用都将立即返回。这种现象只出现一次——计数无法被重置。

CountDownLatch 的一个有用特性是，它不要求调用 countDown 方法的线程等到计数到达零时才继续，而在所有线程都能通过之前，它只是阻止任何线程继续通过一个 await。

从名字可以看出，CountDownLatch是一个倒数计数的锁，当倒数到0时触发事件，也就是开锁，其他人就可以进入了。
在一些应用场合中，需要等待某个条件达到要求后才能做后面的事情；同时当线程都完成后也会触发事件，以便进行后面的操作。

CountDownLatch最重要的方法是countDown()和await()，前者主要是倒数一次，后者是等待倒数到0，如果没有到达0，就只有阻塞等待了。

下面的例子简单的说明了CountDownLatch的使用方法，模拟了100米赛跑，10名选手已经准备就绪，只等裁判一声令下。当所有人都到达终点时，比赛结束。

<pre class="brush: java">
public class CountDownLatchDemo {

    private static final int PLAY_AMOUNT = 10;

    public static void main(String[] args) {

        /*
         * 比赛开始：只要裁判说开始，那么所有跑步选手就可以开始跑了
        **/
        CountDownLatch begin = new CountDownLatch(1);

        /*
         * 每个队员跑到末尾时，则报告一个到达，所有人员都到达时，则比赛结束
        **/
        CountDownLatch end = new CountDownLatch(PLAY_AMOUNT);
        Player[] plays = new Player[PLAY_AMOUNT];
        for (int i = 0; i < PLAY_AMOUNT; i++) {
            plays[i] = new Player(i + 1, begin, end);
        }

    }
}
</pre>

<pre class="brush: java">
class Player implements Runnable {

    private int            id;

    private CountDownLatch begin;

    private CountDownLatch end;

    public Player(int id, CountDownLatch begin, CountDownLatch end) {
        super();
        this.id = id;
        this.begin = begin;
        this.end = end;
    }

    public void run() {
        try {
            begin.await();//必须等到裁判countdown到0的时候才开始   
            Thread.sleep((long) (Math.random() * 100));//模拟跑步需要的时间   
            System.out.println("Play " + id + " has arrived. ");

        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            end.countDown();//向评委报告跑到终点了   
        }
    }
}
</pre>

参考：
1. [CountDownLatch](http://hi.baidu.com/chenwei6111/blog/item/168715272ebd2f0f908f9d6b.html)  
2. [类 CountDownLatch](http://www.cjsdn.net/doc/jdk50/java/util/concurrent/CountDownLatch.html)