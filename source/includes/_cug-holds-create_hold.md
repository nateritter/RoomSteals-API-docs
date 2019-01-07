## Create a Hold

You may retrieve a list of the room types (you'll need the room code / room type id) available to put on hold via an Availability Search and specifying gateway `16` (contracted inventory) (authorized, as mentioned above, using a basic authentication header) to this endpoint. Calling this endpoint successfully will return a 200 response code, but no body.

NOTE: When seting the POST form-data Parameters (seen below), be sure the number of values in the comma separated list specified for "roomCountPerNight" match the number of nights (inclusive) specified in the "blockStartDate" / "blockEndDate" range.

For instance, if the "blockStartDate" = "12/01/2019" and the "blockEndDate" = "12/03/2019", the "roomCountPerNight" should have 3 values, like "1,1,1".  

Additionally, the "blockStartDate" and "blockEndDate" need to be within the parent room type's block dates or the system won't be able to allocate rooms from the parent to hold block.

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
blockStartDate | date | Yes | Start date of held rooms (format: mm/dd/yyyy)
blockEndDate | date | Yes | End date of held rooms (format: mm/dd/yyyy)
blockReleaseDate | date | Yes | Date when hold will expire
