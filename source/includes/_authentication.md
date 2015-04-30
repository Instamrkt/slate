# Authentication

<aside class="success">
Remember — we care about security for your own good!
</aside>

All **REST Api** calls require the presence of an encrypted cookie, which is generated for user upon successful sign in / sign up and returned as a cookie.

All **Websocket Api** calls require the presence of a session key, which is returned to the user in a response to a successful logon /signup message and also set as a cookie - *"user_session_key"*.

To perform a successful signin, one of the following is necessary:

1. User logs in through the website / app with the correct credentials he provided on signup(username + password + (optionally) a 2fa token).
2. User logs in with an externally-provided valid token (Facebook login).

This is performed through the REST API and grants further access to this API and a Websocket API.