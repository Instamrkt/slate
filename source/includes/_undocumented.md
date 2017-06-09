<!---
## Pre-Signup

This is used to register a user in our database without fully creating an account. Goes in pair e.g. with pre-game predictions. **Sends out a welcome email.**

Successful response means all the intended actions (a subset of saving email, sending email and handling the challenge) were performed successfully.

### URL
`/r/n/`

### Request type

`POST`

### Request Parameters

Parameter | Description
--------- | -------
email | The email used to saved. All external communication will be directed to this email.
prediction_saved | Boolean flag indicated whether user is making pregame predictions. Outgoing pre-signup email is dependent on that flag.
challenge_id | If user is coming from a friend&apsos;s challenge (shared through email / social media), include the challenge id here. This will initiate the friendship for users.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
msg | null | Error message. Sent only on **unsuccessful** request.

-->
<!---
## Get Username

> Sample request object:

```javascript
var $       = require('jquery');
var cookies = require('cookie-getter');
var _xsrf   = cookies('_xsrf');

$.get('/r/username', {
  headers: {
    'X-XSRFToken': _xsrf
  },
  'userId': userId
});
```

This is used to retrieve a username of a signed in user.

### URL
`/r/username/`

### Request type

`GET`

### Request Parameters

Parameter | Description
--------- | -------
userId | userId for which username is to be retrieved.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
username | null | New username of the requesting user (= passed usernameNew). Sent only on **successful** request.
msg | null | Error message. Sent only on **unsuccessful** request.


## Set Username

> Sample request object:

```javascript
var $       = require('jquery');
var cookies = require('cookie-getter');
var _xsrf   = cookies('_xsrf');

$.post('/r/username', {
	'_xsrf': _xsrf,
  'usernameNew': username,
  'password': password,
  'userId': userId
});
```

This is used to update a username of a signed in user. Password needs to be passed for confirmation.

### URL
`/r/username/`

### Request type

`POST`

### Request Parameters

Parameter | Description
--------- | -------
usernameNew | New Username sign in.
password | Password of the user attempting to change the login (his id derived from the secure im_user cookie attached).
userId | User's id for which password is to be updated.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
username | null | New username of the requesting user (= passed usernameNew). Sent only on **successful** request.
msg | null | Error message. Sent only on **unsuccessful** request.

-->
<!---

## Password recovery

> Sample request object:

```javascript
var $       = require('jquery');
var cookies = require('cookie-getter');
var _xsrf   = cookies('_xsrf');

var jqxhr = $.ajax({
					url: constants.REQUEST_PASSWORD_RECOVERY_URL,
					headers: {
							'X-XSRFToken': _xsrf,
					},
					data: JSON.stringify({
							'username': usernameOrEmail
					}),
					type: 'POST'
			});
```

This is used to request password recovery. It results in sending out an email with a url in which you can provide a new password (with a recovery code hidden in it).
-->
<!---
### URL
`/r/password/recover/`

### Request type

`POST`

### Request Parameters

Parameter | Description
--------- | -------
username | Username or email of the user for whom password recovery is requested.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
msg | null | Error message. Sent only on **unsuccessful** request.

## Password update (from recovery)


> Sample request object:

```javascript
var $       = require('jquery');
var cookies = require('cookie-getter');
var _xsrf   = cookies('_xsrf');
var jqxhr = $.ajax({
		url: '/r/password/update/from_recovery',
		headers: {
				'X-XSRFToken': _xsrf,
		},
		data: JSON.stringify({
				'password1': pass1,
				'password2': pass2,
				'recoveryCode': recoveryCode,
				'email': email
		}),
		type: 'POST'

});
```

This is used to update password from recovery. Requires the generated and emailed recovery code and an email it was emailed to, since the cookie might not be set in this case (e.g. you are on the new machine and forgot a password).

### URL
`/r/password/update/from_recovery/`

### Request type

`POST`

### Request Parameters

Parameter | Description
--------- | -------
password1 | New password.
password2 | New password confirmation.
recoveryCode | The recovery code generated and included in the emailed url.
email | The email to which the code generated code was sent (the email is also included in the emailed url).

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
msg | null | Error message. Sent only on **unsuccessful** request.
-->

<!---
## Password update (general)

> Sample request object:

```javascript
var $       = require('jquery');
var cookies = require('cookie-getter');
var _xsrf   = cookies('_xsrf');

$.post('/r/password/update/', {
  '_xsrf': _xsrf,
  'passwordOld': passwordOld,
  'passwordNew1': passwordNew1,
  'passwordNew2': passwordNew2,
  'userId': userId
}));
```

This is used to update password from a form on the site / app screen.

### URL
`/r/password/update/`

### Request type

`POST`

### Request Parameters

Parameter | Description
--------- | -------
passwordNew1 | New password.
passwordNew2 | New password confirmation.
passwordOld | Old password to prove action is intentional (and not performed by someone else when actual users leaves an open form).
userId | Id of the user whose the password is to be updated.

### Response Parameters

Parameter | Default | Description
--------- | ------- | -----------
msg | null | Error message. Sent only on **unsuccessful** request.

-->
