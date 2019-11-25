# Java Core Concepts 
The core concepts are as follows 
 * JVM
 * Memory Model
 * Persistence
## Memory model
The Java programming language memory model works by examining each read in an execution trace and checking that the write observed by that read is valid according to certain rules.

The memory model describes possible behaviors of a program. An implementation is free to produce any code it likes, as long as all resulting executions of a program produce a result that can be predicted by the memory model.

Java memory model consist of 2 important sections
 1. **JVM Stack**
 2. **Heap**
 3. **Permanent Generation**

### JVM Stack
* every thread has different stack
* different scoping is maintained inside a stack
* a code block execution ends with the pop of all of its scoped variables from the stack
* If the computation in a thread requires a larger Java Virtual Machine stack than is permitted, the Java Virtual Machine throws a _StackOverflowError_.
* If Java Virtual Machine stacks can be dynamically expanded, and expansion is attempted but insufficient memory can be made available to effect the expansion, or if insufficient memory can be made available to create the initial Java Virtual Machine stack for a new thread, the Java Virtual Machine throws an _OutOfMemoryError_.
> An implementation of JVM can have **Native Method Stacks** to execute methods in native programming languages.


### Heap
* long-lived data, across multiple methods
* java mem - stack = heap
* 1 heap, multiple stack (for each thread)
* The heap parts are: Young Generation, Old or Tenured Generation, and Permanent Generation.

#### Young Generation
The young generation is the place where all the new objects are created. When the young generation is filled, garbage collection is performed. This garbage collection is called **Minor GC**. Young Generation is divided into three parts – ***Eden Memory*** and two ***Survivor Memory*** spaces.

#### Old Generation Memory
Old Generation memory contains the objects that are long-lived and survived after many rounds of Minor GC. Usually, garbage collection is performed in Old Generation memory when it’s full. Old Generation Garbage Collection is called Major GC and usually takes a longer time.

#### Garbage

* Java avoids memory leak by
	* running on virtual machine
	* adopting garbage collection stratefy (LISP used it first)
* unreachable objects are garbage

#### final keyword
* stack to heap connection cannot be broken
* the object's value can be changed but cannot assign to a new object
* unsafe
* no const correction feature in java, const implementation is not there, gives a compiler error on usage

#### Escaping references
 A method which returns a map, can expose the map which can be corrupted by calling environment.
 Ways to avoid this issue
  * make the class an Iterable, but it have remove().
  * return a new Map from the same values, but underlying object can be changed
  * make underlying object read-only

#### Weak & Soft References
A Reference instance is in one of four possible internal states:
    Active: Subject to special treatment by the garbage collector.  Some
    time after the collector detects that the reachability of the
    referent has changed to the appropriate state, it changes the
    instance's state to either Pending or Inactive, depending upon
    whether or not the instance was registered with a queue when it was
    created.  In the former case it also adds the instance to the
    pending-Reference list.  Newly-created instances are Active.
    Pending: An element of the pending-Reference list, waiting to be
    enqueued by the Reference-handler thread.  Unregistered instances
    are never in this state.
    Enqueued: An element of the queue with which the instance was
    registered when it was created.  When an instance is removed from
    its ReferenceQueue, it is made Inactive.  Unregistered instances are
    never in this state.
    Inactive: Nothing more to do.  Once an instance becomes Inactive its
    state will never change again.
The state is encoded in the queue and next fields as follows:
    Active: queue = ReferenceQueue with which instance is registered, or
    ReferenceQueue.NULL if it was not registered with a queue; next =
    null.
    Pending: queue = ReferenceQueue with which instance is registered;
    next = this
    Enqueued: queue = ReferenceQueue.ENQUEUED; next = Following instance
    in queue, or this if at end of list.
    Inactive: queue = ReferenceQueue.NULL; next = this.

Weak reference objects, which do not prevent their referents from being
made finalizable, finalized, and then reclaimed.  Weak references are most
often used to implement canonicalizing mappings.
They might be cleaned-up by Garbage collector

Soft reference objects, which are cleared at the discretion of the garbage
collector in response to memory demand.  Soft references are most often used
to implement memory-sensitive caches.
 
### Permanent Generation
The Permanent generation contains metadata required by the JVM to describe the classes and methods used in the application. The permanent generation is populated by the JVM at runtime based on classes in use by the application. In addition, Java SE library classes and methods may be stored here.

Classes may get collected (unloaded) if the JVM finds they are no longer needed and space may be needed for other classes. The permanent generation is included in a full garbage collection.

#### Method Area
Method Area is part of space in the Perm Gen and used to store class structure (runtime constants and static variables) and code for methods and constructors

##### Runtime Constant Pool
Runtime constant pool is per-class runtime representation of constant pool in a class. It contains class runtime constants and static methods. Runtime constant pool is part of the method area.


### Memory Pool
Memory Pools are created by JVM memory managers to create a pool of immutable objects if the implementation supports it. String Pool is a good example of this kind of memory pool. Memory Pool can belong to Heap or Perm Gen, depending on the JVM memory manager implementation.


