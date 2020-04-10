# JVM
`JVM intelligently identifies if an object lifecycle is ltd to its method / block then it is created in Stack instead of HEAP`

* running a pgm with ltd memory - give JVM args as -Xmx100m
* jvisualvm.exe to monitor the memory allocations
* jvm intelligently removes the garbage from heap to make more memory available for other variable allocation.
* there are marker interfaces which are only understood by JVM, which associates the implementation, e.g. Serializable

## Main Features
1. [Garbage Collection](gc.md)
2. [Serialization]
