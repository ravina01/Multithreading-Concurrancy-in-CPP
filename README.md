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
Lets Revise -

#### 1. Multithreading -
- multiple threads executing within a context of a single process, sharing the same resource but running independently
- thread - smallest unit of execution, light weight process
- Concurrency: The ability to run multiple tasks or processes simultaneously
- Parallelism: A subset of concurrency where tasks are executed literally at the same time, often on multiple processors or cores.

1. Creating Threads: Using libraries (like pthreads in C/C++ or std::thread in C++11) to create and manage threads.
2. Thread Synchronization: Using mechanisms like mutexes, semaphores, and condition variables to coordinate access to shared resources and prevent race conditions.


##### Thread Safety
1. Race Conditions: Occur when two or more threads access shared data simultaneously, leading to unpredictable results. Preventing race conditions is crucial.
2. Atomic Operations: Ensuring that a sequence of operations on shared data is indivisible, typically achieved using atomic variables and operations.
3. Locks: Using mutexes and locks to ensure only one thread can access a critical section at a time.

##### Synchronization Mechanisms
- Mutex (Mutual Exclusion): A lock that ensures only one thread can access a resource at a time.
- Condition Variable: Used to block a thread until a particular condition is met, often used in conjunction with a mutex.
- Deadlock: A situation where two or more threads are waiting indefinitely for resources held by each other.


#### Q&A
- 
1. What is the difference between concurrency and parallelism?
- Concurrency: The ability to manage multiple tasks at the same time by interleaving their execution. It gives the illusion of simultaneous execution.
- Parallelism: The actual simultaneous execution of multiple tasks, typically using multiple processors or cores.

2. How do you prevent race conditions in a multithreaded environment?
- Locks/Mutexes: Ensure only one thread accesses a critical section at a time.
- Atomic Operations: Use atomic variables and operations to perform indivisible actions.
- Synchronization Mechanisms: Employ semaphores, condition variables, and barriers to coordinate thread access to shared resources.

3. Explain the concept of deadlock and how you can avoid it.
Deadlock: A situation where two or more threads are waiting indefinitely for resources held by each other, causing the system to halt.
Avoidance
Techniques: gaurd lock, scoped lock ?
Lock Ordering: Always acquire locks in a consistent global order.
Timeouts: Implement timeouts for lock acquisition.
Deadlock Detection: Use algorithms to detect and resolve deadlocks dynamically.


condition variable -
- allows you to wait on some conditions and once those conditions are met the waiting thread is notified.
- notify other threads - notify_one() and notify_all()
- waiting for some conditions
- need mutex to use condition variable.
- if some thread want to wait on some condition then it has to do below things
- acquire the mutex lock using std::unique_lock<std::mutex> lock(m)
- Execute wait, wait for / wait until. wait operation automatically releases mutex
- when condition_var is notified, thread is awakened.
- the thread should check condition and resume waiting if the wake up was spurious.
  
---

## My Work at Motorola solutions ->
- Goal was to build VAL5 - Video Analytics Library
- to process a decoded video stream at a specified frame rate, returning metadata at the same frame rate.

#### Principles of VAL  
1. parallel execution - Utilize parallel processing to handle video frames concurrently, leveraging multiple threads or cores to speed up the analysis.
2. Extensibility - Design the system in a modular way where new analytics modules can be integrated seamlessly without significant changes to the existing architecture.
3. Class encapsulation - Encapsulate functionality within classes, promoting clean interfaces and making the codebase easier to understand and maintain.

#### Assemblies and Datablocks
- Example: Object detection and PTZ auto-tracking assembly
- Datablocks:
- Definition: Thread-safe containers of elements used for data communication between concurrent modules.
- Types:
1. Timestamp-based: Keyed by timestamp (e.g., frame pyramids).
2. ID-based: Keyed by an integer (e.g., tracked targets).

#### Application Example: Object Detection and PTZ Auto Tracking
- Assemblies: Combine different analytics modules like human, gun, and backpack classifiers.
- Flow Graph: Manages the parallel processing of video frames and communication between these classifiers.
- Datablocks: Used to store and manage the results from different classifiers, ensuring thread-safe access and modification.


#### Detailed Explanation of H6A Features and Workflow
The H6A system is designed with several advanced features to detect and analyze various objects and events within a video stream. Here’s a detailed breakdown of its features, object detection workflow, and the role of qualifiers:

##### Features of H6A
1. Crowd Detection:
- Identifies and analyzes groups of people within the video frame.
- Useful for monitoring large gatherings, ensuring safety, and managing events.

2. Privacy Masking:
- Masks sensitive areas or individuals to protect privacy.
- Often used in surveillance systems to comply with privacy regulations.

3.Facets:
- Analyzes faces to determine attributes such as age and gender.
- Enhances person detection by adding demographic information.
  
4. License Plate Recognition:
- Detects and reads vehicle license plates.
- Useful for traffic monitoring, law enforcement, and access control.

5.Gun Detection:
- Identifies firearms within the video frame.
- Crucial for security and surveillance applications to detect potential threats.

Running on Every Frame:
Qualifiers are executed for each frame to ensure real-time analysis and accurate metadata generation.

#### I used to resolve/diagnose core dumped issues
1. Memory Management Errors:
- Occur when the program tries to access memory that it is not allowed to. This can be due to dereferencing null or invalid pointers, accessing out-of-bounds array elements, or using freed memory.

2. Concurrency Issues:
- Race Conditions: Happen when two or more threads access shared data simultaneously, and at least one thread modifies it. This can lead to inconsistent or unexpected behavior.
- Deadlocks: Occur when two or more threads are waiting for each other to release resources, causing the program to hang.

3. Diagnosing Core Dump Issues
- Core Dump File: Analyze the core dump file using tools like gdb to determine where the crash occurred.
- Detailed Logs: Implement detailed logging to trace the execution flow and identify the last successful operation before the crash.
- G Unit Tests: Write comprehensive unit tests to cover edge cases and ensure proper handling of resources.

GDB (GNU Debugger) is a powerful tool for debugging applications written in languages such as C, C++,
- Compile with Debugging Information - g++ -g -o my_program my_program.cpp
- 2. Run Your Program
Execute your program as usual. If it crashes, a core dump file will be generated (ensure your system is configured to create core dumps).
- 3. Locate the Core Dump File
Core dumps are usually generated in the current working directory or specified directory. The file is typically named core or core.<pid>, where <pid> is the process ID.
4. Launch GDB with the Core Dump - gdb my_program core
5. Backtrace (gdb) bt


















