## L — Liskov substitution principle

Liskov Substitution Principal as defined by __Barbara Liskov & Jeannette Wing__

The Liskov substitution principle states that if S is a subtype of T, then objects of type T may be replaced (or substituted) with objects of type S.
This can be formulated mathematically as
Let ϕ(x) be a property provable about objects x of type T.
Then ϕ(y) should be true for objects y of type S, where S is a subtype of T.

More generally it states that objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.

This is actually in line with what Java also allows. A superclass reference can hold a subclass object i.e. superclass can be replaced by subclass in a superclass reference at any time. So, Java inheritance mechanism follows Liskov Substitution Principle.

__Liskov Substitution Principle: Example__

Lets say we have a class Vehicle and its two sub-classes Car & Bus as shown below:

![GitHub Logo](/LiskovSubstitution-Example.JPG)


Now, we can assign an object of type Car or that of type Bus to a reference of type Vehicle. All the functionality which is inherent in base class Vehicle, and is acquired by Bus and Car via inheritance, can be invoked on a reference of type Vehicle. Referring to the diagram above, it is possible to invoke methods like __getSpeed()__ & __getCubicCapacity()__ on a Vehicle reference which actually holds a Bus/Car object.The actual object’s overridden implementation of these methods will be actually invoked. This is exactly what the Liskov Substitution Principle also states – subtype objects can replace super type objects without affecting the functionality inherent in the super type.

__Liskov Violation Scenario Example: The Circle-Ellipse Problem__

All circles are inherently ellipses with their major and minor axes being equal. In terms of classes if we make a class Ellipse then Circle class will be a child of Ellipse class i.e. it will extend Ellipse class. Nothing wrong in it so far.

Lets now use the Liskov Substitution Principle. In this case an object of type Circle can be assigned to a reference of type Ellipse. So, all the methods in Ellipse can/could be invoked on this object of Circle which is stored in it. One inherent functionality of an ellipse is that its stretchable. I.e. the length of the two axes of an ellipse can be changed. Lets say we have methods __setLengthOfAxisX()__ and __setLengthOfAxisY()__ through which the length of the two axes X & Y of an ellipse can altered.

However, calling any of these two methods on an object of type circle inside a reference of type ellipse would lead to a circle no longer being a circle as in a circle the length of the major and minor axes have to be equal. This is known as the Circle-Ellipse Problem.

This is a typical violation scenario of Liskov Substitution Principle as assigning a subtype object to a super type reference doesn’t work which is not in line with the principle. The principle actually expects it to work smoothly.

__Relation of Liskov Substitution Principle with Open Closed Principle__

Open Closed Principle says that a class should be open for extension and closed for modification. I.e. to modify what a class does, we should not change the original class. Rather, we should override the original class and implement the functionality to be changed in the overriding class. This way when the derived class’s(or the sub-type’s) object is used in place of the parent/super-class, then the overridden functionality is executed on executing the same functionality. This is exactly in line with the Liskov Principle we just saw above.













