
- Calls between different processes across a network *inter-process* are very different from calls within a single process *in-process*
	- *inter-process*: Communication between 2 microservices living on 2 different host via a network
	- *in-process*: Similar to function call

There are a few problem that we need to be aware of when dealing with *inter-process* communication
- Performance
- Changing interfaces
- Error Handling
	- With distributed system, we are vulnerable to a lot of errors that are outside of our control.
	- There are 5 main types of failure modes
		- Crash Failures
		- Omission Failure
			- You sent something, but you didn't get a response.
		- Timing Failure
		- Response Failure
		- Arbitrary Failure

# Styles of Communication
![](https://i.imgur.com/ip0BWdq.png)

- *Synchronous Blocking*: A microservice make a call to another microservice and blocks operation waiting for the response
- *Asynchronous Blocking*: The microservice emitting a call is able to continue with the flow of execution without waiting for the result of the *inter-process* call
- *Request-Response*: A microservice sends a request to another microservice for something to be done. It expects to receive a response informing it of the result 
- *Event Driven*: Microservice emit events. which other microservice consume and react to accordingly. The microservice emitting the event is unaware of which microservices consume the events it emits
- *Common Data*: Not often seen as a communication style, microservices collaborate via some shared data source.

> It's important to note that a microservice architecture as a whole may have a mix of styles of collaboration, and this is typically the norm. It's common for a single microservice to implement more than one form of collaboration.

## Synchronous Blocking
- With synchronous blocking call, a microservice sends a call to another microservice and blocks until the call has completed, and potentially until a response has been received
![](https://i.imgur.com/nunGwHw.png)
- Typically, a synchronous blocking call is one that is waiting for a response from another microservice. This maybe because the result of the call is needed for some further operation, or just because it wants to make sure the call worked and to carry out some sort of retry if not. As a result, almost all synchronous blocking call is a request-response call.
### Advantages
- Easy and simple to implement and learn
- Familiar as most things we do is already in this type of communication
### Disadvantages
- [[Coupling#Temporal Coupling]]
- As the sender of the call is blocking and waiting for the downstream microservice to respond, it also follows that if the downstream microservice responds slowly, or if there is n issue with the latency of the network => Cascading effect caused by 1 single downstream microservice.
### Where to Use it
- For simple microservice architecture or when we don't a lot of chains of call

## Async Nonblocking
- Async nonblocking is the act of sending a call out over the network doesn't block the microservice issuing the call. It is able to carry on with any other processing without having to wait for a response.
- Comes in many form, but we will be looking at the most 3 common styles
	- *Communication through common data*: The upstream microservice change some common data, which one or more microservices later make use of
	- *Request response*
	- *Event Driven communication*: A microservice broadcast an event, which can be thought of as a factual statement about something that has happened. Other microservices can listen for the events they are interested in and react accordingly.
### Advantages
- The microservice making the initial call and the microservice (or microservices) receiving the call decoupled temporarily. The microservices that receive the call do not need to be reachable at the same time the call is made.
### Disadvantages
- Complex
- Too many choices

## Communication Through Common Data
- This pattern is used when one microservices puts data into a defined location, and another microservice (or multiple microservices) then makes use of the data.
- Can be as simple as one microservice dropping a file in a location, and at some point later on another microservice picks up that file and does something with it.
![](https://i.imgur.com/IPErp4F.png)

### Request-Response Communication
- With request-response, a microservice sends a request to a downstream service asking it to do something and expects to receive a response with the result of the request.
- This interaction can be undertaken via a synchronous blocking call, or it could be implemented in an asynchronous non-blocking fashion
![](https://i.imgur.com/BT0m53H.png)
### Implementation
### Where to Use it
- Any situation in which the result of a request is needed before further processing can take place.
- Situation where a microservice wants to know if a call didn't work so taht it can carry out some sort of compensating action.

## Event Driven Communication
- Rather than a microservice asking some other microservice to do something, a microservice emits events that may or may not be received by other microservices. It is an inherently asynchronous interaction, as the event listeners will be running on their own thread of execution.
- An event is a statement about something that has occurred, nearly always something that has happened inside the world of the microservice that is emitting the event.
- The microservice emitting the event has no knowledge of the intent of other microservices to use the event, and indeed it may not even be aware that other microservices exist.
![](https://i.imgur.com/rNFHnkP.png)
- The Warehouse just publishing events, assuming that interested parties will react accordingly. It is unaware of who the recipients of the events are, making event driven interactions much more loosely coupled in general.
- The intent behind an event could be considered the opposite of a request. The event emitter is leaving it up to the recipients to decide what to do. With request, response, the microservice sending the request knows what should be done and is telling the other microservice what it thinks needs to happen next.
### What is an Event?
- With a request, we are asking a microservice to do something and providing the required information for the requested operation to be carried out. With an event, we are broadcasting a fact that other parties might be interested in, but as the microservice emitting an event can't and shouldn't know who receives the event, how do we know what information other parties might need from the event? What should be inside the event

1. Just an ID
- One option is for the event to just contain an identifier for the newly updated/affected resource and let the subscribed microservice fetch this information from the source
![](https://i.imgur.com/OtmOOKZ.png)
- There are some downsides with this approach
	- [[Coupling#Domain Coupling]] as the `Notifications` has to know about the `Customer` microservice
	- If there are too many subscriber and all make the request at the same time, there can be some performance impact.
2. Fully detailed events
- The alternative is to put everything related to that specific affected resource into the event so that the subscribed service does not have to fetch. 
- However there can still be some consideration
	- The size of the event
	- Concerns if we are trying ti limit the scope of which microservices can see what kind of data
	- Once we put data into an event, it becomes part of our contract with the outside world. We have to be aware that if we remove a field from an event, we may break external parties. 