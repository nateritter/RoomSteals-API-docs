---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Hotels For Hope API! You can use our API to access Hotels For Hope API endpoints, which can get information on hundreds of thousands of hotels, their availability, details, as well as create and cancel reservations.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

Hotels For Hope uses a querystring username and password to allow access to the API. You can request Hotels For Hope API credentials by partnering with us and emailing [support](mailto:hello@hotelsforhope.com).

The API credentials are expected in all API requests to the server in the querystring like the following:

`&username=API-USERNAME&password=API-PASSWORD`

<aside class="notice">
You must replace <code>API-USERNAME</code>, <code>API-PASSWORD</code>, and <code>SITEID</code> with your personal API credentials and Site ID.
</aside>

# Common value maps

## `currency`

Supported currencies are located in our static database files. Request access if needed. Major 3-letter currency references (e.g., "USD", "CNY", "CAD", etc.) are supported.

## `gateway`

Optional filter. This should be either left empty to return all sources of supply or specified by a single integer or comma separated list of integers represented by the following:

Gateway ID | Gateway Name
--------- | ------- 
1 | ARN Hotel Wholesale
2 | Travelport Negotiated Commissionable
3 | Travelport Wholesale
4 | Travelport Commissionable
5 | Tourico Hotel Wholesale
16 | ARN Group Commissionable
17 | ARN Hotel Commissionable
20 | ARN On-Hold Commissionable
30 | Expedia
31 | Miki
32 | GetARoom
33 | ARN Group Wholesale
34 | HotelBeds
35 | Bonotel
36 | GTA
37 | CiiRUS
38 | Interval International
39 | RCI
40 | Priceline Wholesale
41 | Priceline Commissionable
42 | Priceline Booking.com​
43 | Mark International
44 | Amadeus Commitionable
45 | Amadeus Negotiated Commissionable
47 | Lots Of Hotels
48 | Jack Travel
49 | Travellanda

## `propertyAmenities`

Optional filter. This should either be an integer or a list of comma separated integers represented by the following:

Amenity ID | Amenity
--------- | ------- 
1 | Airport shuttle
2 | Social hour
3 | Fitness center
4 | Internet access
5 | Free local calls
6 | Complimentary breakfast
7 | Pets allowed
8 | Pool
9 | Restaurant on-site
10 | Kitchen / kitchenette

## `propertyClasses`

Optional filter. This should either be an integer or a list of comma separated integers represented by the following.

Property Class | Star Rating | Description
--------- | ------- | -------
1 | 1 | Economy
2 | 2 | Budget
3 | 3 | Moderate 
4 | 4 | Premium
5 | 5 | Luxury

## `propertyTypes`

Optional filter. This should either be an integer or a list of comma separated integers represented by the following:

Property Type ID | Property Type
--------- | ------- 
1 | Hotel
5 | Motel
6 | Resort
8 | Extended stay property
9 | Bed and breakfast
12 | Vacation rental

#### `roomCode`

This is the `ArnResponse.Availability.HotelAvailability.Hotel.RatePlan.Room.@Code` value specified in the Availability Search results for the room type you are interested.

#### `ratePlanCode`

This is the `ArnResponse.Availability.HotelAvailability.Hotel.RatePlan.@Code` value specified in the Availability Search results for the rate plan you are interested in.

# Hotels

## Availability Search

This endpoint retrieves all hotels which have availability for the dates and locations (or individual hotels) specified.

Make this request for the total number of rooms and the total number of guests.  The response will return a maximum of 50 hotels per request, thus you must use pagination techniques if you need more than 50. That said, 50 is the maximum but may not be the best performing. Use smaller batches to get faster responses.

Responses will contain available rate plans and room types from various suppliers we call "gateways".

Responses may contain full nightly rates and tax information, only first night rates with or without tax information, or the highest or average nightly rate.  This response depends on the supplier/gateway but the most common from most gateways is the first night's rate, thus the property name in our responses (`FirstNightRate`).

