# jdk file structure
https://docs.oracle.com/javase/8/docs/index.html
https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jdkfiles.html

cli: to view the content in the jar
jar tf rt.jar

# how java object layout in memory? 
https://dzone.com/articles/java-object-memory
http://psy-lob-saw.blogspot.com/2013/05/know-thy-java-object-memory-layout.html

# how to look at JVM source code
https://www.slideshare.net/RednaxelaFX/hotspot-vm20120303

# how is the dynamic link between classes work? 

# how jvm work?
https://www.youtube.com/watch?v=ZBJ0u9MaKtM

# types of class loader
a "primordial" class loader and class loader objects

# what is class not trusted

# how primordial load the class

# the comparison between java class loader and c++ class loader
https://www.guru99.com/java-virtual-machine-jvm.html

# book
"Inside the Java Virtual Machine by Bill Venners"
https://www.artima.com/jvm/booklist.html
http://blog.jamesdbloom.com/JVMInternals.html

# java is designed for the network 
platform independence, network-mobility, and security are of prime importance in a networked
computing environment

# other benefits of Java
- object-orientation
- multi-threading
- structured error-handling
- garbage collection
- dynamic linking
- dynamic extension

# how java prevents you from inadvertently corrupting memory
- no way to directly access memory by abitrarily casting pointers to a different type or by using pointer arithmetic,
- automatic garbage collection
- array bounds checking
- checking object references, each time they are used, make sure they are not null

# jvm interept, JIT, install-time compiling
install-time compiling: 
The binary form that you deliver (Java class files) is platform independent, but the binary form that the end-user executes (a monolithic native executable) is platform specific. Because the translation from class files to native executable is done during installation on the end-userís system, optimizations can be made for the userís particular system setup

# obfuscate class files make prevent your class from be decompiled since it alter the class and field names

# java security 
- sandbox tech (sandbox of untrusted applets cannot do many things)
compoents responsible for sandbox
- class loader architecture: preventing untrusted classes from impersonating trusted classes
- class filer verifier
- safety features built into the Java Virtual Machine
- the security manager and the Java API

# class loader
custom class loader should be given a list of names of forbidden and restricted packages

# inside the JVM, the threads come in two flavors
- daemon: ordinarily used by virtual machine self (garbage collection)
- non-daemon: the initial thread of an application, begains at main()

# architecture of JVM 
- class loader subsystem
- excution engine subssystem
- runtime data area

# runtime data area
- method
- heap
- java stack
- pc register
- native method stack

# stack frame
- local variables
- operand stack
- frame data

# what's the constant pool resolution
- class,interface constant values
- fully qualified names of classes and interfaces
- field names and descriptors 
- method names and descriptor

# compare main memory and thread working memory

# java instructions: one rule states that all operations on primitive types, except in some cases longs and doubles, are atomic
The exception to this rule is any long or double variable that is not declared volatile. Rather than
being treated as a single atomic 64-bit value, such variables may be treated by some implementations as
two atomic 32-bit values. Storing a non-volatile long to memory, for example, could involve two 32-
bit write operations. This non-atomic treatment of longs and doubles means that two threads
competing to write two different values to a long or double variable can legally yield a corrupted
result

# when is the first active use for Class and Interface?

# where is field of "final static" stored in the memory
https://www.quora.com/In-which-part-of-memory-final-static-variables-gets-stored-in-java-jvm-memory

# compile-time constants
final static , which can be replaed with constant during compiling 

# can we override the java static method
no 
https://stackoverflow.com/questions/2223386/why-doesnt-java-allow-overriding-of-static-methods
static method data are loaded from the class file

# when loading one class, it will initlize its super classes first 
Superclasses need to be initialized before subclasses, so these classes are likely already loaded.

# class file would only contains the method and field declared by itself, not inherited from its super class or interface

# classs loading and initilization can happen at different time
the time for loading and linking can be dependended on different implementation
but one class should be loaded and linked before initilization 

during preparation, a method table may be allocated for each class which would include all methods for this class including the method inhertied from its super class. this would improve the program running performance 

# the structure of the class file (where is the source code)
https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-4.html#jvms-4.2.2

# type erasure and bridge method
type erasure 
https://www.baeldung.com/java-type-erasure
https://docs.oracle.com/javase/tutorial/java/generics/bridgeMethods.html#bridgeMethods

# method tables
method is in the method area

# garbage collection
Garbage detection is ordinarily accomplished by defining a set of roots and determining reachability
from the roots
Two basic approaches to distinguishing live objects from garbage are reference counting and tracing.

reference count:
advantages: collect quickly
disadvantages: 1. cannot distinguish the reference cycles 2. add overhead of incrementing and decrementing each time

tracing collectors: 
the algorithm is called "mark and sweep"


compacting collectors:
move live objects to one end, and sweep dead objects

copying collectors: 
there are two regions used in turns, move live objects to the new area if the gc process began

generation collectors:
Generational collectors address this inefficiency by grouping objects by age and garbage collecting
younger objects more often than older objects. In this approach, the heap is divided into two or more
sub-heaps, each of which serves one "generation" of objects.

adaptive collectors:

# thread synchronization
the mechanism monitoring

A monitor supports two kinds of thread synchronization: mutual exclusion and cooperation. Mutual
exclusion, which is supported in the Java Virtual Machine via object locks, enables multiple threads to
independently work on shared data without interfering with each other. Cooperation, which is supported
in the Java Virtual Machine via the wait and notify methods of class Object, enables threads to work
together towards a common goal.