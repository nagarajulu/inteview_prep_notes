# Thread Pools in Java 8

**A thread pool reuses previously created threads to execute current tasks and offers a solution to the problem of thread cycle overhead and resource thrashing**

* allow you to take advantage of threading, but focus on the tasks that you want the thread to perform, instead of thread mechanics
* To use thread pools, we first create a object of ExecutorService and pass a set of tasks to it. 
* ThreadPoolExecutor class allows to set the core and maximum pool size.
* The runnables that are run by a particular thread are executed sequentially

## Executor Thread Pool Methods
| Method Name | Purpose | 
|--|--|
| newFixedThreadPool(int)	| Creates a fixed size thread pool.	|	
| newCachedThreadPool() | Creates a thread pool that creates new                                   threads as needed, but will reuse previously                                   constructed threads when they are available|
| newSingleThreadExecutor() | Creates a single thread | 

![TP Exec 1](http://cdncontribute.geeksforgeeks.org/wp-content/uploads/tprun1.jpg)

### Steps for thread pool usage.
1. Create Runnable tasks to be executed
2. Create Executor pool using Executors utility class
3. Pass / submit the runnable tasks to execu
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5Nzk5ODA2MDUsLTQ3OTczMTA1LC0xMz
k5MDkxNjA2LDQ5ODE0MTcyMl19
-->