Rates are always returned in the hotel's native currency.  If you request another `currency`, the correct conversion rate from the hotel's native currency to the desired `DisplayCurrency` will be returned as `DisplayCurrencyMultiplier`. You may use this value to convert the hotel currency to the desired currency for display in your UI. This allows you to show both the original currency and display currency side by side.

<aside class="notice">
We do not advise programming in a gateway-specific manner. Gateways may respond with varying amounts of information at any time. Also, new gateways could be added at any time.
</aside>

<aside class="notice">
If an availability search fails to return rates, we suggest not showing the hotel in your user interface.
</aside>

```shell
curl "https://api.travsrv.com/hotel.aspx?\
username=API-USERNAME\
&password=API-PASSWORD\
&siteid=SITEID\
&latitude=30.099016\
&longitude=-81.601370\
&radius=10\
&inDate=2018-10-20\
&outDate=2018-10-22\
&maxResults=1\
&rooms=1\
&adults=2\
&children=0\
&ipAddress=127.0.0.1\
&userAgent=shell\
&userLanguage=en\
&_type=json"
```

> The above command returns JSON structured like this (edited for brevity):

```json
{
  "ArnResponse": {
    "Info": {
      "@SiteID": "SITEID",
      "@Username": "API-USERNAME",
      "@IpAddress": "127.0.0.1", 
      "@TimeReceived": "2018-04-13T16:01:13.510", 
      "@TimeCompleted": "2018-04-13T16:01:13.651",
      "@Version": "1.0.0.0",
      "@ServiceUrl": "https://api.travsrv.com/hotel.aspx?inDate=2018-10-20&latitude=30.099016&longitude=-81.601370&maxResults=1&outDate=2018-10-22&password=API-PASSWORD&radius=10&siteid=SITEID&username=API-USERNAME",
      "@RequestID": "436E177E-558F-4287-B55A-5DBD9F4C7579"
    },
    "Availability": {
      "@DisplayCurrency": "USD",
      "HotelAvailability": {
        "@InDate": "2018-10-20",
        "@OutDate": "2018-10-22",
        "@Rooms": "1",
        "@Adults": "2",
        "@Children": "0",
        "Hotel": {
          "@HotelID": "272393",
          "@IsEphemeral": "true",
          "RatePlan": [
            {
              "@Code": "F89648494F22FFE25A254AD1CE7D18FD27B862F1A609BD28872D9CD6DDA2E956B160BE3DB7EB0191B160BE3DB7EB01917EA3048807BEE935",
              "@Description": "Best Available",
              "@BuyerOnly": "false",
              "@Gateway": "41",
              "@CommissionStatus": "Commissionable",
              "@BalanceDueAtHotel": "false",
              "Room": {
                "@Code": "HRLM--_eJwFwcuaczAAANAHmgVRCRaziPRvxkyJadx3biWUfkFH9On_fc5BTlaYBzSSx4m6Iwz3cS_pRnKxCtkQzjqPQZm8Lh771NInUK3ri_pDYTzZL6lQtgl0H0_fOj_f5dRd1kT_pX8dtg_pq0LMTJYHFXNuPqi0PxdLuTjEAeTtKNfYZ3Gd3suIjMbm3_prdXk03gvS_pfpUJnL_poCL9kNoyIlpA2DURM1_fqAtELSKYuKh6e248vaSGk9hE9Dns4MYIncb5jC_fcZG_p_fOeSN3Z0N_fib2UqSpSLCPFgqPX2sQTP9rmXEv_ptXOMVOlVLoC_fujzlUz6tJvvFwUEonyP6njavHdzsVVF2EesNGSmILz3twKMBKm_fp7PK2XCcgV8ghjMc93ZaXrLkm622zmAYaTaPSItlgfYOcO2TSZ6hvwXlxw_fSAVdl9fv4Hwe6AJQ",
                "@Name": "Standard room",
                "@Description": "- 1 King Bed - Nonsmoking Room - Free Breakfast, Free Wifi, Fridge, Microwave",
                "@CurrencyCode": "USD",
                "@DisplayCurrencyMultiplier": "1",
                "@USDMultiplier": "1",
                "@ExchangeGMT": "2018-04-13T11:00:26.790",
                "@MaximumBookable": "99",
                "NightlyRate": [
                  {
                    "@Date": "2018-10-20",
                    "@Price": "114.44"
                  },
                  {
                    "@Date": "2018-10-21",
                    "@Price": "114.43"
                  }
                ],
                "Tax": {
                  "@Percent": "11.51",
                  "@Amount": "29.77"
                },
                "GatewayFee": {
                  "@Amount": "0.00"
                },
                "Total": {
                  "@Amount": "258.64",
                  "@IncludesBookingFee": "false"
                },
                "BookingFee": {
                  "@Amount": "0.00",
                  "@CurrencyCode": "USD",
                  "@DisplayCurrencyMultiplier": "1",
                  "@RoomCurrencyMultiplier": "1",
                  "@ExchangeGMT": "2018-04-13T11:00:26.790"
                }
              }
            }
          ]
        }
      }
    }
  }
}
```

