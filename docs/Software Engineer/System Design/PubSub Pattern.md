![](https://i.imgur.com/2hhIQ8I.png)
- The publisher push a message to the topic, and immediately pushed to all the subscribers that subscribe to that publisher. Unlike message queue which batch messages until they are retrieved, pub0sub transfer messages with no or very little queueing
### Use cases
- Chat app
    - In a chat application, participants can subscribe to chat rooms which have a designated Pub/Sub topic.
    - When a user sends a message to a chat room, her chat app instance publishes the message on that chat room’s topic. Subscribers of the topic receive the message.
	![](https://i.imgur.com/8SQFrXx.png)
	- In a private chat, the two chat applications open up a temporary topic. The topic is named by a predefined logic, for example, by combining the user ids.
	![](https://i.imgur.com/koDkAkf.png)
	- [[Chat Application]]
- Event Notification
    - Sending events to large number of recipients
    - Publisher can send events without worrying about which clients are online, dont even need to know if new subscriber is registered to the system
    - Robust system's broker can persist messages and has at-least-once delivery so the publisher can simple fire and forget, knowing that the broker will deliver the messages when the subscribers need them
- Distributed cache
    - Why use Pub/Sub instead of normal message queue ?
- Distributed logging
    - Can send logs to many subscribed destinations simultaneously
- Multiple datasource