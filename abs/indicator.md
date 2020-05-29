# Getting Started

**This service is currently in Beta.**
The statistical data available via this API may not be the latest.  For the most up to date information visit the [ABS website](https://www.abs.gov.au/).

The ABS Indicator REST API (Beta) allows you to request headline economic statistics from the ABS including Australia's key economic indicators. 

All datasets are small, containing only the most in-demand data, so responses are returned as fast as possible.  If you wish to request full economic datasets you can do so using the ABS Data API (Beta).  

The ABS Indicator API uses the Statistical Data and Metadata Exchange (SDMX) standard.  Data is available in XML, JSON and CSV.



## Key Information

### Base URL

This service only responds to a single `GET` method:

> https://indicator.data.abs.gov.au

### Response Format

Data is available in XML, JSON and CSV.  If no format is selected the API will return XML.

Metadata is available in XML and JSON.

### Authentication

Access to this API is managed by API Key.  Details about API Keys will be available soon.

## OpenAPI Specification

The OpenAPI / swagger definition for this service is [here](https://api.gov.au/swagger-ui/index.html?url=https://api.gov.au/github?path%3dabs~indicator.openapi.yaml)

Here's an automatically generated class diagram of the service.

[![Generated class diagram from swagger](https://api.gov.au/graph/swagger.svg?url=https://api.gov.au/github?path%3dabs~indicator.openapi.yaml)](https://api.gov.au/graph/swagger.svg?url=https://api.gov.au/github?path%3dabs~indicator.openapi.yaml)

Click for bigger version


# Using the API

ABS Indicator API (Beta) offers two modes of operation:
-	Data retrieval, where users know the data they want to retrieve (eg. Unemployment rate, monthly trend estimate)
-	Data discovery, where, using a metadata-driven approach, users need to discover the data exposed by the web service.

## GET Data

**/data/{dataflowIdentifier}**

A Dataflow in SDMX is the artefact used to request data.  In the Indicator API, dataflow ID is a single string (e.g. CPI). 

A list of all available dataflows and their IDs can be returned using:
> GET /{dataflows} operation.

The ABS Indicator API will not accept any custom data queries or query parameters.  The GET Data method will return one standard data response for each dataflow.

Response format can be specified as XML, JSON or CSV using the "accept" header.  XML is the default.
- accept: application/xml
- accept: application/json
- accept: text/csv

## GET Metadata

### GET dataflows

**/dataflows**

Return a list of all dataflows available and information about them including ID, name, version and structure. 

Response format can be specified as XML or JSON using the "accept" header.  XML is the default.
-	accept: application/xml
-	accept: application/json

### GET metadata

**/metadata/{dataflowIdentifier}**

Return structural metadata for the specified dataflow including ID, name, version and structure. 

Response format can be specified as XML or JSON using the "accept" header.  XML is the default.
-	accept: application/xml
-	accept: application/json

XML responses include more detailed information about the dataflow and its underlying data structure definition.  Information is provided on Codlists and Concept Schemes which combine to define the dimensions of the data structure.

Codelists provide a list of codes used to define data in the dataflow.  Each code has an ID and a name.  Codes may also have a parent ID which defines a hierarchy within the codelist.

Concept Schemes are groups of related Concepts.  Concepts are associated with all artefacts in the data structure, dimensions, annotations, etc., and define what each artefact is and how it is used.  Concepts include an ID, name and description.

