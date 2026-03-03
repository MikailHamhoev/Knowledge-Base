* JDK, JRE, and JVM

The Java platform is built on three main layers, with the JVM at the core, the JRE surrounding it, and the JDK as the outernost layer meant for development.

** Java Virtual Machine
Platform-dependent engine that runs Java programs. It takes the Java bytecode (.class files) and converts in into machine code. Handles memory management and garbage collection.

** Java Runtime Environment
A software package that provides the minimum requirements to run Java applications. JRE = JVM + Core Libraries (like java.lang, java.util, java.io and other supporting files).

** Java Development Kit
A software package used to develop Java applications. JDK = JRE + Development Tools (like javac).

* JVM Sequential Phases

** Loading
Finding binary .class files with a particular name and creating a Class object from that binary. This is done by a class loader.

** Linking
Process of taking the class interface and combining it into the runtime state of the Java virtual machine so that it can be executed. Consists of three substeps:

*** Verification
Checking bytecode structure.

*** Preparation
Creating static fields for class interface and initializing these fields to their default values.

*** Resolution
Dynamically determining concrete values from symbolic references in the class's constant pool, essentially loading and linking other classes that the current class references.

** Initialization
The final phase. It occurs on the first active use of a class. Assigns the proper initial values to the static fieds. Involves executing the class's static initializers and static initialization blocks.