# Getting Started

<div>
  </br>
<div>
<div style="background-color: #fad2c0; padding-top: 20px; padding-right: 20px; padding-bottom: 20px; padding-left: 20px">
  
  <h2>About the Beta</h2>
  
  <p>This beta release allows you to preview ABS Indicator API before it is released in its final form and gives you the opportunity to provide the ABS with feedback as we work to enhance the service.</p>
  
  <p>We will continue to load new datasets and update existing datasets as soon as possible after embargo on the data is lifted. However, <b>data in this beta release may not necessarily be the most up to date.</b> For the most up to date information visit the <a href="https://www.abs.gov.au/">ABS website</a>.</p>
  
  <p>Availability of the ABS Indicator API (Beta) is not guaranteed. The service may be subject to change.</p>
  
  <p>You can contact the ABS APIs team at <a href="mailto:api.data@abs.gov.au">api.data@abs.gov.au</a>. Please let us know any feedback or issues you have. You can request to join our register of interest to be notified of any changes in the API.  The <a href="https://www.abs.gov.au/websitedbs/D3310114.nsf/Home/Privacy?opendocument#from-banner=GB" target="_blank">ABS privacy policy</a> outlines how the ABS handles any personal information that you provide to us.</p>
  <div>

## Key Information

The ABS Indicator REST API (Beta) allows you to request headline economic statistics from the ABS including Australia's key economic indicators. 

All datasets are small, containing only the most in-demand data, so responses are returned as fast as possible.  If you wish to request full economic datasets you can do so using the ABS Data API (Beta).  

The ABS Indicator API uses the Statistical Data and Metadata Exchange (SDMX) standard.  Data is available in XML, JSON and CSV.

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