### HTTP Request

`GET https://api.travsrv.com/hotel.aspx`

### Query Parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
username | string | Yes | Provided by Hotels For Hope
password | string | Yes | Provided by Hotels For Hope
siteid | integer | Yes | Provided by Hotels For Hope
timeout | int | No | Maximum time to allow for searching gateways, measured in seconds. Default: 15
maxResults | int | No | Number of results to return. Default: 20. Maximum: 50.
hotelIds | int* | No | See below.
refHotelId | int | No | See below.
latitude | float | No | See below.
longitude | float | No | See below.
radius | float | No | See below.
rooms | int | Yes | Number of rooms needed. When searching for more than one room, responses are based on the same room type and occupancy for every room. Maximum: 9 (best results with no more than 4).
inDate | date | Yes | Desired check-in date (format: `YYYY-MM-DD` based on UTC -7 (MST) time zone)
outDate | date | Yes | Desired check-out date (format: `YYYY-MM-DD` based on UTC -7 (MST) time zone)
adults | int | Yes | Total number of adults. For instance, if `rooms=2&adults=2`, the search is for 1 room per adult. Maximum 8 adults per room. Uneven/odd numbers of adults/rooms will be rounded up to support legacy providers.
children | int | Yes | Total number of children. Maximum: 8. Note: Some legacy providers do not honor the `children` parameter.
sortType | string/enum | No | One of `bestvalue`, `dealpercent`, `dealamount`. Default: `bestvalue`. See below.
currency | string | No | Default: `USD`. See common value map.
name | string | No | Partial hotel name filter. E.g., passing "ilton" would return properties like "Hilton Garden Inn", etc.
propertyClasses | int* | No | See common value map.
propertyAmenities | int* | No | See common value map.
propertyTypes | int* | No | See common value map.
gateway | int* | No | See common value map.
ipAddress | string | Yes | The IPv4 address of the person browsing your website. This is used for fraud checking and preventing booking failures.
userAgent | string | Yes | The userAgent of the app/browser of the person browsing your website. This is used for fraud checking and preventing booking failures.
userLanguage | string | Yes | The language of the browser of the person browsing your website. This is used for fraud checking and preventing booking failures.
locale | string | No | Specifies the response language. Default: "US"
page | int | No | Specifies the results page to return (when more than one page of results is available).
_type | string/enum | No | The response format. One of `xml` or `json`. Default: `json`.

#### `hotelIds`, `locationId`, `refHotelId`, `latitude`+`longitude`+`radius`

One of the following is required:

