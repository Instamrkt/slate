# Websocket API: Betting pools


<aside class="notice">
This section assumes you have an active game opened and running. If not, read on how to open it <a href="#open-a-game">here</a>.
</aside>

## Bet pool types

We have two bet pool types recognized by the system:

1. Parimutuel pools.
2. Binary pools.

Parimutuel pools are pools with an arbitrary number of mutually exclusive targets, one of which is a target 'none'. These pools are resolvable automatically. A sample parimutuel pool would look like this: 'Who will score the next goal?' with targets: Brazil, Argentina, none.

Binary pools are pools with two options for selection. They are designed with yes/no questions in mind but not limited to that. They are not automatically resolvable - game cannot be closed until all these pools are resolved.
A sample binary pool would look like this: 'Will a brazilian player score today?' with targets: yes, no. For these pools action is generated internally - the parameter can be skipped.

## Create and open betting pool

<aside class="notice">
Only users listed as admins for the game are authorized for this call.
</aside>

<aside class="success">
This functionality is also provided automatically by instamrkt - pools are opened and resolved automatically.
</aside>

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 385,
    "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
    "description": "Next Goal By Team",
    "action_id": "Goal",
    "targets": "argentina,brazil",
    "pool_type": "1",
    "start_at": "",
    "finish_at": "",
    "stop_accepting_bets_at": "",
    "recreate_on_end": "False"
}
```


> Returns the following JSON structures:

```json
{
    "description": "Next Goal By Team",
    "res": 350,
    "pool_id": "f2dd90e9-50ba-4c23-8161-2541ecd6720c",
    "response_header": {
        "t": 1429260818762
    },
    "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
    "type": 1
}
```

```json
{
    "response_header": {},
    "res": 352,
    "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
    "pool_id": "f2dd90e9-50ba-4c23-8161-2541ecd6720c",
    "description": "Next Goal By Team",
    "type": 1,
    "action_id": "goal",
    "participants": 0,
    "amount_total": 0.0,
    "target_level": 1,
    "is_blocked": false,
    "is_hot": false,
    "distribution": [
        {
            "target_id": "brazil",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 0.0,
            "bets": []
        },
        {
            "target_id": "none",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 0.0,
            "bets": []
        },
        {
            "target_id": "argentina",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 0.0,
            "bets": []
        }
    ],
    "start_at": 1421928724803,
    "finish_at": null,
    "stop_accepting_bets_at": null
}
```

This creates and opens a pool for predictions for people subscribed to the game in the system. Predictions can now be made in this pool. The successful call results in broadcasting the response to all users subscribed to the game. The creator also gets a separate confirmation message.

How to make a prediction is described <a href="#place-bet-in-betting-pool">here</a>.

### Operation code

Name | Code
--------- | -------
create_and_open_betting_pool | 385

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id for which the pool is to be opened.
description | null | The pool description - question which will be desplayed to users.
action_id | null | An action on which the prediction is made. Has to be in a set of actions available for game.
targets | [] | List of targets on which predictions for actoin can happen. Has to be a subset of targets available for game. Sent as a comma-separated string or an array. Targets have to be of same level. That means "argentina, brazil" is correct, but "argentina, brazil:9" is not. **IMPORTANT**: target 'none' will be added to target list for parimutuel pools.
type | 1 | 1 - parimutuel event based, 2 - parimutuel time based, 3 - binary event based, 4 - binary time based.
start_at | now() | Pool opening time (> now()) in epoch millis. If left blank, will be opened as soon as registered by the server. **As of now pools are opened as soon as this call is received by the server.**
finish_at | null | Pool closing time (> open_at) in epoch millis. This will close the pool at a given time. **Warning**: this will automatically resolve th pool to **none**.
is_blocked | false | Is betting in pool now blocked.
is_hot | false | Is betting pool marked as "HOT".
stop_accepting_bets_at | null | Stop accepting bets at time (> now()) in epoch millis. This parameter is optional. Include if you want to close the pool after a certain time.
recreate_on_end | False | Boolean value describing if pool is to be recreated anew upon resolution.

### Response codes

Name | Code | Result
--------- | ------- | -----------
betting_pool_created | 350 | Your confirmation.
new_betting_pool | 352 | New pool has been created and is now opened for predictions (broadcast to all users subscribed to game).

### Response Parameters

#### Betting pool created

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The provided game id.
pool_id | :YOUR_POOL_ID | The server-generated pool id. Necessary for making all pool-related calls.
description | null | The provided pool description - question which will be desplayed to users.
type | 1 | Provided pool type: 1 - parimutuel event based, 2 - parimutuel time based, 3 - binary event based, 4 - binary time based.

#### New petting pool

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The provided game id.
pool_id | :YOUR_POOL_ID | The server-generated pool id. Necessary for making all pool-related calls.
description | null | The provided pool description - question which will be desplayed to users.
type | 1 | Provided pool type: 1 - parimutuel event based, 2 - parimutuel time based, 3 - binary event based, 4 - binary time based.
action_id | null | The provided action on which the prediction is made.
participants | 0 | Number of people who already placed predictions in pool. Will be incremented on every prediction.
amount_total | 0.0 | Amount of money already placed predictions in pool. Will be incremented on every prediction.
target_level | 0 | The computed target level for sent targets. E.g. - 1 for "argentina, brazil", 2 for "argentina:9, brazil:7"
distribution | [] | A list of current predictions, each an object with predictions for a given target (containing a list of all bets, with details, current target multiplier, display name and total amount backing the target).
start_at | request receival time | The open pool time in epoch millis.
live_start_at | request receival time | The real life game start time in epoch millis (only informative).
finish_at | null | The passed pool closing time. (Relevant only for pools of type 2 and 4).
is_blocked | false | Is betting in pool now blocked.
is_hot | false | Is betting pool now marked as "HOT".
stop_accepting_bets_at | null | Provided stop accepting bets at time (> now()) in epoch millis.


## Resolve pool / register game event

<aside class="notice">
Only users listed as admins for the game are authorized for this call.
</aside>

<aside class="success">
This functionality is also provided automatically by instamrkt - pools are opened and resolved automatically.
</aside>

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 395,
    "action_id": "goal",
    "target_id": "brazil",
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce",
    "happened_at": 1421933670549
}
```


