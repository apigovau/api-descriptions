# Getting Started

<div>
  </br>
<div>
<div style="background-color: #fad2c0; padding-top: 20px; padding-right: 20px; padding-bottom: 20px; padding-left: 20px">
  
  <h2>About the Beta</h2>
  
  <p>This beta release allows you to preview ABS Data API before it is released in its final form and gives you the opportunity to provide the ABS with feedback as we work to enhance the service.</p>
  
  <p>We will continue to load new datasets and update existing datasets as soon as possible after embargo on the data is lifted. However, <b>data in this beta release may not necessarily be the most up to date.</b> For the most up to date information visit the <a href="https://www.abs.gov.au/">ABS website</a>.</p>
  
  <p>Availability of the ABS Data API (Beta) is not guaranteed. The service may be subject to change.</p>
  
  <p>If you have any issues with the ABS Data API, you can ask for help and provide feedback through  our <a href="https://community.digital.gov.au/c/api/Discuss-get-help-or-provide-feedback-for-APIs-from-the-Australian-Bureau-of-Statistics/133">API Community of Practice</a>.</p>
  
  <p>Contact the ABS APIs team at <a href="mailto:api.data@abs.gov.au">api.data@abs.gov.au</a>. You can request to join our register of interest to be notified of any changes in the API. The <a href="https://www.abs.gov.au/websitedbs/D3310114.nsf/Home/Privacy?opendocument#from-banner=GB" target="_blank">ABS privacy policy</a> outlines how the ABS handles any personal information that you provide to us.</p>
  </div>

## Key Information

The ABS Data REST API (Beta) allows you request detailed ABS statistics including economic, social and Census data.

Customise your query to return only the data and metadata you are interested in, in the format you want.

<div style="margin-top: 22px;>
  <p class="intro__button">
    <a class="au-btn au-btn--secondary" href="https://api.gov.au/swagger-ui/index.html?url=https://api.gov.au/github?path%3dabs~DataAPI.openapi.yaml" target="_blank">Try it out</a>
  </p>
</div>

### Base URL

This service only responds to a single `GET` method:

> https://api.data.abs.gov.au


### Response Format

Data is available in XML, JSON and CSV.  If no format is selected the API will return XML.

Metadata is primarily available in XML.  Some metadata is also available in JSON.