* `hotelIds`: A list of integers, separated by commas if more than one. Max of 50 hotel IDs may be specified at one time. May be substituted with `refHotelId` or `locationId` or `latitude`+`longitude`+`radius`.
* `refHotelId`: A single hotel ID used to look up the city and return results from that city, as if we had searched by `locationId`. May be substituted with `hotelIds` or `locationId` or `latitude`+`longitude`+`radius`.
* `locationId`: A location ID used to return hotels from within the location's city. May be substituted with `hotelIds` or `refHotelId` or `latitude`+`longitude`+`radius`.
* `latitude` + `longitude` + `radius` (Do not send `radius` if doing `latitude`/`longitude` based deal search)
** `radius`: Measured in miles.  NOTE: If doing a market deal search, leave off radius and just pass `latitude` and `longitude`.

#### `sortType`

Sort Type | Description
--------- | -------
`bestvalue` | The default sort type, based on previous booking trends and manual sorting options.
`dealpercent` | Sorts the hotel results based on prices last seen and returns the best percentage savings we have found historically for the specified dates.
`dealamount` | Sorts the hotel results based on prices last seen and returns the best amount savings we have found historically and for those dates.

## Hotel Detail

This call confirms availability with the supplier / gateway (requests to skip any caching they have in place) and retrieves cancellation and booking policies to display prior to the user placing a reservation.

This call is only available for a single hotel ID, room code, and rate code.

Responses will contain nightly rates, taxes and fees associated with the room, and hotel, payment, and cancellation policies which apply to the rate.

This request is rate limited as it is an expensive request to make. Responses with a 429 status code are in response to over use, should you exceed the rate limit. 

Failed responses indicate the rate is sold out. Subsequent availability calls will flush and re-cache this hotel's rates.

**All fees, penalties, and policies from this response must be shown to the user before creating a reservation.**

All room prices are based on the number of adults per room specified in the request.  A response containing an extra adult fee of `0` may simply mean the pricing is not available. Adding more than the specified number of adults to the room because the extra adult fee is `0` will likely incur a price difference when the guest arrives at the hotel. To avoid this, we advise not depend on the extra adult fee specified in the response when it comes to pricing.


```shell
curl "https://api.travsrv.com/hotel.aspx?\
username=API-USERNAME\
&password=API-PASSWORD\
&siteid=SITEID\
&hotelIds=272393\
&inDate=2018-10-20\
&outDate=2018-10-22\
&maxResults=1\
&rooms=1\
&adults=2\
&children=0\
&gateway=51\
&ratePlanCode=F89648494F22FFE25A254AD1CE7D18FD27B862F1A609BD28872D9CD6DDA2E956B160BE3DB7EB0191B160BE3DB7EB01917EA3048807BEE935\
&roomCode=HRLM--_eJwFwcuaczAAANAHmgVRCRaziPRvxkyJadx3biWUfkFH9On_fc5BTlaYBzSSx4m6Iwz3cS_pRnKxCtkQzjqPQZm8Lh771NInUK3ri_pDYTzZL6lQtgl0H0_fOj_f5dRd1kT_pX8dtg_pq0LMTJYHFXNuPqi0PxdLuTjEAeTtKNfYZ3Gd3suIjMbm3_prdXk03gvS_pfpUJnL_poCL9kNoyIlpA2DURM1_fqAtELSKYuKh6e248vaSGk9hE9Dns4MYIncb5jC_fcZG_p_fOeSN3Z0N_fib2UqSpSLCPFgqPX2sQTP9rmXEv_ptXOMVOlVLoC_fujzlUz6tJvvFwUEonyP6njavHdzsVVF2EesNGSmILz3twKMBKm_fp7PK2XCcgV8ghjMc93ZaXrLkm622zmAYaTaPSItlgfYOcO2TSZ6hvwXlxw_fSAVdl9fv4Hwe6AJQ\
&ipAddress=127.0.0.1\
&userAgent=shell\
&userLanguage=en\
&_type=json"
```

> The above command returns JSON structured like this (edited for brevity):

