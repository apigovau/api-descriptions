---
name : "Trade Mark Goods and Services API"
description : "The Trade Mark Goods and Services (TMGnS) API provides services to query TMGnS reference data"
logo : "https://api.gov.au/img/ip_australia_tm_logo.svg"
tags:
 - "Security:oAuth2"
 - "Technology:Rest/JSON"
 - "OpenAPISpec:RAML"
 - "AgencyAcr:IP Australia"
 - "Status:Published"
 - "Category:TradeMarks"
---

# Getting Started

The Trade Mark Goods and Services (TMGnS) API provides services to query TMGnS reference data including Trade Mark classifications and Goods and Services descriptions.

For more details, and to request access to the API please visit the [IP Australia Developer Portal.](https://portal.api.ipaustralia.gov.au/s/)

## Key Information

### Base URL

**TEST** 

The base URL for the Test instance of TMGnS REST API is:

>https://test.api.ipaustralia.gov.au/public/tmgns-rest-api/v1/

**PRODUCTION**

The base URL for the Production instance TMGnS REST API is:

>https://production.api.ipaustralia.gov.au/public/tmgns-rest-api/v1/

------


### Requests

The TMGnS API has many endpoints basic details about each endpoint have been outlined below, for full details, including example request and response payloads, please view the TMGnS REST API on the [IP Australia Developer Portal.](https://portal.api.ipaustralia.gov.au/s/)

#### Get Trade Mark Class by ID

Retrieves a TradeMarkClass given its id.

Request Path: /tradeMarkClasses/{id}
Method: GET

------

#### Search for Goods and Services Descriptions

This endpoint allows users to search for Goods and Services Descriptions

Request Path: /goodsAndServicesDescriptions
Method: GET

#### Create a new Goods and Services Description

This endpoint allows users to create a new Goods and Services Description

Request Path: /goodsAndServicesDescriptions
Method: POST

#### Retrieve a Goods and Services Description

This endpoint allows users to retrieve a Goods and Services description by its ID.

Request Path: /goodsAndServicesDescriptions/{id}
Method: GET

#### Retrieve all Goods and Services Descriptions

This endpoint allows users to download a file containing all of the Goods and Service Picklist data.

Request Path: /gsDescriptionsFull
Method: GET

### Response Format

All responses for the TMGnS API are in JSON besides /gsDescriptionsFull, which downloads a .json file.

### Contact Us

If you require further information or assistance regarding IP Australia’s APIs, please contact us via:
- Email: MDB-TDS@ipaustralia.gov.au
- Website: [https://www.ipaustralia.gov.au/api-transaction-channel](https://www.ipaustralia.gov.au/api-transaction-channel)
- Phone: 1300 65 10 10
- International Phone: +61 2 6283 2999


# Authentication

## Overview
TMGnS API utilises Client ID enforcement as it's authentication protocol.



