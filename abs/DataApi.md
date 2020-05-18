# Getting Started

**This service is currently in Beta.**
The statistical data available via this API may not be the latest. For the most up to date information visit the [ABS website](https://www.abs.gov.au/).

The ABS Data REST API (Beta) allows you request detailed ABS statistics including economic, social and Census data.

Customise your query to return only the data and metadata you are interested in, in the format you want.



## Key Information

### Base URL

This service only responds to a single `GET` method:

> nsi-stable-siscc.redpelicans.com/rest


### Response Format

Data is available in XML, JSON and CSV.  If no format is selected the API will return XML.

Metadata is primarily available in XML.  Some metadata is also available in JSON.

ABS Data API (Beta) is fully compliant with SDMX 2.1 - the Statistical Data and Metadata Exchange information model. The SDMX-REST Guide is available on [GitHub](https://github.com/sdmx-twg/sdmx-rest/wiki).


## OpenAPI Specification

The OpenAPI / swagger definition for this service is [here](https://api.gov.au/swagger-ui/index.html?url=https://api.gov.au/github?path%3dabs~DataAPI.openapi.yaml#/Get_Data/GetData)

Here's an automatically generated class diagram of the service.

[![Generated class diagram from swagger](https://api.gov.au/graph/swagger.svg?url=https://api.gov.au/github?path%3dabs~DataAPI.openapi.yaml)](https://api.gov.au/graph/swagger.svg?url=https://api.gov.au/github?path%3dabs~DataAPI.openapi.yaml)

Click for bigger version


### Authentication

This is a public and unauthenticated service.


# Using the API

ABS Data API (Beta) offers two modes of operation:
-	Data retrieval, where users know the data they want to retrieve (eg. Unemployment rate, monthly trend estimate)
-	Data discovery, where, using a metadata-driven approach, users need to discover the data exposed by the web service.

## GET Data

Data is returned using the following syntax:
> /data/{dataflowIdentifier}/{dataKey}?startPeriod={date value}&endPeriod={date value}&detail={value}&dimensionAtObservation={value}

### Path Parameters

**dataflowIdentifier**

A Dataflow in SDMX is the artefact used to request data. 

The syntax is the identifier of the agency maintaining the dataflow, followed by the identifier of the dataflow, followed by the dataflow version, separated by commas. {agencyId},{dataflowId},{version} eg. "ABS,CPI,1.0.0".

Version and agencyId are optional. If agencyId is not specified it will default to “all”. If version is not specified it will default to “latest.”

A list of all available dataflows can be returned using:
> GET /dataflow/{agencyId}

**dataKey**

Data in the ABS Data API is structured in multi-dimensional datasets.  The combination of dimensions and dimension members allows statistical data to be uniquely identified. Such a combination is known as a data key or series key and is used to filter data in a query.

Use "all" if you don't want to filter data. Note that this may return large data responses and can cause the API to time out. 

The syntax is explained in the following worked example. Let’s say a dataset for property prices has three dimensions: Measure, Region and Frequency. Each of these dimensions has dimension members identified using codes. Measure dimension has a member Mean Property Price (code: M1), Region has member Australia (code: AUS) and Frequency has Quarterly (code Q). The data key is one or more codes for each dimension, separated by a dot (dimensions must be in the order they are defined in the data structure). The series key for the example above is: “M1.AUS.Q”

Wildcarding is supported by omitting all codes for the dimension to be wildcarded. Eg. to get data for all regions: “M1..Q”

The OR operator is supported using the + character. Eg. to get two Measure dimension members: “M1+M2.AUS.Q”

You can combine wildcarding and the OR operator. Eg. “M1+M2..Q”

### Query Parameters

**startPeriod and endPeriod**

It is possible to define a date range for which observations should be returned by using the startPeriod and/or endPeriod parameters. The values should be given according to the syntax defined in ISO 8601 or as SDMX reporting periods. The values will vary depending on the frequency. Start and end dates are inclusive.

StartPeriod and endPeriod are optional. If no startPeriod is provided, data will be returned from the earliest period available. If no endPeriod is provided data will be returned to the latest period available. 

The supported formats are:
-	**YYYY** for annual data (e.g.: 2019).
-	**YYYY-S[1-2]** for semi-annual data (e.g.: 2019-S1).
-	**YYYY-Q[1-4]** for quarterly data (e.g.: 2019-Q1).
-	**YYYY-MM** for monthly data (e.g.: 2019-01).
-	**YYYY-W[01-53]** for weekly data (e.g.: 2019-W01).
-	**YYYY-MM-DD** for daily and business data (e.g.: 2019-01-01).

**detail**

Using the detail parameter, it is possible to specify the desired amount of information to be returned by the web service. Possible options are:
-	**full**: The data - series and observations - and the attributes will be returned. This is the default.
-	**dataonly**: The attributes will be excluded from the returned message.
-	**serieskeysonly**: Only the series, but without the attributes and the observations, will be returned. This can be useful for performance reasons, to return the series that match a certain query, without returning the actual data.
-	**nodata**: The series, including the attributes, will be returned, but the observations will not be returned.

**dimensionAtObservation**

Using the dimensionAtObservation parameter, you can define the way the data should be organised in the returned message. Possible options are: 
-	**TIME_PERIOD**: This will return a timeseries view of the data. This is the default value.
-	**AllDimensions**: This will return a flat view of the data, with no groupings.
-	**The ID of any other dimension**: This will return a cross-sectional view of the data.

## GET Metadata

Coming soon
