# This Project is Tribute to Domain Driven Design and Messaging Architecture

# Objectives

- To provide a simple example of how to implement `Domain Driven Design` and `Messaging Architecture` in PHP.  
- To show how to implement `CQRS`, `Event Sourcing` and `Hexagonal Architecture` in PHP.
- To show how to implement `Resilient` and `Observable` `Microservices` in PHP.

# Why PHP?

I am [Dariusz Gafka](https://medium.com/@dariuszgafka) author of [Ecotone Framework](https://docs.ecotone.tech/), which is PHP Framework for building resilient, business oriented system in PHP.    
And my aim is to show that no matter of how complex your business is, you can build it in PHP.    
The current tooling is good, yet it can be much better and by joining `Ecotone` with frameworks like `Symfony` or `Laravel`, we can push PHP to the next level.
And really no matter if you work in `E-Commerce`, `ERP` or `Finance based systems`, you can build using `Open-Source in PHP` and be sure that it will be resilient, observable and maintainable.

# Why Ecotone?

[Ecotone is production ready PHP Framework](https://medium.com/dev-genius/building-reactive-message-driven-systems-in-php-ba185697becf) for building resilient, business oriented systems in PHP.    
It provides you with all the tools you need to build `Domain Driven Design`, `CQRS`, `Event Sourcing` and `Hexagonal Architecture` in PHP.    
It integrates with `Symfony` and `Laravel` and allows you to connect Services seamlessly using `Microservices` / `Event-Driven Architecture` like approach.  
It's built continuously from `2017` and used in production by many companies.  
By providing `resiliency` and `observability`, it provides highest level of confidence for building system in any business domain.

# Stack

- [Ecotone](https://docs.ecotone.tech/) - PHP Framework for building resilient, business oriented system in PHP
- [Symfony](https://symfony.com/) - PHP Framework for building web applications
- [Laravel](https://laravel.com/) - PHP Framework for building web applications
- [RabbitMQ](https://www.rabbitmq.com/) - Message Broker for asynchronous communication
- [Ecotone Pulse](https://docs.ecotone.tech/modules/ecotone-pulse) - Monitor for any processing errors and recover from failed messages.
- [Jaeger](https://www.jaegertracing.io/) - Distributed Tracing for monitoring and troubleshooting microservices-based distributed systems

# Business Domain

`Laravel+Ecotone application` is `Customer Service`, where customers can report issues. This happens using `CQRS` combined with `Eloquent Models`.  
![Laravel CQRS](documentation/customer-service.png "Laravel CQRS")  
Whenever new issue is reported, email is send to the with confirmation. This happens using `Asynchronous Event Handlers`, which are backed by `RabbitMQ`.     
![Laravel asynchronous event handling](documentation/issue-reported.png "Laravel asynchronous events")

`Symfony+Ecotone application` provides `Backoffice Service`, place where employees can be registered.
All issues reported by customers are synchronized to `Backoffice Service` and are correlated with tickets.  
The synchronization between `Customer Service` and `Backoffice Service` is done using `Events` and `Distributed Bus` for cross service communication via `RabbitMQ`.  
![Symfony Microservice](documentation/ddd-cqrs-event-sourcing-php-hexagonal-architecture.png "Symfony Microservice")
Employees can work on the tickets provides information about the status. As we want to know full history, Tickets are `Event-Sourced`.    
From Event Sourced tickets we build different `Read Model Projections`, which are used to display information about the ticket.  

The whole communication between Services is monitored and traced using [OpenTelemetry](https://opentelemetry.io/) with `Jaeger`.  
Any issue that happens in the system is reported to `Ecotone Pulse`, which is used to recover from failed messages.  

# Playing with the demo 

Run `docker-compose up -d` to start all services.  

Go to [Customer Service](http://localhost:3000/) to report issue as a customer.   
Whenever issue is reported, confirming email will be sent to the customer, which can be found [Mailbox](http://localhost:3004/).   
First time email is sent it's set up for failing, so you can get feeling of working with system that can recover from error messages using [Ecotone Pulse](http://localhost:3006/service/customer_service).  
From [Backoffice Service](http://localhost:3001/prepared-tickets) you may start working on ticket, that was correlated with issue reported by customer.  
You may also check [Jaeger](http://localhost:3007/) to get full overview of how communication looks like and how each component behave and what is happening.