## Get hands dirty

Let's play with some API. There is no better way to understand what API is other than playing with it and seeing it in action.

### Prerequisites

You need some software installed first.

#### CLI

Install [websocat](https://github.com/vi/websocat#installation) to perform WebSocket request from your terminal

#### Browser

- Get [Postman](https://www.postman.com/) account to use Postman Workspaces
- Install [Postman Desktop Agent](https://www.postman.com/downloads/postman-agent/) to perform WebSocket requests

### Play with WebSocket API from Kraken

#### WebSocat

1. Establish a connection with the API
    ```bash
    websocat wss://ws.kraken.com
    ```
2. Ping the API to see if it responds. Just type the below message and hit Enter:
    ```bash
    {"event": "ping"}
    ```
3. Now subscribe to the event `ticker` stream that sends messages with currency price. Just type the below message and hit Enter:
    ```bash
    {  "event": "subscribe",  "pair": [    "XBT/USD",    "XBT/EUR"  ],  "subscription": {    "name": "ticker"  }}
    ```
4. You should now see a constant stream of data sent by the server. You do not have to ask the API every second for an update, as the update is pushed to you.

5. Now unsubscribe from info about one currency pair. Just type the below message and hit Enter:
    ```bash
    {    "event": "unsubscribe",    "pair": [        "XBT/USD"    ],    "subscription": {        "name": "ticker"    }}
    ```
6. You should see a message that you unsubscribed and the rest of the stream is just about EUR

#### Postman

1. Go to [this Postman Workspace with WebSocket APIs](https://www.postman.com/derberg/workspace/websocket-apis-mix)
2. Use `Kraken API` collection to first just ping the server and then to also send subscribe and unsubscribe messages