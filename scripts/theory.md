## What is API

It's like a conversation interface, but for the digital world.

- Do you like when people call you `Mr/Mrs/Miss`?
- What is your pronoun?
- Do you care about your titles, should we say `doctor` or `professor`
- Do you want to be asked a question you do not have an answer to?
- Are you in the mood for a talk?

Wouldn't it be nice to have it documented and publicly available? Oh wait -> [Solid Project](https://solidproject.org/)

## Sync vs Async

![](assets/websocket-shrek.webp)

## Specifications

* HTTP protocol -> [OpenAPI](https://github.com/OAI/OpenAPI-Specification) for REST API or others specs like GraphQL
* Async-related protocols -> [AsyncAPI](https://github.com/asyncapi/spec), [CloudEvents](https://github.com/cloudevents/spec) and [Schema Registry](https://github.com/cloudevents/spec/blob/main/schemaregistry/spec.md)

## Event Driven Architecture (EDA)

Q: Why do you do event-driven API, is REST API not enough?
A: Because I want to know what is happening in real-time without asking

## EDA Docs

- Most complex to document because of different protocols
- Not widely visible as REST API
  - [Programmableweb](https://www.programmableweb.com/category/all/apis) - 24,471 (Async APIs around 170)
  - [RapidAPI](https://rapidapi.com/categories) - Thousands! 
  - [APIs Guru](https://apis.guru/) - 2,351 OpenAPI examples!
  - [APITracker](https://www.apitracker.io/specifications/asyncapi) - 7 AsyncAPI examples...
- Rest is the same, spec doesn't solve everything

## EDA protocols

amqp, http, ibmmq, jms, kafka, anypointmq, mqtt, solace, stomp, websocket, mercure

## EDA Actors - Simplified

* Clients & Server
  <br/>
    ```mermaid
    graph TD
    server1[Server -> ws://localhost/travel/status]
    user1[Client -> Browser]
    user1 --> server1
    user2[Client -> Mobile]
    user2 --> server1
    ```
  <br/>
* Producers, consumers and message broker in the middle -> `fire and forget`
  <br/>
    ```mermaid
    graph TD
    server1[(mqtt://localhost:1883)]
    FlightMonitorService[Flight Monitor Service]
    FlightMonitorService -- flight/update --> server1
    FlightNotifierService[Flight Notifier Service]
    server1 -- flight/update --> FlightNotifierService
    FlightSubscriberService[Flight Subscriber Service]
    FlightSubscriberService -- flight/queue --> server1
    server1 -- flight/queue --> FlightMonitorService
    ```

