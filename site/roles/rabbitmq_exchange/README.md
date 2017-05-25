# ansible-modules-rabbitmq
RabbitMQ modules for ansible

This repository provides the following ansible modules:
- rabbitmq_exchange

All these modules use rabbitMQ REST endpoint for their operation.

### rabbitmq_exchange
Allows create/delete operations on rabbitMQ Exchanges

####Example:
```
# Create direct exchange
- rabbitmq_exchange: name=directExchange

# Create topic exchange on vhost
- rabbitmq_exchange: name=topicExchange type=topic vhost=myVhost
```

### rabbitmq_queue
Allows create/delete operations on rabbitMQ Queues

####Example:
```
# Create a queue
- rabbitmq_queue: name=myQueue

# Create a queue on remote host
- rabbitmq_queue: name=myRemoteQueue login_user=user login_password=secret login_host=remote.example.org
```

### rabbitmq_binding
Allows create/delete on exchange bindings

####Example:
```
# Bind myQueue to directExchange with routing key info
- rabbitmq_binding: name=directExchange destination=myQueue type=queue routingKey=info

# Bind directExchange to topicExchange with routing key *.info
- rabbitmq_binding: name=topicExchange destination=topicExchange type=exchange routingKey="*.info"
```
