# Multithreading and Concurrancy in CPP

---

Multithreading and concurrency are essential concepts in software development, especially for performance-critical applications like camera imaging algorithms. Here's an overview of what you should know about these topics:

### Fundamental Concepts -

#### 1. Multithreading:

- There is always main thread() in each application, it's default thread, In this thread only we create other threads.
- Achieve parallelism by diving a process into multiple threads.
- A thread is also lightweight process.
- Ways to create Threads in C++ -
   - 1. Function Pointers
     2. Lambda Functions
     3. Member Functions
     4. Static member functions
- Performace is currency of computing.
- joining threads with main thread is important otherwise will get - core dumped issue!

#### Parallelism vs Concurrancy -
- Concurrancy is often used interchangeably with parallelism - so lets separate those 2 terms ->
- 1.Concurrancy we have to keep in mid the synchronization, we have to wait on shared resoy=urces/ threads to finish their tasks,
- Parallelism - everything happens at once, instataneously 
  

![image](https://github.com/user-attachments/assets/c046ea13-f116-4272-b3a6-aaaa78f6886e)

![image](https://github.com/user-attachments/assets/e9ea70a1-02b1-4b95-9e57-c4a0cf34eb94)

- Morre's law - as the transistors get closer and closer - more heat is generated.
- 

![image](https://github.com/user-attachments/assets/4043f703-38ee-4eb1-8075-d5fda5242159)

![image](https://github.com/user-attachments/assets/a5d4f24d-c4d3-464b-95a2-8056292fb74c)

![image](https://github.com/user-attachments/assets/001557e9-329f-4835-ba8a-5b40e5f6b625)


![image](https://github.com/user-attachments/assets/a114e307-bbba-4378-8a1b-89fd3271f17e)

- thread - C++ 11 -> manages a separate thread
- jthread C++20 -> std::thread with support for auto-joining and cancellation.

- thread creations
- thread join() - blocks the current thread until the thread identified by *this finishes its execution
- 

![image](https://github.com/user-attachments/assets/37d781df-f3c7-4bd0-8bfa-bda2629dd744)

![image](https://github.com/user-attachments/assets/d8ed909f-8c5d-4a6d-b212-b555b42d0501)


Thread using lambda functions ->
- threads are not copy able, there are no copy constructors doesnt make sense to copy constructor.


Running Multiple threads from a vector-> Thread support library.
- 

-  A thread has ->
-  get_id -> unique ids / thread
-  operations -> join, detach, swap. 


![image](https://github.com/user-attachments/assets/44be92fa-6ec6-481a-9f59-c12fae5f47d5)

- Problem with above code is its launched sequentially one after the other.
- hence when we print get_id we get the same id for the thread it won't execute all in parallel. it will each thread to finish and then launch next thread. which is sequential and not in parallel. It will wait for each thread to terminate
- How to fix ?


![image](https://github.com/user-attachments/assets/35450a7b-f148-4299-87d9-33e4f387b2c2)

solutions - launch all threads as soon as possible with different ids and then wait for each to execute and once that is done main thread will continue its next execution in line. The only problem is first print statement (see below image) is printed while second thread is launched so we get jumbled output, hence we need some kind of -> synchronization here. 

![image](https://github.com/user-attachments/assets/a3266bc3-a09f-48fa-9997-3507dde5b66f)


#### jthread std::jthread in C++ 20 compiler | supports auto-joining and cancellattions
- cleans up the code and we dont have to worry about the -> joining the threads.
![image](https://github.com/user-attachments/assets/13df9199-4c52-4540-b1a9-cad4d7d44084)
- here, we get to see output as 100 or sometimes 99/ 96 as two threads are accessing the shared value at the same time.

#### std::mutex lock and preventing data races in C++ 
- two theads might access the data at the same time. and then try to update the shared value at the same time so we might not get correct result in this case.
- solution - std::mutex - mutual exclusion
- only one thread can access the shared value, and then other threads has to wait.
- we will lock and unlock the mutex so other thread wont update the value of shared space/val
- will get result as 100 every time, as we are using mutex locks
- we can not have other thread access one piece of code which is protected by mutex locks
- one thread accessing piece of the code - its synchronized
  
![image](https://github.com/user-attachments/assets/ad6b9472-84f8-492e-aa4f-9cc8af1c8442)

![image](https://github.com/user-attachments/assets/059f939d-5977-48e7-8f98-9139c2f2dd1f)

#### lets talk more about pitfalls of concurrancy and how to deal with it.

#### Preventing deadlock with std::lock_guard in modern C++

- deadlock - lock is never retunrned as in unlock() is not called.
- none of the threads in line are able to make progress because the lock is not returned. It freezes the whole execution of the program.
- how to fix ?
- lock_gaurd, unique_lock, scoped_lock(prefered)
- to avoid manual lock and unlock we can use lock_gaurds. which does the same job ?
- cleaner code
- let see if we create a try and catch block, will have to manually unlock the mutex hence will use lock_gaurd
- RAII ?
- Now we have multiple threads execution, lock_gaurds, mutex, 

  
![image](https://github.com/user-attachments/assets/74886027-dcaf-41ab-84fa-06ee9bbb8558)


#### Using std::atomic in modern C++ to update a shared value
- atomic operations
- we can use this to simplify our code here.
- will have to little bit careful here and use the operations listed by atomic- ++, --, =+, =-
- atomic works with all primitive data types- char, int, uint, 

![image](https://github.com/user-attachments/assets/b0586e4c-797a-403e-8b87-5b46b00d4441)

---
