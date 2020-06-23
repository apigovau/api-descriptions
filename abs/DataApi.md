# Getting Started

<div>
  </br>
<div>
<div style="background-color: #fad2c0; padding-top: 20px; padding-right: 20px; padding-bottom: 20px; padding-left: 20px">
  
  <h2>About the Beta</h2>
  
  <p>This beta release allows you to preview ABS Data API before it is released in its final form and gives you the opportunity to provide the ABS with feedback as we work to enhance the service.</p>
  
  <p>We will continue to load new datasets and update existing datasets as soon as possible after embargo on the data is lifted. However, <b>data in this beta release may not necessarily be the most up to date.</b> For the most up to date information visit the <a href="https://www.abs.gov.au/">ABS website</a>.</p>
  
  <p>Availability of the ABS Data API (Beta) is not guaranteed. The service may be subject to change.</p>
  
  <p>You can contact the ABS APIs team at <a href="mailto:api.data@abs.gov.au">api.data@abs.gov.au</a>. Please let us know any feedback or issues you have. You can request to join our register of interest to be notified of any changes in the API.  The <a href="https://www.abs.gov.au/websitedbs/D3310114.nsf/Home/Privacy?opendocument#from-banner=GB" target="_blank">ABS privacy policy</a> outlines how the ABS handles any personal information that you provide to us.</p>
  </div>

## Key Information

The ABS Data REST API (Beta) allows you request detailed ABS statistics including economic, social and Census data.

Customise your query to return only the data and metadata you are interested in, in the format you want.

<div>
  </br>
<p class="intro__button"><a class="au-btn au-btn--secondary" href="https://api.gov.au/swagger-ui/index.html?url=https://api.gov.au/github?path%3dabs~DataAPI.openapi.yaml" target="_blank">Try it out</a></p>
</div>

### Base URL

This service only responds to a single `GET` method:

> https://api.data.abs.gov.au


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

Metadata is returned using the following syntax:
> /{structureType}/{agencyId}/{structureId}/{structureVersion}? references={reference value}& detail={level of detail}

Only Structure Type and Agency ID are mandatory.

### Path Parameters

**Structure Type**

The type of metadata you want to retrieve. Available structures:
-	datastructure
-	dataflow
-	codelist
-	conceptscheme
-	categoryscheme
-	contentconstraint
-	actualconstraint
-	agencyscheme
-	categorisation
-	hierarchicalcodelist

**agencyId**

The ID of the agency maintaining the structures. All structures in this API are currently maintained by the Australian Bureau of Statistics with Agency ID **ABS**.

**structureId**

The ID of the structure you are requesting. “all” will return all available structures of the specified structure type. If no structure ID is given, all is the default. 

**structureVersion**

The version of the structure to retrieve. Three numbers separated by points, eg. "1.0.0".

If no version is given then the latest version available will be returned.

### Query Parameters

**references**

Instruct the API to return (or not) artefacts referenced by the artefact(s) you are querying. Eg. the codelists and concepts used by the data structure you are querying. You can also retrieve the artefacts that use the artefact you are querying, eg. the dataflows that use the data structure queried.

-	**none**: No references will be returned. This is the default.
-	**parents**: The artefacts that use the artefact matching the query (for example, the dataflows that use the data structure definition matching the query) will be returned.
-	**parentsandsiblings**: The artefacts that use the artefact matching the query, as well as the artefacts referenced by these artefacts will be returned.
-	**children**: The artefacts referenced by the matching artefact will be returned (for example, the concept schemes and code lists used in a DSD).
-	**descendants**: References of references, up to any level, will also be returned.
-	**all**: The combination of parentsandsiblings and descendants.

In addition, a specific structure type may also be used (e.g. codelist, dataflow, etc.).

**detail**

Specify the desired amount of detail to be returned. For example, it is possible to instruct the web service to return only basic information about the resource, this is known in SDMX as a stub.

-	**allstubs**: All artefacts will be returned as stubs.
-	**referencestubs**: The referenced artefacts will be returned as stubs.
-	**referencepartial**: The referenced item schemes should only include items used by the artefact to be returned. For example, a concept scheme would only contain the concepts used in a DSD, and its isPartial flag would be set to true. As another example, if a dataflow is constrained, then the codelists returned should only contain the subset of codes allowed by that constraint.
-	**allcompletestubs**: All artefacts should be returned as complete stubs, containing identification information, the artefacts' name, description, annotations and isFinal information.
-	**referencecompletestubs**: The referenced artefacts should be returned as complete stubs, containing identification information, the artefacts' name, description, annotations and isFinal information.
-	**full**: All available information for all artefacts will be returned. This is the default.


<style>
.alert-warning {
  color: rgb(138,109,59) !important;
}
</style>
