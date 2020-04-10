# Garbage Collection
* gc() may not run when we invoke it
* GC calls finalize() on object
* never close resources in finalize as it might not be called.
* The process of clearing out garbage is ***Mark & Sweep***
* From Java 7, JVM maintains generational heap memory to assist GC process
### Mark & Sweep
* mark
	* pause the pgm execution
	* checks single live variable
	* all references are recursively marked
	* optionally all marked objects are allocated contagious memory
* sweep
	* references of unreachable unmarked objects are removed

## Generational GC Process
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

### GC available for Java
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


#### The Serial GC
The serial collector is the default for client style machines in Java SE 5 and 6. With the serial collector, both minor and major garbage collections are done serially (using a single virtual CPU). In addition, it uses a mark-compact collection method.
>The Serial GC is the garbage collector of choice for most applications that do not have low pause time requirements and run on client-style machines.

#### The Parallel GC
The parallel garbage collector uses multiple threads to perform the young genertion garbage collection. By default on a host with N CPUs, the parallel garbage collector uses N garbage collector threads in the collection. The Parallel collector is also called a throughput collector. Since it can use multilple CPUs to speed up application throughput.

> This collector should be used when a lot of work need to be done and long pauses are acceptable.

1. **Parallel GC** : multi-thread young generation collector with a single-threaded old generation collector. The option also does single-threaded compaction of old generation.
2. **Old Parallel GC** : the GC is both a multithreaded young generation collector and multithreaded old generation collector. It is also a multithreaded compacting collector.

#### The Concurrent Mark Sweep (CMS) Collector
collects the tenured generation. It attempts to minimize the pauses due to garbage collection by doing most of the garbage collection work concurrently with the application threads. Normally the concurrent low pause collector does not copy or compact the live objects. A garbage collection is done without moving the live objects. If fragmentation becomes a problem, allocate a larger heap.
* CMS collector on young generation uses the same algorithm as that of the parallel collector.
> should be used for applications that require low pause times and can share resources with the garbage collector


#### The G1 Garbage Collector
designed to be the long term replacement for the CMS collector. The G1 collector is a parallel, concurrent, and incrementally compacting low-pause garbage collector that has quite a different layout.
