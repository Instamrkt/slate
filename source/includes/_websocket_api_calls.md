# Websocket API Calls: General

<aside class="notice">
All websocket calls are expecting a json with the parameters listed for each call.
</aside>

## Logon

Some functionality requires you to send a logon request apart from connecting. Documenation for this calls explicitly says it is presumed you are logged in.

This is performed automatically for you if you are reconnecting (returning to the site after less than 15 minutes). Otherwise, this cll needs to be made explicitly.

<!-- ```python
```

```shell
``` -->

> Expects the following JSON structure:

```json
{
  "header": {
    "t": "CURRENT_EPOCH_MILLIS",
    "api_key": "YOUR_API_KEY",
    "api_version": 1.0
  },
  "op": 100
}
```


> Returns the following JSON structure:

```json
{
  "response_header": {
      "t": 1417011631016
    },
  "res": 100,
  "session_key": "4"
}
```

This authenticates you with the api server and allows to send all requests.

### Operation code

login = 100

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
- | - | -

### Responses

Name | Code | Result
--------- | ------- | -----------
logon_success | 100 | You are logged in.
logon_fail | 110 | You have **NOT** been logged in.
logon_fail_already_logged_in | 112 | You are logged in.