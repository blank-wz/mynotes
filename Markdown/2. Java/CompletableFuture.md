# 一，简介

## 1.1 是什么

JDK8 引入的用于异步任务编排的类

## 1.2 特点与优势

链式操作、灵活可控的异常处理机制、组合操作、任务可取消



# 二， 异步任务的执行

## 1.1 runAsync

````java
@Test
public void testRunAsync() {
    // 泛型表示没有返回值
    CompletableFuture<Void> cf = CompletableFuture.runAsync(() -> {
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("我是在线程" + Thread.currentThread().getName() + "里执行的")；
    });
    // false
    System.out.println("判断异步任务执行完成1：" + cf.isDone());
    /*
    null
    join 和 get 都是阻塞等待cf执行完成，并获取异步任务执行的结果，区别：
    1. join 不抛出异常
    2. get 抛异常，且可执行等待结果的超时时间
    */
    System.out.println("cf.join() = " + cf.join());
    System.out.println("cf.get() = " + cf.get()); // 需要捕获或抛出异常
    // true
    System.out.println("判断异步任务执行完成2：" + cf.isDone());
}
````



## 1.2 supplyAsync

supplyAsync 与 runAsync 的唯一不用在于可以获取异步任务的返回值

````java
@Test
public void testSupplyAsync() throws ExecutionException, InterruptedException {
    CompletableFuture<String> cf = CompletableFuture.supplyAsync(() -> {
        System.out.println("我实在线程：" + Thread.currentThread().getName() + "里执行的");
        return "supplyAsync的返回值";
    });
    System.out.println("cf.get() = " + cf.get());
} 
````



## 1.3 ForkjoinPool.commonPool 与自定义线程池

runAsync/supplyAsync 的默认线程池： ForkJoinPool

避免不同业务之前那的线程池互相影响，需要做到线程池隔离，所以不推荐使用 ForkJoinPool.commonPool

````java
private static final ExecutorService executorService = new ThreadPoolExecutor(
	1,
    10,
    60L,
    TimeUnit.SECONDS,
    new NamedThreadFactory("completableFuture-test-pool-thread-%d", false),
    // new ThreadFactoryBuilder().setNameFormat("prdt_params-pool-%d").build()
    new ThreadPoolExecutor.AbortPolicy()
);
````



## 1.4 其他创建 CompletableFuture 的方法

CompletableFuture.completedFuture(U value) : 返回一个结果为给定值的且已经执行完成的 CompletableFuture

# 三，链式处理

## 1.1 thenApply