> Returns the following JSON structures:

```json
{
    "response_header": {},
    "res": 366,
    "action_id": "goal",
    "target_id": "brazil",
    "target_display_name": "brazil",
    "current_count": {
        "brazil": "1"
    },
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce",
    "happened_at": 1421933670549
}
```

```json
{
    "response_header": {},
    "res": 362,
    "pool_id": "7d48ecbc-cd97-41fa-94e8-ca0a8ff22629",
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce",
    "winning_target": "brazil",
    "description": "Next Goal By Team",
    "winners": {"3": 10.0},
    "losers": {"1": 7.0, "2": 3.0},
    "invalidated_bettors": [],
    "is_money_return": false,
    "resolved_at": 1421933670582
}
```

This registers an event for a game (e.g. a goal, yellow card etc.). The event consists of an action and a target. This resolves all pools opened for action containing the target in its available to target. People who predicted the target win, all others lose.

Events of higher levels resolve also pools for events of lower levels. E.g., an event "goal" for target "brazil:9", will resolve all pools opened for "goal" containing "brazil:9" **and** "brazil" in its targets.

Pool resolution in turn will cause sending out a bet_pool_resolved message, also described below. It also sends out a <a href="#balance-update-server-message">new balance</a> message to all users participating in a pool.

### Operation code

Name | Code
--------- | -------
register_game_event | 395

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id for which the pool is to be opened.
description | "" | The pool description - question which will be desplayed to users.
action_id | null | An action which happened in the game. Has to be contained in a set of actions available for game.
target_id | null | Target which performed the action. Has to be contained in a set of targets available for game. **Furhtermore**, has to be of maximum target level. E.g. if game contains targets such as "brazil:9, argentina:9, brazil, argentina", only targets "brazil:9, argentina:9" will be accepted (a team cannot score a goal without a player scoring).
game_id | :YOUR_GAME_ID | The game id for which event happened.
happened_at | null | Epoch millis when event happened. If skipped, defaulted to message receival time.

### Response codes

