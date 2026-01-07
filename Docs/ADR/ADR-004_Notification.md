## Context and Problem Statement

ABC Limited would like major banking, telecom and airlines to utilise a complaints management system, the system must be capable of providing timely updates to consumers until the end of the ticket lifecycle, this could include when the ticket has been submitted, status updates, if the ticket has been escalated and resolution. 

The key architectural challenge was selecting a suitable notification system to enable the system to send notifications without impacting the systems performance or delaying API responses. A strategy was made to ensure there is no bottlenecks when it comes to performance and availability.

---

## Considered Options

### Celery + Redis
- Keeps and caches everything in memory (RAM) therefore it has high performance and speed, good for timely responses consumers require.
- Scale horizontally
- API responses remain fast and responsive. 

### AWS SQS
- Will not require scaling the queues, as AWS takes care of all the scaling and performance 
- No infrastructure required to set up SQS as it is a managed service. 
- Pay service, if the message is long the bill will also be high. 

### Third-Party Notification Platforms
- Designed to handle large volume of notifications and scale as the user base will be required to go annually 
- Limited control over for example the user experience and it may not align to the CMS system needs. 

---

## Decision Outcome

Currently the CMS uses asynchronous background processing for notifications, I have decided to implement using Celery as the task queue and Redis as the message broker. Notification related tasks, such as escalation alerts, OTP alerts and if the ticket has been resolved, 
are placed onto a background queue and processed independently of the main backend API. Ensuring that user facing API requests remain responsive while notifications are delivered promptly in the background.
---

## Rationale

In my CMS system I have chosen asynchronous processing (Celery & Redis). The process enables notifications tasks to be offloaded to background workers to allow the system to room smoothly without any delays from core CMS system management operations. 
Celery offers a reliable framework to execute background tasks whilst Redis offers fast message brokering which is suitable when workloads are high. 

---

## Consequences

### Positive Consequences
- Celery is free and open-source, reducing costs for ABC Limited  
- Simple and lightweight installation ( `pip install -U celery`)  
- Celery is compatible with various message brokers such as Redis, Amazon SQS, RabbitMQ 

### Negative Consequences
- Redis for large datasets can become expensive, especially when scaling up as it stores data in RAM
- Potential data loss, Redis keeps everything in RAM therefore if server crashes data is lost. 
