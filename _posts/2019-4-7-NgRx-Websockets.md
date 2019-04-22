---
layout: post
title: NgRx Websockets
image: https://user-images.githubusercontent.com/18272237/55688709-a5c2c300-5949-11e9-88c5-dc44b25dc963.png
---
Best way to store and receive tasks from the NgRx store?

#### What is it? `Protocol`
Websocket is a web protocol that is a simple solution for bidirectional application, which uses a single TCP connection for traffic in both directions.

#### What does it do? `Provides real-time data`
The Websocket protocol interacts between a client web browser and web server to bring real-time data transfer from and to the server. It enables two-way communication between the client & server to produce real-time applications.

#### How do you use/implement this? `Handshake to data transfer`
The Websocket protocol has two parts: a handshake and the data transfer. Uses one single port for clients and web severs. URI schemes such as `http` is not used, instead `ws` and `wss` are used for unencrypted and encrypted connections, respectively. The rest of the URI components are defined to used URI generic syntax.
Example: `ws://localhost:8000/api/app`  

#### Why is it useful? `Uses ONE TCP Connection(One port)`
Provides a mechanism for browser-based applications to use two-way communication with servers on one TCP connection(does not rely on opening multiple HTTP connections). Most web browsers support this protocol to bring chat, games and many more real-time applications to clients.

---
## Questions:
* What is it?
* What does it do?
* How do you use/implement it?
* If it useful, why?


## Actions:
* `InitializeWebsocketConnection` - Attempt to connect to websocket server, set websocket url
* `WebsocketConnectionEstablished` - Websocket connection was successfully established, change store status
* `WebsocketConnectionFailed` - Failed to connect to websocket server
* `WebsocketListener` - Listen for message types to dispatch actions, attempt to connect to websocket server for the first time
* `RequestUserTasks` - Send message to websocket server
* `ReceivedUserTasks` - Store the received tasks from the websocket server
* `WebsocketConnectionClosed` - Close websocket connection, change store status


{: .mb-0}
#### `Connect to Websocket`
<em> Workflow of establishing a connection with the backend websocket server to request the authenticated users tasks. </em>

{: .table .table-sm .table-bordered .table-striped .table-hover .table-dark }
| [WEBSOCKET] Initialize Websocket Connection       |
| [WEBSOCKET] Websocket Listener                    |
| [TASKS] Request User Tasks                        |
| [TASKS] Received User Tasks                       |
| [WEBSOCKET] Websocket Connection Closed           |
{: .w-auto .mb-4 .text-center }

* `Need to create a tasks interface for the data`
* `Need to refactor the workflow for the entire project to include the users tasks as part of the initialization process`


***
[webSocket rxjs](https://rxjs-dev.firebaseapp.com/api/webSocket/webSocket)

[Websocket Protocol](https://tools.ietf.org/html/rfc6455)

[Websocket Status Codes](https://tools.ietf.org/html/rfc6455#section-7.4)
