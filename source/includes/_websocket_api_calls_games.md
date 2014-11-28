# Websocket API: Games

<aside class="notice">
This section assumes you have an active game source opened. If not, read on how to open it <a href="#open-a-game-source">here</a>.
</aside>




## Create and open a game

<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 370,
  "source_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "game_name": "Argentina - Brazil",
  "actions": ["Goal", "Offside", ".."],
  "targets": ["Argentina", "Brazil", "Argentina:1", "Argentina:2", "Brazil:7", ".."],
  "type": "football",
  "currency": "EUR",
  "create_at": 1417104170,
  "open_at": 1417104170,
  "live_start_at": 1417105170,
  "live_finish_at": 1417106170
}
```


> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 340,
  "source_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "actions": {
  	"goal": {
  		"name": "Goal"
  	},
  	"offside": {
  		"name": "Offside"
  	}
  },
  "game_name": "Argentina - Brazil",
  "type": "football",
  "open_at": 1417104170,
  "live_start_at": 1417105170,
  "live_finish_at": 1417106170,
  "close_at": null,
  "targets": {
		"brazil": {
			"symbol": "bra",
			"display_name": "Brazil",
			"targets": {
				"7": {
					"symbol": "7",
					"display_name": "7",
					"targets": {}
				}
			}
		},
		"none": {
			"symbol": "non",
			"display_name": "None",
			"targets": {}
		},
		"argentina": {
			"symbol": "arg",
			"display_name": "argentina",
			"targets": {
				"1": {
					"symbol": "1",
					"display_name": "1",
					"targets": {}
				},
				"2": {
					"symbol": "2",
					"display_name": "2",
					"targets": {}
				}
			}
		}
	}
}
```

This registers the game in the system and opens it automatically. Betting pools can now be opened for this game.

### Operation code

Name | Code
--------- | -------
create_and_open_game | 370

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | null | Your source id.
game_name | null | The name you want to assign to your game, e.g. "Argentina - Brazil".
type | null | The game type of your game, e.g. "football", "basketball", etc.
actions | [] | List of actions that can happen in the game, e.g. "yellow-card", "goal", "offside", etc. Sent as a comma-separated string or an array.
targets | [] | List of targets taking part in the game. Sent as a comma-separated string or an array. Targets can be infinitely nested. Nesting is indicated by including a ':' in the target. E.g. "Argentina:1" means "Argentina, player 1". Server automatically builds a nested structure (see the example to the right). You can send targets of different nesting levels. An event registered for Argentina:1 will resolve pools for Argentina:1 and Argentina (e.g. Messi scores - pools for Messi's goals and Argentina goals get resovled).
open_at | now() | Game opening time (> now()) in epoch millis. No pool opening allowed before that time.
close_at | now() | Game closing time (> open_at) in epoch millis. This will close the game at a given time. **Warning**: this will automatically resolve all the parimutuel pools running for the game. If not included (**recommended**), game close request has to be sent automatically.
live_start_at | now() | Game real life starting time (> now()) in epoch millis. This can be different than open_at (e.g. you might want to allow betting before the game starts in real life). Serves **only** as an informative value.
live_finish_at | now() | Game real life finishing time (> now()) in epoch millis. This can be different than close_at (e.g. you might want to close betting before the game finishes in real life). Serves **only** as an informative value.
currency | null | The currency for which you want to allow betting for in the game.

### Response codes

Name | Code | Result
--------- | ------- | -----------
game_created_and_opened | 340 | Your game has been created and opened. You can now open betting pools for the game.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | :YOUR_SOURCE_ID | Your source id.
game_id | :YOUR_GAME_ID | The server-generated game id. Necessary for making all game-related calls.
game_name | :YOUR_GAME_NAME | The game name you provided.
type | :YOUR_GAME_TYPE | The game type you provided.
actions | {} | A dictionary containing the actions you listed. **key** - action id, **value** - action details (name).
targets | {} | A nested, recursive dictionary containing the targets you listed. **key** - target id, **value** - target details (symbol, display name and _sub_ targets). Please look at the example on the right.
open_at | request receival time | The open game time in epoch millis.
live_start_at | request receival time | The real life game start time in epoch millis (only informative).
live_finish_at | null | The real life game finish time in epoch millis (only informative).
close_at | null | The close game time in epoch millis (if null, game close request needs to be sent explicitly).




## Create a game

<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 3701,
  "source_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "game_name": "Argentina - Brazil",
  "type": "football",
  "currency": "EUR",
  "create_at": 1417104170,
  "open_at": 1417104170,
  "live_start_at": 1417105170,
  "live_finish_at": 1417106170
}
```


> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 3401,
  "source_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "game_name": "Argentina - Brazil",
  "type": "football",
  "open_at": 1417104170,
  "live_start_at": 1417105170,
  "live_finish_at": 1417106170,
  "close_at": null
}
```

