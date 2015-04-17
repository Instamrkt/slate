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
  "res": 901,
  "sender": "vinnie",
  "text": "This is a message.",
  "received_at": 1421928724803,
  "channel": "69c623bf-046a-43b6-8de5-a0b3ae9b142e"
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

### Response code

Name | Code | Result
--------- | ------- | -----------
new_text_message | 901 | New text message. Broadcast to all users subscribed to game which id was passed as a target.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
text | null | Your message text.
sender | :YOUR_USERNAME | Sender's username. Recommended display: "sender: text".
received_at | epoch_millis | When the message was received by the server.
channel | null | The specified channel id to which the message is broadcast. Can be e.g. equal to a game_id for a game specific chat.