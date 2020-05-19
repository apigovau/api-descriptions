---
name : "Australian Design Search API"
description : "The Australian Design Search API allows you to search for Australian Designs and retrieve data displayed on [Australian Design Search](https://search.ipaustralia.gov.au/designs/search/quick)."
logo : "https://api.gov.au/img/ip_australia_design_logo.svg"
tags:
 - "Security:oAuth2"
 - "Technology:Rest/JSON"
 - "OpenAPISpec:RAML"
 - "AgencyAcr:IP Australia"
 - "Status:Published"
 - "Category:Search"
---

# Getting Started

The Australian Design Search API allows you to search for Australian Designs.

For more details, and to request access to the API please visit the [IP Australia Developer Portal.](https://portal.api.ipaustralia.gov.au/s/)

## Key Information

### Base URL

**TEST** 

The base URL for the Test instance of Australian Design Search API is:

>https://test.api.ipaustralia.gov.au/public/australian-design-search-api/v1/

**PRODUCTION**

The base URL for the Production instance Australian Design Search API is:

>https://production.api.ipaustralia.gov.au/public/australian-design-search-api/v1/

------


### Requests

Australian Design Search API has two endpoints, a quick search and a get by Trade Mark Number. 

Basic details about each endpoint have been outlined below, for full details, including example request and response payloads, please view the Australian Design Search API on the [IP Australia Developer Portal.](https://portal.api.ipaustralia.gov.au/s/)

#### Quick Search

The quick search endpoint enables a user to search for a list of Australian Design Numbers that match the given criteria. Users can search by a given string, and filter the results based on classification and status. The request also supports limiting the search to only Designs that have been updated since a given date. 

Request Path: /search/quick
Method: POST
Request Payload Example:
```json
{
  "changedSinceDate": "2001-10-01",
  "filters": {
    "classificationFilter": [
      "0202c"
    ],
    "statusFilter": [
      "REGISTERED"
    ]
  },
  "query": "clothing"
}
```

------

#### Get By Australian Design Number

This endpoint allows users to get all of the information about a given Design. The response contains all publicly available information about the Design. 

Request Path: /design/{ipRightIdentifier}
Method: GET

### Response Format

All responses are in JSON

### Contact Us

If you require further information or assistance regarding IP Australia’s APIs, please contact us via:
- Email: MDB-TDS@ipaustralia.gov.au
- Website: [https://www.ipaustralia.gov.au/api-transaction-channel](https://www.ipaustralia.gov.au/api-transaction-channel)
- Phone: 1300 65 10 10
- International Phone: +61 2 6283 2999


# Authentication

## Overview

IPAustralia APIs enforce the [OAuth 2.0 protocol](https://oauth.net/2/) for application and user authorisation. OAuth is the industry standard for assuring that online transactions are secure and your application must need provide a valid access token for each request it makes to the IP Australia API.

The OAuth 2.0 framework specifies several grant types for different use cases and **IP Australia External Token API** provides access tokens for following grant types:

**Client credentials grant**  - _Application access token_

_**This grant is intended for B2B Partner Applications to get access token using the provided client id/secret keys. **_

The Client Credentials grant is used when applications request an access token to access their own resources, not on behalf of a user. 

**Authorization code grant - **_User access Token_

_**This grant is intended for 3rd Party Developer Apps to get access token on behalf of their end user. **_

_To be implemented in future_

------

## External Token API Endpoints

**TEST** 

```
https://test.api.ipaustralia.gov.au/public/external-token-api/v1/access_token
```

**PRODUCTION**

```
https://production.api.ipaustralia.gov.au/public/external-token-api/v1/access_token
```

------

## How to call API with OAuth Security Token

1. _**Obtain OAuth2 Access Token From External Token API**_

Send HTTP Post Request to IPAustralia's External Token API with following form parameters:

```
grant_type=client_credentials
client_id={Client_ID}
client_secret={Client_Secret}
```

_Sample Request_

```
POST https://test.api.ipaustralia.gov.au/public/external-token-api/v1/access_token HTTP/1.1
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&
client_id=73oam0iel1n9tf98cn00lja662&
client_secret=17gt5kvgkgn82ocu8dlh6gq50t1lke569i1jhigrvtjbiae807j2
```

OAuth Access token (Json Web Token) is returned in the response JSON Payload under "access\_token" as below:

_Sample Response:_

```
HTTP/1.1 200 OK

{
   "access_token" : "eyJraWQiOiJWc2kyU0l0SkpvN1pzekRzWWNDUWlUU1wvTFJQeHEyVW50MDc5ZjNSSTFpbz0iLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI3M29hbTBpZWwxbjl0Zjk4Y24wMGxqYTY2MiIsInRva2VuX3VzZSI6ImFjY2VzcyIsInNjb3BlIjoiaHR0cHM6XC9cL2FwaS5pcGF1c3RyYWxpYS5nb3YuYXVcL2IyYlwvaXByaWdodHNcL2FnZW50IiwiYXV0aF90aW1lIjoxNTQ2NDg1MjA1LCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAuYXAtc291dGhlYXN0LTIuYW1hem9uYXdzLmNvbVwvYXAtc291dGhlYXN0LTJfV3pLa2FnemhQIiwiZXhwIjoxNTQ2NDg4ODA1LCJpYXQiOjE1NDY0ODUyMDUsInZlcnNpb24iOjIsImp0aSI6ImRmYzliYTlkLWYzYzUtNDA4YS04Y2Y5LTA1ZmNhZDhhN2JjZiIsImNsaWVudF9pZCI6Ijczb2FtMGllbDFuOXRmOThjbjAwbGphNjYyIn0.nl-Gncp1uBK1qvOMCQY45oA8ekgu6q77SfOU94xeC1lkqM_dX4sASxIw5IambMd6ofJwXG5pz3Fw-zx4td_h36Wj4tIEdUkbk9Z2xadw8HNndBiPxqjrB3VfjkL22Dq7Tf0HCxJ3zQ_NSGb_5gbTA2RHx2lS7Z0qDGgk8JsriauZ4p-s-XALDjsvmaCAm_uQ67XSB1t3SRsUI9DFMA0974Cnn3a_lmyZ7WpgIEVhFffjTmBhiD2p6c8Jg2Mc6Beas9zLUgPtR8aNrHddCuzkXf19Q7s3C0c7hurPRMnfQcnKODYaLrWv9a56ZtNnmkuzMm_W3dcSXWyLH-3eFxgrrA",
   "expires_in" : 3600,
   "token_type" : "Bearer"
}
```

_Access Token is valid for 1 hour, hence the same token can be reused across multiple API requests._

2.** **_**Pass OAuth Access Token in the HTTP Header of the actual API Request**_

Send the above OAuth Token in HTTP Authorisation Header of the API Request in the below format:

```
Authorizaton: {token_type} + space + {access_token}
```

_Sample Patent Renewal Request_

```

POST https://test.api.ipaustralia.gov.au/public/ipright-management-b2b-api/v1/patents/2018219977/renew HTTP/1.1

Authorization: Bearer eyJraWQiOiJWc2kyU0l0SkpvN1pzekRzWWNDUWlUU1wvTFJQeHEyVW50MDc5ZjNSSTFpbz0iLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI3M29hbTBpZWwxbjl0Zjk4Y24wMGxqYTY2MiIsInRva2VuX3VzZSI6ImFjY2VzcyIsInNjb3BlIjoiaHR0cHM6XC9cL2FwaS5pcGF1c3RyYWxpYS5nb3YuYXVcL2IyYlwvaXByaWdodHNcL2FnZW50IiwiYXV0aF90aW1lIjoxNTQ2NDg1MjA1LCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAuYXAtc291dGhlYXN0LTIuYW1hem9uYXdzLmNvbVwvYXAtc291dGhlYXN0LTJfV3pLa2FnemhQIiwiZXhwIjoxNTQ2NDg4ODA1LCJpYXQiOjE1NDY0ODUyMDUsInZlcnNpb24iOjIsImp0aSI6ImRmYzliYTlkLWYzYzUtNDA4YS04Y2Y5LTA1ZmNhZDhhN2JjZiIsImNsaWVudF9pZCI6Ijczb2FtMGllbDFuOXRmOThjbjAwbGphNjYyIn0.nl-Gncp1uBK1qvOMCQY45oA8ekgu6q77SfOU94xeC1lkqM_dX4sASxIw5IambMd6ofJwXG5pz3Fw-zx4td_h36Wj4tIEdUkbk9Z2xadw8HNndBiPxqjrB3VfjkL22Dq7Tf0HCxJ3zQ_NSGb_5gbTA2RHx2lS7Z0qDGgk8JsriauZ4p-s-XALDjsvmaCAm_uQ67XSB1t3SRsUI9DFMA0974Cnn3a_lmyZ7WpgIEVhFffjTmBhiD2p6c8Jg2Mc6Beas9zLUgPtR8aNrHddCuzkXf19Q7s3C0c7hurPRMnfQcnKODYaLrWv9a56ZtNnmkuzMm_W3dcSXWyLH-3eFxgrrA

{

   "submitterReference" : "MyPatentRenewal",

   "numOfYearsToRenew" : "1"

}
```








