---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  # - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Hotels For Hope API! You can use our API to access Hotels For Hope API endpoints, which can get information on hundreds of thousands of hotels, their availability, details, as well as create and cancel reservations.

We have examples using a shell. You can view code examples in the dark area to the right, and (when languages become available) you can switch the programming language of the examples with the tabs in the top right.

# Authentication

Hotels For Hope uses a querystring username and password to allow access to the API. You can request Hotels For Hope API credentials by partnering with us and emailing [support](mailto:hello@hotelsforhope.com).

The API credentials are expected in all API requests to the server in the querystring like the following:

`&username=API-USERNAME&password=API-PASSWORD`

<aside class="notice">
You must replace <code>API-USERNAME</code>, <code>API-PASSWORD</code>, and <code>SITEID</code> with your personal API credentials and Site ID.
</aside>

# Hotels

## Availability Search

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
&maxresults=1"
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
      "@ServiceUrl": "https://api.travsrv.com/hotel.aspx?inDate=2018-10-20&latitude=30.099016&longitude=-81.601370&maxresults=1&outDate=2018-10-22&password=API-PASSWORD&radius=10&siteid=SITEID&username=API-USERNAME",
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
                "@Code": "",
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

This endpoint retrieves all hotels which have availability for the dates and locations (or individual hotels) specified.

### HTTP Request

`GET https://api.travsrv.com/hotel.aspx`

### Query Parameters

Parameter | Type | Required | Description
--------- | ------- | ------- | -----------
username | string | Yes | Provided by Hotels For Hope
password | string | Yes | Provided by Hotels For Hope
siteid | integer | Yes | Provided by Hotels For Hope
timeout | int | No | Maximum time to allow for searching gateways, measured in seconds. Default: 15
maxresults | int | No | Number of results to return. Default: 20. Maximum: 50.
hotelids | int* | No | See below.
refHotelid | int | No | See below.
latitude | float | No | See below.
longitude | float | No | See below.
radius | float | No | See below.
rooms | int | Yes | Number of rooms needed. When searching for more than one room, responses are based on the same room type and occupancy for every room. Maximum: 9 (best results with no more than 4).
inDate | date | Yes | Desired check-in date (format: `YYYY-MM-DD` based on UTC -7 (MST) time zone)
outDate | date | Yes | Desired check-out date (format: `YYYY-MM-DD` based on UTC -7 (MST) time zone)
adults | int | Yes | Total number of adults. For instance, if `rooms=2&adults=2`, the search is for 1 room per adult. Maximum 8 adults per room. Uneven/odd numbers of adults/rooms will be rounded up to support legacy providers.
children | int | Yes | Total number of children. Maximum: 8. Note: Some legacy providers do not honor the `children` parameter.
sorttype | string/enum | No | One of `bestvalue`, `dealpercent`, `dealamount`. Default: `bestvalue`. See below.
currency | string | No | Default: `USD`. See below.
name | string | No | Partial hotel name filter. E.g., passing "ilton" would return properties like "Hilton Garden Inn", etc.
propertyclasses | int* | No | See below.
propertyamenities | int* | No | See below.
propertytypes | int* | No | See below.
gateway | int* | No | See below.
ipAddress | string | Yes | The IPv4 address of the person browsing your website. This is used for fraud checking and preventing booking failures.
userAgent | string | Yes | The userAgent of the app/browser of the person browsing your website. This is used for fraud checking and preventing booking failures.
userLanguage | string | Yes | The language of the browser of the person browsing your website. This is used for fraud checking and preventing booking failures.
locale | string | No | Specifies the response language. Default: "US"
page | int | No | Specifies the results page to return (when more than one page of results is available).
_type | string/enum | Yes | The response format. One of `xml` or `json`. Default: `json`.

#### `hotelids`, `locationid`, `refHotelid`, `latitude`+`longitude`+`radius`

One of the following is required:

* `hotelids`: A list of integers, separated by commas if more than one. Max of 50 hotel IDs may be specified at one time. May be substituted with `refHotelid` or `locationid` or `latitude`+`longitude`+`radius`.
* `refHotelid`: A single hotel ID used to look up the city and return results from that city, as if we had searched by `locationid`. May be substituted with `hotelids` or `locationid` or `latitude`+`longitude`+`radius`.
* `locationid`: A location ID used to return hotels from within the location's city. May be substituted with `hotelids` or `refHotelid` or `latitude`+`longitude`+`radius`.
* `latitude` + `longitude` + `radius` (Do not send `radius` if doing `latitude`/`longitude` based deal search)
** `radius`: Measured in miles.  NOTE: If doing a market deal search, leave off radius and just pass `latitude` and `longitude`.

#### `sorttype`

Sort Type | Description
--------- | -------
`bestvalue` | The default sort type, based on previous booking trends and manual sorting options.
`dealpercent` | Sorts the hotel results based on prices last seen and returns the best percentage savings we have found historically for the specified dates.
`dealamount` | Sorts the hotel results based on prices last seen and returns the best amount savings we have found historically and for those dates.

#### `currency`

Supported currencies are located in our static database files. Request access if needed. Major 3-letter currency references (e.g., "USD", "CNY", "CAD", etc.) are supported.

#### `propertyclasses`

Optional filter. This should either be an integer or a list of comma separated integers represented by the following.

Property Class | Star Rating | Description
--------- | ------- | -------
1 | 1 | Economy
2 | 2 | Budget
3 | 3 | Moderate 
4 | 4 | Premium
5 | 5 | Luxury

#### `propertyamenities`

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

#### `propertytypes`

Optional filter. This should either be an integer or a list of comma separated integers represented by the following:

Property Type ID | Property Type
--------- | ------- 
1 | Hotel
5 | Motel
6 | Resort
8 | Extended stay property
9 | Bed and breakfast
12 | Vacation rental

#### `gateway`

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

