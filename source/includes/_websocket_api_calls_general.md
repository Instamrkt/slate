# Websocket API: General

<aside class="notice">
All websocket calls are expecting a json with the parameters listed for each call.
</aside>

<aside class="notice">
Each request needs to contain a valid header. Each response contains a response header. Both are described <a href="#headers">below</a>.
</aside>

## Connecting

### Connection URL

The connection url:
`wss://instamrkt.com:9999?id=:YOUR_USER_ID&n=:YOUR_GENERATED_NONCE&h=:YOUR_HEXDIGEST`

### Obtaining id and the session key

Both the user_session_key and id are returned in cookies for HTTP users and also returned in the JSON from login or signup REST requests.

The session keys are valid for 24 hours. It is refreshed on every <a href="#login">login</a> request.

### Nonce and hexdigest

```python
import hashlib

sha       = hashlib.sha256()

sha.update(user_id)
sha.update(nonce)
sha.update(key)

hexdigest = sha.hexdigest()
```

Nonce is the user provided random value, e.g. current timestamp in epoch millis.

Digest is the concatenation of strings fed to the SHA256 algorithm in the following sequence:

1. User id
2. Nonce
3. Session key

A python example of how to generate one can be found on the right.

## Headers

### Header
```json
{
  "session_key": "//not used currently",
  "t": 1422006034742,
  "api_version": 1.0
}
```

Each client request needs to contain a header depicted on the right.

### Response header
```json
{
  "t": 1422007246302
}
```

Each response contains a header depicted on the right.

## Ping

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 1
}
```


> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 1
}
```

You can use this to make sure the websocket connection is still open and ready for sending / receiving messages.

### Operation code

Name | Code
--------- | -------
ping | 1

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
- | - | -

### Response code

Name | Code | Result
--------- | ------- | -----------
pong | 1 | You connection is valid.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
- | - | -


## Opcodes


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 2
}
```


> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 2,
  "opcodes": {
    "logon": 100,
    "opcodes": 2,
    "...":
  },
  "response_codes": {
    "logon_success": 100,
    "response_codes": 2,
    "...":
  }
}
```

You can use this to dynamically load the list of supported operations with operation codes.

### Operation code

Name | Code
--------- | -------
get_opcodes | 2

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
- | - | -

### Response code

Name | Code | Result
--------- | ------- | -----------
opcodes | 2 | You received a valid list of opcodes and response codes.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
opcodes | {} | A dictionary containing all supported opcodes: **key** - opcode name, **value** - opcode.
response_codes | {} | A dictionary containing all supported response codes: **key** - response code name, **value** - response code.


## Logon

Some functionality requires you to send a logon request apart from connecting. Documenation for this calls explicitly says it is presumed you are logged in.

This is performed automatically for you if you are reconnecting (returning to the site after less than 15 minutes). Otherwise, this cll needs to be made explicitly.

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 100
}
```


> Returns the following JSON structure:

```json
{
  "response_header": {},
  "res": 100,
  "session_key": "4"
}
```

This authenticates you with the api server and allows to send all requests.

### Operation code

Name | Code
--------- | -------
logon | 100

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
- | - | -

### Response codes

Name | Code | Result
--------- | ------- | -----------
logon_success | 100 | You are logged in.
logon_fail | 110 | You have **NOT** been logged in.
logon_fail_already_logged_in | 112 | You are logged in.