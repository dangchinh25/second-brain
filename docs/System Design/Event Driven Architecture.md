### Event
- There are 2 kind of events in an *Event Driven Architecture* system (*EDA*)
	- *Event/Message* contains information about something that happens, to notify others => Will be the focus
	- *Command* is like an order/request to others to execute something and expect a response from it
- An *event* can contain data or just notify something happened and it has to be *immutable*

### Component
- An *EDA* typically has 3 components: *Producer*, *Broker*, *Consumer*
- A *Producer* simply means thing that produces an *Event* into the *Broker*
- A *Consumer* means thing that consume *Event* from the *Broker* and do something with it
- A *Broker* takes in multiple *Events* from many different *Producers* and route/publish them to appropriate *Consumers*
- A service can be both *Producer* and *Consumer*
- This whole can also be called *Publish/Subscribe Architecture/Model/Pattern*

### Advantages
- **Decoupling**: In a typical microservices architecture, we may want a `service1` communicates to `service2`. So `service1` needs to know about the existence of `service2` and vice versa => *dependency*. With *EDA Broker*,  `service1` only needs to publish an *event* and `service2` only needs to know how to react to it => Reduce both *coupling* & *dependency*. With this pattern, we can easily add more service to listen to event from `service1` without any concern and it would just work.
- **Dependency Inversion**:  Similar to above
- **Scalability**:
	- The *event* can be persisted, making it easier to retrieve event later
	- No *Single Point of Failure*, using the *persisted events*, when a *Consumer* service comes up after fault, it can easily retrieve and process the *event*

### Disadvantages
- **Performances** due to the intermediate *Broker*
- **Consistency** as there is some delay waiting for the *Broker* and all the *Consumers* to finish process all the *events*
	- Ref: [[RDBMS#ACID]], [[Consistency patterns#Eventual Consistency]]
- **Complexity** to the overall system

### When to use
- When **Scalability** > **Performances**
- **Data Replication** to use **Event** to notify all **Consumer** to receive the same information