## JVM
`JVM intelligently identifies if an object lifecycle is ltd to its method / block then it is created in Stack instead of HEAP`

* running a pgm with ltd memory - give JVM args as -Xmx100m
* jvisualvm.exe to monitor the memory allocations

## Garbage Collection
* gc() may not run when we invoke it
* GC calls finalize() on object
* never close resources in finalize as it might not be called.
* The process of clearing out garbage is ***Mark & Sweep***
* From Java 7, JVM maintains generational heap memory to assist GC process
#### Mark & Sweep
* mark
	* pause the pgm execution
	* checks single live variable
	* all references are recursively marked
	* optionally all marked objects are allocated contagious memory
* sweep
	* references of unreachable unmarked objects are removed

### Generational GC Process
1. First, any new objects are allocated to the eden space. Both survivor spaces start out empty.
2. When the eden space fills up, a minor garbage collection is triggered.
	* All minor garbage collections are always "Stop the World" events. This means that all application threads are stopped until the operation completes.
3. Referenced objects are moved to the first survivor space. Unreferenced objects are deleted when the eden space is cleared.
4. At the next minor GC, the same thing happens for the eden space. Unreferenced objects are deleted and referenced objects are moved to a survivor space. However, in this case, they are moved to the second survivor space (S1). In addition, objects from the last minor GC on the first survivor space (S0) have their age incremented and get moved to S1. Once all surviving objects have been moved to S1, both S0 and eden are cleared. we now have differently aged object in the survivor space.
5. At subsequent next minor GC, the same process repeats. However every time the survivor spaces switch. Surviving objects are aged. Eden and other Survivor spaces are cleared.
6. After a minor GC, when aged objects reach a certain age threshold they are promoted from young generation to old generation.
7. As minor GCs continue to occure objects will continue to be promoted to the old generation space.
8. Eventually, a major GC will be performed on the old generation which cleans up and compacts that space.
	* Major garbage collection are also Stop the World events. Often a major collection is much slower because it involves all live objects. So for Responsive applications, major garbage collections should be minimized.

#### GC available for Java
There are many different command line switches that can be used with Java. This section describes some of the more commonly used switches that are also used in this OBE.

Switch|Description
---	| ---
-Xms|Sets the initial heap size for when the JVM starts.
-Xmx|Sets the maximum heap size.
-Xmn|Sets the size of the Young Generation.
-XX:PermSize|Sets the starting size of the Permanent Generation.
-XX:MaxPermSize|Sets the maximum size of the Permanent Generation
-XX:+UseSerialGC | To enable the Serial Collector
-XX:+UseParallelGC | to enable new Parallel GC
-XX:+UseParallelOldGC | to enable old Parallel GC
-XX:ParallelGCThreads | To control number of garbage collector threads in Parallel GC
-XX:+UseConcMarkSweepGC | To enable the CMS Collector
-XX:ParallelCMSThreads=<n> | To set the number of threads in CMS collector
-XX:+UseG1GC | To enable the G1 Collector
-XX:SurvivorRatio | For providing ratio of Eden space and Survivor Space, for example if Young Generation size is 10m and VM switch is -XX:SurvivorRatio=2 then 5m will be reserved for Eden Space and 2.5m each for both the Survivor spaces. The default value is 8.
-XX:NewRatio | For providing ratio of old/new generation sizes. The default value is 2.
-verbose:gc | GC will print message on each run
-XX:+PrintCommandLineFlags | will print which GC is used
-XX:HeapDumpOnOutOfMemory | will take heap dump when app crashes due to OOM


##### The Serial GC
The serial collector is the default for client style machines in Java SE 5 and 6. With the serial collector, both minor and major garbage collections are done serially (using a single virtual CPU). In addition, it uses a mark-compact collection method.
>The Serial GC is the garbage collector of choice for most applications that do not have low pause time requirements and run on client-style machines.

##### The Parallel GC
The parallel garbage collector uses multiple threads to perform the young genertion garbage collection. By default on a host with N CPUs, the parallel garbage collector uses N garbage collector threads in the collection. The Parallel collector is also called a throughput collector. Since it can use multilple CPUs to speed up application throughput. 

> This collector should be used when a lot of work need to be done and long pauses are acceptable.

1. **Parallel GC** : multi-thread young generation collector with a single-threaded old generation collector. The option also does single-threaded compaction of old generation.
2. **Old Parallel GC** : the GC is both a multithreaded young generation collector and multithreaded old generation collector. It is also a multithreaded compacting collector.

##### The Concurrent Mark Sweep (CMS) Collector
collects the tenured generation. It attempts to minimize the pauses due to garbage collection by doing most of the garbage collection work concurrently with the application threads. Normally the concurrent low pause collector does not copy or compact the live objects. A garbage collection is done without moving the live objects. If fragmentation becomes a problem, allocate a larger heap.
* CMS collector on young generation uses the same algorithm as that of the parallel collector.
> should be used for applications that require low pause times and can share resources with the garbage collector


##### The G1 Garbage Collector
designed to be the long term replacement for the CMS collector. The G1 collector is a parallel, concurrent, and incrementally compacting low-pause garbage collector that has quite a different layout.
