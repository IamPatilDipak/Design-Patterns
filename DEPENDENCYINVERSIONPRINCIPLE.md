## D - Dependency inversion principle

The dependency inversion principle is a way to decouple software modules.

Depending on which source one refers to, Dependency Inversion Principle, as defined by **Robert C. Martin**, can be defined in any of the following ways

    Depend upon Abstractions. Do not depend upon concretions.
                         OR                          
    Abstractions should not depend upon details. Details should depend upon abstractions.
                         OR                          
    High-level modules should not depend on low-level modules. Both should depend on abstractions.


**Understanding the definition of Dependency Inversion Principle**

To comply with this principle, we need to use a design pattern known as a dependency inversion pattern, most often solved by using dependency injection.

All three definitions of Dependency Inversion mentioned above mean the same. What each of the definitions imply is that Dependency Inversion Principle fundamentally reverses the way in which dependencies are managed in software systems.

Rather than having the higher-level module directly depending on lower-level modules to carry out their responsibilities, this principle instead makes the higher-level module rely on an ‘abstraction’ or an ‘abstract interface’ representing lower-level module. The actual implementation of lower level module can then vary. As long as the lower-level module’s implementation is accessible to the higher-level module via the abstract interface, the higher-level module is able to invoke it.

To understand the Dependency Inversion Principle better, let us first take a look at how the traditional procedural systems have their dependencies organised. In procedural systems, higher level modules depend on lower level modules to fulfil their responsibilities. The diagram below shows the procedural dependency structure –

