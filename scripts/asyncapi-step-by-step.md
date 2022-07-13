### 1. Choose your editor

* [AsyncAPI Studio](https://studio.asyncapi.com/)
* [AsyncAPI CLI](https://github.com/asyncapi/cli)
* [VSCode extension](https://marketplace.visualstudio.com/items?itemName=ivangsa.asyncapi-preview)
* [IDEA plugin](https://plugins.jetbrains.com/plugin/15673-asyncapi)

### 2. Choose between YAML or JSON

Yes, just joking, no human would choose JSON...right? 

### 3. Specify AsyncAPI version

We release regularly every few months. Learn more from [official release process instruction](https://github.com/asyncapi/spec/blob/master/RELEASE_PROCESS.md).

```yaml
asyncapi: 2.4.0 #https://github.com/asyncapi/spec/releases
```

### 4. Provide overal info about the App

```yaml
info:
  title: Shrek App
  version: '1.0.0'
  description: | # Markdown supported, multiline YAML to the rescue
    Purpose of this app is to have some fun with AsyncAPI and WebSocket and define an interface for ... Shrek.
    ![](https://media.giphy.com/media/10Ug6rDDuG3YoU/giphy-downsized.gif)
    You can use this API to chat with Shrek bot or to get updates about artifical travels to different locations.
```

### 5. Specify how users can connect to the App

> In case you do not like WebSocket example. Take [this Kafka example](https://github.com/asyncapi/spec/blob/v2.4.0/examples/streetlights-kafka.yml#L17)

```yaml
servers:
  swamp:
    url: localhost
    protocol: ws
```

> Sometimes it is the server that points directly to the app (WebSocket) but can also be a remote server, broker address.

### 6. What are the communication channels

In case of WebSockets you could say these are like endpoints. In case of most broker-centric architecture they are also called topics.

```yaml
channels:
  /chat: {}
  /travel/status: {}
```

### 7. What are the messages there floating in your system

`Components` enable you to better structure information in AsyncAPI files. It is all about the readability after all.

```yaml
components:
  messages: # events
    chatMessage:
      summary: Message that you send or receive from chat
      payload:
        type: string
```

### 8. Maybe let's first try with schema now

```yaml
components:
  schemas: # payload
    travelData:
        type: object
        properties:
            destination:
                description: Name of travel destination.
                type: string
            distance:
                description: How much distance left to the target.
                type: string
            arrival:
                description: Time left to get there.
                type: string
```

and now reference inside the message.

```yaml
components:
  messages:
    travelInfo:
      summary: Message that contains information about travel status.
      payload: 
        $ref: "#/components/schemas/travelData"
      examples:
        - payload:
            destination: Far far away
            distance: Beyond the seven mountains and seven forests
            arrival: Pretty soon
```

### 9. Reference messages in channels

Remember that you also need to specify if message is something users can consume or actually send to the app.

```yaml
channels:
  /chat:
    subscribe:
      summary: Client can receive chat messages.
      operationId: subChatMessage
      message:
        $ref: '#/components/messages/chatMessage'
    publish:
      summary: Client can send chat messages.
      operationId: pubChatMessage
      message:
        $ref: '#/components/messages/chatMessage'
  /travel/status:
    subscribe:
      summary: Client can receive travel info status.
      operationId: subTravelInfo
      message:
        $ref: '#/components/messages/travelInfo'
```

### 10. Use bindings to specify query params for the channel

```yaml
channels:
  /travel/status:
    subscribe:
      summary: Client can receive travel info status.
      operationId: subTravelInfo
      message:
        $ref: '#/components/messages/travelInfo'
    bindings: 
      ws:
        bindingVersion: 0.1.0
        query:
          type: object
          description: |
            You can specify to receive travel status info from specific location
          properties:
            destination:
              type: string
              description: Airport code
              enum:
                - BCN
                - KTW
                - LCY
```

### 11. Provide info about response using extension

Let's say your chat has an option to specify when the answer to the chat message should go.

Like you have email from some system that specifies you should not reply, but do something else.

```yaml
components:
  messages: # events
    chatMessage:
      summary: Message that you send or receive from chat
      payload:
        type: string
      x-response:
        description: Location of the information about where reply should be sent to
        location: "$message.payload#/replyTo"
```

### BONUS

- You could have just copy [this AsyncAPI file](./asyncapi.yml) instead of following step by step
- For `traits` example just have a look at [this Kafka example](https://github.com/asyncapi/spec/blob/v2.4.0/examples/streetlights-kafka.yml#L159)
- For more reuse examples with `$ref` across files, look at [this social media example](https://github.com/asyncapi/spec/blob/v2.4.0/examples/social-media/comments-service/asyncapi.yaml)





