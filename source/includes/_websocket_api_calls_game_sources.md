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
  "source_name": ":YOUR_SOURCE_NAME"
}
```

> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 1,
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
source_name | "" | The name you want to assign to your source (your domain suggested).

### Response codes

Name | Code | Result
--------- | ------- | -----------
game_source_opened | 335 | Your source has been registered. You can now open and host games.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
source_id | null | The new server-assigned source id with which you, as a game provider, will be identified.



## Close a game source