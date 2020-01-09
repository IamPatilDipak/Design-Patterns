## I — Interface segregation principle

**Robert C. Martin** has defined Interface Segregation Principle as – 
Many client specific interfaces are better than one general purpose interface


The interface segregation principle states that no client should be forced to depend on methods it does not use.

Put more simply: Do not add additional functionality to an existing interface by adding new methods.

Instead, create a new interface and let your class implement multiple interfaces if needed.

**Examining the definition of Interface Segregation Principle** – To understand Interface Segregation’s definition, let us assume that there is a system which has multiple functionalities and various clients using those functionalities via some interface such as a service or an API (Application Programming Interface).

What the Interface Segregation Principle advocates is that instead of having a single interface catering to all the clients, i.e. holding all the methods for all the clients, it is better to have multiple interfaces with each interface containing methods for a client-specific functionality or to have functionally cohesive interfaces.

Let us understand the above statement diagrammatically to make better sense of it. Take a look at the diagram below –

There are multiple clients – Client1, Client1….Clientn. The GrandInterface has methods pertaining to Client1’s required functionality are shown in a block named [Client1 specific methods], Client2 specific functionality in [Client2 specific methods] and so on…

The **GrandInterface** shown above is an ideal example of a fat interface. It contains methods specific to all the clients. The problem with this design approach is that all clients will have to unnecessarily implement all other clients’ methods just to make their interface compile. Even if, instead of implementing, the individual clients implement other client-specific methods with an UnsupportedOperationException, still its a colossal waste of time as well necessary exposure of methods and then their mandatory implementations. A fat interface is thus an object-oriented designer’s nightmare because it makes the overall design rigid due to the enormous effort required to manage changes across all clients when making a change to a functionality/method pertaining to only one client.

Interface Segregation Principle avoids the design drawbacks associated with a fat interface by refactoring each fat interface into multiple segregated interfaces. Each segregated interface is a lean interface as it only contains methods which are required for a specific client. The refactored code with multiple lean interfaces would then result into the below design –
