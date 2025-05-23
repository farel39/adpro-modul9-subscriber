# Reflection:
## What is AMQP?
AMQP (Advanced Message Queuing Protocol) is an open standard protocol for message-oriented middleware. It enables applications to send and receive messages asynchronously and reliably across different platforms and technologies.
Key features of AMQP:

* Message-oriented: Focuses on the delivery of discrete messages
* Queuing: Stores messages until the receiving application can process them
* Routing: Supports various message routing patterns (point-to-point, publish-subscribe)
* Reliability: Guarantees message delivery with features like acknowledgments and transactions
* Security: Provides authentication and encryption mechanisms
* Platform-independence: Allows communication between applications written in different languages

AMQP is commonly implemented by message brokers like RabbitMQ, Apache ActiveMQ, and Azure Service Bus.
## What does "amqp://guest:guest@localhost:5672" mean?
Breaking down this connection string:

* amqp:// - The protocol identifier
* First "guest" - Username for authentication to the message broker
* Second "guest" - Password for authentication to the message broker
* localhost - The hostname where the message broker is running (your local machine)
*  5672 - The default port number for AMQP connections

This connection string tells the application to connect to a RabbitMQ server running on your local machine at port 5672, using "guest" as both the username and password for authentication.

## Proof of Simulating slow subscriber
![Proof of Simulating slow subscriber.](Simulating%20slow%20subscriber.png)
The image shows the condition of the first chart that has a spike when simulating a subscriber who is slow in processing data. It can be seen that there are about more than 10 messages in the queue at one time for a few seconds and then the number decreases. This happens because the subscriber takes 10ms to process a message, so the messages pile up temporarily in rabbitmq when the publisher publishes many events in a short time. Also i run the publisher app 3 times in quick succession which made it possible that the queue rises above 10.

## Proof of Running 3 Subscriber Simultaniously 
![Proof of Running 3 Subscriber Simultaniously .](Running%203%20Subscriber.png)
The image shows 3 subscriber programs that receive messages from the publisher that are run several times.
## Proof of Monitoring rabbitmq with 3 Subscriber
![Proof of Monitoring rabbitmq with 3 Subscriber.](Monitoring%203%20Subscriber.png)

The image shows the rabbitmq chart. It can be seen that queued messages have a lower peaked spike of only 3 compare to more than 10 in only 1 subscriber. This is because messages in the queue will be sent to one subscriber, not to all subscribers, so when waiting for one subscriber to process data, rabbitmq can send the next message to another subscriber who has finished processing data or is in an empty state. This is also seen in the previous image because each subscriber receives messages in a different order.