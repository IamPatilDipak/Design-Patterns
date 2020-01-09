# SOLID Principles

SOLID is an acronym for 5 important design principles when doing OOP (Object Oriented Programming).

The intention of these principles is to make software designs more understandable, easier to maintain and easier to extend.
As a software engineer, these 5 principles are essential to know.

In this article, will be explaining these principles, giving examples of how they are violated, and how to correct them so they are compliant with SOLID. Examples will be given in Java, but applies to any OOP language.

## S — Single responsibility principle

The Single Responsibility Principle states that every module or class should have responsibility over a single part of the functionality  provided by the software.

You may have heard the quote: “Do one thing and do it well”.

In the article Principles of Object Oriented Design, Robert C. Martin defines a responsibility as a *__‘reason to change’__, and concludes that a class or module should have one, and only one, reason to be changed.*

A class should have a single responsibility, where a responsibility is nothing but a reason to change.

Let’s take an example of how to write a piece of code that violates this principle.

```
class User {
    void CreatePost(Database db, string postMessage) {
        try {
            db.Add(postMessage);
        }catch (Exception e) {
            db.LogError("An error occured: ", e.ToString());
            File.WriteAllText("LocalErrors.txt", e.ToString());
        }
    }
}
```

We notice that how the `CreatePost()` method has too much responsibility, given that it can both create a new post, log an error in the database, and log an error in a local file. This violates the single responsibility principle.

Let’s try to correct it.

```
class Post {
    private ErrorLogger errorLogger = new ErrorLogger();

    void CreatePost(Database db, string postMessage) {
        try {
            db.Add(postMessage);
        }catch (Exception e)
        {
            errorLogger.log(e.ToString())
        }
    }
}

class ErrorLogger {
    void log(string error) {
      db.LogError("An error occured: ", error);
      File.WriteAllText("LocalErrors.txt", error);
    }
}
```

By abstracting the functionality that handles the error logging, we no longer violate the single responsibility principle.
Now we have two classes that each has one responsibility; to create a post and to log an error, respectively.

## O — Open/closed principle

The open/closed principle states that software entities (classes, modules, functions, etc.) should be open for extensions, but closed for modification.

Open/Closed Principle was originally defined by __Bertrand Meyer__ in his book Object Oriented Software Construction. 

Bertrand Meyer’s definition was divided into 3 statements.The first 2 statements defined the notions of Open & Closed modules(or classes) –

__1st statement__ – As per Meyer a module is Open when – A module will be said to be open if it is still available for extension. 
For example, it should be possible to add fields to the data structures it contains, or new elements to the set of functions it performs.

__What it means__ – If attributes or behavior can be added to a class it can be said to be “open”.

__2nd statement__ – As per Meyer a module is Closed when – A module will be said to be closed if it is available for use by other modules. This assumes that the module has been given a well-defined, stable description (the interface in the sense of information hiding).

__What it means__ – If a class is re-usable or specifically available for extending as a base class then it is closed. One of the basic requirements for a class to be closed in this way is that its attributes and methods should be finalized because if they change all the classes which inherit the base class are affected.

Meyer’s third statement gave the final shape to the Open/Closed principle which is so much in practice in Object Oriented Programming.

__3rd statement__ – Meyer defined that a class adheres to the Open/Closed Principle when – the class is closed, since it may be compiled, stored in a library, baselined, and used by client classes. But it is also open, since any new class may use it as parent, adding new features. When a descendant class is defined, there is no need to change the original or to disturb its clients.

__What it means__ – A class can be open and closed at the same time. To elaborate on this further, we need to next consider the previous two statements by Meyer where he describes the conditions for a class to be Open and Closed together.

__Conditions for a class to be Open & Closed together (and satisfy the Open/Closed Principle)__

* A class can be considered to be closed if its runtime or compiled class is available for use as a base class which can be extended by child classes. Baselining here refers to making sure that changes are guaranteed to not happen. In short – The said class is open for extension.
* A class can be considered to be open if its functionality can be enhanced by sub-classing it. When a class is a sub-class then by using the Liskov Substitution rule it can be replaced by its sub-class. This sub-class behaves as its parent class but is an enhanced version of it. In short – The said class is open for modification(via extension).

__Example of Open/Closed Principle__

Lets say we need to calculate areas of various shapes. We start with creating a class for our first shape Rectangle which has 2 attributes length & width–

```
public class Rectangle{
 public double length;
 public double width;
}
```

Next we create a class AreaCalculator to calculate area of this Rectangle which has a method calculateRectangleArea() which takes the Rectangle as an input parameter and calculates its area –

```
public class AreaCalculator{
  public double calculateRectangleArea(Rectangle rectangle){
    return rectangle.length *rectangle.width;
  }
}
```
So far so good. Now lets say we get our second shape circle. So we promptly create a new class Circle with a single attribute radius -

