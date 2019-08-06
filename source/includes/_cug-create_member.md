## Create a New Member

Using the Site Admin's token you may create a new member by POSTing a *URI encoded* JSON string to this endpoint. Calling this endpoint successfully generates a member token, found in the `CurrentToken` property, which you should save for them if you want to use the "Get a Member by Token" call.

You may notice this call is exactly the same as the "Update an Existing Member" call.

> The JSON data to stringify and encode may look something like this (not yet encoded):

```json
{
    "Points": 100,
    "Names": [{
        "ReferralId": "RST-1",
        "FirstName": "Testme",
        "LastName": "Tester",
        "Email": "mytestuser1@gmail.com",
        "Address1": "123 Main Street",
        "HomePhone": "5551231212"
    }],
    "AdditionalInfo": "{\"partner\":\"roomsteals.com\",\"id\":\"12345\",\"name\":\"Testme Tester\",\"email\":\"mytestuser1@gmail.com\"}"
}
```

<aside class="notice">
The `Points` attribute is only necessary if you want to update the number of points for the member specified. Please note that it will update the member's points to the value specified, not increment or decrement.
</aside>

<aside class="notice">
The JSON structure includes an array of `Names`, which seems to infer the ability to create more than one person. However, in reality, you should only send this call for one member at a time.
</aside>

<aside class="notice">
To receive credit as a partner, you must include a slash escaped JSON string as the `AdditionalInfo` value which includes a key of `partner` and the agreed upon partner domain name to be recognized.  The example uses `roomsteals.com` to show how it should be formatted.  You may also send additional information if desired (such as `id`, `name`, `email`, etc.) which may be used for auditing purposes.
</aside>

> URL Encoded and POSTed via curl:

```shell
curl -X POST \
  https://api.travsrv.com/MemberAPI.aspx \
  -H 'Authorization: Basic {BASE64-ENCODED-STRING}' \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'siteid={SITEID}&token={ADMIN-TOKEN}&memberData=%7B%22Points%22%3A100%2C%22Names%22%3A%5B%7B%22ReferralId%22%3A%22RST-1%22%2C%22FirstName%22%3A%22Testme%22%2C%22LastName%22%3A%22Tester%22%2C%22Email%22%3A%22mytestuser1%40gmail.com%22%2C%22Address1%22%3A%22123%20Main%20Street%22%2C%22HomePhone%22%3A%225551231212%22%7D%5D%2C%22AdditionalInfo%22%3A%22%7B%5C%22partner%5C%22%3A%5C%22roomsteals.com%5C%22%2C%5C%22id%5C%22%3A%5C%2212345%5C%22%2C%5C%22name%5C%22%3A%5C%22Testme%20Tester%5C%22%2C%5C%22email%5C%22%3A%5C%22mytestuser1%40gmail.com%5C%22%7D%22%7D'
```

> The above command returns JSON structured like this:

```json
{
    "MemberId": 54321,
    "Rewards": 0,
    "Points": 100,
    "RedemptionMultiplier": 1.0,
    "EarnMultiplier": 1.0,
    "Names": [{
        "FirstName": "Testme",
        "LastName": "Tester",
        "BirthDate": null,
        "Email": "mytestuser1@gmail.com",
        "Address1": "123 Main Street",
        "Address2": null,
        "City": null,
        "State": null,
        "Country": null,
        "Postal": null,
        "Longitude": null,
        "Latitude": null,
        "HomePhone": "5551231212",
        "ReferralId": "RST-1",
        "Password": null,
        "IsActive": true,
        "DeleteMember": false,
        "UpdateMemberUsername": false,
        "FullName": "Testme Tester"
    }],
    "DebugData": " Successful creation of Member RT-12345-RS-1",
    "Error": null,
    "CurrentToken": "E7MUlbdRdlq2RwSs8V4%2fhza25Xwca%2fZLodi%2faddmPl%2bJfpCU7VovYjoSKaMk34PvkSDpD1mqvezzQG0abXzuXP1%2baaiIKCLw7ehGaiI7BNI7Pb%2fYK%2fGJf4fCxKCz5EodA79iweA6gc2nCEOdWmkarTuy4Cd%2f5WtNkU043rF42sshCKGkg%2bIavKgJ6emdL7msPJRykM2hf%2fmHjdDOV%2b7jtuOHpk5bAVVIc5jhUqmqJMa5908EK0VoX1OUT60SkDcw2YLBeXEg6sYPu1Q7mTPc5VUhJ%2b6C6wHM08eOKMrKt6LNRxoB0kXcPyVS1azt1LR48yAegw%2fKXPbdgrCBycjsDedm0ItP9SmW1C6Byw4nt5zivxf%2f0ZIMF07wtZ4JhWVqGuhetKPDE3ddzOLRPyjNgetWddHqoq8Tba%2bKWDcIADYnqgH5NVdVSKvyH5VWY3vMHyhlZQiW23z1a6lZReASYfMMycNfDU2X4EhDOEa0tvUYajpsRlnDIkNcLjxT4KPyrZhl5tVsHECCY0Sasy%2f6zh9ce%2b3HE%2bOEtux%2bEHKfBWrkzwt1vpwyn%2fnXzVd%2bQumpQLw5DOZ2DltHZs%2bfmQ96MoMrBgSx8jS%2bQkR3NQjGAysUOqXK%2fAl38ryHzGe0nSeMkLo5BRYEgiEJK%2bftnZqsEQbZC98E8Fyt2zMGiofGQrR1i5v3gRoOCfqjNYJQAft4ru6GCR5kpm0CsvVvOnKmnA%3d%3d",
    "TransactionResponse": "true",
    "MetaTag": null,
    "MemberUsername": "RT-12345-mytestuser1@gmail.com",
    "MemberProvider": null,
    "IsArnProvider": true,
    "AdditionalInfo": "{\"partner\":\"roomsteals.com\",\"id\":\"12345\",\"name\":\"Testme Tester\",\"email\":\"mytestuser1@gmail.com\"}"
    "MemberType": "Member"
}
```

### HTTP Request

`POST https://api.travsrv.com/MemberAPI.aspx`

### memberData JSON Object Properties

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
siteid | integer | Yes | Provided by RoomSteals
token | string | Yes | Site Admin Token
Points | integer | No | Number of points for the user
Names | string | Yes | Stringified, URI encoded JSON array of JSON object containing the params below
ReferralId | string | Yes | An agreed upon 3 letter partner prefix concatonated with a hyphen and the ID of the user on the partner's system
FirstName | string | Yes | The user's first name
LastName | string | Yes | The user's last name
Email | string | Yes | The email address of the user
Address1 | string | No | The user's full address
HomePhone | string | No | The user's phone number
AdditionalInfo | string | Yes | Slash-escaped JSON string including at lesat the `partner` key where the value is the partner's domain name
