- Message queue is a data structure
- Message Broker is a type of application that use message queue
- [https://stackoverflow.com/questions/50061928/message-queue-and-message-broker-differences](https://stackoverflow.com/questions/50061928/message-queue-and-message-broker-differences)

# Core Concept
- **Message Broker** handles the transfer of data between different services without them knowing anything about each other
- A broker allows loosely coupled communication, meaning that the message sender doesn't know who will receive the message
- The broker is responsible for routing the message to the proper destination
- Distributing the message system allows increasing the subscribers as needed to handle more messages

## Queues (Point-to-Point Message)
- The publisher send a message to the broker
- The broker selects the subscriber that will receive the published message for processing
- Only one subscriber will receive a message published to the queue unless the subscriber fails to process the message within a given time period
![](https://i.imgur.com/9nsiTjN.png)

- [[RabbitMQ]]
## Topics (Publish/Subscribe Message)

- Topic-bases message allows to publish every message to a topic that distributes it to every subscriber currently registered
- The broker doesn't care if the message was processed by all subscribers or just a subset of them
- [[PubSub Pattern]]