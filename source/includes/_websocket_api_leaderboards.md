# Websocket API: Leaderboards

## Global Full Leaderboard


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 450,
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 450,
  "game_type": "INSTAMRKT",
  "currency", "DON",
  "leaderboard": [
        {
            "username": "instamrkt",
            "points": 999.17,
            "rank": 1
        },
        {
            "username": "vinnie",
            "points": 100.0,
            "rank": 2
        },
        {
            "username": "boodles",
            "points": 98.68,
            "rank": 3
        }
  ]
}
```

This requests the full global leaderboard for a given game type (defaults to `INSTAMRKT`) on the platform.

### Operation code

Name | Code
--------- | -------
get_leaderboard | 450

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_type | INSTAMRKT | Game type for which to return global leaderboard.
currency | DON | Currency for which the leadeboard should be returned.
start | 0 | The rank at which to start the leaderboard (e.g. 3 will not show the top three).
end | 1000000 | The rank at which to finish the leaderboard (e.g. 10 ranks from 11 on).

### Response code

Name | Code | Result
--------- | ------- | -----------
leaderboard | 450 | You have requested the global leaderboard.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_type | INSTAMRKT | Game type for which to return global leaderboard.
currency | DON | Currency for which the leadeboard should be returned.
leaderboard | [] | A list sorted by `rank`. Each object contains `username`, `points` and `rank`.

## Global Personalized Leaderboard
## Friends Leaderboard

## Game Full Leaderboard

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 451,
  "game_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1"
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 451,
  "game_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1"
  "leaderboard": [
        {
            "username": "instamrkt",
            "points": 999.17,
            "rank": 1
        },
        {
            "username": "vinnie",
            "points": 100.0,
            "rank": 2
        },
        {
            "username": "boodles",
            "points": 98.68,
            "rank": 3
        }
  ]
}
```

This requests the full global leaderboard for a given game.

### Operation code

Name | Code
--------- | -------
get_leaderboard_for_game | 451

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | - | Game id for which to return global leaderboard.
start | 0 | The rank at which to start the leaderboard (e.g. 3 will not show the top three).
end | 1000000 | The rank at which to finish the leaderboard (e.g. 10 ranks from 11 on).

### Response code

Name | Code | Result
--------- | ------- | -----------
leaderboard_for_game | 451 | You have requested the global leaderboard for a given game.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | - | Game id for which to return global leaderboard.
leaderboard | [] | A list sorted by `rank`. Each object contains `username`, `points` and `rank`.

## Game Personalized Leaderboard
## Game Friends Leaderboard