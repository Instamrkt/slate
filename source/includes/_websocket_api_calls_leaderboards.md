# Websocket API: Leaderboards

## Global Full Leaderboard


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 450
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 450,
  "game_type": "INSTAMRKT",
  "currency": "DON",
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

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 4505,
  "top_count": 3,
  "bottom_count": 3,
  "surrounding_count": 1
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 450,
  "game_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
  "currency": "DON",
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
        },
        {
            "username": "boodles2",
            "points": 50.47,
            "rank": 43
        },
        {
            "username": "boodles3",
            "points": 50.12,
            "rank": 44
        },
        {
            "username": "boodles4",
            "points": 49.68,
            "rank": 45
        },
        {
            "username": "last",
            "points": 17.21,
            "rank": 100
        }
  ],
  "break_after_rank": [3, 45]
}
```

This requests the full personalized leaderboard for a given game type (defaults to `INSTAMRKT`) on the platform. The response includes top `top_count` users on the leaderboard, the requesting user plus surroundings (specified by `surroundings_count`) and bottom `bottom_count` users.

### Operation code

Name | Code
--------- | -------
get_user_tailored_leaderboard | 4505

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_type | INSTAMRKT | Game type for which to return global leaderboard.
currency | DON | Currency for which the leadeboard should be returned.
top_count | 3 | How many users to include on the top of the leaderboard.
surrounding_count | 1 | How many users to include right before and after the requesting user.
bottom_count | 3 | How many users to include on the bottom of the leaderboard.

### Response code

Name | Code | Result
--------- | ------- | -----------
leaderboard_tailored | 4505 | You have requested the global personalized leaderboard.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_type | INSTAMRKT | Game type for which to return global leaderboard.
currency | DON | Currency for which the leadeboard should be returned.
leaderboard | [] | A list sorted by `rank`. Each object contains `username`, `points` and `rank`.
break_after_rank | [] | If the leaderboard is not continuous, the ranks after which breaks happen are included in this list (check sample code on the right).


## Friends Leaderboard


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 452
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 452,
  "game_type": "INSTAMRKT",
  "currency": "DON",
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

This requests the friends global leaderboard for a given game type (defaults to `INSTAMRKT`) on the platform. Is a subset of a global leaderboard, includes only friends of a requestor.

### Operation code

Name | Code
--------- | -------
get_friends_leaderboard | 452

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


## Game Source Full Leaderboard

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 4516,
  "source_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1"
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 4516,
  "source_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
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

This requests the full global leaderboard for a given game source.

### Operation code

Name | Code
--------- | -------
get_leaderboard_for_game_source | 4516

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | - | Source id for which to return global leaderboard.
start | 0 | The rank at which to start the leaderboard (e.g. 3 will not show the top three).
end | 1000000 | The rank at which to finish the leaderboard (e.g. 10 ranks from 11 on).

### Response code

Name | Code | Result
--------- | ------- | -----------
leaderboard_for_game_source | 4516 | You have requested the global leaderboard for a given game source.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | - | Game id for which to return global leaderboard.
leaderboard | [] | A list sorted by `rank`. Each object contains `username`, `points` and `rank`.

## Game Source Personalized Leaderboard

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 4517,
  "source_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
  "top_count": 3,
  "bottom_count": 3,
  "surrounding_count": 1
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 4517,
  "currency": "DON",
  "source_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
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
        },
        {
            "username": "boodles2",
            "points": 50.47,
            "rank": 43
        },
        {
            "username": "boodles3",
            "points": 50.12,
            "rank": 44
        },
        {
            "username": "boodles4",
            "points": 49.68,
            "rank": 45
        },
        {
            "username": "last",
            "points": 17.21,
            "rank": 100
        }
  ],
  "break_after_rank": [3, 45]
}
```

This requests the full personalized leaderboard for a given game source. The response includes top `top_count` users on the leaderboard, the requesting user plus surroundings (specified by `surroundings_count`) and bottom `bottom_count` users.

### Operation code

