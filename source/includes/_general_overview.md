# General overview

General overview of the system and its APIs.

Instamrkt is completely API driven. As such, we served as the first client developers against the API while building the demo live football (soccer) prediction app.

The Instamrkt API is divided into two parts:

1. REST API
2. Websocket API

The Websocket API is used for distributing all real-time game-related information. Everything else can (and should) be obtainable through the REST API.


## System architecture

![System architecture](images/architecture.png)

There are three primary components.

1. API server
2. Resolver
3. Event server

### API server

The API server is the primary connection point for all clients. It is a websocket that distributes information to clients via JSON messages. All clients that use the websocket (in contrast to the REST API) connect here. Clients receive solicited (responses to requests) and unsolicited (events and updates) messages from this stream.
**Clients will only interact with the API server** but it is important to understand the overall architecture of the system. 

### Resolver

The *Resolver* is a process that receives events and is responsible for checking all open pools and determining if the incoming event is sufficient to definitively determine an outcome.

### Event Server

*Event Server* is the source of events that can potentially resolve pools or simply events that should be passed on to clients to inform the of current state of the game. _This is an optional part of the system, as events can also be registered manually through the API by admins with appropriate privilege._

## User flow overview

User flow is as follows:

1. User logs in through the website / app with the signup credentials he provided (username + password + (optionally) a 2fa token) or with an externally-provided token (Facebook login). This is performed through the REST API. The call sets cookies for user id ("im_user") and a session key ("user_session_key") is generated which is necessary to connect to the Websocket API Server. These fields are also included in the JSON response to the REST loginrequest as "id" and "user_session_key" respectively. If it is an admin login the call returns "admin_session_key".

2. User gets to the game screen / page and using the obtained session key <a href="#connecting"> connects</a> to the Websocket API.

3. User is now connected to the API and can subscribe to games to receive push updates about them and place predictions.

Please check our <a href="#sample-scenarios">Sample scenarios</a> section for examples of more detailed game flow.