ABS Data API (Beta) is fully compliant with SDMX 2.1 - the Statistical Data and Metadata Exchange information model. The SDMX-REST Guide is available on [GitHub](https://github.com/sdmx-twg/sdmx-rest/wiki).

You can specify the response in the API URL using the "format" query parameter. E.g: https://api.data.abs.gov.au/data/jv/all?startPeriod=2020&format=jsondata 
- XML is returned by default
- Structure specific XML (good for time series): "format=structurespecificdata" 
- JSON: "format=jsondata" 
- CSV: "format=csv"


You can also use the "accept" header to specify the response format as a header when you make an API call. 
E.g: "accept: application/xml"
- XML: application/xml
- Structure specific XML: application/vnd.sdmx.structurespecificdata+xml
- JSON: application/vnd.sdmx.data+json
- CSV: text/csv
- CSV: application/vnd.sdmx.data+csv
- CSV with labels for codelists: application/vnd.sdmx.data+csv;labels=both
- CSV file with labels for codelists: application/vnd.sdmx.data+csv;file=true;labels=both




## OpenAPI Specification

The OpenAPI / swagger definition for this service is [here](https://api.gov.au/swagger-ui/index.html?url=https://api.gov.au/github?path%3dabs~DataAPI.openapi.yaml#/Get_Data/GetData)

Here's an automatically generated class diagram of the service.

[![Generated class diagram from swagger](https://api.gov.au/graph/swagger.svg?url=https://api.gov.au/github?path%3dabs~DataAPI.openapi.yaml)](https://api.gov.au/graph/swagger.svg?url=https://api.gov.au/github?path%3dabs~DataAPI.openapi.yaml)

Click for bigger version


### Authentication

Unauthenticated access to the ABS Data API is allowed for the purposes of investigation and testing.

If you decide to use the ABS Data API for a system or application, we strongly encourage you to register for an API key. The key will allow you to make API requests without being impacted by other users of the system.  If you have issues, it will be easier for us to investigate them.

If you wish to register for an API key, please contact us at [api.data@abs.gov.au](mailto:api.data@abs.gov.au).  Providing your name or organisation name and a brief description of what you will use the API for.  It may take a few days for us to respond. 

If you have been issued an API Key, you need to add it as a header in your API requests: “x-api-key: xxxxxxxxxxxxxxx”

If you are using the linked Swagger user interface to test the API, click ‘Authorize’ enter your API Key and then click 'Authorize' again.



# Using the API

ABS Data API (Beta) offers two modes of operation:
-	Data retrieval, where users know the data they want to retrieve (eg. Unemployment rate, monthly trend estimate).
-	Data discovery, where, using a metadata-driven approach, users need to discover the data exposed by the web service.

## GET Data

Data is returned using the following syntax:
`/data/{dataflowIdentifier}/{dataKey}?startPeriod={date value}&endPeriod={date value}&detail={value}&dimensionAtObservation={value}`

### Path Parameters

#### dataflowIdentifier

A Dataflow in SDMX is the artefact used to request data. 

The syntax is the identifier of the agency maintaining the dataflow, followed by the identifier of the dataflow, followed by the dataflow version, separated by commas. `{agencyId},{dataflowId},{version}` eg. "ABS,CPI,1.0.0".

Version and agencyId are optional. If agencyId is not specified it will default to “all”. If version is not specified it will default to “latest.”

A list of all available dataflows can be returned using:
`GET /dataflow/{agencyId}`

#### dataKey

Data in the ABS Data API is structured in multi-dimensional datasets.  The combination of dimensions and dimension members allows statistical data to be uniquely identified. Such a combination is known as a data key or series key and is used to filter data in a query.

Use "all" if you don't want to filter data. Note that this may return large data responses and can cause the API to time out. 

The syntax is explained in the following worked example. Let’s say a dataset for property prices has three dimensions: Measure, Region and Frequency. Each of these dimensions has dimension members identified using codes. Measure dimension has a member Mean Property Price (code: M1), Region has member Australia (code: AUS) and Frequency has Quarterly (code Q). The data key is one or more codes for each dimension, separated by a dot (dimensions must be in the order they are defined in the data structure). The series key for the example above is: “M1.AUS.Q”

Wildcarding is supported by omitting all codes for the dimension to be wildcarded. Eg. to get data for all regions: “M1..Q”

The OR operator is supported using the + character. Eg. to get two Measure dimension members: “M1+M2.AUS.Q”

You can combine wildcarding and the OR operator. Eg. “M1+M2..Q”

### Query Parameters

#### startPeriod and endPeriod

It is possible to define a date range for which observations should be returned by using the startPeriod and/or endPeriod parameters. The values should be given according to the syntax defined in ISO 8601 or as SDMX reporting periods. The values will vary depending on the frequency. Start and end dates are inclusive.

StartPeriod and endPeriod are optional. If no startPeriod is provided, data will be returned from the earliest period available. If no endPeriod is provided data will be returned to the latest period available. 

The supported formats are:
-	**YYYY** for annual data (e.g.: 2019).
-	**YYYY-S[1-2]** for semi-annual data (e.g.: 2019-S1).
-	**YYYY-Q[1-4]** for quarterly data (e.g.: 2019-Q1).
-	**YYYY-MM** for monthly data (e.g.: 2019-01).
-	**YYYY-W[01-53]** for weekly data (e.g.: 2019-W01).
-	**YYYY-MM-DD** for daily and business data (e.g.: 2019-01-01).

#### detail

Using the detail parameter, it is possible to specify the desired amount of information to be returned by the web service. Possible options are:
-	**full**: The data - series and observations - and the attributes will be returned. This is the default.
-	**dataonly**: The attributes will be excluded from the returned message.
-	**serieskeysonly**: Only the series, but without the attributes and the observations, will be returned. This can be useful for performance reasons, to return the series that match a certain query, without returning the actual data.
-	**nodata**: The series, including the attributes, will be returned, but the observations will not be returned.

#### dimensionAtObservation

Using the dimensionAtObservation parameter, you can define the way the data should be organised in the returned message. Possible options are: 
-	**TIME_PERIOD**: This will return a time series view of the data. This is the default value.
-	**AllDimensions**: This will return a flat view of the data, with no groupings.
-	**The ID of any other dimension**: This will return a cross-sectional view of the data.

### Format

Response format can be specified as XML, JSON or CSV using the "accept" header.  XML is the default.
- XML (default): application/xml
- Structure specific XML (good for time series): application/vnd.sdmx.structurespecificdata+xml
- JSON: application/vnd.sdmx.data+json
- CSV: text/csv
- CSV: application/vnd.sdmx.data+csv
- CSV with labels for codelists: application/vnd.sdmx.data+csv;labels=both
- CSV file with labels for codelists: application/vnd.sdmx.data+csv;file=true;labels=both


## GET Metadata

Metadata is returned using the following syntax:
`/{structureType}/{agencyId}/{structureId}/{structureVersion}? references={reference value}& detail={level of detail}`

Only Structure Type and Agency ID are mandatory.

### Path Parameters

#### Structure Type

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

#### agencyId

The ID of the agency maintaining the structures. All structures in this API are currently maintained by the Australian Bureau of Statistics with Agency ID `ABS`.

#### structureId

The ID of the structure you are requesting. “all” will return all available structures of the specified structure type. If no structure ID is given, all is the default. 

#### structureVersion

The version of the structure to retrieve. Three numbers separated by points, eg. "1.0.0".

If no version is given then the latest version available will be returned.

### Query Parameters

#### references

Instruct the API to return (or not) artefacts referenced by the artefact(s) you are querying. Eg. the codelists and concepts used by the data structure you are querying. You can also retrieve the artefacts that use the artefact you are querying, eg. the dataflows that use the data structure queried.

-	**none**: No references will be returned. This is the default.
-	**parents**: The artefacts that use the artefact matching the query (for example, the dataflows that use the data structure definition matching the query) will be returned.
-	**parentsandsiblings**: The artefacts that use the artefact matching the query, as well as the artefacts referenced by these artefacts will be returned.
-	**children**: The artefacts referenced by the matching artefact will be returned (for example, the concept schemes and code lists used in a DSD).
-	**descendants**: References of references, up to any level, will also be returned.
-	**all**: The combination of parentsandsiblings and descendants.

In addition, a specific structure type may also be used (e.g. codelist, dataflow, etc.).

#### detail

Specify the desired amount of detail to be returned. For example, it is possible to instruct the web service to return only basic information about the resource, this is known in SDMX as a stub.

-	**allstubs**: All artefacts will be returned as stubs.
-	**referencestubs**: The referenced artefacts will be returned as stubs.
-	**referencepartial**: The referenced item schemes should only include items used by the artefact to be returned. For example, a concept scheme would only contain the concepts used in a DSD, and its isPartial flag would be set to true. As another example, if a dataflow is constrained, then the codelists returned should only contain the subset of codes allowed by that constraint.
-	**allcompletestubs**: All artefacts should be returned as complete stubs, containing identification information, the artefacts' name, description, annotations and isFinal information.
-	**referencecompletestubs**: The referenced artefacts should be returned as complete stubs, containing identification information, the artefacts' name, description, annotations and isFinal information.
-	**full**: All available information for all artefacts will be returned. This is the default.

### Format

Response format can be specified as XML or JSON using the "accept" header.  XML is the default.
-	accept: application/xml
- accept: application/vnd.sdmx.structure+json


# Understanding SDMX Data

The ABS Data API provides data in the Statistical Data and Metadata eXchange (SDMX) format.  The API is compliant with the SDMX version 2.1 Information Model. 

SDMX is an initiative that aims to foster common standards and guidelines for the exchange and sharing of statistical data and metadata, where the two are presented together, with an emphasis on aggregated data. Metadata gives context to the data exchanged, so information is immediately understandable and more useful than if it was presented without the relevant metadata. More information on the SDMX standard is available at [SDMX.org](https://sdmx.org/).

Data is in the ABS Data API is available in SDMX-ML (XML), SDMX-JSON and SDMX-CSV.


## SDMX-JSON

SDMX-JSON conforms to the JSON standard specification and supports the SDMX 2.1 Information Model.  This guide will focus on interpreting SDMX-JSON data responses, it has been adapted from the SDMX Technical Working Group’s [SDMX-JSON Field Guide](https://github.com/sdmx-twg/sdmx-json/blob/master/data-message/docs/1-sdmx-json-field-guide.md) which goes into more detail on the standard and its implementations.

### Overview

The most important concepts to understand are observations, dimensions, attributes, and annotations. 

-	Observations are the actual data points or numbers being measured.
-	Dimensions provide the structure of the dataset. Each observation is uniquely identified by the combination of one member from each dimension. 
-	Attributes don’t help to identify observations, but add useful additional information to them (like the unit of measure or the number of decimals). 
-	Annotations are similar to attributes in that they provide additional information. They can be referenced by any other SDMX object (including dimensions, attributes and observations).

Observations within a dataset can be grouped in different ways to assist in reading the data.  A grouping of observations is known as a series.  The most common way to group data is by the Time dimension (aka a time series). For example, the unemployment rate for Australia is measured each month and these measures can be grouped together into a time series. Similarly, you can group a collection of observations made at the same point in time, in a "cross-section". For example the unemployment rate for each state and territory for a single month (this would be a series grouped on the Region dimension). You can also return ungrouped data as a flat list of observations.  

Grouping by Time is the default in the ABS Data API.  

### SDMX-JSON Data Message Objects

#### message

Message is the top level object and it contains the data as well as the structural metadata needed to interpret that data.
-	meta - Contains information about the message.
-	data - The main part of the message containing observations and structural information.


#### meta

Provides meta-information about the message, such as when it was prepared.
-	id - a unique identifier for this response, the ID will change with every request even if you are calling the same data. 
-	test - true/false - indicates whether the message is for test purposes or not.
-	schema - URL to the schema to validate the message.
-	prepared - timestamp for when the message was prepared. Time zone is the sender’s location, for the ABS Data API this is Australian Eastern Standard Time.
-	content-languages - languages used in the massage.
-	sender - information about the sender including an ID and name.

```json
{
  "meta": {
    "schema": "https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/data-message/tools/schemas/1.0/sdmx-json-data-schema.json",
    "id": "IREF44b98b50333f442d9875d836628f18fc",
    "prepared": "2021-01-22T11:37:40Z",
    "test": true,
    "content-languages": [
      "en"
    ],
    "sender": {
      "id": "ABS",
      "name": "Australian Bureau of Statistics",
      "names": {
        "en": "unknown"
      }
    }
  },
```


#### data

The main part of the message containing observations and structural information
-	datasets - an array of dataSet objects. This is where the observations (i.e. the actual numbers) will be.
-	structure - contains the information needed to interpret the observations, such as lists of dimensions, attributes and annotations. 


#### structure

Provides the structural metadata necessary to interpret the data. The structure section gives you the dimensions, attributes and annotations used in the message. It also describes to which level in the hierarchy these are attached.
-	name - the name of the dataset you are viewing.
-	dimensions - describes the dimensions used in the message as well as the levels in the hierarchy (dataSet, series or observations) to which these dimensions are attached. 
-	attributes - describes the attributes used in the message as well as the levels in the hierarchy (dataSet, series or observations) to which these dimensions are attached.
-	annotations - an array of annotation objects that may be referred to by any other SDMX objects or components.


#### dimensions, attributes

Describes the dimensions/attributes used in the message as well as the levels in the hierarchy (dataSet, series or observations) to which these dimensions/attributes are attached.
-	dataSet - an array of components (dimensions/annotations) provided if dimensions or attributes are presented at the dataSet level. 
-	series - an array of components (dimensions/annotations) provided if dimensions or attributes are presented at the series level. If you call the ABS Data API for data as a series (e.g. time series) then all dimensions and attributes will be at the series level.
-	observation - an array of components (dimensions/annotations) provided if dimensions or attributes are presented at the observation level. If you call the ABS Data API for flat data `dimensionAtObservation=AllDimensions` then all dimensions and attributes will be at the observation level.


#### component (dimension/attribute)

The dimensions and attributes presented in the message are also called components. Each component contains basic information about the component (such as its name and id) as well as the list of component values used in the message. Each of the components may contain the following fields:
-	id - identifier for this dimension or attribute - unique within a data structure
-	name - human-readable name for the dimension or attribute
-	description - if used it will provided additional information about the dimension or attribute
-	keyPosition - number - always present for dimensions but not supplied for attributes. Indicates the position of the dimension in the Data Structure Definition, starting at 0. It is provided for all dimensions including Time. The information in this field is consistent with the order of dimensions in the "key" parameter string when requesting data from the API. 
-	roles - defines the role of the dimension or attribute. These are defined by the Concept applied to that dimension (more information on Concepts is available under Worked Examples). The role will often be the same as the dimension id but may be different to assist in interpreting the data. E.g. geographic dimensions that can used to map data may have the role of REGION even if the dimension id is something else.
-	relationship - always present for attributes but not supplied for dimensions. This relationship expresses the attachment level of the attribute as defined in the data structure definition. Depending on the message context (especially the data query) an attribute value can however be attached physically in the message at a different level.
-	values - an array of component values. These are the individual dimension members or attribute values.


#### component value (dimension member/attribute value)

An individual value for a given component. That is, dimension members for the given dimension or values for the given attribute.
-	id - identifier for this dimension member or attribute value - unique within this dimension or attribute.
-	name - human-readable name for this dimension member or attribute value.
-	description - if used it will provided additional information about the dimension member or attribute value.
-	order - an integer signifying the original order of this value within its codelist.  You can use order to reconstruct the component value hierarchy. E.g order allows you to display dimension members in the correct order when visualising data.  Note that the order of observations and component values in their array is not significant.
-	parent - the ID of the parent for this component value (if this value is part of a hierarchical codelist). The parent value will only be present in the message if it also has data present.
-	annotations - a collection of indices of the corresponding annotations for this component value. Indices refer to a position in the array of annotations in the structure field.


#### annotations

The annotations section contains an array of annotations that can be referenced by other SDMX objects such as structure, component, component value, dataSets, series and observations.

Annotations provide additional information about the objects that reference them.

When referencing an annotation, an SDMX object will specify a number corresponding to a position in the annotations array.  `0` is the first annotation in the array, `1` is the second and so on.
-	type - defines the use for this annotation. 
-	text - human-readable text of the annotation.
-	title - non-localised title for the annotation.  In the ABS Data API, this field is generally used by LAYOUT annotations to define IDs for dimensions and dimension members that can be used to construct a default table view of the data.


#### dataSets

This is where the data (i.e. the observations) will be. Typically, there should only be one dataSet in the message. 

There are between 2 and 3 levels in a dataSet object, depending on the way data in the message is organised. 

A dataSet may contain a flat list of observations. If this is the case, we have 2 levels in the data part of the message: the dataSet level and the observation level. A dataSet may also organise observations in logical groups called series. These groups can represent time series or cross-sections, see [Overview](#overview) for more information. 

Dimensions and attributes may be specified at any of these 3 levels. 

If the dataSet is a flat list of observations, observations will be found directly under a dataSet object. This structure has all dimensions at observation level.  To request data in this structure, you should specify `dimensionAtObservation=AllDimensions` as a query parameter. 

If the dataSet represents a time series or cross section, then observations will be found under the series objects.  If this is the case, we have 3 levels in the data part of the message: the dataSet level, the series level and the observation level with only one dimension at observation level.

dataSet properties are:
-	action - describes the intention of the data transmission from the sender's side. This will generally be `Information`.
-	links - a collection of links to additional information regarding the dataSet.
-	annotations - a collection of indices of the corresponding annotations for the dataSet. Indices refer to a position in the array of annotations in the structure field.
-	attributes - a collection of indices of the corresponding values of all attributes presented at the dataSet level. Each value is an index in the values array of the respective component object within the structure.attributes.dataSet array. ABS Data API does not typically defined attributes at dataset level.  If this is present, it indicates the attribute applies to all observations.
-	series - a collection of series objects, used when the observations contained in the dataSet are presented in logical groups (time series or cross-sections). E.g. when calling the API with the query parameter "dimensionAtObservation=TIME_PERIOD" (default) or with the "dimensionAtObservation" parameter set to the ID of any other dimension. The series element is not present if data has been requested as a flat view of observations.
-	observations - a collection of observations used in cases when a dataSet is presented as a flat view of observations, e.g. when calling the API with the query parameter "dimensionAtObservation=AllDimensions".

examples:

```json
        "action": "Information",
        "links": [
          {
            "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ABS:EXAMPLE(1.0.0)",
            "rel": "DataStructure"
          }
        ],
        "annotations": [ 0, 1, 2 ],
        "observations": {
             # observation object #
          }
```

```json
        "action": "Information",
        "links": [
          {
            "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ABS:EXAMPLE (1.0.0)",
            "rel": "DataStructure"
          }
        ],
        "annotations": [ 0, 1, 2 ],
        "series": {
             # series object #
          }

```


#### series

A collection of series objects, used when the observations contained in the dataSet are presented in logical groups (time series or cross-sections). Each underlying series is represented as a name/value pair in the series object.

A series is uniquely identified through the content of the name in the name/value pair otherwise known as the dimension key. This is the indices for the corresponding values of all dimensions presented at series level separated by a colon "":". See [dimension key](#dimension-key) for more information.

The value in the name/value pair is an object containing:
-	annotations - a collection of indices of the corresponding annotations for the series. Indices refer to a position in the array of annotations in the structure field.
-	attributes - collection of indices of the corresponding values of all attributes presented at the series level. Each value is an index in the values array of the respective component object within the structure.attributes.series array. This is used for attributes that have the same value for all observations in a series. When an attribute has no value for a specific series, then null is used instead of the index.
-	observations - collection of observations. Presented under the series object when observations are grouped as a time series or cross-section. E.g. when calling the API with the query parameter `dimensionAtObservation=TIME_PERIOD` (default) or with `dimensionAtObservation` set to the ID of any other dimension then the dimensionAtObservation dimension.


#### dimension key

Dimension keys link observation values (i.e. the actual data) to the dimensions and dimension members that give them meaning. Dimension keys are the series of numbers separated by colons `:` under `data.dataSets.series` or `data.dataSet.observations`. Each dimension key is uniquely describing an observation or a series of observations by combining one member from each dimension (except the dimension at observation level if the data is presented as a series).

There is one number in the key per dimension. The order of dimensions in the key is defined by the dimension `keyPosition` in the structure section of the message. The first dimension in the key is `"keyPosition": 0`, the second is `"keyPosition": 1`, and so on.  

The numbers themselves identify one dimension member for each dimension in the key. Dimension members are defined in the `values` array for that dimension in the structure section. The order of dimension members is the order they appear in the values array. A `0` in the dimension key means the first value in the array for that dimension, a `1` means the second value, and so on.

Example call: https://api.data.abs.gov.au/data/ABS,RES_DWELL/1.1+1GSYD+1RNSW.Q?detail=Full&startPeriod=2020-Q1&endPeriod=2020-Q2&format=jsondata 

This call returns two observations each for two time periods.  The data is presented as a time series (the default presentation).

As a CSV this data would be presented as follows, the first row is the header:
```csv
DATAFLOW,MEASURE,REGION,FREQ,TIME_PERIOD,OBS_VALUE,UNIT_MEASURE,UNIT_MULT,OBS_STATUS,OBS_COMMENT
ABS:RES_DWELL(1.0.0),1,1RNSW,Q,2020-Q1,10210,NUM,0,r,
ABS:RES_DWELL(1.0.0),1,1RNSW,Q,2020-Q2,9555,NUM,0,r,
ABS:RES_DWELL(1.0.0),1,1GSYD,Q,2020-Q1,10119,NUM,0,r,
ABS:RES_DWELL(1.0.0),1,1GSYD,Q,2020-Q2,9493,NUM,0,r,
```

In SDMX-JSON, using `dimensionAtObservation=TIME_PERIOD` (default), the observations are grouped by time series with the `TIME_PERIOD` dimension at observation level.  Dimension and attribute values are replaced by their indices:

```json
"series": {
          "0:0:0": {
                  "attributes": [0, 0],
                  "annotations": [],
                  "observations": {
                         "0": [10210, 0, null],
                         "1": [9555, 0, null]
                  }
          },
          "0:1:0": {
                  "attributes": [0, 0],
                  "annotations": [],
                  "observations": {
                         "0": [10119, 0, null],
                         "1": [9493, 0, null]
                  }
             }
        }
```

`0:0:0` is the dimension key (the indices for the dimension values).  There are three numbers because this response has three dimensions at the series level.  Looking at the structure call below you can see that the dimension with key position 0 is MEASURE, key position 1 is REGION and key position 2 is FREQ.

`[0,0]` are the indices for the attribute values.  There are two numbers because there are two attributes at the series level. These correspond with values in the attributes array similar to the dimension key.

```txt
series 1: 
    0:0:0 corresponds to the first value for all three dimensions: 
      “MEASURE:1”, “REGION: 1RNSW”, “FREQ:Q”
    The attributes for this series are “UNIT_MEASURE:NUM”, “UNIT_MULT:0"
    There are no annotations for the series
    This series has two observations:
      Observation 1:
          “0” corresponds to the first value of the dimension at observation-level “TIME_PERIOD: 2020-Q1”
          The value for this observation is 10210
          There are two attributes for this observation
          the first value for the first observation-level attribute “OBS_STATUS:r”
          no value for the second observation-level attribute “OBS_COMMENT:null”
      Observation 2:
          “1” corresponds to the second Time dimension value “TIME_PERIOD: 2020-Q2”
          The value for this observation is 9555
          The attributes for this observation are “OBS_STATUS:r”, “OBS_COMMENT:null” 

series 2:  
     0:1:0 corresponds to the three indices for “MEASURE:1”, “REGION: 1GSYD”, “FREQ:Q”
    The attributes for this series are “UNIT_MEASURE:NUM”, “UNIT_MULT:0"
    There are no annotations for the series
    This series has two observations:
      Observation 1:
          “0” corresponds to the first Time dimension value “TIME_PERIOD: 2020-Q1”
          The value for this observation is 10119
          The attributes for this observation are “OBS_STATUS:r”, “OBS_COMMENT:null”
      Observation 2:
          “1” corresponds to the second Time dimension value “TIME_PERIOD: 2020-Q2”
          The value for this observation is 9493
          The attributes for this observation are “OBS_STATUS:r”, “OBS_COMMENT:null” 
```

Here's the structure section for that data response:
```json
      "dimensions": {
        "dataset": [],
        "series": [
          {
            "id": "MEASURE",
            "name": "Measure",
            "names": {"en": "Measure"},
            "keyPosition": 0,
            "roles": ["MEASURE"],
            "values": [
              {
                "id": "1",
                "order": 0,
                "name": "Number of Established House Transfers",
                "names": {"en": "Number of Established House Transfers"}
              }
            ]
          },
          {
            "id": "REGION",
            "name": "Region",
            "names": {"en": "Region"},
            "keyPosition": 1,
            "roles": ["REGION"],
            "values": [
              {
                "id": "1RNSW",
                "order": 3,
                "name": "Rest of NSW",
                "names": {"en": "Rest of NSW"},
                "parent": "1"
              },
              {
                "id": "1GSYD",
                "order": 2,
                "name": "Greater Sydney",
                "names": {"en": "Greater Sydney"},
                "parent": "1"
              }
            ]
          },
          {
            "id": "FREQ",
            "name": "Frequency",
            "names": {
              "en": "Frequency"
            },
            "keyPosition": 2,
            "roles": [
              "FREQ"
            ],
            "values": [
              {
                "id": "Q",
                "order": 8,
                "name": "Quarterly",
                "names": {
                  "en": "Quarterly"
                }
              }
            ]
          }
        ],
        "observation": [
          {
            "id": "TIME_PERIOD",
            "name": "Time Period",
            "names": {"en": "Time Period"},
            "keyPosition": 3,
            "roles": ["TIME_PERIOD"],
            "values": [
              {
                "start": "2020-01-01T00:00:00Z",
                "end": "2020-03-31T00:00:00Z",
                "id": "2020-Q1",
                "name": "2020-Q1",
                "names": {"en": "2020-Q1"}
              },
              {
                "start": "2020-04-01T00:00:00Z",
                "end": "2020-06-30T00:00:00Z",
                "id": "2020-Q2",
                "name": "2020-Q2",
                "names": {"en": "2020-Q2"}
              }
            ]
          }
        ]
      },
      "attributes": {
        "dataSet": [],
        "series": [
          {
            "id": "UNIT_MEASURE",
            "name": "Unit of Measure",
            "names": {"en": "Unit of Measure"},
            "roles": ["UNIT_MEASURE"],
            "relationship": {
              "dimensions": ["MEASURE"]
            },
            "values": [
              {
                "id": "NUM",
                "order": 1,
                "name": "Number",
                "names": {"en": "Number"}
              }
            ],
            "annotations": [1]
          },
          {
            "id": "UNIT_MULT",
            "name": "Unit of Multiplier",
            "names": {
              "en": "Unit of Multiplier"
            },
            "roles": [
              "UNIT_MULT"
            ],
            "relationship": {
              "dimensions": [
                "MEASURE"
              ]
            },
            "values": [
              {
                "id": "0",
                "order": 0,
                "name": "Units",
                "names": {"en": "Units"}
              }
            ],
            "annotations": [2]
          }
        ],
        "observation": [
          {
            "id": "OBS_STATUS",
            "name": "Observation Status",
            "names": {"en": "Observation Status"},
            "roles": ["OBS_STATUS"],
            "relationship": {"primaryMeasure": "OBS_VALUE"},
            "values": [
              {
                "id": "r",
                "order": 7,
                "name": "revised",
                "names": {"en": "revised"}
              }
            ]
          },
          {
            "id": "OBS_COMMENT",
            "name": "Observation Comment",
            "names": {"en": "Observation Comment"},
            "roles": ["OBS_COMMENT"],
            "relationship": {"primaryMeasure": "OBS_VALUE"},
            "values": []
          }
        ]
      },
      "annotations": [
        {
          "type": "NonProductionDataflow",
          "text": "true",
          "texts": {"en": "true"}
        }
        {
          "type": "CONTEXT",
          "text": "If a unit multiplier exists the data is recorded according to the combination of the unit multiplier and the unit of measure.",
        },
        {
          "type": "CONTEXT",
          "text": "Codes for unit of multiplier are the exponent in base 10 so that multiplying the observation by 10^UNIT_MULT gives a value expressed in the unit of measure.",
        }
      ]
```


#### observations

A collection of observations. Each observation is represented as a name/value pair in the observations object.

An observation is uniquely identified through the content of the name in the name/value pair, which is the indices of the corresponding values of all dimensions presented at observation level (indices in the values array of the respective component object within the `structure.dimensions.observation` array) separated by a colon ":". There’s one single index per observation for time series and cross-section representations, but there will be more than one when the data are represented as a flat view of observations.

The value in the name/value pair is an array containing the observation value (first position), followed by the indices of the corresponding values of attributes presented at observation level up to the number of attributes defined at observation level, then the indices of the corresponding values of annotations of that observation, if any are present. Therefore, elements after the observation value are for the observation level attributes and for annotations of that observation. 

The data type for observation value is number or string. The data type for a reported missing observation value is a null. The index for an attribute is the corresponding index in the values array of the respective component object within the `structure.attributes.observation` array. It is nulled for unused optional attributes when the attribute index needs to be included. The index for an annotation is the index in the array of annotations in the structure field.


Example call: https://api.data.abs.gov.au/data/ABS,RES_DWELL/1.1+1GSYD+1RNSW.Q?detail=Full&startPeriod=2020-Q1&endPeriod=2020-Q2&format=jsondata&dimensionAtObservation=AllDimensions 

This call returns two observations each for two time periods. The `dimensionAtObservation` parameter is set to `AllDimensions` which returns a flat data file.

As a CSV this data would be presented as follows, the first row is the header:

```csv
DATAFLOW,MEASURE,REGION,FREQ,TIME_PERIOD,OBS_VALUE,UNIT_MEASURE,UNIT_MULT,OBS_STATUS,OBS_COMMENT
ABS:RES_DWELL(1.0.0),1,1RNSW,Q,2020-Q1,10210,NUM,0,r,
ABS:RES_DWELL(1.0.0),1,1RNSW,Q,2020-Q2,9555,NUM,0,r,
ABS:RES_DWELL(1.0.0),1,1GSYD,Q,2020-Q1,10119,NUM,0,r,
ABS:RES_DWELL(1.0.0),1,1GSYD,Q,2020-Q2,9493,NUM,0,r,
```

In SDMX-JSON, the observations are presented in a similar flattened way, but dimension and attribute values are replaced by their indices:

```json
        "observations": {
          "0:0:0:0": [10210, 0, 0, 0, null],
          "0:0:0:1": [9555, 0, 0, 0, null],
          "0:1:0:0": [10119, 0, 0, 0, null],
          "0:1:0:1": [9493, 0, 0, 0, null]
        }
```

```txt
Observation 1: 
	“0:0:0:0” corresponds to the four indices for “MEASURE:1”, “REGION: 1RNSW”, “FREQ:Q”, “TIME_PERIOD: 2020-Q1”
	The value for this observation is 10210
	The following four values are the attributes. Attributes for this observation are:
		“UNIT_MEASURE:NUM”
		“UNIT_MULT:0”
		“OBS_STATUS:r”
		“OBS_COMMENT:null”

Observation 2: 
	“0:0:0:1” corresponds to the four indices for “MEASURE:1”, “REGION: 1RNSW”, “FREQ:Q”, “TIME_PERIOD: 2020-Q2”
	The value for this observation is 9555
	Observation attributes: “UNIT_MEASURE:NUM”, “UNIT_MULT:0”, “OBS_STATUS:r”, “OBS_COMMENT:null”

Observation 3: 
	“0:1:0:0” corresponds to the four indices for “MEASURE:1”, “REGION: 1GSYD”, “FREQ:Q”, “TIME_PERIOD: 2020-Q1”
	The value for this observation is 10119
	Observation attributes: “UNIT_MEASURE:NUM”, “UNIT_MULT:0”, “OBS_STATUS:r”, “OBS_COMMENT:null”

Observation 4: 
	“0:1:0:1” corresponds to the four indices for “MEASURE:1”, “REGION: 1GSYD”, “FREQ:Q”, “TIME_PERIOD: 2020-Q2”
	The value for this observation is 9493
	Observation attributes: “UNIT_MEASURE:NUM”, “UNIT_MULT:0”, “OBS_STATUS:r”, “OBS_COMMENT:null”
```

Here's the structure section for that data response:
```json
"dimensions": {
        "dataset": [],
        "series": [],
        "observation": [
          {
            "id": "MEASURE",
            "name": "Measure",
            "names": {"en": "Measure"},
            "keyPosition": 0,
            "roles": ["MEASURE"],
            "values": [
              {
                "id": "1",
                "order": 0,
                "name": "Number of Established House Transfers",
                "names": {"en": "Number of Established House Transfers"},
              }
            ]
          },
          {
            "id": "REGION",
            "name": "Region",
            "names": {"en": "Region"},
            "keyPosition": 1,
            "roles": ["REGION"],
            "values": [
              {
                "id": "1RNSW",
                "order": 3,
                "name": "Rest of NSW",
                "names": {"en": "Rest of NSW"},
                "parent": "1"
              },
              {
                "id": "1GSYD",
                "order": 2,
                "name": "Greater Sydney",
                "names": {"en": "Greater Sydney"},
                "parent": "1"
              }
            ]
          },
          {
            "id": "FREQ",
            "name": "Frequency",
            "names": {
              "en": "Frequency"
            },
            "keyPosition": 2,
            "roles": ["FREQ"],
            "values": [
              {
                "id": "Q",
                "order": 8,
                "name": "Quarterly",
                "names": {"en": "Quarterly"}
              }
            ]
          },
          {
            "id": "TIME_PERIOD",
            "name": "Time Period",
            "names": {"en": "Time Period"},
            "keyPosition": 3,
            "roles": ["TIME_PERIOD"],
            "values": [
              {
                "start": "2020-01-01T00:00:00Z",
                "end": "2020-03-31T00:00:00Z",
                "id": "2020-Q1",
                "name": "2020-Q1",
                "names": {"en": "2020-Q1"}
              },
              {
                "start": "2020-04-01T00:00:00Z",
                "end": "2020-06-30T00:00:00Z",
                "id": "2020-Q2",
                "name": "2020-Q2",
                "names": {"en": "2020-Q2"}
              }
            ]
          }
        ]
      },
      "attributes": {
        "dataSet": [],
        "series": [],
        "observation": [
          {
            "id": "UNIT_MEASURE",
            "name": "Unit of Measure",
            "names": {"en": "Unit of Measure"},
            "roles": ["UNIT_MEASURE"],
            "relationship": {
              "dimensions": ["MEASURE"]
            },
            "values": [
              {
                "id": "NUM",
                "order": 1,
                "name": "Number",
                "names": {"en": "Number"}
              }
            ],
            "annotations": [1]
          },
          {
            "id": "UNIT_MULT",
            "name": "Unit of Multiplier",
            "names": {"en": "Unit of Multiplier"},
            "roles": ["UNIT_MULT"],
            "relationship": {
              "dimensions": ["MEASURE"]
            },
            "values": [
              {
                "id": "0",
                "order": 0,
                "name": "Units",
                "names": {
                  "en": "Units"
                }
              }
            ],
            "annotations": [2]
          },
          {
            "id": "OBS_STATUS",
            "name": "Observation Status",
            "names": {
              "en": "Observation Status"
            },
            "roles": [
              "OBS_STATUS"
            ],
            "relationship": {
              "primaryMeasure": "OBS_VALUE"
            },
            "values": [
              {
                "id": "r",
                "order": 7,
                "name": "revised",
                "names": {
                  "en": "revised"
                }
              }
            ]
          },
          {
            "id": "OBS_COMMENT",
            "name": "Observation Comment",
            "names": {
              "en": "Observation Comment"
            },
            "roles": [
              "OBS_COMMENT"
            ],
            "relationship": {
              "primaryMeasure": "OBS_VALUE"
            },
            "values": []
          }
        ]
      },
      "annotations": [
        {
          "type": "NonProductionDataflow",
          "text": "true",
          "texts": {"en": "true"}
        }
        {
          "type": "CONTEXT",
          "text": "If a unit multiplier exists the data is recorded according to the combination of the unit multiplier and the unit of measure.",
        },
        {
          "type": "CONTEXT",
          "text": "Codes for unit of multiplier are the exponent in base 10 so that multiplying the observation by 10^UNIT_MULT gives a value expressed in the unit of measure.",
        }
      ]
```


## SDMX CSV

SDMX-CSV Data Message is an SDMX data exchange format based on the RFC 4180 specification (determined column number, "comma" separated).

More information on the SDMX-CSV standard is available on the SDMX Technical Working Group’s [SDMX-CSV Field Guide](https://github.com/sdmx-twg/sdmx-csv/blob/master/data-message/docs/sdmx-csv-field-guide.md). 


### Format

#### Rows:

-	In an SDMX-CSV file, the first row is always the header. The header defines what each of the columns in the data file contains.  
-	Every subsequent row in the file contains information related to one specific observation.  That is, in SDMX-CSV, each row contains just one data point and the metadata to define that data point.


#### Columns:

The comma separator `,` is used to separate columns. The first column defines the dataflow. Then there is one column for each dimension defined in the data structure definition (DSD). One column for data observations. And one column for each attribute defined in the DSD regardless of whether the attribute is used.

Column headers (first row):
-	The first column is the dataflow column and the header is always the term `DATAFLOW`.
-	For a dimension column, it is the dimension's ID or both ID and label .
-	`OBS_VALUE` is the column for data observations.
-	For an attribute column, it is the attribute's ID or both ID and label.

Column content (all rows after header):
-	The first column always defines the dataflow. The dataflow is given in the format `agencyId:dataflowId(version)` e.g. `ABS:CPI(1.0.0)`.
-	All the dimensions defined in the DSD follow, one dimension per column. Dimensions are always in the order defined by the dimensions position number in the DSD. More information on dimension ordering is available in [worked examples](#worked-examples).
-	The final dimension column is always Time.
-	The next column is always data observations, defined by the `OBS_VALUE` header.
-	The final columns are attributes such as Unit of Measure.

Codes and Labels
There are two options when returning data in CSV format; codes only or codes and labels. More information on how to request each is available in the [Response Format](#response-format) section. 
-	Codes - each column will contain only the ID Code for its contents e.g. “TOT”.
-	Codes and Labels - each column will contain the ID Code and then the Label for it’s contents, separated by a colon : e.g. `TOT:Total`.

Example: 
Codes only: https://dotstat-intra.infra.abs.gov.au/DisseminateNSIService/Rest/data/ABS,ANA_AGG,/M1.GPM_PCA+GPM.20.AUS.Q?startPeriod=2019-Q4&endPeriod=2020-Q1&format=csv 

```csv
DATAFLOW,MEASURE,DATA_ITEM,TSEST,REGION,FREQ,TIME_PERIOD,OBS_VALUE,UNIT_MEASURE,UNIT_MULT,OBS_STATUS,OBS_COMMENT
ABS:ANA_AGG(1.1.0),M1,GPM,20,AUS,Q,2019-Q4,496921,AUD,6,,
ABS:ANA_AGG(1.1.0),M1,GPM,20,AUS,Q,2020-Q1,495533,AUD,6,,
ABS:ANA_AGG(1.1.0),M1,GPM_PCA,20,AUS,Q,2019-Q4,19452,AUD,0,,
ABS:ANA_AGG(1.1.0),M1,GPM_PCA,20,AUS,Q,2020-Q1,19334,AUD,0,,
```

Codes and Labels:
```csv
DATAFLOW,MEASURE: Measure,DATA_ITEM: Data Item,TSEST: Adjustment Type,REGION: Region,FREQ: Frequency,TIME_PERIOD: Time Period,OBS_VALUE,UNIT_MEASURE: Unit of Measure,UNIT_MULT: Unit of Multiplier,OBS_STATUS: Observation Status,OBS_COMMENT: Observation Comment
ABS:ANA_AGG(1.1.0),M1: Chain volume measures,GPM: Gross domestic product,20: Seasonally Adjusted,AUS: Australia,Q: Quarterly,2019-Q4,496921,AUD: Australian Dollars,6: Millions,,
ABS:ANA_AGG(1.1.0),M1: Chain volume measures,GPM: Gross domestic product,20: Seasonally Adjusted,AUS: Australia,Q: Quarterly,2020-Q1,495533,AUD: Australian Dollars,6: Millions,,
ABS:ANA_AGG(1.1.0),M1: Chain volume measures,GPM_PCA: GDP per capita,20: Seasonally Adjusted,AUS: Australia,Q: Quarterly,2019-Q4,19452,AUD: Australian Dollars,0: Units,,
ABS:ANA_AGG(1.1.0),M1: Chain volume measures,GPM_PCA: GDP per capita,20: Seasonally Adjusted,AUS: Australia,Q: Quarterly,2020-Q1,19334,AUD: Australian Dollars,0: Units,,
```


# Worked Examples

## Explore a dataset and construct a data request

### Introduction

The ABS DataAPIs provide machine-to-machine access to ABS data via an SDMX RESTful web service. The official [SDMX wiki](https://github.com/sdmx-twg/sdmx-rest/wiki) explains the standard in detail. SDMX is complex and can be difficult to learn, this tutorial aims to take you through the basics you need to know to use the ABS Data API.  We’ll touch on a number of SDMX concepts but the focus is on constructing a data request.

#### Which Data?

For this example, lets say we want to find out on average how much mid-strength beer each Australian drank in 2008. We know the ABS publishes a collection called `Apparent Consumption of Alcohol`, so that's where we'll start.

We'll be going through this process step by step and explaining a little bit along the way, if you just want to API calls you can skip to the end.

### Step 1: Finding a Dataflow

All data in the ABS APIs comes out of a dataflow (a type of SDMX structure). You can think of a dataflow as a table of data (it isn't, but you can kind of think of it as one). To begin our search, we'll need to find a dataflow containing data from the `Apparent Consumption of Alcohol` collection. Luckily, all SDMX structures have their own URL we can call to list them (as well as do some other exciting things if we like). Simply by visiting [https://api.data.abs.gov.au/dataflow](https://api.data.abs.gov.au/dataflow) we get to see a list of all the dataflows the ABS offers. There are a lot of them. If we search for `Apparent Consumption of Alcohol` however, we find one:

```xml
   <structure:Dataflow id="ALC" agencyID="ABS" version="1.0.0" isFinal="true">
      <common:Annotations>
         <common:Annotation>
            <common:AnnotationType>NonProductionDataflow</common:AnnotationType>
            <common:AnnotationText xml:lang="en">true</common:AnnotationText>
         </common:Annotation>
      </common:Annotations>
      <common:Name xml:lang="en">Apparent Consumption of Alcohol, Australia</common:Name>
      <structure:Structure>
         <Ref id="ALC" version="1.0.0" agencyID="ABS" package="datastructure" class="DataStructure" />
      </structure:Structure>
   </structure:Dataflow>
```

### Step 2: Reviewing the Structure

Dataflows are how we look at the data in the ABS API, but there's another structure that defines how the data is structured (which is super important for querying it and understanding the response). This is pretty appropriately called the Data Structure Definition, or DSD for short. If we’re thinking of a dataflow as a table, we can think about DSDs as the definition of what rows and columns there are in the table, and what values they can take (and a bunch of other things…).

What has this got to do with getting the data? It's important to know how the data is structured so we can build a query. Now, our dataflow is pretty small, so we could just get all the data and deal with finding the value we want... but some of the other dataflows are very large. So we should learn how to query the API to avoid getting a whole bunch of data we don't want, crashing our computer or exceeding the size limits for the API.

Data queries to the API look like this: `https://api.data.abs.gov.au/data/{flowRef}/{dataKey}?{queryParameters}`. We're going to need to provide a `flowRef` so the API knows what dataflow to get data from, and a `dataKey` to filter the data we get back. We'll leave the extra parameters to the next section.

#### FlowRef

There's a few ways we can define the `flowRef`, but we'll be using the simplest one. From the first request we made above we know `Dataflow id=“ALC”` therefore our flowRef is `ALC`

#### dataKey

The dataKey section of the URL lets us tell the ABS API that we only want a subset of the available data. We do that by providing the values we want from each `dimension`, separated by the `.` character. You can think of the dimensions as the columns and rows of the table defined by the DSD. So, this is why we need to look at the DSD our dataflow is using. Firstly, we need to know what order the dimensions appear in the DSD, because that's the order we need to put them in the `dataKey`. Secondly, we need to know what values are available, and which ones correspond to the information I want to retrieve.

Each of the dimensions in our dataflow's DSD is represented by a `codelist`. A codelist... lists... codes... So, we need to look at the DSD and see what order the dimensions are (and what they are) and then look at each of the codelists and work out what code corresponds to the information we want (remember, it's how much mid-strength beer each Australian drank in 2008).

Now, we can use the same URL we used for the dataflow (because DSDs also have their own endpoint), but let’s go one step further and get a specific DSD instead of all of them. To do that we need to know how to tell the API which DSD we want. There are three pieces of information that together let us uniquely identify a dataflow or a DSD (or any maintainable structure for that matter):

- The agency id: This is who owns the structure, and in the ABS API it's always the ABS... so it's `ABS`
- The structure id itself
- The version number of the structure: this is used when structures need to change over time. A classic example would be adding a country to the list of countries... the old list will still be around, but we'll create a new version of it

Now, we don't actually have to provide a version number for our DSD, because if you don't give one, the API assumes you just want the latest one... which is true! We do just want the latest one! The question now is where to get the structure id for the DSD for our dataflow. Well, luckily we've already seen it, when we were looking for our dataflow in step 1 above.

```xml
<structure:Structure>
  <Ref id="ALC" version="1.0.0" agencyID="ABS" package="datastructure" class="DataStructure"/>
</structure:Structure>
```

Given this, we can get the DSD using the url: [https://api.data.abs.gov.au/datastructure/ABS/ALC](https://api.data.abs.gov.au/datastructure/ABS/ALC). 

```xml
<message:Structures>
   <structure:DataStructures>
      <structure:DataStructure id="ALC" agencyID="ABS" version="1.0.0" isFinal="true">
         <common:Name xml:lang="en">Apparent Consumption of Alcohol, Australia</common:Name>
         <structure:DataStructureComponents>
            <structure:DimensionList id="DimensionDescriptor">
               <structure:Dimension id="TYP" position="1">
                  <structure:ConceptIdentity>
                     <Ref id="TYP" maintainableParentID="CS_ALC" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_ALC_TYP" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist" />
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
               </structure:Dimension>
               <structure:Dimension id="MEA" position="2">
                  <structure:ConceptIdentity>
                     <Ref id="MEASURE" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_ALC_MEASURE" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist" />
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
               </structure:Dimension>
```

This gives us the Data Structure with dimensions. The order of dimensions is given by their position eg. `position="1"`.

But, we don't just need to know the order of the dimensions, but what values they take. That's defined by the codelist each dimension uses. Now, we could do the same sort of thing we did with getting to the DSD from the dataflow. Each dimension will refer to a codelist like we can see above for the codelist `CL_ALC_TYP`.

We could call the API to get each of the codelists in turn using URLs like [https://api.data.abs.gov.au/codelist/ABS/CL_ALC_TYP/1.0.0](https://api.data.abs.gov.au/codelist/ABS/CL_ALC_TYP/1.0.0) (we included the version number 1.0.0 here). But that means we have to make a call for every dimension. Let’s save time and use our first query parameter when getting structure information: `references`. This parameter lets us retrieve not just the specified structure from the API, but some of the structures it references as well. We're going to specify the value `codelist`, telling the API that we want to retrieve any codelists referred to by our DSD: [https://api.data.abs.gov.au/datastructure/ABS/ALC?references=codelist](https://api.data.abs.gov.au/datastructure/ABS/ALC?references=codelist) (Some codelists removed for brevity):


```xml
<message:Structures>
   <structure:Codelists>
      <structure:Codelist id="CL_ALC_BEVT" agencyID="ABS" version="1.0.0" isFinal="true">
         <common:Name xml:lang="en">Beverage Type</common:Name>
         <structure:Code id="1">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">1</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Beer</common:Name>
         </structure:Code>
         <structure:Code id="2">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">2</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Wine</common:Name>
         </structure:Code>
         <structure:Code id="3">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">3</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Spirits and RTDs</common:Name>
         </structure:Code>
         <structure:Code id="4">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">4</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Total all beverages</common:Name>
         </structure:Code>
         <structure:Code id="5">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">5</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Cider</common:Name>
         </structure:Code>
      </structure:Codelist>
      <structure:Codelist id="CL_ALC_MEASURE" agencyID="ABS" version="1.0.0" isFinal="true">
         <common:Name xml:lang="en">Measure</common:Name>
         <structure:Code id="1">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">1</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Total apparent consumption ('000 litres)</common:Name>
         </structure:Code>
         <structure:Code id="2">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">2</common:AnnotationText>
               </common:Annotation>
               <common:Annotation>
                  <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">Litres per person aged 15 years and over</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Per capita apparent consumption (litres)</common:Name>
         </structure:Code>
      </structure:Codelist>
      <structure:Codelist id="CL_ALC_SUB" agencyID="ABS" version="1.0.0" isFinal="true">
         <common:Name xml:lang="en">Beverage Subtype/Strength</common:Name>
         <structure:Code id="1">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">1</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Low alcohol beer</common:Name>
         </structure:Code>
         <structure:Code id="2">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">2</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Other alcohol beer</common:Name>
         </structure:Code>
         <structure:Code id="3">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">3</common:AnnotationText>
               </common:Annotation>
               <common:Annotation>
                  <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">Alcohol volume of low strength beer is greater than 1.15% and less than or equal to 3.0%</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Low strength beer</common:Name>
         </structure:Code>
         <structure:Code id="4">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">4</common:AnnotationText>
               </common:Annotation>
               <common:Annotation>
                  <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">Alcohol volume of mid strength beer is greater than 3.0% and less than or equal to 3.5%</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Mid strength beer</common:Name>
         </structure:Code>
         <structure:Code id="5">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">5</common:AnnotationText>
               </common:Annotation>
               <common:Annotation>
                  <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">Alcohol volume of full strength beer is greater than 3.5%</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Full strength beer</common:Name>
         </structure:Code>
         <structure:Code id="6">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">6</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Total beer</common:Name>
         </structure:Code>
         <structure:Code id="7">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">7</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">White table wine</common:Name>
         </structure:Code>
         <structure:Code id="8">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">8</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Red table wine</common:Name>
         </structure:Code>
         <structure:Code id="9">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">9</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Other wines</common:Name>
         </structure:Code>
         <structure:Code id="10">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">10</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Total wine</common:Name>
         </structure:Code>
         <structure:Code id="11">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">11</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Spirits</common:Name>
         </structure:Code>
         <structure:Code id="12">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">12</common:AnnotationText>
               </common:Annotation>
               <common:Annotation>
                  <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">Ready to Drink (Pre-mixed) Beverages</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">RTDs</common:Name>
         </structure:Code>
         <structure:Code id="13">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">13</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Total spirits and RTDs</common:Name>
         </structure:Code>
         <structure:Code id="15">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">14</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Cider</common:Name>
         </structure:Code>
         <structure:Code id="14">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">15</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Total all beverages</common:Name>
         </structure:Code>
      </structure:Codelist>
      <structure:Codelist id="CL_ALC_TYP" agencyID="ABS" version="1.0.0" isFinal="true">
         <common:Name xml:lang="en">Type of Volume</common:Name>
         <structure:Code id="1">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">1</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Volume of pure alcohol</common:Name>
         </structure:Code>
         <structure:Code id="2">
            <common:Annotations>
               <common:Annotation>
                  <common:AnnotationType>ORDER</common:AnnotationType>
                  <common:AnnotationText xml:lang="en">2</common:AnnotationText>
               </common:Annotation>
            </common:Annotations>
            <common:Name xml:lang="en">Volume of beverage</common:Name>
         </structure:Code>
      </structure:Codelist>
   </structure:Codelists>
   <structure:DataStructures>
      <structure:DataStructure id="ALC" agencyID="ABS" version="1.0.0" isFinal="true">
         <common:Name xml:lang="en">Apparent Consumption of Alcohol, Australia</common:Name>
         <structure:DataStructureComponents>
            <structure:DimensionList id="DimensionDescriptor">
               <structure:Dimension id="TYP" position="1">
                  <structure:ConceptIdentity>
                     <Ref id="TYP" maintainableParentID="CS_ALC" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_ALC_TYP" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist"/>
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
               </structure:Dimension>
               <structure:Dimension id="MEA" position="2">
                  <structure:ConceptIdentity>
                     <Ref id="MEASURE" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_ALC_MEASURE" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist"/>
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
               </structure:Dimension>
               <structure:Dimension id="BEVT" position="3">
                  <structure:ConceptIdentity>
                     <Ref id="BEVT" maintainableParentID="CS_ALC" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_ALC_BEVT" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist"/>
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
               </structure:Dimension>
               <structure:Dimension id="SUB" position="4">
                  <structure:ConceptIdentity>
                     <Ref id="SUB" maintainableParentID="CS_ALC" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_ALC_SUB" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist"/>
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
               </structure:Dimension>
               <structure:Dimension id="FREQUENCY" position="5">
                  <structure:ConceptIdentity>
                     <Ref id="FREQ" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_FREQ" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist"/>
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
               </structure:Dimension>
               <structure:TimeDimension id="TIME_PERIOD" position="6">
                  <structure:ConceptIdentity>
                     <Ref id="TIME_PERIOD" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:TextFormat textType="ObservationalTimePeriod"/>
                  </structure:LocalRepresentation>
               </structure:TimeDimension>
            </structure:DimensionList>
            <structure:AttributeList id="AttributeDescriptor">
               <structure:Attribute id="UNIT_MEASURE" assignmentStatus="Conditional">
                  <structure:ConceptIdentity>
                     <Ref id="UNIT_MEASURE" maintainableParentID="CS_ATTRIBUTE" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_UNIT_MEASURE" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist"/>
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
                  <structure:AttributeRelationship>
                     <structure:None/>
                  </structure:AttributeRelationship>
               </structure:Attribute>
               <structure:Attribute id="UNIT_MULT" assignmentStatus="Conditional">
                  <structure:ConceptIdentity>
                     <Ref id="UNIT_MULT" maintainableParentID="CS_ATTRIBUTE" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_UNIT_MULT" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist"/>
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
                  <structure:AttributeRelationship>
                     <structure:Dimension>
                        <Ref id="MEA"/>
                     </structure:Dimension>
                  </structure:AttributeRelationship>
               </structure:Attribute>
               <structure:Attribute id="OBS_STATUS" assignmentStatus="Conditional">
                  <structure:ConceptIdentity>
                     <Ref id="OBS_STATUS" maintainableParentID="CS_ATTRIBUTE" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:LocalRepresentation>
                     <structure:Enumeration>
                        <Ref id="CL_OBS_STATUS" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist"/>
                     </structure:Enumeration>
                  </structure:LocalRepresentation>
                  <structure:AttributeRelationship>
                     <structure:PrimaryMeasure>
                        <Ref id="OBS_VALUE"/>
                     </structure:PrimaryMeasure>
                  </structure:AttributeRelationship>
               </structure:Attribute>
               <structure:Attribute id="OBS_COMMENT" assignmentStatus="Conditional">
                  <structure:ConceptIdentity>
                     <Ref id="OBS_COMMENT" maintainableParentID="CS_ATTRIBUTE" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
                  <structure:AttributeRelationship>
                     <structure:PrimaryMeasure>
                        <Ref id="OBS_VALUE"/>
                     </structure:PrimaryMeasure>
                  </structure:AttributeRelationship>
               </structure:Attribute>
            </structure:AttributeList>
            <structure:MeasureList id="MeasureDescriptor">
               <structure:PrimaryMeasure id="OBS_VALUE">
                  <structure:ConceptIdentity>
                     <Ref id="OBS_VALUE" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept"/>
                  </structure:ConceptIdentity>
               </structure:PrimaryMeasure>
            </structure:MeasureList>
         </structure:DataStructureComponents>
      </structure:DataStructure>
   </structure:DataStructures>
</message:Structures>
```

#### Concept

We've worked out how to get the codelists that define the values each dimension can take.  However, it may not always be clear from these values what the dimensions actually are. To find this we need to look at the `concept` it refers to. The concept gives the dimension its meaning and its name. As codes are stored in codelists, concepts are stored in... conceptschemes (conceptlists would be too obvious). So, we want the DSD to get the dimensions and their order, the referenced concepts (via their conceptschemes) to work out what they are, and the referenced codelists (to work out what values we need).

We're going back to the `references` query parameter again. We could use `conceptscheme`, but instead we’ll use the value `children` to tell the API we want all directly-referenced structures (which will include both the codelists, and the conceptschemes). Finally, we have all the information we need. Our API call is [https://api.data.abs.gov.au/datastructure/ABS/ALC?references=children](https://api.data.abs.gov.au/datastructure/ABS/ALC?references=children) (Some codelists and conceptschemes we're not using removed for brevity):

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--NSI Web Service v7.13.0.0-->
<message:Structure xmlns:message="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/message" xmlns:structure="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/structure" xmlns:common="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/common">
  <message:Header>
    <message:ID>IDREF1399</message:ID>
    <message:Test>false</message:Test>
    <message:Prepared>2020-09-28T11:44:17.6726618+10:00</message:Prepared>
    <message:Sender id="Unknown" />
    <message:Receiver id="Unknown" />
  </message:Header>
  <message:Structures>
    <structure:Codelists>
      <structure:Codelist id="CL_ALC_BEVT" agencyID="ABS" version="1.0.0" isFinal="true">
        <common:Name xml:lang="en">Beverage Type</common:Name>
        <structure:Code id="1">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">1</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Beer</common:Name>
        </structure:Code>
        <structure:Code id="2">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">2</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Wine</common:Name>
        </structure:Code>
        <structure:Code id="3">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">3</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Spirits and RTDs</common:Name>
        </structure:Code>
        <structure:Code id="4">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">4</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Total all beverages</common:Name>
        </structure:Code>
        <structure:Code id="5">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">5</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Cider</common:Name>
        </structure:Code>
      </structure:Codelist>
      <structure:Codelist id="CL_ALC_MEASURE" agencyID="ABS" version="1.0.0" isFinal="true">
        <common:Name xml:lang="en">Measure</common:Name>
        <structure:Code id="1">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">1</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Total apparent consumption ('000 litres)</common:Name>
        </structure:Code>
        <structure:Code id="2">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">2</common:AnnotationText>
            </common:Annotation>
            <common:Annotation>
              <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
              <common:AnnotationText xml:lang="en">Litres per person aged 15 years and over</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Per capita apparent consumption (litres)</common:Name>
        </structure:Code>
      </structure:Codelist>
      <structure:Codelist id="CL_ALC_SUB" agencyID="ABS" version="1.0.0" isFinal="true">
        <common:Name xml:lang="en">Beverage Subtype/Strength</common:Name>
        <structure:Code id="1">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">1</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Low alcohol beer</common:Name>
        </structure:Code>
        <structure:Code id="2">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">2</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Other alcohol beer</common:Name>
        </structure:Code>
        <structure:Code id="3">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">3</common:AnnotationText>
            </common:Annotation>
            <common:Annotation>
              <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
              <common:AnnotationText xml:lang="en">Alcohol volume of low strength beer is greater than 1.15% and less than or equal to 3.0%</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Low strength beer</common:Name>
        </structure:Code>
        <structure:Code id="4">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">4</common:AnnotationText>
            </common:Annotation>
            <common:Annotation>
              <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
              <common:AnnotationText xml:lang="en">Alcohol volume of mid strength beer is greater than 3.0% and less than or equal to 3.5%</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Mid strength beer</common:Name>
        </structure:Code>
        <structure:Code id="5">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">5</common:AnnotationText>
            </common:Annotation>
            <common:Annotation>
              <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
              <common:AnnotationText xml:lang="en">Alcohol volume of full strength beer is greater than 3.5%</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Full strength beer</common:Name>
        </structure:Code>
        <structure:Code id="6">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">6</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Total beer</common:Name>
        </structure:Code>
        <structure:Code id="7">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">7</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">White table wine</common:Name>
        </structure:Code>
        <structure:Code id="8">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">8</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Red table wine</common:Name>
        </structure:Code>
        <structure:Code id="9">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">9</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Other wines</common:Name>
        </structure:Code>
        <structure:Code id="10">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">10</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Total wine</common:Name>
        </structure:Code>
        <structure:Code id="11">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">11</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Spirits</common:Name>
        </structure:Code>
        <structure:Code id="12">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">12</common:AnnotationText>
            </common:Annotation>
            <common:Annotation>
              <common:AnnotationType>FURTHER_INFORMATION</common:AnnotationType>
              <common:AnnotationText xml:lang="en">Ready to Drink (Pre-mixed) Beverages</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">RTDs</common:Name>
        </structure:Code>
        <structure:Code id="13">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">13</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Total spirits and RTDs</common:Name>
        </structure:Code>
        <structure:Code id="15">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">14</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Cider</common:Name>
        </structure:Code>
        <structure:Code id="14">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">15</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Total all beverages</common:Name>
        </structure:Code>
      </structure:Codelist>
      <structure:Codelist id="CL_ALC_TYP" agencyID="ABS" version="1.0.0" isFinal="true">
        <common:Name xml:lang="en">Type of Volume</common:Name>
        <structure:Code id="1">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">1</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Volume of pure alcohol</common:Name>
        </structure:Code>
        <structure:Code id="2">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>ORDER</common:AnnotationType>
              <common:AnnotationText xml:lang="en">2</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Volume of beverage</common:Name>
        </structure:Code>
      </structure:Codelist>
      <structure:Codelist id="CL_FREQ" agencyID="ABS" version="1.0.0" isFinal="true">
        <common:Name xml:lang="en">Frequency</common:Name>
        <structure:Code id="H">
          <common:Name xml:lang="en">Hourly</common:Name>
          <common:Description xml:lang="en">To be used for data collected or disseminated every hour.</common:Description>
        </structure:Code>
        <structure:Code id="D">
          <common:Name xml:lang="en">Daily</common:Name>
          <common:Description xml:lang="en">To be used for data collected or disseminated every day.</common:Description>
        </structure:Code>
        <structure:Code id="N">
          <common:Name xml:lang="en">Minutely</common:Name>
          <common:Description xml:lang="en">While N denotes "minutely", usually, there may be no observations every minute (for several series the frequency is usually "irregular" within a day/days). And though observations may be sparse (not collected or disseminated every minute), missing values do not need to be given for the minutes when no observations exist: in any case the time stamp determines when an observation is observed.</common:Description>
        </structure:Code>
        <structure:Code id="B">
          <common:Name xml:lang="en">Daily or businessweek</common:Name>
          <common:Description xml:lang="en">Similar to "daily", however there are no observations for Saturdays and Sundays (so, neither "missing values" nor "numeric values" should be provided for Saturday and Sunday). This treatment ("business") is one way to deal with such cases, but it is not the only option. Such a time series could alternatively be considered daily ("D"), thus, with missing values in the weekend.</common:Description>
        </structure:Code>
        <structure:Code id="W">
          <common:Name xml:lang="en">Weekly</common:Name>
          <common:Description xml:lang="en">To be used for data collected or disseminated every week.</common:Description>
        </structure:Code>
        <structure:Code id="S">
          <common:Name xml:lang="en">Half-yearly, semester</common:Name>
          <common:Description xml:lang="en">To be used for data collected or disseminated every semester.</common:Description>
        </structure:Code>
        <structure:Code id="A">
          <common:Name xml:lang="en">Annual</common:Name>
          <common:Description xml:lang="en">To be used for data collected or disseminated every year.</common:Description>
        </structure:Code>
        <structure:Code id="M">
          <common:Name xml:lang="en">Monthly</common:Name>
          <common:Description xml:lang="en">To be used for data collected or disseminated every month.</common:Description>
        </structure:Code>
        <structure:Code id="Q">
          <common:Name xml:lang="en">Quarterly</common:Name>
          <common:Description xml:lang="en">To be used for data collected or disseminated every quarter.</common:Description>
        </structure:Code>
      </structure:Codelist>
      <structure:Codelist id="CL_OBS_STATUS" agencyID="ABS" version="1.0.0" isFinal="true">
        <!-- Removed codelist -->
      </structure:Codelist>
      <structure:Codelist id="CL_UNIT_MEASURE" agencyID="ABS" version="1.0.0" isFinal="true">
        <!-- Removed codelist -->
      </structure:Codelist>
      <structure:Codelist id="CL_UNIT_MULT" agencyID="ABS" version="1.0.0" isFinal="true">
        <!-- Removed codelist -->
      </structure:Codelist>
    </structure:Codelists>
    <structure:Concepts>
      <structure:ConceptScheme id="CS_ALC" agencyID="ABS" version="1.0.0" isFinal="true">
        <common:Name xml:lang="en">Apparent Consumption of Alcohol Concepts</common:Name>
        <structure:Concept id="BEVT">
          <common:Name xml:lang="en">Beverage Type</common:Name>
        </structure:Concept>
        <structure:Concept id="SUB">
          <common:Name xml:lang="en">Beverage Subtype/Strength</common:Name>
        </structure:Concept>
        <structure:Concept id="TYP">
          <common:Name xml:lang="en">Type of Volume</common:Name>
        </structure:Concept>
      </structure:ConceptScheme>
      <structure:ConceptScheme id="CS_ATTRIBUTE" agencyID="ABS" version="1.0.0" isFinal="true">
        <!-- Remove conceptscheme -->
      </structure:ConceptScheme>
      <structure:ConceptScheme id="CS_COMMON" agencyID="ABS" version="1.0.0" isFinal="true">
        <common:Name xml:lang="en">Common Concepts</common:Name>
        <structure:Concept id="OBS_VALUE">
          <common:Name xml:lang="en">Observation Value</common:Name>
          <common:Description xml:lang="en">The observed value of the variable identified by the series dimension values.</common:Description>
        </structure:Concept>
        <structure:Concept id="TIME_PERIOD">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>CONTEXT</common:AnnotationType>
              <common:AnnotationText xml:lang="en">The measurement represented by each observation corresponds to a specific point in time (e.g. a single day) or a period (e.g. a month, a fiscal year, or a calendar year).</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Time Period</common:Name>
          <common:Description xml:lang="en">Timespan or point in time for which the observation refers.</common:Description>
        </structure:Concept>
        <structure:Concept id="FREQ">
          <common:Name xml:lang="en">Frequency</common:Name>
          <common:Description xml:lang="en">Rate at which data is collated.</common:Description>
        </structure:Concept>
        <structure:Concept id="MEASURE">
          <common:Name xml:lang="en">Measure</common:Name>
        </structure:Concept>
        <structure:Concept id="TSEST">
          <common:Annotations>
            <common:Annotation>
              <common:AnnotationType>OTHER_LINKS</common:AnnotationType>
              <common:AnnotationText xml:lang="en">https://www.abs.gov.au/websitedbs/D3310114.nsf/home/Time+Series+Analysis:+The+Basics</common:AnnotationText>
            </common:Annotation>
          </common:Annotations>
          <common:Name xml:lang="en">Adjustment Type</common:Name>
          <common:Description xml:lang="en">An original time series can be decomposed into three components: the trend (the general direction of the series), the seasonal component (systematic, calendar related movements) and the irregular component (unsystematic, short term fluctuations). Seasonally adjusted series are produced by estimating the seasonal component and removing this from the original series. In most economic data the seasonal component is a combination of seasonal influences (e.g. the effect of the weather or social traditions) plus other kinds of calendar related variations such as Chinese New Year and Christmas. The seasonal adjustment methodology takes into account both seasonal and other calendar related factors that evolve over time to reflect changes in activity patterns.</common:Description>
        </structure:Concept>
      </structure:ConceptScheme>
    </structure:Concepts>
    <structure:DataStructures>
      <structure:DataStructure id="ALC" agencyID="ABS" version="1.0.0" isFinal="true">
        <common:Name xml:lang="en">Apparent Consumption of Alcohol, Australia</common:Name>
        <structure:DataStructureComponents>
          <structure:DimensionList id="DimensionDescriptor">
            <structure:Dimension id="TYP" position="1">
              <structure:ConceptIdentity>
                <Ref id="TYP" maintainableParentID="CS_ALC" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
              </structure:ConceptIdentity>
              <structure:LocalRepresentation>
                <structure:Enumeration>
                  <Ref id="CL_ALC_TYP" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist" />
                </structure:Enumeration>
              </structure:LocalRepresentation>
            </structure:Dimension>
            <structure:Dimension id="MEA" position="2">
              <structure:ConceptIdentity>
                <Ref id="MEASURE" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
              </structure:ConceptIdentity>
              <structure:LocalRepresentation>
                <structure:Enumeration>
                  <Ref id="CL_ALC_MEASURE" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist" />
                </structure:Enumeration>
              </structure:LocalRepresentation>
            </structure:Dimension>
            <structure:Dimension id="BEVT" position="3">
              <structure:ConceptIdentity>
                <Ref id="BEVT" maintainableParentID="CS_ALC" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
              </structure:ConceptIdentity>
              <structure:LocalRepresentation>
                <structure:Enumeration>
                  <Ref id="CL_ALC_BEVT" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist" />
                </structure:Enumeration>
              </structure:LocalRepresentation>
            </structure:Dimension>
            <structure:Dimension id="SUB" position="4">
              <structure:ConceptIdentity>
                <Ref id="SUB" maintainableParentID="CS_ALC" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
              </structure:ConceptIdentity>
              <structure:LocalRepresentation>
                <structure:Enumeration>
                  <Ref id="CL_ALC_SUB" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist" />
                </structure:Enumeration>
              </structure:LocalRepresentation>
            </structure:Dimension>
            <structure:Dimension id="FREQUENCY" position="5">
              <structure:ConceptIdentity>
                <Ref id="FREQ" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
              </structure:ConceptIdentity>
              <structure:LocalRepresentation>
                <structure:Enumeration>
                  <Ref id="CL_FREQ" version="1.0.0" agencyID="ABS" package="codelist" class="Codelist" />
                </structure:Enumeration>
              </structure:LocalRepresentation>
            </structure:Dimension>
            <structure:TimeDimension id="TIME_PERIOD" position="6">
              <structure:ConceptIdentity>
                <Ref id="TIME_PERIOD" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
              </structure:ConceptIdentity>
              <structure:LocalRepresentation>
                <structure:TextFormat textType="ObservationalTimePeriod" />
              </structure:LocalRepresentation>
            </structure:TimeDimension>
          </structure:DimensionList>
          <structure:AttributeList id="AttributeDescriptor">
            <!-- Removed attributes -->
          </structure:AttributeList>
          <structure:MeasureList id="MeasureDescriptor">
            <structure:PrimaryMeasure id="OBS_VALUE">
              <structure:ConceptIdentity>
                <Ref id="OBS_VALUE" maintainableParentID="CS_COMMON" maintainableParentVersion="1.0.0" agencyID="ABS" package="conceptscheme" class="Concept" />
              </structure:ConceptIdentity>
            </structure:PrimaryMeasure>
          </structure:MeasureList>
        </structure:DataStructureComponents>
      </structure:DataStructure>
    </structure:DataStructures>
  </message:Structures>
</message:Structure>
```

From the retrieved XML we can construct an idea of the dimensions:

- Type of Volume
- Measure
- Beverage Type
- Beverage Subtype/Strength
- Reporting frequency
- Time period

We'll leave time period off for now, as it's handled differently to other dimensions. But by looking at the codelists I can identify that I want the following values:

- Type of Volume: Volume of beverage, code value `1`
- Measure: Per capita apparent consumption (litres), code value `2`
- Beverage Type: Beer, code value `1`
- Beverage Subtype/Strength: Mid strength beer, code value `4`
- Reporting frequency: Annual, code value `A`

### Step 3: Getting the Data

All the above work means I can put together the following url to get data: [https://api.data.abs.gov.au/data/ALC/1.2.1.4.A](https://api.data.abs.gov.au/data/ALC/1.2.1.4.A).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- NSI Web Service v7.13.0.0 -->
<message:GenericData xmlns:message="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/message" xmlns:common="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/common" xmlns:footer="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/message/footer" xmlns:generic="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/data/generic" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   <message:Header>
      <message:ID>IREFd9f24ed4685d4679aa008c8cdbe8a150</message:ID>
      <message:Test>true</message:Test>
      <message:Prepared>2020-09-28T12:05:50</message:Prepared>
      <message:Sender id="NOT_CONFIGURED" />
      <message:Structure structureID="ABS_ALC_1_0_0" dimensionAtObservation="TIME_PERIOD">
         <common:StructureUsage>
            <Ref agencyID="ABS" id="ALC" version="1.0.0" />
         </common:StructureUsage>
      </message:Structure>
      <message:DataSetAction>Information</message:DataSetAction>
   </message:Header>
   <message:DataSet action="Information" structureRef="ABS_ALC_1_0_0">
      <generic:Attributes>
         <generic:Value id="UNIT_MEASURE" value="L" />
      </generic:Attributes>
      <generic:Series>
         <generic:SeriesKey>
            <generic:Value id="TYP" value="1" />
            <generic:Value id="MEA" value="2" />
            <generic:Value id="BEVT" value="1" />
            <generic:Value id="SUB" value="4" />
            <generic:Value id="FREQUENCY" value="A" />
         </generic:SeriesKey>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2001" />
            <generic:ObsValue value="0.46" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2002" />
            <generic:ObsValue value="0.45" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2003" />
            <generic:ObsValue value="0.49" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2004" />
            <generic:ObsValue value="0.54" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2005" />
            <generic:ObsValue value="0.52" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2006" />
            <generic:ObsValue value="0.54" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2007" />
            <generic:ObsValue value="0.56" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2008" />
            <generic:ObsValue value="0.56" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2009" />
            <generic:ObsValue value="0.57" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2010" />
            <generic:ObsValue value="0.57" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2011" />
            <generic:ObsValue value="0.57" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2012" />
            <generic:ObsValue value="0.57" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2013" />
            <generic:ObsValue value="0.58" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2014" />
            <generic:ObsValue value="0.61" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2015" />
            <generic:ObsValue value="0.58" />
         </generic:Obs>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2016" />
            <generic:ObsValue value="0.63" />
         </generic:Obs>
      </generic:Series>
   </message:DataSet>
</message:GenericData>
```

That’s data for all time periods, but we only want to know about the consumption in 2008. To return just the year we want, we're going to introduce a couple of query parameters to our query. Time period is treated differently to the other dimensions, you specify time period using the query parameters `startPeriod` and `endPeriod`. By setting startPeriod to 2008 and endPeriod to 2008 we can get back only one observation. So, we use [https://api.data.abs.gov.au/data/ALC/1.2.1.4.A?startPeriod=2008&endPeriod=2008](https://api.data.abs.gov.au/data/ALC/1.2.1.4.A?startPeriod=2008&endPeriod=2008):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<message:GenericData xmlns:message="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/message" xmlns:common="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/common" xmlns:footer="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/message/footer" xmlns:generic="http://www.sdmx.org/resources/sdmxml/schemas/v2_1/data/generic" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   <message:Header>
      <message:ID>IREF5a16a03e911d4196a9954a5455c36c3b</message:ID>
      <message:Test>true</message:Test>
      <message:Prepared>2020-09-28T12:09:40</message:Prepared>
      <message:Sender id="NOT_CONFIGURED" />
      <message:Structure structureID="ABS_ALC_1_0_0" dimensionAtObservation="TIME_PERIOD">
         <common:StructureUsage>
            <Ref agencyID="ABS" id="ALC" version="1.0.0" />
         </common:StructureUsage>
      </message:Structure>
      <message:DataSetAction>Information</message:DataSetAction>
   </message:Header>
   <message:DataSet action="Information" structureRef="ABS_ALC_1_0_0">
      <generic:Attributes>
         <generic:Value id="UNIT_MEASURE" value="L" />
      </generic:Attributes>
      <generic:Series>
         <generic:SeriesKey>
            <generic:Value id="TYP" value="1" />
            <generic:Value id="MEA" value="2" />
            <generic:Value id="BEVT" value="1" />
            <generic:Value id="SUB" value="4" />
            <generic:Value id="FREQUENCY" value="A" />
         </generic:SeriesKey>
         <generic:Obs>
            <generic:ObsDimension id="TIME_PERIOD" value="2008" />
            <generic:ObsValue value="0.56" />
         </generic:Obs>
      </generic:Series>
   </message:DataSet>
</message:GenericData>
```

There we go! The answer is `0.56`. But 0.56 of what? We could infer this from the measure dimension “Per capita apparent consumption (litres)” but there’s a better way - attributes. Attributes provide additional information about the data and we can see one here with the id `UNIT_MEASURE`. We can head back to the XML we got for the DSD to work out what it is, and what the value of `L` means. Doing so tells us that UNIT_MEASURE is the "Base unit in which the observation value is expressed" and `L` means "Litres”.

So, finally we have our answer: In 2008 the average per person consumption of mid strength beer was 0.56L (of pure alcohol).

### Summary

Call [https://api.data.abs.gov.au/dataflow](https://api.data.abs.gov.au/dataflow) to find our dataflow is `ALC`, with agency `ABS`.

Call [https://api.data.abs.gov.au/datastructure/ABS/ALC?references=children](https://api.data.abs.gov.au/datastructure/ABS/ALC?references=children) so we get the DSD and all the codelists and conceptschemes.
You can also call [https://api.data.abs.gov.au/dataflow/ABS/ALC?references=descendants](https://api.data.abs.gov.au/dataflow/ABS/ALC?references=descendants) `descendants` will call references of references to any level.

Build the data call [https://api.data.abs.gov.au/data/ALC/1.2.1.4.A?startPeriod=2008&endPeriod=2008](https://api.data.abs.gov.au/data/ALC/1.2.1.4.A?startPeriod=2008&endPeriod=2008) to get the data. 


## Constructing more detailed data queries

### Explore Dataflow

The example above takes you through constructing an API request to return 1 observation. However, the ABS Data API has a lot of flexibility in how you can construct requests for the data you are interested in.

In this example we'll explore the Residential Dwellings dataflow which has the ID `RES_DWELL`. The URL below will return all information we need to know about the Residential Dwellings dataflow to construct our data request.

[https://api.data.abs.gov.au/dataflow/ABS/RES_DWELL?references=descendants](https://api.data.abs.gov.au/dataflow/ABS/RES_DWELL?references=descendants)

### Use "+" to request multiple dimension members

Once you know the Dataflow ID, Codelists, Concepts, dimensions and dimension position for the data you want to call, you can construct a GET Data request. 

The OR operator is supported using the `+` character. The RES_DWELL dataflow has three dimensions; Measure, Region, Frequency. To use the OR operator we specify two or more codes for one dimension separated by +.

In this example we will request the Number of established house transfers for two regions; Sydney and the Rest of NSW:
- Measure - we will request code `1` for Number of Established House Transfers
- Region - we will request two codes `1GSYD+1RNSW` for Greater Sydney and Rest of NSW respectively
- Frequency - we will request `Q` for Quarterly
- startPeriod and endPeriod - we will request data from the fourth quarter of 2019 to the first quarter of 2020 inclusive

This data request looks like:
[https://api.data.abs.gov.au/data/ABS,RES_DWELL/1.1GSYD+1RNSW.Q?detail=Full&startPeriod=2019-Q4&endPeriod=2020-Q1](https://api.data.abs.gov.au/data/ABS,RES_DWELL/1.1GSYD+1RNSW.Q?detail=Full&startPeriod=2019-Q4&endPeriod=2020-Q1)

### Use wildcards to request all dimension members

Wildcarding is supported by specifying no codes for a given dimension. This will return all available observations for that dimension. 

To request all members of the Region dimension, replace the region codes `1GSDY+1RNSW` with an empty string as a wildcard for the Region dimension: [https://api.data.abs.gov.au/data/ABS,RES_DWELL/1..Q?startPeriod=2019-Q4&endPeriod=2020-Q1](https://api.data.abs.gov.au/data/ABS,RES_DWELL/1..Q?startPeriod=2019-Q4&endPeriod=2020-Q1)   

This will return the same result as if you had specified all Region IDs separated by the plus character: [https://api.data.abs.gov.au/data/ABS,RES_DWELL/1.1GSYD+1RNSW+2GMEL+2RVIC+3GBRI+3RQLD +4GADE+4RSAU+5GPER+5RWAU+6GHOB+6RTAS+7GDAR+7RNTE+8ACTE.Q?startPeriod=2019-Q2&endPeriod=2020-Q1](https://api.data.abs.gov.au/data/ABS,RES_DWELL/1.1GSYD+1RNSW+2GMEL+2RVIC+3GBRI+3RQLD+4GADE+4RSAU+5GPER+5RWAU+6GHOB+6RTAS+7GDAR+7RNTE+8ACTE.Q?startPeriod=2019-Q2&endPeriod=2020-Q1) 

### Use "all" to request all data

To retrieve all (unfiltered) observations for, replace the entire dataKey expression with `all`: [https://api.data.abs.gov.au/data/ABS,RES_DWELL/all?startPeriod=2019-Q4&endPeriod=2020-Q1](https://api.data.abs.gov.au/data/ABS,RES_DWELL/all?startPeriod=2019-Q4&endPeriod=2020-Q1) 

Leaving the series key blank will default to all.


# Helpful Files & Links

## ABS Data API documentation files
[ABS Data API Swagger file](https://api.gov.au/github?path=abs~DataAPI.openapi.yaml) (right click and save as). 
This swagger file can be imported into open source API testing tools such as SoapUI or Postman.

ABS Data API Web Application Description Language (WADL) file: [https://api.data.abs.gov.au/application.wadl](https://api.data.abs.gov.au/application.wadl)

## Example files

### Available Dataflows
**ABS Data API list of available dataflows** Files containing a list of available dataflows and example data API calls. This list was current as of 06/10/2021.  We recommend calling https://api.data.abs.gov.au/dataflow/all?detail=allstubs for the most up to date list.
- [Excel](
https://github.com/apigovau/api-descriptions/raw/master/abs/files/ABS%20Data%20API%20-%20All%20Dataflows%2006.10.2021.xlsx) 
- [CSV](
https://github.com/apigovau/api-descriptions/raw/master/abs/files/ABS%20Data%20API%20-%20All%20Dataflows%2006.10.2021.csv)


## SDMX API plugins

### R
Read RESTful SDMX API into R:
- GitHub Resources: [https://github.com/mdequeljoe/readsdmx](https://github.com/mdequeljoe/readsdmx)
- R SDMX Tutorial: [https://cran.r-project.org/web/packages/rsdmx/vignettes/quickstart.html](https://cran.r-project.org/web/packages/rsdmx/vignettes/quickstart.html)

### Python
SDMX for Python data ecosystem: [https://pandasdmx.readthedocs.io/en/v1.0](https://pandasdmx.readthedocs.io/en/v1.0/)


# Troubleshooting

### Request returned too much data or took too long

ABS Data APIs have an API Gateway with the following limitations:
- Maximum response time is **30 seconds**.  Exceeding this results in a 504 error and the message "Endpoint request timed out".
- Maximum response size is **10MB**.  Exceeding this can result in a 504 or 500 error. 

**Solutions**

Make sure you are calling the API with compression enabled, ensure your headers include “Accept-Encoding: gzip, deflate, br”.  Compression reduces API response size significantly.  However, vary large data requests may still exceed the API gateway limit even with compression.

Wait for a minute or two and then repeat your request.  Requests that take longer than 30 seconds to serve are still cached by our system.  A subsequent, identical request may return a response. 

If you are still recieving an error after trying the above solutions, then you will need to break your data request into several smaller requests. The simplest way to do this is using the startPeriod and endPeriod query parameters to subset data on time range. Or use dataKey to subset data on dimension members (e.g. request all data for each region separately).  For information on this see: GET Data, Path Parameters, dataKey.

### Request URL too long

The maximum allowed length of the ‘dataKey’ section of the URL is **5,000 characters**. Requests that exceed this limit will return a 400 error. 

**Solution**

To work around the maximum allowed URL length, we recommend using wildcards to request all members of a given dimension rather than specifying the code for each dimension member individually in the URL. Simply remove all codes for a given dimension to request all members for that dimension.  For more information on this see: GET Data, Path Parameters, dataKey.

### Structures are not available

A request that used to return data or structures now returns a 404 error and a message that requested structures could not be found. 

**Solution**

The most likely cause for this error is an incorrect version number.  All structures in the ABS Data API have a version number e.g. "1.0.0".  If a structure has been updated to a new version and the old version removed, any requests to the old version will result in a 404 error and a message that the requested structures could not be found.  Old versions of data structures are removed to prevent access to out of date data.

Data structures and Dataflows may be updated to new versions occasionally.  We recommend not including version numbers in your data or data structure API requests to ensure you always return the latest version. 

Removing the version number from your requests will default to requesting the latest available version. E.g: https://api.data.abs.gov.au/datastructure/ABS/JV/ or https://api.data.abs.gov.au/data/ABS,JV,/all