Name | Code
--------- | -------
get_user_tailored_leaderboard_for_game_source | 4517

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id| - | Which game source to request the leaderboard for.
top_count | 3 | How many users to include on the top of the leaderboard.
surrounding_count | 1 | How many users to include right before and after the requesting user.
bottom_count | 3 | How many users to include on the bottom of the leaderboard.

### Response code

Name | Code | Result
--------- | ------- | -----------
leaderboard_for_game_source_tailored | 4517 | You have requested the personalized leaderboard for a game source.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | - | Source for which the leaderboard was requested.
leaderboard | [] | A list sorted by `rank`. Each object contains `username`, `points` and `rank`.
break_after_rank | [] | If the leaderboard is not continuous, the ranks after which breaks happen are included in this list (check sample code on the right).

## Game Source Friends Leaderboard

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 454,
  "source_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1"
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 454,
  "source_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
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

This requests the friends leaderboard for a given game source. The leaderboard is a subset of game global leaderboard - includes only friends of a requesting user.

### Operation code

Name | Code
--------- | -------
get_friends_leaderboard_for_game_source | 454

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | - | Source id for which to return global leaderboard.
start | 0 | The rank at which to start the leaderboard (e.g. 3 will not show the top three).
end | 1000000 | The rank at which to finish the leaderboard (e.g. 10 ranks from 11 on).

### Response code

Name | Code | Result
--------- | ------- | -----------
leaderboard_friends_for_game_source | 454 | You have requested the global leaderboard for a given game source.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | - | Source id for which to return global leaderboard.
leaderboard | [] | A list sorted by `rank`. Each object contains `username`, `points` and `rank`.


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
  "game_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
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

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 4505,
  "game_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
  "top_count": 3,
  "bottom_count": 3,
  "surrounding_count": 1
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 450,
  "game_id":"f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
  "currency": "DON",
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
        },
        {
            "username": "boodles2",
            "points": 50.47,
            "rank": 43
        },
        {
            "username": "boodles3",
            "points": 50.12,
            "rank": 44
        },
        {
            "username": "boodles4",
            "points": 49.68,
            "rank": 45
        },
        {
            "username": "last",
            "points": 17.21,
            "rank": 100
        }
  ],
  "break_after_rank": [3, 45]
}
```

This requests the full personalized leaderboard for a given game. The response includes top `top_count` users on the leaderboard, the requesting user plus surroundings (specified by `surroundings_count`) and bottom `bottom_count` users.

### Operation code

Name | Code
--------- | -------
get_user_tailored_leaderboard | 4505

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_type | INSTAMRKT | Game type for which to return global leaderboard.
currency | DON | Currency for which the leadeboard should be returned.
game_id| - | Which game to request the leaderboard for.
top_count | 3 | How many users to include on the top of the leaderboard.
surrounding_count | 1 | How many users to include right before and after the requesting user.
bottom_count | 3 | How many users to include on the bottom of the leaderboard.

### Response code

Name | Code | Result
--------- | ------- | -----------
leaderboard_for_game_tailored | 4515 | You have requested the personalized leaderboard for a game.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | - | Game for which the leaderboard was requested.
leaderboard | [] | A list sorted by `rank`. Each object contains `username`, `points` and `rank`.
break_after_rank | [] | If the leaderboard is not continuous, the ranks after which breaks happen are included in this list (check sample code on the right).

## Game Friends Leaderboard

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 453,
  "game_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1"
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 453,
  "game_id": "f6951935-d201-43b9-b3ee-8bb7f6f3ecb1",
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

This requests the friends leaderboard for a given game. The leaderboard is a subset of game global leaderboard - includes only friends of a requesting user.

### Operation code

Name | Code
--------- | -------
get_friends_leaderboard_for_game | 453

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | - | Game id for which to return global leaderboard.
start | 0 | The rank at which to start the leaderboard (e.g. 3 will not show the top three).
end | 1000000 | The rank at which to finish the leaderboard (e.g. 10 ranks from 11 on).

### Response code

Name | Code | Result
--------- | ------- | -----------
leaderboard_friends_for_game | 451 | You have requested the global leaderboard for a given game.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
game_id | - | Game id for which to return global leaderboard.
leaderboard | [] | A list sorted by `rank`. Each object contains `username`, `points` and `rank`.