Name | Code | Result
--------- | ------- | -----------
new_game_event | 366 | Event was registered. Broadcast to all game subscribers too.
bet_pool_resolved | 362 | Pool resolved. Sent for every pool for action and target. Broadcast to all game subscribers.

### Response Parameters

#### New game event

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id for which the pool is to be opened.
description | "" | The pool description - question which will be desplayed to users.
action_id | null | The provided action which happened in the game.
target_id | null | The provided target which performed the action.
target_display_name | null | The provided target's display name.
current_count | {} | A dictionary containing current count for action in the game - **key** - target_id, **value** - count of action for target.
game_id | :YOUR_GAME_ID | The provided game id for which event happened.
happened_at | null | Epoch millis when event happened. If skipped, defaulted to message receival time.

#### Pool resolved

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id for which the pool was resolved.
pool_id | :YOUR_POOL_ID | The resolved pool id.
description | null | The provided pool description.
winning_target | null | The winning target for the pool (from the subset of targets available for pool).
winners | {} | The dictionary containing all winners. **key** - user id, **value** - how much each user won (in propotion of what they input into the pool).
losers | {} | The dictionary containing all losers. **key** - user id, **value** - how much each user lost (in propotion of what they input into the pool).
invalidated_bettors | [] | The list of user ids of people who placed their prediction too late (usually between the time of event happening and its registering by the server).
resolved_at | event receival time | The time of pool resolution in epoch millis.

## Pool expired (server message)


> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 363,
    "pool_id": "7d48ecbc-cd97-41fa-94e8-ca0a8ff22629",
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce",
    "winning_target": "brazil",
    "description": "Next Goal By Team",
    "winners": {"3": 10.0},
    "losers": {"1": 7.0, "2": 3.0},
    "invalidated_bettors": [],
    "is_money_return": false,
    "expired_at": 1421933670582
}
```

When a pool is created as a time-based and it expires (e.g.: "Will Brazil score before 65:00?") that means the pool will be resolved with target 'none'. That means people who predicted this target are victorious. The message broadcast to all game subscribers.

### Response code

Name | Code | Result
--------- | ------- | -----------
bet_pool_expored | 363 | Pool expired (resolved to 'none'). Sent for every pool for action and target. Broadcast to all game subscribers.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id for which the pool was resolved.
pool_id | :YOUR_POOL_ID | The resolved pool id.
description | null | The provided pool description.
winning_target | null | The winning target for the pool (from the subset of targets available for pool).
winners | {} | The dictionary containing all winners. **key** - user id, **value** - how much each user won (in propotion of what they input into the pool).
losers | {} | The dictionary containing all losers. **key** - user id, **value** - how much each user lost (in propotion of what they input into the pool).
invalidated_bettors | [] | The list of user ids of people who placed their prediction too late (usually between the time of event happening and its registering by the server).
resolved_at | event receival time | The time of pool resolution in epoch millis.


## List all open pools for game

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 387,
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce"
}
```


> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 3535,
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce",
    "pools_list": [
        {
            "start_at": "1421933670629",
            "description": "Next Goal By Team",
            "amount_total": 0.0,
            "finish_at": "None",
            "is_blocked": false,
            "is_hot": false,
            "distribution": [
                {
                    "target_id": "brazil",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                },
                {
                    "target_id": "none",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                },
                {
                    "target_id": "netherlands",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                }
            ],
            "target_level": 1,
            "participants": 0,
            "pool_id": "0ec3850f-f862-4baf-bada-73394a68bbdd",
            "stop_accepting_bets_at": "None",
            "type": 1,
            "action_id": "goal"
        },
        {
            "start_at": "1421935990222",
            "description": "Next Card By Team",
            "amount_total": 0.0,
            "finish_at": "None",
            "is_blocked": false,
            "is_hot": false,
            "distribution": [
                {
                    "target_id": "brazil",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                },
                {
                    "target_id": "none",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                },
                {
                    "target_id": "netherlands",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                }
            ],
            "target_level": 1,
            "participants": 0,
            "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d",
            "stop_accepting_bets_at": "None",
            "type": 1,
            "action_id": "card"
        }
    ]
