# Websocket API: Game sources

<aside class="notice">
This section assumes you are connected as an instamrkt developer. If you do not have a developer's account, please <a href="https://developers.instamrkt.com">register here</a>.
</aside>

## Open a game source


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 365,
  "source_name": ":YOUR_SOURCE_NAME",
  "description": "Your source description."
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 335,
  "source_id": ":YOUR_SOURCE_ID"
}
```

This called is used to register yourself as a game provider.


### Operation code

Name | Code
--------- | -------
open_game_source | 365

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_name | null | The name you want to assign to your source (your domain suggested).
description | null | The description (up to 512 chars) you want to assign to your source. This parameter is optional.

### Response code

Name | Code | Result
--------- | ------- | -----------
game_source_opened | 335 | Your source has been registered. You can now open and host games.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | null | The new server-assigned source id with which you, as a game provider, will be identified. Necessary for making all game source-related calls.


## Close a game source

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 366,
  "source_id": ":YOUR_SOURCE_ID"
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 336,
  "source_id": ":YOUR_SOURCE_ID"
}
```

This called is used to close a game source you are hosting. This means you will not be able to host games anymore.

<aside class="warning">
This will automatically close all the games (and resolve pools accordingly) hosted by the source. Make sure you really want to send this request.
</aside>


### Operation code

Name | Code
--------- | -------
close_game_source | 366

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | null | The source id assigned by the server upon source opening.

### Response code

Name | Code | Result
--------- | ------- | -----------
game_source_closed | 336 | Your source has been closed. No more games can be hosted for this source.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | null | Your source id.


## List game sources I admin


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 611
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 621,
  "sources": [
    {
      "id": "30cee1fa-fb20-41a6-a61c-0e0335abc2a9",
      "name": "instamrkt",
      "description": "Default source."
    }
  ]
}
```

This called is used to list game sources you are admin of.


### Operation code

Name | Code
--------- | -------
list_game_sources_i_am_admin_of | 611

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
- | - | -

### Response code

Name | Code | Result
--------- | ------- | -----------
game_sources_i_am_admin_of_list | 621 | You have requested a list of game sources you are admin of.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
sources | [] | List of game sources user is admin of. Each object consists of `id`, `name` and `description`.