```
public class Circle{
 public double radius; 
}
```
Then we modify AreaCalculator class to add circle calculations through a new method calculateCircleArea() –

```
public class AreaCalculator{
  public double calculateRectangleArea(Rectangle rectangle){
    return rectangle.length *rectangle.width;
  }
  public double calculateCircleArea(Circle circle){
    return (22/7)*circle.radius*circle.radius;
  } 
}
```

However, note that there were flaws in the way we designed our solution above.

Lets say we have a new shape pentagon next. In that case we will again end up modifying AreaCalculator class. As the types of shapes grows this becomes messier as AreaCalculator keeps on changing and any consumers of this class will have to keep on updating their libraries which contain AreaCalculator. As a result, AreaCalculator class will not be baselined(finalized) with surety as every time a new shape comes it will be modified. So, this design is not closed for modification.

Also, note that this design is not extensible, i.e what if complicated shapes keep coming, AreaCalculator will need to keep on adding their computation logic in newer methods. We are not really expanding the scope of shapes; rather we are simply doing piece-meal(bit-by-bit) solution for every shape that is added.

__Modification of above design to comply with Open/Closed Principle__

Let us now see a more elegant design which solves the flaws in the above design by adhering to the Open/Closed Principle. We will first of all make the design extensible. For this we need to first define a base type Shape and have Circle & Rectangle implement Shape interface –

```
public interface Shape{
  public double calculateArea();
}
 
public class Rectangle implements Shape{
  double length;
  double width;
  public double calculateArea(){
    return length * width;
  }
}
 
public class Circle implements Shape{
  public double radius;
  public double calculateArea(){
    return (22/7)*radius*radius;
  }
}
```

__The design has now undergone the following important changes to become extensible in nature –__

* There is a base interface Shape. All shapes now implement the base interface Shape
* Shape interface has an abstract method calculateArea(). Both circle & rectangle provide their own overridden implementation of      calculateArea() method using their own attributes.
* We have brought-in a degree of extensibility as shapes are now an instance of Shape interfaces. This allows us to use Shape instead of individual classes wherever these classes are used by any consumer.

The last point above mentioned consumer of these shapes. In our case consumer will be the AreaCalculator class which would now look like this –

```
public class AreaCalculator{
  public double calculateShapeArea(Shape shape){
    return shape.calculateArea();
  }
}
```

This AreaCalculator class now fully removes our design flaws noted above and gives a clean solution which adheres to the Open-Closed Principle.

__The design is now correct as per Open Closed Principle due to the following reasons__ –

* The design is open for extension as more shapes can be added without modifying the existing code. We just need to create a new class for the new shape and implement the calculateArea() method with a formula specific to that new shape.
* This design is also closed for modification. AreaCalculator class is complete w.r.t area calculations. It now caters to all the shapes which exists now, as well as to those that may be created later.

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

Now, we can assign an object of type Car or that of type Bus to a reference of type Vehicle. All the functionality which is inherent in base class Vehicle, and is acquired by Bus and Car via inheritance, can be invoked on a reference of type Vehicle. Referring to the diagram above, it is possible to invoke methods like __getSpeed()__ & __getCubicCapacity()__ on a Vehicle reference which actually holds a Bus/Car object.The actual object’s overridden implementation of these methods will be actually invoked. This is exactly what the Liskov Substitution Principle also states – subtype objects can replace super type objects without affecting the functionality inherent in the super type.

__Liskov Violation Scenario Example: The Circle-Ellipse Problem__

All circles are inherently ellipses with their major and minor axes being equal. In terms of classes if we make a class Ellipse then Circle class will be a child of Ellipse class i.e. it will extend Ellipse class. Nothing wrong in it so far.

Lets now use the Liskov Substitution Principle. In this case an object of type Circle can be assigned to a reference of type Ellipse. So, all the methods in Ellipse can/could be invoked on this object of Circle which is stored in it. One inherent functionality of an ellipse is that its stretchable. I.e. the length of the two axes of an ellipse can be changed. Lets say we have methods __setLengthOfAxisX()__ and __setLengthOfAxisY()__ through which the length of the two axes X & Y of an ellipse can altered.

However, calling any of these two methods on an object of type circle inside a reference of type ellipse would lead to a circle no longer being a circle as in a circle the length of the major and minor axes have to be equal. This is known as the Circle-Ellipse Problem.

This is a typical violation scenario of Liskov Substitution Principle as assigning a subtype object to a super type reference doesn’t work which is not in line with the principle. The principle actually expects it to work smoothly.

__Relation of Liskov Substitution Principle with Open Closed Principle__

Open Closed Principle says that a class should be open for extension and closed for modification. I.e. to modify what a class does, we should not change the original class. Rather, we should override the original class and implement the functionality to be changed in the overriding class. This way when the derived class’s(or the sub-type’s) object is used in place of the parent/super-class, then the overridden functionality is executed on executing the same functionality. This is exactly in line with the Liskov Principle we just saw above.