```

List all pools opened for predictions for a given game.

### Operation code

Name | Code
--------- | -------
list_all_betting_pools_for_game | 387

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id for which the pools are to be listed.

### Response code

Name | Code | Result
--------- | ------- | -----------
all_betting_pools_for_game | 3535 | Opened betting pools for game have been listed to you.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id for which the pools are listed.
pools_list | [] | A list of pools opened for game.

## List my open pools for game

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 388,
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce"
}
```


> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 353,
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce",
    "pools_list": [
        {
            "start_at": "1421933670629",
            "description": "Next Goal By Team",
            "amount_total": 0.0,
            "finish_at": "None",
            "is_blocked": false,
            "is_hot": false,
            "distribution": [
                {
                    "target_id": "brazil",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                },
                {
                    "target_id": "none",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                },
                {
                    "target_id": "netherlands",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                }
            ],
            "target_level": 1,
            "participants": 0,
            "pool_id": "0ec3850f-f862-4baf-bada-73394a68bbdd",
            "stop_accepting_bets_at": "None",
            "type": 1,
            "action_id": "goal"
        }
    ]
```

List all pools opened for predictions for a given game **in which you have not yet placed a prediction**.

### Operation code

Name | Code
--------- | -------
list_my_open_betting_pools_for_game | 388

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id for which the pools are to be listed.

### Response code

Name | Code | Result
--------- | ------- | -----------
you_open_pools_list_for_game | 353 | Opened betting pools for game in which you have not placed a prediction have been listed to you.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id for which the pools are listed.
pools_list | [] | A lost of pools opened for game in which you have not placed a prediction.


## List my running pools for game

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 389,
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce"
}
```


> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 354,
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce",
    "pools_list": [
        {
            "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d",
            "start_at": "1421935990222",
            "amount_total": 1.0,
            "finish_at": "None",
            "is_blocked": false,
            "is_hot": false,
            "distribution": [
                {
                    "target_id": "brazil",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 1.0,
                    "bets": [
                        {
                            "bet_id": "c34bcc05-58ea-4294-9310-9f8cbd121e29",
                            "placed_at": 1421937458,
                            "amount": 1.0,
                            "user_id": 1
                        }
                    ]
                },
                {
                    "target_id": "none",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                },
                {
                    "target_id": "netherlands",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                }
            ],
            "name": "Next Card By Team",
            "target_level": 1,
            "participants": 1,
            "user_backed": {
                "brazil": 1.0
            },
            "stop_accepting_bets_at": "None",
            "type": 1,
            "action_id": "card"
        }
    ]
```

List all pools opened for predictions for a given game **in which you have not yet placed a prediction**.

### Operation code

Name | Code
--------- | -------
list_my_running_pools_for_game | 389

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id for which the pools are to be listed.

### Response code

Name | Code | Result
--------- | ------- | -----------
your_running_pools_for_game | 354 | Opened betting pools for game in which you have placed a prediction have been listed to you.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id for which the pools are listed.
pools_list | [] | A list of pools opened for game in which you have placed a prediction.

## List my resolved pools for game

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 390,
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce"
}
```


> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 355,
    "pools_list": [
        {
            "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d",
            "total_winnings": 0.0,
            "resolved_at": 1421938211000,
            "start_at": 1421935990000,
            "description": "Next Goal By Team",
            "winning_target": "brazil",
            "distribution":[
                {
                    "target_id": "brazil",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 1.0,
                    "bets": [
                        {
                            "bet_id": "c34bcc05-58ea-4294-9310-9f8cbd121e29",
                            "placed_at": 1421937458,
                            "amount": 1.0,
                            "user_id": 1
                        }
                    ]
                },
                {
                    "target_id": "none",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                },
                {
                    "target_id": "netherlands",
                    "backing_people": 0,
                    "to_win_if_backed": 1.0,
                    "display_name": "brazil",
                    "backing_money": 0.0,
                    "bets": []
                }
            ],
            "participants": 1,
            "pool_type": 1,
            "user_backed": {
                "brazil": 1.0
            },
            "stop_accepting_bets_at": 1421938211000,
            "game_name": "Netherlands - Brazil",
            "amount_total": 1.0,
            "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce",
            "target_level": 1,
            "action_id": "goal"
        }
    ],
    "game_id": "1b9a4a42-0c8a-46c0-95ee-c1e649e16fce"
}
```

List all pools for a given game **in which you have placed a prediction** and the pool has been resolved.

### Operation code

Name | Code
--------- | -------
list_my_resolved_pools_for_game | 390

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id for which the pools are to be listed.

### Response code

Name | Code | Result
--------- | ------- | -----------
your_resolved_pools_for_game | 355 | Betting pools for game in which you had placed a prediction that had been resolved have been listed to you.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id for which the pools are listed.
pools_list | [] | A list of pools opened for game in which you have placed a prediction and that haved been resolved.

## Place bet in betting pool

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 392,
    "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d",
    "target": "brazil",
    "amount": "1"
}
```


