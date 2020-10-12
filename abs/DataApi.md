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
- XML: "application/xml"
- Structure specific XML: "application/vnd.sdmx.structurespecificdata+xml"
- JSON: "accept: application/vnd.sdmx.data+json"
- CSV: "accept: application/vnd.sdmx.data+csv"
- CSV with labels for codelists: "accept: application/vnd.sdmx.data+csv;labels=both"




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

# Worked Examples

## Explore a dataset and construct a data request

### Introduction

The ABS DataAPIs provide machine-to-machine access to ABS data via an SDMX RESTful web service. There official [SDMX wiki](https://github.com/sdmx-twg/sdmx-rest/wiki) explains the standard in detail. SDMX is complex and can be difficult to learn, this tutorial aims to take you through the basics you need to know to use the ABS Data API.  We’ll touch on a number of SDMX concepts but the focus is on constructing a data request.

**Which Data?**

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

**FlowRef**

There's a few ways we can define the `flowRef`, but we'll be using the simplest one. From the first request we made above we know `Dataflow id=“ALC”` therefore our flowRef is `ALC`

**dataKey**

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

We could call the API to get each of the codelists in turn using URLs like [https://api.data.abs.gov.au/codelist/ABS/CL_ALC_TYP/1.0.0](https://api.data.abs.gov.au/codelist/ABS/CL_ALC_TYP/1.0.0) (we included the version number 1.0.0 here). But that means we have to make a call for every dimension. Let’s save time and use our first query parameter when getting structure information: `references`. This parameter lets us retrieve not just the specified structure from the API, but some of the structures it references as well. We're going to specify the value `codelist`, telling the API that we want to retrieve any codelists referred to by our DSD: [https://api.data.abs.gov.au/datastructure/ABS/ALC?references=codelist](https://api.data.abs.gov.au/datastructure/ABS/ALC?references=codelist):

```xml
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
```

We've worked out how to get the codelists that define the values each dimension can take, however, it’s not clear what the highlighted dimension is. To find this we need to look at the `concept` it refers to. The concept gives the dimension its meaning and its name. As codes are stored in codelists, concepts are stored in... conceptschemes (conceptlists would be too obvious). So, we want the DSD to get the dimensions and their order, the referenced concepts (via their conceptschemes) to work out what they are, and the referenced codelists (to work out what values we need).

We're going back to the `references` query parameter. Instead of `codelist`, we’ll use the value `children` to tell the API we want all directly-referenced structures (which will include both the codelists, and the conceptschemes). Finally, we have all the information we need. Our API call is [https://api.data.abs.gov.au/datastructure/ABS/ALC?references=children](https://api.data.abs.gov.au/datastructure/ABS/ALC?references=children) (Some codelists and conceptschemes we're not using removed for brevity):

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

Build the data call [https://api.data.abs.gov.au/dataflow/ABS/ALC?references=descendants](https://api.data.abs.gov.au/data/ALC/1.2.1.4.A?startPeriod=2008&endPeriod=2008) to get the data. 


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


# Troubleshooting

### Request returned too much data

The ABS data API has a maximum size allowed for all data responses of 10MB with compression applied. This translates to around 90MB of data once it is uncompressed. Requests that exceed this limit will return a 500 error.

To call the API with compression enabled, ensure your headers include “Accept-Encoding: gzip,deflate”

If you are exceeding the maximum size even with compression, then you will need to break your data request into several, smaller requests. The simplest way to do this is using the startPeriod and endPeriod query parameters to subset data on time range. Or use dataKey to subset data on dimension members (e.g. request all data for each region separately).  For information on this see: GET Data, Path Parameters, dataKey.

### Request URL too long

The maximum allowed length of the ‘dataKey’ section of the URL is 5,000 characters. Requests that exceed this limit will return a 400 error. 

To work around the maximum allowed URL length, we recommend using wildcards to request all members of a given dimension rather than specifying the code for each dimension member individually in the URL. For information on this see: GET Data, Path Parameters, dataKey.


