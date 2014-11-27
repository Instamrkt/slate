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
## Cpen a game
<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>
## Become an admin for a game
<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>
## Get game details
## Update game details
<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>
## Join game
## Leave game
## Close a game
<aside class="notice">
Only users listed as admins for the source are authorized for this call.
</aside>
## List games for source