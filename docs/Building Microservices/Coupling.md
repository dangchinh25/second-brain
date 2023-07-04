- When services are loosely coupled, a change to one service should not require a change to another. The whole point of a microservice is being able to make a change to one service and deploy it without needing to change any other part of the system.
- A classic mistake is to pick an integration style that tightly binds one service to another, causing changes inside the service to require a change to consumers.
- A loosly coupled service nows as little as it needs to about the services which it collaborates. This also means we probably want to limit the number of different types of calls from one service to another, chatty communication can lead to tight coupling.

## Domain Coupling
- Domain coupling describes a situation in which one microservice needs to interact with another microservice, because the first microservice needs to make use of the functionality that the other microservice provides

![](https://i.imgur.com/eW87W50.jpg)
- In this example, `Order Processor` calls the `Warehouse` to reserve stock, and `Payment` microservice to take payment. The `Order Process` is now coupled to `Warehouse` and `Payment` microservices for this operation.
- In a microservice architecture, this type of interaction is largely unavoidable. A microservice-based system relis on multiple microservices collaborating in order for it to do its work. We still want to keep this minimum, and if we see a single microservice depending on multiple downstream services this way, it may imply a service that is doing too much.
## Temporal Coupling
- Quite similar to [[Coupling#Domain Coupling]], it refers to a situation in which one microservice needs another microservice to do something at the same time for the operation to complete => Both microservices need to be up and available to communicate with each other at the same time for the operation to complete.
## Pass-Through Coupling
- Describe a situtation in which one microservice passes data to another microservice purely because the data is needed by some other microservice further downstream.
- It implies not only that the caller knows not just that the microservice it is invoking calls but also the microservice downstream.

![](https://i.imgur.com/wpIGmrR.jpg)
- `Order Processor` is sending a request to `Warehouse` to prepare an order for dispatch. As part of the request payload, we send along a `Shipping Manifest`. This `Shipping Manifest` contains not only the address of the customer but also the shipping type. The `Warehouse` just passes this manifest on to the downstream `Shipping` microservice.
## Common Coupling
- Common coupling occurs when two or more microservices make use of a common set of data. A simple and common example of this form of coupling would be multiple microservices making use of the same share database, but it could also manifest itself in the use of share memory or a shared file system.
- The main issue with common coupling is that changes to the structure of the data can impact multiple microservices at once.
- For example, if microservice A and microservice both access the same database, a change in the schema would result in having to update the schema in both microservice.
## Content Coupling

![](https://i.imgur.com/YNv6DNt.jpg)
- `Order` service that is supposed to manage the allowable state changes to orders in our system.
- The `Order Processor` is sending requests to the `Order` service, delegating not just the exact change in state that will be made but also responsibility for deciding what state transition are allowed.
- The `Warehouse` service is directly updating the table in which order data is stored, by passing any functionality in the `Order` service that might check for allowable changes. We have to hope that the `Warehouse` service has a consistent set of logic to ensure that only valid changes are made.