# Websocket API: Chat

## Send chat message

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 900,
  "targets": "69c623bf-046a-43b6-8de5-a0b3ae9b142e",
  "text": "This is a message."
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

This sends a chat message to user's subscribed to specified games. If target is left empty, the message is broadcast to all games user is subscribed to. Therefore, specifying a single (the one user is actively following) game is recommended.

### Operation code

Name | Code
--------- | -------
text_message | 900

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
text | null | The text you want to send.
targets | null | An array with string of game ids, or a single string containing a game id to which the message is to be broadcast. Single target recommended.

### Response codes

Name | Code | Result
--------- | ------- | -----------
new_text_message | 901 | New text message. Broadcast to all users subscribed to game which id was passed as a target.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
text | null | Your message text.
sender | :YOUR_USERNAME | Sender's username. Recommended display: "sender: text".