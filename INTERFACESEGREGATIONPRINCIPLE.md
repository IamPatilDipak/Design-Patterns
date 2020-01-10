## I — Interface segregation principle

**Robert C. Martin** has defined Interface Segregation Principle as – 
Many client specific interfaces are better than one general purpose interface


The interface segregation principle states that no client should be forced to depend on methods it does not use.

Put more simply: Do not add additional functionality to an existing interface by adding new methods.

Instead, create a new interface and let your class implement multiple interfaces if needed.

**Examining the definition of Interface Segregation Principle** – To understand Interface Segregation’s definition, let us assume that there is a system which has multiple functionalities and various clients using those functionalities via some interface such as a service or an API (Application Programming Interface).

What the Interface Segregation Principle advocates is that instead of having a single interface catering to all the clients, i.e. holding all the methods for all the clients, it is better to have multiple interfaces with each interface containing methods for a client-specific functionality or to have functionally cohesive interfaces.

Let us understand the above statement diagrammatically to make better sense of it. Take a look at the diagram below –

![GitHub Logo](/InterfaceSegregation-Example1.JPG)

There are multiple clients – Client1, Client1….Clientn. The GrandInterface has methods pertaining to Client1’s required functionality are shown in a block named [Client1 specific methods], Client2 specific functionality in [Client2 specific methods] and so on…

The **GrandInterface** shown above is an ideal example of a fat interface. It contains methods specific to all the clients. The problem with this design approach is that all clients will have to unnecessarily implement all other clients’ methods just to make their interface compile. Even if, instead of implementing, the individual clients implement other client-specific methods with an UnsupportedOperationException, still its a colossal waste of time as well necessary exposure of methods and then their mandatory implementations. A fat interface is thus an object-oriented designer’s nightmare because it makes the overall design rigid due to the enormous effort required to manage changes across all clients when making a change to a functionality/method pertaining to only one client.

Interface Segregation Principle avoids the design drawbacks associated with a fat interface by refactoring each fat interface into multiple segregated interfaces. Each segregated interface is a lean interface as it only contains methods which are required for a specific client. The refactored code with multiple lean interfaces would then result into the below design –

![GitHub Logo](/InterfaceSegregation-Example2.JPG)

**What do we mean by client-specific lean interfaces** – An important thing to note at this point is that when we mention client-specific lean interfaces, we do not really mean interfaces pertaining to each individual customer or user. What we instead mean is that each lean interface caters to a consumers of a specific type of functionality or a specific set of customers all of whom have the same functional needs.

Let us look at a real-world object-oriented use case example in Java to understand how a fat interface can be refactored in to multiple lean interfaces when Interface Segregation Principle is applied to it.

**Real-world object-oriented use case example in Java**
Let us say that there is a Restaurant interface which contains methods for accepting orders from online customers, dial-in or telephone customers and walk-in customers. It also contains methods for handling online payments (for online customers) and in-person payments (for walk-in customers as well as telephone customers when their order is delivered at home).

Now let us create a Java Interface for Restaurant as described above and name it as RestaurantInterface.java –

```
public interface RestaurantInterface {
  public  void acceptOnlineOrder();
  public  void takeTelephoneOrder();
  public  void payOnline();
  public  void walkInCustomerOrder();
  public  void payInPerson();
}
```
There are 5 methods defined in RestaurantInterface which are for accepting online order, taking telephonic order, accepting orders from a walk-in customer, accepting online payment and accepting payment in person.

Let us start by implementing the RestaurantInterface for online customers as OnlineClientImpl.java –

```
public class OnlineClientImpl implements RestaurantInterface{
  @Override
  public void acceptOnlineOrder() {
    //logic for placing online order
  }
  @Override
  public void takeTelephoneOrder() {
    //Not Applicable for Online Order
    throw new UnsupportedOperationException();      
  }
  @Override
  public void payOnline() {
    //logic for paying online       
  }
  @Override
  public void walkInCustomerOrder() {
    //Not Applicable for Online Order
    throw new UnsupportedOperationException();
  }
  @Override
  public void payInPerson() {
    //Not Applicable for Online Order
    throw new UnsupportedOperationException();      
  }
}
```

**Salient problems with the above Restaurant design** –
* Since the above code(OnlineClientImpl.java) is for online orders, the methods for telephonic order placement, walk-in order placement and in-person payment throw UnsupportedOperationException.

* Online, telephonic and walk-in clients use the RestaurantInterface implementation specific to each of them. Similar to the above OnlineClientImpl.java, the implementation classes for Telephonic client and Walk-in client will have methods which will be unsupported.

* Since the 5 methods are part of the RestaurantInterface, the implementation classes have to implement all 5 of them. The methods that each of the implementation classes don’t implement, they throw UnsupportedOperationException in those method implementations. As you can clearly see – implementing all methods is inefficient.

* Any change in any of the methods of the RestaurantInterface will be propagated to all implementation classes. Maintenance of code then starts becoming really cumbersome and regression effects of changes will keep increasing.

* RestaurantInterface.java breaks Single Responsibility Principle (read tutorial) because the logic for payments as well as that for order placement is grouped together in a single interface.

**Applying Interface Segregation Principle**

To overcome the above mentioned problems with restaurant design, we apply Interface Segregation Principle to refactor the above design and do the following changes –

* Separate out payment and order placement functionalities into two separate lean interfaces. Let’s name them PaymentInterface.java and OrderInterface.java.

* Each of the clients use one implementation each of PaymentInterface and OrderInterface. For example – OnlineClient.java uses OnlinePaymentImpl and OnlineOrderImpl and so on.

* Single Responsibility Principle is now adhered to as Payment interface(PaymentInterface.java) and Ordering interface(OrderInterface) are separate and have their client-specific implementations. I.e. two separate hierarchies.

* Change in any one of the order or payment interfaces does not affect the other. They are independent now.
There will be no need to do any dummy implementation or throw an UnsupportedOperationException as each interface has only methods it will always use.

**Diagrammatic view of application of Interface Segregation Principle**

Have a look at the diagram below. It clearly shows the design before and after application of the Interface Segregation Principle. The independent class hierarchies of segregated lean interfaces of order placement and payment processing are clearly visible in different colours.

![GitHub Logo](/InterfaceSegregation-Example3.JPG)