```json
{
  "ArnResponse": {
    "Info": {
      "@SiteID": "SITEID",
      "@Username": "API-USERNAME",
      "@IpAddress": "24.113.225.28",
      "@TimeReceived": "2018-04-17T00:41:27.367",
      "@TimeCompleted": "2018-04-17T00:41:27.804",
      "@Version": "1.0.0.0",
      "@ServiceUrl": "http://api.travsrv.com/hotel.aspx?_type=json&adults=2&children=0&gateway=51&hotelIds=272393&inDate=2018-10-20&ipAddress=127.0.0.1&maxResults=1&outDate=2018-10-22&password=API-PASSWORD&ratePlanCode=&roomCode=HRLM--_eJwFwcuaczAAANAHmgVRCRaziPRvxkyJadx3biWUfkFH9On_fc5BTlaYBzSSx4m6Iwz3cS_pRnKxCtkQzjqPQZm8Lh771NInUK3ri_pDYTzZL6lQtgl0H0_fOj_f5dRd1kT_pX8dtg_pq0LMTJYHFXNuPqi0PxdLuTjEAeTtKNfYZ3Gd3suIjMbm3_prdXk03gvS_pfpUJnL_poCL9kNoyIlpA2DURM1_fqAtELSKYuKh6e248vaSGk9hE9Dns4MYIncb5jC_fcZG_p_fOeSN3Z0N_fib2UqSpSLCPFgqPX2sQTP9rmXEv_ptXOMVOlVLoC_fujzlUz6tJvvFwUEonyP6njavHdzsVVF2EesNGSmILz3twKMBKm_fp7PK2XCcgV8ghjMc93ZaXrLkm622zmAYaTaPSItlgfYOcO2TSZ6hvwXlxw_fSAVdl9fv4Hwe6AJQ&rooms=1&siteid=SITEID&userAgent=shell&userLanguage=en&username=API-USERNAME",
      "@RequestID": "2999AB6A-6DB9-4FFF-9586-BBED51BB6108"
    },
    "Availability": {
      "@DisplayCurrency": "USD",
      "HotelAvailability": {
        "@InDate": "2018-10-20",
        "@OutDate": "2018-10-22",
        "@Rooms": "1",
        "@Adults": "2",
        "@Children": "0",
        "Hotel": {
          "@HotelID": "272393",
          "@IsEphemeral": "true",
          "RatePlan": [
            {
              "@Code": "F89648494F22FFE25A254AD1CE7D18FD27B862F1A609BD28872D9CD6DDA2E956B160BE3DB7EB0191B160BE3DB7EB01917EA3048807BEE935",
              "@Description": "Best Available",
              "@BuyerOnly": "false",
              "@Gateway": "41",
              "@CommissionStatus": "Commissionable",
              "@BalanceDueAtHotel": "false",
              "Room": {
                "@Code": "",
                "@Name": "Standard room",
                "@Description": "- 1 King Bed - Nonsmoking Room - Free Breakfast, Free Wifi, Fridge, Microwave",
                "@CurrencyCode": "USD",
                "@DisplayCurrencyMultiplier": "1",
                "@USDMultiplier": "1",
                "@ExchangeGMT": "2018-04-17T00:00:06.027",
                "@MaximumBookable": "99",
                "NightlyRate": [
                  {
                    "@Date": "2018-10-20",
                    "@Price": "114.44"
                  },
                  {
                    "@Date": "2018-10-21",
                    "@Price": "114.43"
                  }
                ],
                "Tax": {
                  "@Percent": "11.51",
                  "@Amount": "29.77"
                },
                "GatewayFee": {
                  "@Amount": "0.00"
                },
                "Total": {
                  "@Amount": "258.64",
                  "@IncludesBookingFee": "false"
                },
                "BookingFee": {
                  "@Amount": "0.00",
                  "@CurrencyCode": "USD",
                  "@DisplayCurrencyMultiplier": "1",
                  "@RoomCurrencyMultiplier": "1",
                  "@ExchangeGMT": "2018-04-17T00:00:06.027"
                }
              }
            }
          ]
        }
      }
    }
  }
}
```

### HTTP Request

`GET https://api.travsrv.com/hotel.aspx`

