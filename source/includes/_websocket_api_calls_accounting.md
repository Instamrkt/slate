# Websocket API: Accounting

## Current balance

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 800,
  "currency": "DON"
}
```

> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 800,
    "currency": "DON",
    "current_balance": 100.0
}
```


Retrieves your **current balance** (current balance = available balance + exposure).

**Available balance** is the amount that can be used for making predictions.

**Exposure** is the amount placed in currently running, unresolved bet pools.


### Operation code

Name | Code
--------- | -------
get_my_current_balance | 800

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
currency | "DON" | The currency in which balance is to be retrieved. Defaults to DONuts, can be skipped.

### Response code

Name | Code | Result
--------- | ------- | -----------
your_current_balance | 800 | You have retrieved your current balance.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
currency | "DON" | The currency in which balance was requested.
current_balance | 0.0 | Current balance of the requestor.

## Available balance


> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 810,
  "currency": "DON"
}
```

> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 810,
    "currency": "DON",
    "available_balance": 100.0
}
```


Retrieves your **available balance** (available balance = current balance - exposure).

**Available balance** is the amount that can be used for making predictions.

**Exposure** is the amount placed in currently running, unresolved bet pools.


### Operation code

Name | Code
--------- | -------
get_my_available_balance | 810

### Call Parameters

Parameter | Default | Description
--------- | ------- | -----------
currency | "DON" | The currency in which balance is to be retrieved. Defaults to DONuts, can be skipped.

### Response code

Name | Code | Result
--------- | ------- | -----------
your_available_balance | 810 | You have retrieved your available balance.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
currency | "DON" | The currency in which balance was requested.
available_balance | 0.0 | Available balance of the requestor.

## Exposure

> Expects the following JSON structure:

```json
{
  "header": {},
  "op": 840,
  "currency": "DON"
}
```

> Returns the following JSON structure:

```json
{
    "response_header": {},
    "res": 840,
    "currency": "DON",
    "exposure": 10.0
}
```

Retrieves your **exposure** (exposure = current balance - available balance).

**Available balance** is the amount that can be used for making predictions.

**Exposure** is the amount placed in currently running, unresolved bet pools.


### Operation code

Name | Code
--------- | -------
get_my_exposure | 840

### Response code

Name | Code | Result
--------- | ------- | -----------
your_exposure | 840 | You have retrieved your exposure.


### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
exposure | null | Amount of exposure in a given currency.
currency | null | Requested currency.


## Balance update (server message)

<aside class="notice">
This is an **unsolicited** server message, pushed at the user through the websocket on every balance update.
</aside>


> Sends the following JSON structure:

```json
{
    "response_header": {},
    "res": 815,
    "current_balance": 1000.0,
    "available_balance": 1000.0,
    "currency": "DON"
}
```

Updates you with your changed current and available balance.

### Response code

Name | Code | Result
--------- | ------- | -----------
balance_update | 815 | You have received your current and available balance update.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
current_balance| 0.0 | Your up-to-date current balance.
available_balance| 0.0 | Your up-to-date available balance.
currency | "DON" | The currency in which the balances were updated.

