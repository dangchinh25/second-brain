![](https://i.imgur.com/QxABnL7.png)
- Publisher and Consumer connects to the RabbitMQ server via **AMQP connection** (underlying is still TCP but using AMQP Protocol)
    - Publisher send message to RabbitMQ server
    - RabbitMQ server push message to consumer when received
- 1 connection may have multiple channels
- A queue data structure is used inside the RabbitMQ server to manage message
![](https://i.imgur.com/fVKvIJf.png)
- Once a Consumer receive the message, it has to acknowledge the message so that the RabbitMQ server knows to dequeue the message from the queue, otherwise, when another consumer tries to get new message it will get the one that has already been processed by the other