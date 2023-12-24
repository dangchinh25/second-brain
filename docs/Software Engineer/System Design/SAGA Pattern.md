- Ref:
	- https://www.youtube.com/watch?v=C0rGwyJkDTU

![](https://i.imgur.com/z29TkBP.png)
- In *Saga pattern*, we execute a series of [[Transactions]] across multiple services and try to make it look like one.
- If there occur error in any of the steps, we try to compensate that with another action to ensure consistency and data integrity => *Compensating action* 
	- Example: A needs to be followed by B, C, D. B & C success but then D fails, w e then have to perform compensating action to roll back A & B & C.

# Orchestration
- There will be one main central services that orchestrate everything, including initiating the cross-service call and also perform compensating action as needed.
![](https://i.imgur.com/mqu0DSp.png)
- The black line means the initiating call, the red line means the compensating action.

- **Benefit**
	- Good for complex workflows involving many participants or new participants added over time.
	- Suitable when there is control over every participant in the process, and control over the flow of activities.
	- Doesn't introduce cyclical dependencies, because the orchestrator unilaterally depends on the saga participants.
	- Saga participants don't need to know about commands for other participants. Clear separation of concerns simplifies business logic.
- **Drawbacks**
	- Additional design complexity requires an implementation of a coordination logic.
	- There's an additional point of failure, because the orchestrator manages the complete workflow.
	 
# Choreography
- There will no central orchestrator, every communication between services will happen via events.
- Every services will react to event, and know what to do and how to compensate in case of errors.
![](https://i.imgur.com/cbihBuj.png)
- Black box and line means that an event is published with that topic
- Blue box and line means a service, here a service can subscribe to an event and can also publish events.

> There's a difference between [[SAGA Pattern#Orchestration | Orchestration]] and [[SAGA Pattern | Choreography]] 
>  - *Orchestration* requires a response from each of the cross-service calls
>  - Choreography does not call about the response or the subscriber at all, it just does it own job and publishes an event.
>  - In case of errors, a service may execute its own compensating action and publish a failure event, if any of the other services need to do any kind of compensating action it can just subscribe to the event and execute its own thing.
>  - *Choreography* is more [[Event Driven Architecture]]

- **Benefit**
	- Good for simple workflows that require few participants and don't need a coordination logic.
	- Doesn't require additional service implementation and maintenance.
	- Doesn't introduce a single point of failure, since the responsibilities are distributed across the saga participants.
- **Drawback**
	- Workflow can become confusing when adding new steps, as it's difficult to track which saga participants listen to which commands.
	- There's a risk of cyclic dependency between saga participants because they have to consume each other's commands.

# Usage
- Use the Saga pattern when you need to:
	- Ensure data consistency in a distributed system without tight coupling.
	- Roll back or compensate if one of the operations in the sequence fails.

- The Saga pattern is less suitable for:
	- Tightly coupled transactions.
	- Compensating transactions that occur in earlier participants.
	- Cyclic dependencies.

> **NOTES**: The Saga pattern can also be used in a monolith applications. If one operation execute a series of chaining local database call, we can and should use some kind of *compensating action* to revert the successful one if errors occur in a later step.