# Thread Pools in Java 8

**A thread pool reuses previously created threads to execute current tasks and offers a solution to the problem of thread cycle overhead and resource thrashing**

* allow you to take advantage of threading, but focus on the tasks that you want the thread to perform, instead of thread mechanics
* To use thread pools, we first create a object of ExecutorService and pass a set of tasks to it. 
* ThreadPoolExecutor class allows to set the core and maximum pool size.
* The runnables that are run by a particular thread are executed sequentially

## Executor Thread Pool Methods
class `Executors` provides convenient factory methods for creating different kinds of executor services.

| Method Name | Purpose | 
|--|--|
| newFixedThreadPool(int)	| Creates a fixed size thread pool.	|	
| newCachedThreadPool() | Creates a thread pool that creates new                                   threads as needed, but will reuse previously                                   constructed threads when they are available|
| newSingleThreadExecutor() | Creates a single thread | 

![TP Exec 1](http://cdncontribute.geeksforgeeks.org/wp-content/uploads/tprun1.jpg)

### Steps for thread pool usage.
1. Create Runnable tasks to be executed
2. Create Executor pool using Executors utility class
3. Pass / submit the runnable tasks to executor pool for execution
4. Shutdown the executor pool

Example:
    
    ExecutorService executor = Executors.newSingleThreadExecutor();
    executor.submit(() -> {
	    String threadName = Thread.currentThread().getName();
	    System.out.println("Hello " + threadName);
    });
    // => Hello pool-1-thread-1
    

An `ExecutorService` provides two methods for that purpose: `shutdown()` waits for currently running tasks to finish while `shutdownNow()` interrupts all running tasks and shut the executor down immediately

#### Betterway to shutdown

    ```java
    try {
	    System.out.println("attempt to shutdown executor");
	    executor.shutdown();
	    executor.awaitTermination(5, TimeUnit.SECONDS);
    }
    catch (InterruptedException e) {
	    System.err.println("tasks interrupted");
    }
    finally {
    if (!executor.isTerminated()) {
        System.err.println("cancel non-finished tasks");
    }
    executor.shutdownNow();
    System.out.println("shutdown finished");
    }
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNjY4NzA5OTYsNzE5MDI2MzE5LC0xNT
gyMDI1ODU1LC0xNTAwOTIyOTA1LC00Nzk3MzEwNSwtMTM5OTA5
MTYwNiw0OTgxNDE3MjJdfQ==
-->