> Returns the following JSON structures:

```json
{
    "response_header": {},
    "res": 356,
    "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d",
    "bet_id": "c34bcc05-58ea-4294-9310-9f8cbd121e29",
    "amount": "1"
}
```

```json
{
    "response_header": {},
    "res": 361,
    "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d",
    "target_level": 1,
    "distribution": [
        {
            "target_id": "brazil",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 1.0
        },
        {
            "target_id": "none",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 0.0
        },
        {
            "target_id": "netherlands",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 0.0
        }
    ],
    "new_bets": [
        {
            "bet_id": "c34bcc05-58ea-4294-9310-9f8cbd121e29",
            "placed_at": 1421937458,
            "amount": 1.0,
            "user_id": 1
        }
    ]
}
```

Places a prediction for the specified target for the specified amount in the specified pool.

If user does not have sufficient available funds, an appropriate dedicated error message is returned.

### Operation code

Name | Code
--------- | -------
place_bet_in_betting_pool | 392

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | null | Pool id in which the prediction is to be placed.
target | null | The target which you want to back.
amount | 0.0 | Amount that you want to wager.

### Response codes

Name | Code | Result
--------- | ------- | -----------
bet_placed_in_betting_pool | 356 | You have successfully placed your prediction.
betting_pool_bets_distribution_update | 361 | You have changed the pool distribution. This is broadcast to all game subscribers.

### Response Parameters

#### Placing confirmation

Parameter | Default | Description
--------- | ------- | -----------
pool_id | :YOUR_POOL_ID | The pool id for which the prediction was placed.
pool_id | :NEW_BET_ID | Server-generated bet id.
target | null | The target which you have backed.
amount | 0.0 | The amount which you have specified.

#### Updated distribution

Parameter | Default | Description
--------- | ------- | -----------
pool_id | :YOUR_POOL_ID | The pool id for which the distribution was updated.
target_level | 0 | The computed target level for sent targets. E.g. - 1 for "argentina, brazil", 2 for "argentina:9, brazil:7"
distribution | [] | A list of current predictions, each an object with predictions for a given target (*does not* contain a list of all bets, contains current target multiplier, display name and total amount backing the target).
new_bets | [] | A list of new bets that caused the distribution update, with details.


## Get betting pool distribution

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 393,
    "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d"
}
```


> Returns the following JSON structures:

```json
{
    "response_header": {},
    "res": 356,
    "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d",
    "bet_id": "c34bcc05-58ea-4294-9310-9f8cbd121e29",
    "amount": "1"
}
```

```json
{
    "response_header": {},
    "res": 360,
    "pool_id": "6cca4a84-d3e5-4d56-9f5e-acb50ecd5f1d",
    "target_level": 1,
    "distribution": [
        {
            "target_id": "brazil",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 1.0,
            "bets": [
                {
                    "bet_id": "c34bcc05-58ea-4294-9310-9f8cbd121e29",
                    "placed_at": 1421937458,
                    "amount": 1.0,
                    "user_id": 1
                }
            ]
        },
        {
            "target_id": "none",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 0.0,
            "bets": []
        },
        {
            "target_id": "netherlands",
            "backing_people": 0,
            "to_win_if_backed": 1.0,
            "display_name": "brazil",
            "backing_money": 0.0,
            "bets": []
        }
    ]
}
```

Places a prediction for the specified target for the specified amount in the specified pool.

If user does not have sufficient available funds, an appropriate dedicated error message is returned.

### Operation code

Name | Code
--------- | -------
get_betting_pool_bets_distribution | 393

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | null | Pool id for which distribution is to be returned.

### Response code

Name | Code | Result
--------- | ------- | -----------
new_betting_pool_bets_distribution | 360 | You have received the pool distribution.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | :YOUR_POOL_ID | The pool id for which the distribution was requested.
target_level | 0 | The computed target level for sent targets. E.g. - 1 for "argentina, brazil", 2 for "argentina:9, brazil:7"
distribution | [] | A list of current predictions, each an object with predictions for a given target (containing a list of all bets, with details, current target multiplier, display name and total amount backing the target).

## Block betting in a pool

<aside class="notice">
Only users listed as admins for the game are authorized for this call.
</aside>

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 397,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117",
    "block_at": null
}
```


> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 368,
    "blocked_at": 1421942161542,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117"
}
```

Blocks placing predictions in the specified pool until unblocked (or pool resolved).

### Operation code

Name | Code
--------- | -------
block_betting_in_betting_pool | 397

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | null | Pool id which is supposed to be blocked.
block_at | epoch_millis | Block pool at a later time - epoch_millis (> now()).

### Response code

Name | Code | Result
--------- | ------- | -----------
betting_in_pool_blocked | 368 | You have blocked a given pool. This is broadcast to all game subscribers.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | :YOUR_POOL_ID | The pool id for pool which has been blocked.
blocked_at | epoch_millis | The timestamp indicating when the pool was blocked.


## Block betting for action

<aside class="notice">
Only users listed as admins for the game are authorized for this call.
</aside>

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 3975,
    "game_id": "a183b2da-b561-4bc9-99b5-078e570e0117",
    "action_id": "goal",
    "block_at": null
}
```


> Returns the following JSON structures:

```json
{
    "response_header": {},
    "res": 370,
    "game_id": "a183b2da-b561-4bc9-99b5-078e570e0117",
    "action_id": "goal",
    "blocked_at": 1421942161542
}
```

```json
{
    "response_header": {},
    "res": 368,
    "blocked_at": 1421942161542,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117"
}
```

Blocks placing predictions in all pools for specified action in a specified game until unblocked (or pools resolved).

### Operation code

Name | Code
--------- | -------
block_betting_for_game_and_action | 3975

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id in which pools are to be blocked.
action_id | null | Action for which pools are to be blocked.
block_at | epoch_millis | Block pools at a later time - epoch_millis (> now()).

### Response codes

Name | Code | Result
--------- | ------- | -----------
betting_for_game_and_action_blocked | 370 | You have blocked given pools. This is broadcast to all game subscribers.
betting_in_pool_blocked | 368 | You have blocked a given pool. This is broadcast to all game subscribers for all pools in game for given action.

### Response Parameters

#### Blocking confirmation

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_POOL_ID | The game id for game in which pools have been blocked.
action_id | null | Action for which pools have been blocked.
blocked_at | epoch_millis | The timestamp indicating when the pools were blocked.

#### Blocked pool

Described in <a href="#block-betting-in-a-pool">this section</a>.

## Allow betting in a pool

<aside class="notice">
Only users listed as admins for the game are authorized for this call.
</aside>

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 398,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117",
    "allow_at": null
}
```

> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 369,
    "allowed_at": 1421942161542,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117"
}
```

Allow for placing predictions in the specified pool. Needs to called only after explicitly blocking the pool.

### Operation code

Name | Code
--------- | -------
allow_betting_in_betting_pool | 398

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | null | Pool id for which placing predictions is to be allowed.
allow_at | epoch_millis | Allow betting pool at a later time - epoch_millis (> now()).

### Response code

Name | Code | Result
--------- | ------- | -----------
betting_in_pool_allowed | 369 | You have allowed for placing predictions in a given pool. This is broadcast to all game subscribers.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | :YOUR_POOL_ID | The pool id for pool in which placing predictions has been allowed.
allowed_at | epoch_millis | The timestamp indicating when placing predictions in the given pool was allowed.

## Allow betting for action

