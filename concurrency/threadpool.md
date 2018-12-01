# Thread Pools in Java 8

**A thread pool reuses previously created threads to execute current tasks and offers a solution to the problem of thread cycle overhead and resource thrashing**

* allow you to take advantage of threading, but focus on the tasks that you want the thread to perform, instead of thread mechanics
* To use thread pools, we first create a object of ExecutorService and pass a set of tasks to it. 
* ThreadPoolExecutor class allows to set the core and maximum pool size.
* The runnables that are run by a particular thread are executed sequentially

## Executor Thread Pool Methods
| Method Name | Purpose | 
|--|--|
| 	| 	|	
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbODYyNjkzNjY2LC0xMzk5MDkxNjA2LDQ5OD
E0MTcyMl19
-->