This registers the game in the system as an upcoming game. An <a href="#open-a-game">open game</a> request needs to be sent afterwards to open a game for betting.

To allow betting, targets and actions need to be added to the game either with the <a href="#open-a-game">open game</a> request, or with an <a href="#update-game-details">update game details</a> request.

### Operation code

Name | Code
--------- | -------
create_game | 3701

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | null | Your source id.
game_name | null | The name you want to assign to your game, e.g. "Argentina - Brazil".
type | null | The game type of your game, e.g. "football", "basketball", etc.
open_at | now() | Game opening time (> now()) in epoch millis. No pool opening allowed before that time.
close_at | now() | Game closing time (> open_at) in epoch millis. This will close the game at a given time. **Warning**: this will automatically resolve all the parimutuel pools running for the game. If not included (**recommended**), game close request has to be sent automatically.
live_start_at | now() | Game real life starting time (> now()) in epoch millis. This can be different than open_at (e.g. you might want to allow betting before the game starts in real life). Serves **only** as an informative value.
live_finish_at | now() | Game real life finishing time (> now()) in epoch millis. This can be different than close_at (e.g. you might want to close betting before the game finishes in real life). Serves **only** as an informative value.
currency | null | The currency for which you want to allow betting for in the game.

### Response codes

Name | Code | Result
--------- | ------- | -----------
game_created | 3401 | Your game has been created and is listed as an upcoming game. You still need to open it for betting and add actions and targets.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | :YOUR_SOURCE_ID | Your source id.
game_id | :YOUR_GAME_ID | The server-generated game id. Necessary for making all game-related calls.
game_name | :YOUR_GAME_NAME | The game name you provided.
type | :YOUR_GAME_TYPE | The game type you provided.
open_at | request receival time | The open game time in epoch millis.
live_start_at | request receival time | The real life game start time in epoch millis (only informative).
live_finish_at | null | The real life game finish time in epoch millis (only informative).
close_at | null | The close game time in epoch millis (if null, game close request needs to be sent explicitly).




## Open a game

<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 3702,
  "actions": ["Goal", "Offside", ".."],
  "targets": ["Argentina", "Brazil", "Argentina:1", "Argentina:2", "Brazil:7", ".."]
}
```


> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 3402,
  "source_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "game_name": "Argentina - Brazil",
  "type": "football",
  "open_at": 1417104170,
  "live_start_at": 1417105170,
  "live_finish_at": 1417106170,
  "close_at": null
}
```

This opens a previously created upcoming game (with a <a href="#create-a-game">create game</a> request) for betting.

### Operation code

Name | Code
--------- | -------
open_game | 3702

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | The game id for the game you want opened (sent in response to create_game request).
actions | [] | List of actions that can happen in the game, e.g. "yellow-card", "goal", "offside", etc. Sent as a comma-separated string or an array.
targets | [] | List of targets taking part in the game. Sent as a comma-separated string or an array. Targets can be infinitely nested. Nesting is indicated by including a ':' in the target. E.g. "Argentina:1" means "Argentina, player 1". Server automatically builds a nested structure (see the example to the right). You can send targets of different nesting levels. An event registered for Argentina:1 will resolve pools for Argentina:1 and Argentina (e.g. Messi scores - pools for Messi's goals and Argentina goals get resovled).

### Response codes

Name | Code | Result
--------- | ------- | -----------
game_created_and_opened | 340 | Your game has been opened. You can now open betting pools for the game. **Warning**: This is the same response as for the <a href="#create-and-open-a-game">create and open game</a> request. The state after processing both calls is semantically equivalent.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | :YOUR_SOURCE_ID | Your source id.
game_id | :YOUR_GAME_ID | The server-generated game id. Necessary for making all game-related calls.
game_name | :YOUR_GAME_NAME | The game name you provided.
type | :YOUR_GAME_TYPE | The game type you provided.
actions | {} | A dictionary containing the actions you listed. **key** - action id, **value** - action details (name).
targets | {} | A nested, recursive dictionary containing the targets you listed. **key** - target id, **value** - target details (symbol, display name and _sub_ targets). Please look at the example on the right.
open_at | request receival time | The open game time in epoch millis.
live_start_at | request receival time | The real life game start time in epoch millis (only informative).
live_finish_at | null | The real life game finish time in epoch millis (only informative).
close_at | null | The close game time in epoch millis (if null, game close request needs to be sent explicitly).




## Become an admin for a game
<aside class="notice">
Only the game source admin is allowed to send request as of now. Authorizing additional users to do that will come soon.
</aside>

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 616,
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9"
}
```


> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 640,
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9"
}
```

> or

```json
{
  "response_header": {},
  "res": 645,
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "reason": "This game already has enough admins."
}
```

Game creation and opening requires game-source level privileges. All other game related calls require explicitly asking for game-level privileges. These can be obtained through this call.

### Operation code

