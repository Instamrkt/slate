# Websocket API: Errors

## Generic error

> Sends the following JSON structure:

```json
{
    "response_header": {},
    "res": 666,
    "text": "You are not authorized to open a game source."
}
```

This message is sent if:

* servers fails to process your message correclty,
* some of your input data is incorrect,
* some of the input data is missing,
* you are not authorized to perform the operation you are intending to.

### Response code

Name | Code | Result
--------- | ------- | -----------
error | 666 | Your requested operation has resulted in an error.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
text| null | Error explanation.
