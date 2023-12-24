
- Ref:
	- https://www.youtube.com/watch?v=i2eVTk2Fb40
![](https://i.imgur.com/f0Zfmb6.png)
- If we are using [[Event Driven Architecture]], we can (and should) store state changes of the system/database using *events* in some kind of *Event Logs*
- The *events* in the *Event Logs* should be stored in the order that it was created.
- By reading from the *Event Logs*, we can know the history of changes of the data, and can also recreate the data/rebuild the state of the system at any point => *Event Sourcing*
- **Features**
	- **Complete Rebuild**: We can always delete the database, and then let the system process the event logs again to recreate everything
	- **Temporal Query**: Because we have the history of changes, we can always query to see what was the state before the current state.

# Usage
- **Audit Log**: When we want to keep track of what happen in the past.
- **Parallel Processing**:
![](https://i.imgur.com/8GuPnHh.png)
	- We can have multiple system subscribing to the same data sources
	- When a change happens, all these services can process the changes in parallel
	- This is really helpful if we want to duplicate the data in multiple system easily
	- Not all services will process the event at the same time, so for some period of time, the data between services will be out of sync, but will gradually catchup/sync with each other => [[Consistency patterns#Eventual Consistency | Eventual Consistency]]