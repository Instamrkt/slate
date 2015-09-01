# Websocket API: Friends

## Friends Predictions in Pool

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 460,
  "pool_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "is_resolved": false
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 460,
  "pool_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "predictions": [
        {
            "user_id": 1,
            "username": "instamrkt",
            "target": "brazil",
            "amount": 1.0,
            "placed_at": 1417106170,
            "bet_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9"
        }
  ]
}
```

This requests the list of predictions made by requestor's friends in a given pool.

### Operation code

Name | Code
--------- | -------
get_my_friends_predictions_for_pool | 460

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | - | Pool for which to return the list of friends predictions.
is_resolved | False | Indicates whether it is a resolved or an active pool.

### Response code

Name | Code | Result
--------- | ------- | -----------
your_friends_predictions_for_pool | 460 | You have requested the list of your friend predictions in a given pool.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
pool_id | - | Pool for which the list of friends predictions is returned.
predictions | [] | A list of prediction objects. Each object contains `user_id`, `username`, `target`, `amount`, `placed_at`, and `bet_id`.

## Friends Predictions in Game

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 461,
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9"
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 461,
  "game_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
  "predictions": {
  		"running": [
	        {
	            "user_id": 1,
	            "username": "instamrkt",
	            "target": "brazil",
	            "amount": 1.0,
	            "placed_at": 1417106170,
	            "bet_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
	            "pool_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9"
	        }
	    ],
	    "resolved": [
	        {
	            "user_id": 2,
	            "username": "vinnie",
	            "target": "netherlands",
	            "amount": 3.0,
	            "placed_at": 1417106170,
	            "bet_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
	            "pool_id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9"
	        }
	    ]
  }
}
```

This requests the list of predictions made by requestor's friends in all pools in a given game.

### Operation code

Name | Code
--------- | -------
get_my_friends_predictions_for_game | 461

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | - | Game for which to return the list of pools with lists of friends predictions.

### Response code

Name | Code | Result
--------- | ------- | -----------
your_friends_predictions_for_game | 461 | You have requested the list of your friend predictions in all pools in a given game.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | - | Pool for which the list of friends predictions is returned.
predictions | {} | A dictionary consisting of two lists, stored under keys `running` and `resolved`. Each value is a list of prediction objects. Each object contains `user_id`, `username`, `target`, `amount`, `placed_at`, and `bet_id`.