<aside class="notice">
Only users listed as admins for the game are authorized for this call.
</aside>

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 3985,
    "game_id": "a183b2da-b561-4bc9-99b5-078e570e0117",
    "action_id": "goal",
    "allow_at": null
}
```


> Returns the following JSON structures:

```json
{
    "response_header": {},
    "res": 371,
    "game_id": "a183b2da-b561-4bc9-99b5-078e570e0117",
    "action_id": "goal",
    "allowed_at": 1421942161542
}
```

```json
{
    "response_header": {},
    "res": 369,
    "allowed_at": 1421942161542,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117"
}
```

Allows for placing predictions in all pools for specified action in a specified game. Only necessary if action blocked previously.

### Operation code

Name | Code
--------- | -------
allow_betting_for_game_and_action | 3985

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id in which predictions in pools are to be allowed.
action_id | null | Action for which predictions in pools are to be allowed.
allow_at | epoch_millis | Allow for predcitions in pools at a later time - epoch_millis (> now()).

### Response codes

Name | Code | Result
--------- | ------- | -----------
betting_for_game_and_action_allowed | 371 | You have allowed for placing predictions in given pools. This is broadcast to all game subscribers.
betting_in_pool_allowed | 369 | You have allowed for placing predictions in a given pool. This is broadcast to all game subscribers for all pools in game for given action.

### Response Parameters

#### Allowing confirmation

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_POOL_ID | The game id for game in which placing predictions in pools has been allowed.
action_id | null | Action for which placing predictions in pools has been allowed.
allowed_at | epoch_millis | The timestamp indicating when the placing predictions in pools was allowed.

#### Unblocked pool

Described in <a href="#allow-betting-in-a-pool">this section</a>.

## Suggest hot pool

<aside class="notice">
Only users listed as admins for the game are authorized for this call.
</aside>

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 396,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117",
    "likely_to_happen_at": null
}
```

> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 367,
    "hot_since": 1421942161542,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117"
}
```

Marks pool as hot at _likely_to_happen_at_ time. Allows clients to move pool up the list or put a special label on it.

### Operation code

Name | Code
--------- | -------
suggest_hot_betting_pool | 396

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | null | Pool id for which placing predictions is to be allowed.
likely_to_happen_at | epoch_millis | At what time (epoch_millis) broadcas new hot pool message to clients.

### Response code

Name | Code | Result
--------- | ------- | -----------
hot_betting_pool_suggested | 367 | You have marked pool as *hot*. All clients subscribed to game notified.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | :YOUR_POOL_ID | The pool id for pool which has been marked as hot.
hot_since | likely_to_happen_at | Since when the pool is marked as hot.

## Suggest hot pool for action and target

<aside class="notice">
Only users listed as admins for the game are authorized for this call.
</aside>

> Expects the following JSON structure:

```json
{
    "header": {},
    "op": 3961,
    "game_id": "a183b2da-b561-4bc9-99b5-078e570e0117",
    "action_id": "goal",
    "target_id": "brazil",
    "likely_to_happen_at": null
}
```

> Returns the following JSON structures:

```json
{
    "response_header": {},
    "res": 367,
    "hot_since": 1421942161542,
    "pool_id": "a183b2da-b561-4bc9-99b5-078e570e0117"
}
```

Marks *every* pool relevant for game and action and target as hot at _likely_to_happen_at_ time. Allows clients to move pool up the list or put a special label on it.

### Operation code

Name | Code
--------- | -------
suggest_hot_betting_pool_for_action_and_target | 3961

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | Game id in which the pools are to be marked hot.
action_id | null | Action for which the pools are to be marked hot.
target_id | null | Target for which the pools are to be marked hot.
likely_to_happen_at | epoch_millis | At what time (epoch_millis) broadcas new hot pool message(s) to clients.

### Response codes

Name | Code | Result
--------- | ------- | -----------
betting_for_game_and_action_allowed | 371 | You have allowed for placing predictions in given pools. This is broadcast to all game subscribers.
betting_in_pool_allowed | 369 | You have allowed for placing predictions in a given pool. This is broadcast to all game subscribers for all pools in game for given action.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | :YOUR_POOL_ID | The pool id for pool which has been marked as hot.
hot_since | likely_to_happen_at | Since when the pool is marked as hot.