Name | Code
--------- | -------
become_an_admin_for_game | 616

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | The game id for which you want to become an admin.

### Response codes

Name | Code | Result
--------- | ------- | -----------
become_an_admin_for_game_success | 640 | You have become an admin for the game. You can create pools and register events.
become_an_admin_for_game_fail | 645 | You have **not** become an admin for the game. This can happen in 2 cases: you have no privileges or the game already has enough admins.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id you provided.
reason | :YOUR_GAME_ID | The reason why you couldn't become an admin. Sent **only** with become_an_admin_for_game_fail response code.




## Get game details

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 373,
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9"
}
```


> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 342,
    "status": "game_in_progress_betting_active",
    "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
    "name": "Argentina - Brazil",
    "past_events": {
        "106a9151-2997-4c96-bc29-c020da54fa67": {
            "happened_at": 1417180267000,
            "target_id": "argentina:1",
            "action_id": "goal"
        },
        "13bc5390-d86c-45b8-8327-7b313b80103d": {
            "happened_at": 1417180282000,
            "target_id": "brazil:7",
            "action_id": "offside"
        }
    },
    "actions": {
	  	"goal": {
	  		"name": "Goal"
	  	},
	  	"offside": {
	  		"name": "Offside"
	  	}
	  },
    "currency": "DON",
    "type": "4d24a1cb-5465-4ab3-befd-99fb39b7b796",
    "targets": {
		"brazil": {
			"symbol": "bra",
			"display_name": "Brazil",
			"targets": {
				"7": {
					"symbol": "7",
					"display_name": "7",
					"targets": {}
				}
			}
		},
		"none": {
			"symbol": "non",
			"display_name": "None",
			"targets": {}
		},
		"argentina": {
			"symbol": "arg",
			"display_name": "argentina",
			"targets": {
				"1": {
					"symbol": "1",
					"display_name": "1",
					"targets": {}
				},
				"2": {
					"symbol": "2",
					"display_name": "2",
					"targets": {}
				}
			}
		}
	},
    "open_at": 1417104170,
    "live_start_at": 1417105170,
    "live_finish_at": null,
    "close_at": null
```


Obtain full details for a game with given id.

### Operation code

Name | Code
--------- | -------
get_game_details | 372

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | The game id of which you want to obtain details.

### Response codes

Name | Code | Result
--------- | ------- | -----------
game_details | 342 | You have received the requested game details.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id you provided.
game_name | :YOUR_GAME_NAME | The game name you provided.
status | null | Current game status. Possible values: "betting_not_yet_active", "pre_game_betting_active", "game_in_progress_betting_active", "game_in_progress_betting_closed", "game_closed".
type | :YOUR_GAME_TYPE | The uuid of the game type you provided.
actions | {} | A dictionary containing the actions for the game. **key** - action id, **value** - action details (name).
targets | {} | A nested, recursive dictionary containing the targets for the game. **key** - target id, **value** - target details (symbol, display name and _sub_ targets). Please look at the example on the right.
past_events | {} | A dictionary containing all the events that happened in the game so far. **key** - event id, **value** - event details (happened_at, target_id, action_id).
open_at | request receival time | The open game time in epoch millis.
live_start_at | request receival time | The real life game start time in epoch millis (only informative).
live_finish_at | null | The real life game finish time in epoch millis (only informative).
close_at | null | The close game time in epoch millis (if null, game close request needs to be sent explicitly).
currency | null | The currency for which you want to allow betting for in the game.




## Update game details
<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 373,
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "actions": ["Goal", "Offside"],
  "targets": ["Argentina", "Brazil", "Argentina:1", "Argentina:2", "Brazil:7"]
}
```


> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 343,
    "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9"
}
```


Update game details - add new targets or actions.

This publishes the updated game details to users subscribed to the game.


### Operation code

Name | Code
--------- | -------
update_game_details | 373

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | null | The game id of which you want to update details.
actions | [] | List of actions that you want to add to the game, e.g. "yellow-card", "goal", "offside", etc. Sent as a comma-separated string or an array.
targets | [] | List of targets that you want to add to the game. Sent as a comma-separated string or an array. Targets can be infinitely nested. Nesting is indicated by including a ':' in the target. E.g. "Argentina:1" means "Argentina, player 1". Server automatically builds a nested structure (see the example to the right). You can send targets of different nesting levels. An event registered for Argentina:1 will resolve pools for Argentina:1 and Argentina (e.g. Messi scores - pools for Messi's goals and Argentina goals get resovled).


### Response codes

Name | Code | Result
--------- | ------- | -----------
game_details_updated | 343 | You have updated the game details (confirmation).
game_details | 342 | New game details are published to everyone subscribed to the game (including you). Check <a href="#get-game-details"> get game details</a> section for a list of response parameter for this message.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | :YOUR_GAME_ID | The game id you provided.


## Join game
## Leave game
## Close a game
<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>
## List games for source