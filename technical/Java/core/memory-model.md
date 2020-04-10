# Memory model
The Java programming language memory model works by examining each read in an execution trace and checking that the write observed by that read is valid according to certain rules.

The memory model describes possible behaviors of a program. An implementation is free to produce any code it likes, as long as all resulting executions of a program produce a result that can be predicted by the memory model.

Java memory model consist of 2 important sections
 1. **JVM Stack**
 2. **Heap**
 3. **Permanent Generation**

## JVM Stack
* every thread has different stack
* different scoping is maintained inside a stack
* a code block execution ends with the pop of all of its scoped variables from the stack
* If the computation in a thread requires a larger Java Virtual Machine stack than is permitted, the Java Virtual Machine throws a _StackOverflowError_.
* If Java Virtual Machine stacks can be dynamically expanded, and expansion is attempted but insufficient memory can be made available to effect the expansion, or if insufficient memory can be made available to create the initial Java Virtual Machine stack for a new thread, the Java Virtual Machine throws an _OutOfMemoryError_.
> An implementation of JVM can have **Native Method Stacks** to execute methods in native programming languages.


## Heap
* long-lived data, across multiple methods
* java mem - stack = heap
* 1 heap, multiple stack (for each thread)
* The heap parts are: Young Generation, Old or Tenured Generation, and Permanent Generation.

### Young Generation
The young generation is the place where all the new objects are created. When the young generation is filled, garbage collection is performed. This garbage collection is called **Minor GC**. Young Generation is divided into three parts – ***Eden Memory*** and two ***Survivor Memory*** spaces.

### Old Generation Memory
Old Generation memory contains the objects that are long-lived and survived after many rounds of Minor GC. Usually, garbage collection is performed in Old Generation memory when it’s full. Old Generation Garbage Collection is called Major GC and usually takes a longer time.

### Garbage

* Java avoids memory leak by
	* running on virtual machine
	* adopting garbage collection stratefy (LISP used it first)
* unreachable objects are garbage

### final keyword
* stack to heap connection cannot be broken
* the object's value can be changed but cannot assign to a new object
* unsafe
* no const correction feature in java, const implementation is not there, gives a compiler error on usage

### Escaping references
 A method which returns a map, can expose the map which can be corrupted by calling environment.
 Ways to avoid this issue
  * make the class an Iterable, but it have remove().
  * return a new Map from the same values, but underlying object can be changed
  * make underlying object read-only

### Weak & Soft References
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

## Permanent Generation
The Permanent generation contains metadata required by the JVM to describe the classes and methods used in the application. The permanent generation is populated by the JVM at runtime based on classes in use by the application. In addition, Java SE library classes and methods may be stored here.

Classes may get collected (unloaded) if the JVM finds they are no longer needed and space may be needed for other classes. The permanent generation is included in a full garbage collection.

### Method Area
Method Area is part of space in the Perm Gen and used to store class structure (runtime constants and static variables) and code for methods and constructors

#### Runtime Constant Pool
Runtime constant pool is per-class runtime representation of constant pool in a class. It contains class runtime constants and static methods. Runtime constant pool is part of the method area.


## Memory Pool
Memory Pools are created by JVM memory managers to create a pool of immutable objects if the implementation supports it. String Pool is a good example of this kind of memory pool. Memory Pool can belong to Heap or Perm Gen, depending on the JVM memory manager implementation.
