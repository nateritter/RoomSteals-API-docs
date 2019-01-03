## Create a Hold

You may retrieve a list of the room types (you'll need the room code / room type id) available to put on hold via an Availability Search and specifying gateway `16` (contracted inventory) (authorized, as mentioned above, using a basic authentication header) to this endpoint. Calling this endpoint successfully will return a 200 response code, but no body.

```shell
curl -X POST \
"https://groups.alliancereservations.com/services/external/roomtypesonhold?&roomTypeId={PARENT-BLOCK-ROOM-TYPE-ID}&createHold" \
-H 'Authorization: Basic {BASE64-ENCODED-STRING}'
```

> The above command returns JSON structured like this:

```json
```

### HTTP Request

`POST https://groups.alliancereservations.com/services/external/roomtypesonhold`

### Query Parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
roomTypeId | integer | Yes | `ArnResponse.Availability.HotelAvailability.Hotel.RatePlan.Room.@Code` from an Availability Search (parent block)
createHold | void | Yes | Specifies this is a create

### POST form-data Parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
blockDescription | string | Yes | Name this on hold block
roomCountPerNight | integer | Yes | Number of rooms to hold
blockStartDate | date | Yes | Start date of held rooms
blockEndDate | date | Yes | End date of held rooms
blockReleaseDate | date | Yes | Date when hold will expire
