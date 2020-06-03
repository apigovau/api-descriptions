# Getting Started

The Superannuation Dashboard API will let you find out information about the status of the Superannuation supporting services.

## Key Information



### Base URL

All URLs referenced in this documentation have the following base:

> http://sses.status.ato.gov.au/api_get/



### Response Format

Responses are in JSON


## OpenAPI Specification

The OpenAPI / Swagger documentation for the API is [here](/swagger-ui/index.html?url=https://api.gov.au/swagger/superannuation-dashboard.yaml)

Here's an automatically generated class diagram of the service.

[![Generated class diagram from swagger](https://graph.ausdx.io/swagger.svg?url=https://api.gov.au/swagger/superannuation-dashboard.yaml)](https://graph.ausdx.io/swagger.svg?url=https://api.gov.au/swagger/superannuation-dashboard.yaml)

Click for bigger version

## Authentication

There is no authentication

# Using the API



## Epoch Time

All times and dates in this API are output in epoch time format.

Epoch time (also known as POSIX time or Unix time) is a system for describing instants in time, defined as the number of seconds that have elapsed since 00:00:00 Coordinated Universal Time (UTC), Thursday, 1 January 1970, not counting leap seconds.

It is valuable for developers as it automatically incorporates daylight savings time and time zones. 

There are several functions in most programming languages for converting epoch time into a human readable format.

For more information on epoch time please visit [wikipedia](https://en.wikipedia.org/wiki/Unix_time) 

# Planned Outages

The current planned outages are available at this URL

> http://sses.status.ato.gov.au/api_get/plannedOutage 

Here is an example output from the plannedOutage service:

```json
{
 "preview": false,
 "init_offset": 0,
 "messages": [
 
 ],
 "fields": [
 "all_services",
 "end_date",
 "message",
 "start_date"
 ],
 "rows": [
 [
 "SuperTICK1",
 "SuperTICK2",
 "SuperTICK3",
 "SuperMATCH2",
 "EmployerTICK",
 "SuperMATCH",
 "FVS",
 "EPF"
 ],
 "1482127200",
 "Services may become available sooner(from 12 / 02 / 2017 3: 50 am onwards)",
 "1482105720"
 ]
}
```


## Fields


Here are the response fields, and what they mean:

| Filed | Description | Sample |
| ------------- |-------------| -----|
| all_services | Lists all the services that are currently undergoing a planned outage. | <ul><li>SuperTick1</li><li>SuperTick2</li><li>SuperTick3</li><li>SuperMatch2</li></ul> The following services are not available <ul><li>EmployerTick</li><li>SuperMatch</li><li>FVS</li><li>EPF</li></ul> |
| end_date | The end date of the planned outage in epoch time | 1482127200 |
| message | 	The message that accompanies the planned outage | Services may become available sooner (from 12/02/2017 3:50 am onwards) |
| start_date | The start date of the planned outage, in epoch time | 1482105720 |


## Multiple outages

Multiple outages look like this:

```json
{
 "preview": false,
 "init_offset": 0,
 "messages": [
 
 ],
 "fields": [
 "all_services",
 "end_date",
 "message",
 "start_date"
 ],
 "rows": [
 [
 [
 "SuperTICK1",
 "SuperTICK2",
 "SuperTICK3",
 "SuperMATCH2",
 "EmployerTICK",
 "SuperMATCH",
 "FVS",
 "EPF"
 ],
 "1482127200",
 "planned outagetest",
 "1482105840"
 ],
 [
 [
 "SuperTICK1",
 "SuperTICK2",
 "SuperTICK3",
 "SuperMATCH2",
 "EmployerTICK",
 "SuperMATCH",
 "FVS",
 "EPF"
 ],
 "1482127200",
 "testing",
 "1482124740"
 ]
 ]
}
```

## No outages

When no outages exist, the response will look like this:

```json
{
 "preview": false,
 "init_offset": 0,
 "post_process_count": 0,
 "messages": [],
 "fields": [],
 "rows": []
}
```

# Announcements

Announcements are available at this URL

> http://sses.status.ato.gov.au/api_get/announcement 

Here is an example output from the announcement service:

```json
{
	"preview": false,
	"init_offset": 0,
	"messages": [],
	"fields": ["datePosted", "message", "title"],
	"rows": [
		["1482066000", "Drive safely", "Happy Holidays"]
	]
}
```


## Fields

Here are the fields used in the announcements service:


| Filed | Description | Sample |
| ------------- |-------------| -----|
| datePosted | The date that the announcement was posted, in epoch time | 1482127200 |
| message | The message that accompanies the announcement | Drive Safely |
| title | The title of the announcement | Happy Holidays |


## Multiple announcements

Multiple announcements look like this:

```json
{
	"preview": false,
	"init_offset": 0,
	"messages": [],
	"fields": ["datePosted", "message", "title"],
	"rows": [
		["1482112020", "Drive safely", "Happy Holidays"],
		["1482124200", "Enjoy the break", "Happy New Year"]
	]
}
```

## No announcements

No announcements look like this:

```json
{
	"preview": false,
	"init_offset": 0,
	"post_process_count": 0,
	"messages": [],
	"fields": [],
	"rows": []
}
```

# Current Service Status

Current services status is available at this URL

> http://sses.status.ato.gov.au/api_get/currentStatus

Here is an example output:


```json
{
	"preview": false,
	"init_offset": 0,
	"messages": [],
	"fields": ["Service_Name", "Status", "time"],
	"rows": [
		["SuperMATCH2", "Operational", "1486610400"],
		["SuperTICK1", "Operational", "1486610400"],
		["SuperTICK2", "Operational", "1486610400"],
		["SuperTICK3", "Operational", "1486610400"]
	]
}
```


## Fields

Here are the response fields, and what they mean:

| Filed | Description | Sample |
| ------------- |-------------| -----|
| service_name | Specifies the services for which status is available | <ul><li>SuperTick1</li><li>SuperTick2</li><li>SuperTick3</li><li>SuperMatch2</li></ul> |
| status | Specifies the current status of the service | Operational |
| time | The current epoch time for the service's status | 1482105720 |


## No Status

When there are no statuses available, the response will look like this:

```json
{
	"preview": false,
	"init_offset": 0,
	"post_process_count": 0,
	"messages": [],
	"fields": [],
	"rows": []
}
```

