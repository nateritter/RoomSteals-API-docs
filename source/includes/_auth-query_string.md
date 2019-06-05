## Query String

RoomSteals uses a querystring username and password to allow access to the API. You can request RoomSteals API credentials by partnering with us and emailing [support](mailto:hello@roomsteals.com).

The API credentials are expected in all API requests to the server in the querystring like the following:

`&username={API-USERNAME}&password={API-PASSWORD}&siteid={SITEID}`

<aside class="notice">
You must replace <code>{API-USERNAME}</code>, <code>{API-PASSWORD}</code>, and <code>{SITEID}</code> with your personal API credentials and Site ID respectively.
</aside>

<aside class="warning">
Considering the sensitive nature of query string based API calls, we highly suggest not exposing these credentials by using them in client-side scripting languages like JavaScript. We recommend making calls with these credentials only using server-side technologies.
</aside>
