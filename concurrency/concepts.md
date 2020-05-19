# Concepts

## Program vs Process vs Thread

### Program

* A program is a set of instructions and associated data that resides on the disk and is loaded by the operating system to 
perform some task

* An executable file or a python script file are examples of programs

### Process

* A process is a program in execution. 

* A process is an execution environment that consists of instructions, user-data, and system-data segments, 
as well as lots of other resources such as CPU, memory, address-space, disk and network I/O acquired at runtime

### Thread

* Thread is the smallest unit of execution in a process. A thread simply executes instructions serially. 

* A process can have multiple threads running as part of it. 

* Usually, there would be some state associated with the process that is shared among all the threads and in turn each thread 
would have some state private to itself. 

## Concurrency vs Parallelism

### Concurrency
* A concurrent program is one that can be decomposed into constituent parts and each part can be executed out of order or 
in partial order without affecting the final outcome. 

* A system capable of running several distinct programs or more than one independent unit of the same program in overlapping time 
intervals is called a **concurrent system**. 

* The execution of two programs or units of the same program may not happen simultaneously.

* A concurrent system can have two programs in progress at the same time where progress doesn't imply execution. 

* One program can be suspended while the other executes. Both programs are able to make progress as their execution is interleaved. 

* In concurrent systems, the goal is to maximize throughput and minimize latency. 

* For example, a browser running on a single core machine has to be responsive to user clicks but also be able to render HTML on 
screen as quickly as possible. 

* Concurrent systems achieve lower latency and higher throughput when programs running on the system require frequent network or 
disk I/O.

### Parallelism

* A parallel system is one which necessarily has the ability to execute multiple programs at the same time. 

* Usually, this capability is aided by hardware in the form of multicore processors on individual machines or as computing clusters 
where several machines are hooked up to solve independent pieces of a problem simultaneously. 

* Remember an individual problem has to be concurrent in nature, that is portions of it can be worked on independently without 
affecting the final outcome before it can be executed in parallel.

* Example problems include matrix multiplication, 3D rendering, data analysis, and particle simulation.

### Concurrency vs Parallelism

* concurrent system need not be parallel, whereas a parallel system is indeed concurrent

* Concurrency is about dealing with lots of things at once. Parallelism is about doing lots of things at once

## Throuput vs Latency

* Throughput is defined as the rate of doing work or how much work gets done per unit of time

* Latency is defined as the time required to complete a task or produce a result. 

* Latency is also referred to as response time. Over network, it also includes n/w delays.

## Critical Sections and Race Conditions

* Critical section is any piece of code that has the possibility of being executed concurrently by more than one thread of the 
application and exposes any shared data or resources used by the application for access.

* Race conditions happen when threads run through critical sections without thread synchronization

## Deadlocks, Liveness and Reentrant Locks

### Deadlock

* Deadlocks occur when two or more threads aren't able to make any progress because the resource required by the first thread is held by the second
and the resource required by the second thread is held by the first.

### Liveness

* Ability of a program or an application to execute in a timely manner is called liveness. If a program experiences a deadlock 
then it's not exhibiting liveness.

### Live-lock

* A live-lock occurs when two threads continuously react in response to the actions by the other thread without making any real progress

* The best analogy is to think of two persons trying to cross each other in a hallway

### Starvation

* thread can experience starvation, when it never gets CPU time or access to shared resource, because of other greedy threads.

### Reentrant Lock

* Re-entrant locks allow for re-locking or re-entering of a synchronization lock.

## Mutex vs Semaphore

* Mutual exclusion. 

* A mutex is used to guard shared data such as a linked-list, an array or any primitive type. A mutex allows only a single thread to 
access a resource or critical section.

* Once a thread acquires a mutex, all other threads attempting to acquire the same mutex are blocked until the first thread 
releases the mutex

### Semaphore

* Semaphore is used for limiting access to a collection of resources. 

* Think of semaphore as having a limited number of permits to give out. 
  * If a semaphore has given out all the permits it has, then any new thread that comes along requesting for a permit will be blocked, 
till an earlier thread with a permit returns it to the semaphore. 

* A typical example would be a pool of database connections that can be handed out to requesting threads. Say there are ten available 
connections but 50 requesting threads. In such a scenario, a semaphore can only give out ten permits or connections at any given point
in time

* Semaphores can also be used for signaling among threads. This is an important distinction as it allows threads to cooperatively 
work towards completing a task. 

* A mutex, on the other hand, is strictly limited to serializing access to shared state among competing threads.