### Query Parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
username | string | Yes | Provided by Hotels For Hope
password | string | Yes | Provided by Hotels For Hope
siteid | integer | Yes | Provided by Hotels For Hope
timeout | int | No | Maximum time to allow for searching gateways, measured in seconds. Default: 15
hotelIds | int | No | The particular hotel id for which more detail is being requested.
rooms | int | Yes | Number of rooms needed. When searching for more than one room, responses are based on the same room type and occupancy for every room. Maximum: 9 (best results with no more than 4).
inDate | date | Yes | Desired check-in date (format: `YYYY-MM-DD` based on UTC -7 (MST) time zone)
outDate | date | Yes | Desired check-out date (format: `YYYY-MM-DD` based on UTC -7 (MST) time zone)
adults | int | Yes | Total number of adults. For instance, if `rooms=2&adults=2`, the search is for 1 room per adult. Maximum 8 adults per room. Uneven/odd numbers of adults/rooms will be rounded up to support legacy providers.
children | int | Yes | Total number of children. Maximum: 8. Note: Some legacy providers do not honor the `children` parameter.
currency | string | No | Default: `USD`. See common value map.
ratePlanCode | string | Yes | See common value map.
roomCode | string | Yes | See common value map.
gateway | int | No | See common value map.
ipAddress | string | Yes | The IPv4 address of the person browsing your website. This is used for fraud checking and preventing booking failures.
userAgent | string | Yes | The userAgent of the app/browser of the person browsing your website. This is used for fraud checking and preventing booking failures.
userLanguage | string | Yes | The language of the browser of the person browsing your website. This is used for fraud checking and preventing booking failures.
locale | string | No | Specifies the response language. Default: "US"
_type | string/enum | No | The response format. One of `xml` or `json`. Default: `json`.

## Hotel Reservation Creation

```shell
curl -X POST "https://api.travsrv.com/hotel.aspx?\
username=API-USERNAME\
&password=API-PASSWORD\
&siteid=SITEID\
...
&ipAddress=127.0.0.1\
&userAgent=shell\
&userLanguage=en\
&_type=json"
```

> The above command returns JSON structured like this (edited for brevity):

```json
{
}
```

### HTTP Request

`POST https://api.travsrv.com/hotel.aspx`

