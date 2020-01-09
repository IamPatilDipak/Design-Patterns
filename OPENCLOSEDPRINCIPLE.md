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

