# Getting started with the Instamrkt API (1.0)

## Overview
We use the word "user" to mean the end user of your app/site/device and "you" to mean the developer implenenting your app/site/device and integrating with Instamrkt.  

### Introduction
Instamrkt is an API based prediction market with 2 modes of operation. Both the REST version of the API and the websocket mode support similary functionality but with different operational characteristics. Websockets are push based while REST must be polled to retrieve updates. 

Regardless of the connection mechanism much of the terminology is the same so let's start with that. 

### Basic terminology
Every registerd site or app has a unique *source* defined by a **source_id** that identifies the owner of the games underneath it. It serves as a top level grouping for all games that happen underneath it. The **source_id** is a globally unique string. 

A *game* defined by a **game_id** is a single game (full game, not set or half or quarter or period) - e.g. Chelsea v. Man United, Federer v. Nadal, Knicks v. Nets. The **game_id** is a globally unique string. 

It is worth noting that games are not unique by name - only by id. That is, you could have 2 games called "World Cup Game 3" each with a different id. Names matter, don't do this, it's confusing for everybody.  

A *pool* defined by a **pool_id** is a single question about an *event* in the game - e.g. "Will Federer's next serve be an ace?" is a pool. The  *pool_id* is a globally unique string. 

This is a good time to introduce the concepts of *actions* and *targets*. Every *pool* contains at MOST 1 *action* and 2 or more *targets*. In the previous example of "Will Federer's next serve be an ace?" In this case the options available to the user would be "Yes" and "No". These are targets. Federer's ace is the action. 

A more complex pool structure might allow the user to choose from a set of targets (team members for example). Something like "Who will get substituted next?" with options available to the user of "Messi, Neymar, Suarez, None". In this case "Messi, Neymar, Suarez" are targets. "None" is a special target appended to all *parimutuel* pools that indicates "None of these choices". It is up to you whether or not to display the "None" target. 

Instamrkt supports several variations of pools but the basic distinctions are between *parimutuel* and *binary*. 

**Parimutuel**
Parimutuel pools are defined by having multiple targets (usually more than 2) and the "None" target which allows a user to choose the target for the specified action. 

**Binary**
Binary pools only have two targets which are "yes" and "no". A user may choose only those options when submitting a prediction. A synthetic ID is generated for the "yes" option while the no option target is "None".


### What happens to unresolved predictions?
COMING **TODO**

### The flow of your application
**Login**
 
*We assume that you have signed up (registered) on the Instamrkt.com site already - either directly on our site or by using an embedded script.*

Make a <a href="#loin">REST request to login to Instamrkt</a> by passing username, password and optionally a next parameter indicating a URL to redirect to after login. 

**Websocket Connection**

Using the *user_session_key* and *user_id* a conection to the websocket may now be established. 

To connect to the websocket create the connection string as follows:
Create and HMAC using SHA256 of (id, nonce, user_session_key) where *id* is the user_id returned from the login message, *nonce* is a random string (we use current UTC epoch time in milliseconds) and *user_session_key* is the value of the same name returned in the login or signup request. 

**Retrieving Game Lists**

Each source identified by its *source_id* is its own encapsulated universe of games. Games are not shareable across sources. The reason for this is to separate games for different users of the platform. 

**You must kow the source_id for the source you are trying to display games for.** 

There is currently no way to get a list of sources. You are provided with a unique source_id when you sign up for the platform. You do not need to keep the source_id a secret. 

For testing you can use Instamrkt development source_id which is:
`c295ecdc-9194-428a-8353-20513856cda1`

Using your source_id or the Instamrkt development id make a websocket request for all available games in that source by sending message <a href="#list-games-for-source">list_games_for_source (code 380)</a> to the server. 

The response will be a list of games that are currently open to receive predictions. 

To get a list of all upcoming games please use the REST call to <a href="#games-upcoming">list upcoming games</a>


**Subscribing to a game** 

You will not receive game related messages until you have <a href="#join-game">subscribed to a game</a>

From the list of returned games choose the *game_id* you wish to join and send message <a href="#join-game">join_game (code 363)</a>. When you receive a successful response you will start to receive all updates for that game. Note that joining a game triggers the server to notify all other users in your source that a new user (you) has joined the game. Please see the <a href="#join-game#">join_game</a> published message for more information.  

**Getting available pools** 

Pools can be in one of three states. 

1. Open (Available)
2. Running
3. Closed (Resolved)

**Open**

Open pools are pools in which a user has not yet made a prediction. You can obtain a list of open pools by sending message <a href="#list-all-open-pools-for-game">list_all_open_pools_for_game (code 387)</a>. These are intended to be displayed as available for a user to make a prediction. 

If you only want to see open pools for yourself send message <a href="">list_my_open_betting_pools_for_game (code 388)</a>. 

**Running** 

Running pools have a prediction made by a user but do not yet have an outcome. They are in a pending state. For example, if a question arrives "who will score the next goal" and the user selects "Messi" the pool will remain in the *running* state until a goal is scored. If no goal is scored please see the section <a href="">What happens to unresolved predictions?</a>. Obviously, we must wait until the next goal is scored before we move the pool from the *running* state to the *resolved* state. Instamrkt will notify you when a pool must be moved from state to state. 

**Closed**

Closed pools have a user prediction, an outcome and a status. Essential pieces of information to convey to the user are:

1. The prediction they made
2. The actual outcome
3. Optional - The points won through this prediction
4. Optional - How many people the user beat

**Retrieving Pool State** 
You can request the state of each of the various pools with by sending the following messages:

NOTE that requesting *running* and *closed* pools will only return the users pools. 

1. *Open* - send the message <a href="">list_open_pools_for_game (code 387)</a> for all opened pools and <list_my_open_pools_for_game (code 388)</a> for users open pools.
2. *Running* - send the message <a href="">list_my_running_pools_for_game (code 389)</a>
3. *Closed* - send the message <a href="">list_my_resolved_pools_for_game (code 390)</a>



### NOTES
The action is usually specified in the title of the pool or chosen by the user if the interface allows it. A user may choose at MOST one target and one action. A pool is fully specified if both a target and an action are chosen. Instamrkt does not support compound predictions (parlays) at this time. 