### Query Parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
username | string | Yes | Provided by Hotels For Hope
password | string | Yes | Provided by Hotels For Hope
siteid | integer | Yes | Provided by Hotels For Hope
hotelIds | int | No | The particular hotel id for which more detail is being requested.
agentRefNumber | string | No | A reference ID you may use for your own tracking purposes. This is included in a reservation webhook, if used.
rooms | int | Yes | Number of rooms needed. When searching for more than one room, responses are based on the same room type and occupancy for every room. Maximum: 9 (best results with no more than 4).
inDate | date | Yes | Desired check-in date (format: `YYYY-MM-DD` based on UTC -7 (MST) time zone)
outDate | date | Yes | Desired check-out date (format: `YYYY-MM-DD` based on UTC -7 (MST) time zone)
adults | int | Yes | Total number of adults. For instance, if `rooms=2&adults=2`, the search is for 1 room per adult. Maximum 8 adults per room. Uneven/odd numbers of adults/rooms will be rounded up to support legacy providers.
children | int | Yes | Total number of children. Maximum: 8. Note: Some legacy providers do not honor the `children` parameter.
currency | string | No | Default: `USD`. See common value map.
ratePlanCode | string | Yes | See common value map.
roomCode | string | Yes | See common value map.
gateway | int | No | See common value map.
campaignCode | string | No | See below.
guestTitle | string | No | Primary guest's title (e.g., "Mr.", "Mrs.", etc.)
guestFirstName | string | Yes | Primary guest's first name
guestLastName | string | Yes | Primary guest's last name.
guestEmail | string | Yes | Primary guest's email address.
guestPhoneCountry | string | Yes | Primary guest's phone number country code.
guestPhoneArea | string | Yes | Primary guest's phone number area code. 
guestPhoneNumber | string | Yes | Primary guest's phone number (without country or area code)
guestPhoneExtension | string | No | Primary guest's phone number extension.
addressAddress | string | Yes | Primary guest's address.
addressCity | string | Yes | Primary guest's city.
addressRegion | string | Yes | Primary guest's state / region.
addressPostalCode | string | Yes | Primary guest's postal code.
addressCountryCode | string | Yes | Primary guest's country code.
roomCostPrice | decimal | Yes | See below.
roomCostTaxAmount | decimal | Yes | See below.
roomCostGatewayFee | decimal | Yes | See below.
roomCostTotalAmount | decimal | Yes | See below.
roomCostCurrencyCode | string | Yes | See below.
bookingFeeAmount | decimal | Yes | See below.
creditCardType | string | Yes | Credit card type ("AMEX", "MasterCard", "VISA", etc.)
creditCardNumber | string | Yes | Credit card number.
creditCardExpiration | string | Yes | Credit card expiration date (format: `YYYY-MM-DD`)
creditCardCVV2 | int | Yes | Credit card CVV2 number.
creditCardHolder | string | Yes | Name on credit card.
creditCardAddress | string | Yes | Credit card billing address.
creditCardCity | string | Yes | Credit card billing city.
creditCardRegion | string | Yes | Credit card billing state / region.
creditCardPostalCode | string | Yes | Credit card billing postal code.
creditCardCountryCode | string | Yes | Credit card billing country code.
ipAddress | string | Yes | The IPv4 address of the person browsing your website. This is used for fraud checking and preventing booking failures.
userAgent | string | Yes | The userAgent of the app/browser of the person browsing your website. This is used for fraud checking and preventing booking failures.
userLanguage | string | Yes | The language of the browser of the person browsing your website. This is used for fraud checking and preventing booking failures.
locale | string | No | Specifies the response language. Default: "US"
_type | string/enum | No | The response format. One of `xml` or `json`. Default: `json`.

#### `campaignCode`

You may pass in a string which you want to persist with the reservation.  This is analagous to a "subid" and can be used for reconciling with your own marketing campaigns, or really anything else you want to do with it.  The string you provide in this parameter will be added to the reservation being made and available in available reporting.

#### `roomCostPrice`

Send the exact same information received from the Hotel Detail Request (assuming the same `currency` designation in both requests. This is used to assure the expected reservation amounts match the actual prices the hotel has given.

#### `roomCostTaxAmount`

Send the exact same information received from the Hotel Detail Request (assuming the same `currency` designation in both requests. This is used to assure the expected reservation amounts match the actual prices the hotel has given.

#### `roomCostGatewayFee`

Send the exact same information received from the Hotel Detail Request (assuming the same `currency` designation in both requests. This is used to assure the expected reservation amounts match the actual prices the hotel has given.

#### `roomCostTotalAmount`

Send the exact same information received from the Hotel Detail Request (assuming the same `currency` designation in both requests. This is used to assure the expected reservation amounts match the actual prices the hotel has given.

#### `roomCostCurrencyCode`

Send the exact same information received from the Hotel Detail Request (assuming the same `currency` designation in both requests. This is used to assure the expected reservation amounts match the actual prices the hotel has given.

#### `bookingFeeAmount`

Send the exact same information received from the Hotel Detail Request (assuming the same `currency` designation in both requests. This is used to assure the expected reservation amounts match the actual prices the hotel has given.

## Hotel Reservation Cancellation

```shell
curl -X POST "https://api.travsrv.com/hotel.aspx?\
username=API-USERNAME\
&password=API-PASSWORD\
&siteid=SITEID\
...
&ipAddress=127.0.0.1\
&userAgent=shell\
&userLanguage=en\
&_type=json"
```

> The above command returns JSON structured like this (edited for brevity):

```json
{
}
```

### HTTP Request

`POST https://api.travsrv.com/hotel.aspx`

### Query Parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
