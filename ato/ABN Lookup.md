**This is a prototype only**

Go [here](https://abr.business.gov.au/Tools/WebServices) or the real version of the ABN lookup servcie.

# Getting started
The ABN Lookup web services allow you to integrate ABN Lookup validation and data into your own applications. There are many uses for the web services, including:

- ABN validation and pre-fill on forms
- Keeping ABN details stored in your database up-to-date

Access to the ABN Lookup web services is free of charge.

The web services provide a number of search options that are not available through the ABN Lookup website. For example, through the web services you can:

- Request a list of ABNs for a selected postcode with optional filters such as date updated, date registered, etc.
- Control how many matching names are returned by the name search.
- Create advanced name search queries to better control search results

**Important:** We recommend that you avoid use of [trading names](https://abr.business.gov.au/Help/Glossary#tradingname) as these have not been updated since 2012 and will be retired in 2018.

## Limitations

The following answers to common questions will help you to understand whether the ABN Lookup web services will meet your needs.

**Can I update my ABN details using the ABN Lookup web services?**

No, you cannot update your ABN details or apply for an ABN using the ABN Lookup web services. For more information, see:

- [Update your ABN details](https://abr.gov.au/For-Business,-Super-funds---Charities/Updating-or-cancelling-your-ABN/Update-your-ABN-details/) 
- [Applying for an ABN](https://abr.gov.au/For-Business,-Super-funds---Charities/Applying-for-an-ABN/) 

## Information and resources


For more information about the ABN Lookup web services, see:

- [Schema (18 KB ZIP)](http://abn.business.gov.au/Downloads/AbnLookupSchema.zip) for a definition of the data returned by the web services. Note: The root document is **abrPublicPayloadSearchResults.xsd**
- [Sample code](http://abn.business.gov.au/Tools/SampleCode) for a number of different languages and platforms including Visual Basic, C#, Java, php, Python, Microsoft Access and Excel.

## Service description (WSDL)

The Web services Description Language (WSDL) is a language for describing web services and how to access them.

The ABN Lookup WSDL and test pages can be found at:

- [ABN Lookup web services](http://abr.business.gov.au/abrxmlsearch/abrxmlsearch.asmx)
- [ABN Lookup web services (RPC)](http://abr.business.gov.au/abrxmlsearchRPC/abrxmlsearch.asmx) for RPC style encoding

## JSON Support


We currently provide limited JSON support for ABN and name searching. A sample page and JavaScript are available from [http://abr.business.gov.au/json/](http://abr.business.gov.au/json/)

## Keep in touch

We use your contact details to keep you informed of new features and planned outages for the ABN Lookup web services. Please [contact us](http://abn.business.gov.au/ContactUs/Form) and provide your GUID and new contact details to help us keep in touch.

# Registration

To register for access to the ABN Lookup web services:

- read and understand the [limitations](Getting+started#limitations) of the web services below
- accept the [web services agreement](https://abr.business.gov.au/Tools/WebServicesAgreement) in order to complete the registration form.

We will process your application and e-mail you an authentication GUID (Globally Unique Identifier) which is required to access the ABN Lookup web services.
# Search by ABN
There are five versions of the _Search by ABN_ method. All versions are accessible using SOAP or Http get/post protocols. The _Search by ABN_ methods are:

1. SearchByABNv201408 - **latest**
 - Returns everything returned by SearchByABNv201205 plus DGR Item Number and ACNC registration where they exist
2. SearchByABNv201205
 - Returns everything returned by SearchByABNv200709 plus registered business names where they exist
3. SearchByABNv200709
 - Returns everything returned by SearchByABNv200506 plus superannuation specific information where it exists.
 - **Important**: Registered business names **not included** in response.
4. SearchByABNv200506
 - Returns everything returned by ABRSearchByABN plus tax concession information where it exists.
 - **Important**: Registered business names **not included** in response.
5. ABRSearchByABN
 - Original version released in 2004.
 - **Important**: Registered business names **not included** in response.

The table below describes the *Search by ABN* request.

#### Description of Search by ABN request

| Element | Comments
| --- | ---|
| Search String | ABN to search for |
| Include Historical Details | Valid values are "Y", "N". Use "Y" to include historical information in the response |
| Authentication GUID | The GUID provided when you registered for access to the web services |

The table below describes the validation rules for the *Search by ABN* request.

#### Validation rules for Search by ABN request

| Element | Comments | Exception Text |
| --- | ---| --- |
| Search String | Invalid identifier | Search string does not match search type: CurrentABN |
| Search String | ABN not found | No records found |
| Include Historical Details | Invalid value | The Include History flag must be "Y" or "N" |
| Authentication GUID | GUID not found | The Authentication GUID is invalid |
# Search by ASIC number

There are five versions of the _Search by ASIC_ number method. All versions are accessible using SOAP or Http get/post protocols. The _Search by ASIC_ methods are:

1. SearchByASICv201408 - **latest**
 - Returns everything returned by SearchByASICv201205 plus DGR Item Number and ACNC registration where they exist
2. SearchByASICv201205
 - Returns everything returned by SearchByASICv200709 plus registered business names where they exist
3. SearchByASICv200709
 - Returns everything returned by SearchByASICv200506 plus superannuation specific information where it exists.
 - **Important**: Registered business names **not included** in response.
4. SearchByASICv200506
 - Returns everything returned by ABRSearchByASIC plus tax concession information where it exists.
 - **Important**: Registered business names **not included** in response.
5. ABRSearchByASIC
 - Original version released in 2004.
 - **Important**: Registered business names **not included** in response.

The table below describes the *Search by ASIC( request.

#### Description of Search by ASIC request

| Element | Comments
| --- | ---|
| Search String | ACN to search for |
| Include Historical Details | Valid values are "Y", "N". Use "Y" to include historical information in the response |
| Authentication GUID | The GUID provided when you registered for access to the web services |

The table below describes the validation rules for the *Search by ASIC* request.

#### Validation rules for Search by ASIC request

| Element | Comments | Exception Text |
| --- | ---| --- |
| Search String | Invalid identifier | Search string does not match search type: ASIC |
| Search String | ACN not found | No records found |
| Include Historical Details | Invalid value | The Include History flag must be "Y" or "N" |
| Authentication GUID | GUID not found | The Authentication GUID is invalid |

# Search by Name

The _Search by Name_ methods return a list of names that match the search criteria. [Search tips](https://abr.business.gov.au/Help/SearchTips) has information to help you understand the ABN Lookup search.

There are five versions of the _Search by Name_ method. The _Simple Protocol_ versions have the same functionality as the corresponding _SOAP_ versions but can be called using Http get/post protocols. The _Search by Name_ methods are:

1. ABRSearchByNameAdvanced2017 (SOAP only) and ABRSearchByNameAdvancedSimpleProtocol2017 - **latest**
 - Includes _Advanced2012_ options plus the option to filter by active ABNs.
2. ABRSearchByNameAdvanced2012 (SOAP only) and ABRSearchByNameAdvancedSimpleProtocol2012
 - Includes _Advanced2006_ options plus the option to filter by registered business names.
3. ABRSearchByNameAdvanced2006 (SOAP only) and ABRSearchByNameAdvancedSimpleProtocol2006
 - Includes _Advanced_ options plus the option to specify the maximum number of matching records to return.
 - The default is 200 but can be any positive integer.
4. ABRSearchByNameAdvanced (SOAP only) and ABRSearchByNameAdvancedSimpleProtocol
 - Includes options to narrow the search and to limit the records returned based on a match score.
5. ABRSearchByName (SOAP only) and ABRSearchByNameSimpleProtocol

The table below describes the _Search by Name_ request.

#### Description of Search by Name request

| Element | Comments |
| --- | --- |
| Name | String to search for |
| Postcode| Match against postcode of the main business location |
| Name Type Filters legalName businessName tradingName | Restrict search to selected name types – i.e. Legal name and/or business name. Valid values are "Y" , "N" or blank. By default all Name Types are searched. If none of the options are "Y" (i.e. they are all "N" or blank), then all Name Types are included in the search |
| State Filters NSW, SA, ACT, VIC, WA, NT, QLD, TAS | Restrict search to selected states in the main business location. Valid values are "Y", "N" or blank. The default is to include all states. If none of the options are "Y" (i.e. they are either "N" or blank), all states are included |
| authenticationGUID | GUID provided when you registered for access to the web services |
| searchWidth | Defines the search strategy. Only available in the advanced name search. Valid values are "Typical" and "Narrow" |
| minimumScore | The lowest acceptable score. Only available in the advanced name search. Accepted values are positive integers between 50 and 100 |
| maxSearchResults | The maximum number of records to return from the search. Default is 200. Only available in the advanced 2006 name search. Must be a positive integer |
| activeABNsOnly | Restrict search to only active ABNs. Only available in the advanced 2017 name search. Valid values are "Y", "N" or blank |

The table below describes the validation rules for the _Search by Name_ request.

#### Validation rules Search by Name request

| Element | Error Condition | Exception Text |
| --- | --- | --- |
| Name | An ABN is entered | An ABN is invalid in a name search |
| Name | An ASIC number is entered | An ASIC Number is invalid in a name search |
| Name Type Filters | Invalid value – i.e. not "Y", "N" or blank | The name type element has an invalid value according to its data type |
| State Filters | Invalid value – i.e. not "Y", "N" or blank | The state element has an invalid value according to its data type |
| postcode | Exceeds 12 characters | The 'postcode' element has an invalid value according to its data type |
| authenticationGUID | GUID not found | The authenticationGUID is invalid |
| searchWidth | Invalid value | The 'searchWidth' element has an invalid value according to its data type |
| minimumScore | Invalid value | The 'minimumScore' element has an invalid value according to its data type |
| maxSearchResults | Invalid value | The 'maxSearchResults ' element has an invalid value according to its data type |

# Search with Filters

The *Search with Filters* methods return a list of matching ABNs. These methods are intended to be used with *Search by ABN* methods. The _Search with Filters_ methods are:

1. SearchByPostcode
1. SearchByABNStatus
1. SearchByUpdateEvent
1. SearchByRegistrationEvent
1. SearchByCharity

The table below describes the *Search with filters* request.

#### Description of Search with Filters request

| Element | Comments |
| --- | --- |
| postcode | Postcode of the main business location to filter on |
| state | State of the main business location to filter on |
| activeABNsOnly | Include only ABNs that are currently active. Valid values are "Y" or "N" |
| currentGSTRegistrationOnly | Include only ABNs that are currently registered for GST. Valid values are "Y" or "N" |
| entityTypeCode | Include only ABNs that belong to the selected entity type code. Valid values are a 3 letter [entity type code](Reference+data#entity-types) or blank. The default is to include all entity types |
| concessionTypeCode | Include only ABNs that are registered for the selected concession type code. Valid values are a 4 letter [concession type code](Reference+data#charity-tax-concessions) or blank. The default is to include all concession types |
| updateDate | Include only ABNs updated on this date. Must be a valid date in the form yyyy-mm-dd |
| month | Include only ABNs updated in this month (and year). Must be an integer between 1 and 12 |
| year | Include only ABNs updated in this year (and month). Must be an 4 digit integer representing a year between 1999 and the current year |
| authenticationGuid | The GUID provided when you registered for access to the web services |

The table below describes the validation rules for the _Search with Filters_ request.

#### Validation rules for Search with Filters request


| Element | Error Condition | Exception Text |
| --- | --- | --- |
| year | Invalid value – must be a integer between 1999 and the current year | Year must be between 1999 and *current year* |
| month | Invalid value – must be an integer between 1 and 12 | Month must be between 1 and 12 |
| authenticationGuid | GUID not found | The Authentication GUID is invalid |
# Web services response

The ABN Lookup web services return a _payload_ containing the original request and the search response. Depending on the request and the outcome, the body of the response contains one of the following:

- If the request was successful:
 - [Business entity](#business-entity)
 - [Search results list](#search-results-list)
 - [ABN list](#abn-list)
- If the request was unsuccessful:
 - [Exception](Exceptions)

For more information refer to the [ABN Lookup web services schema (18 KB ZIP)](http://abn.business.gov.au/Downloads/AbnLookupSchema.zip).

## Business entity

**Important:** We recommend that you avoid use of [trading names](http://abn.business.gov.au/Help/Glossary#tradingname) as these have not been updated since 2012 and will be retired in 2018.

The _Search by ABN_ and _Search by ASIC_ methods return a Business entity. Below is a summary of the important rules associated with a Business entity.

- Broadly speaking, a Business entity is either an individual or a non-individual
- With the exception of the ABN and ABN status, all attributes are optional.
 - This is to handle suppressed ABN details.
 - The only information available on ABN Lookup for suppressed ABNs is the ABN, ABN status and GST and charity tax concession registrations
- There are 8 possible name types returned by the web services:
 - Legal Name – individuals only
 - Same as [entity name](http://abn.business.gov.au/Help/Glossary#entityname) on the ABN details screens
 - Main Name – non-individuals only
 - Same as [entity name](http://abn.business.gov.au/Help/Glossary#entityname) on the ABN details screens
 - Business Name – introduced May 2012
 - Main Trading Name
 - **Important:** [trading names](http://abn.business.gov.au/Help/Glossary#tradingname) have not been updated since 2012 and will be retired in 2018
 - Other Trading Name
 - **Important:** [trading names](http://abn.business.gov.au/Help/Glossary#tradingname) have not been updated since 2012 and will be retired in 2018
 - DGR Name (Deductible Gift Recipient)
 - PBI Employer Name (Public Benevolent Institution)
 - AWEF Name (Approved Worker Entitlement Fund)
- In the case of an individual
 - There is either zero (if ABN details have been suppressed) or one current Legal Name
 - There is no limit to the number of historical Legal Names
 - An individual will never have a Main Name
 - All other name types are optional and there is no limit to the number they may have either currently or historically
- In the case of a non-individual
 - There is either zero (if ABN details have been suppressed) or one current Main Name
 - There is no limit to the number of historical Main Names
 - A non-individual will never have a legal name
 - All other name types are optional and there is no limit to the number they may have either currently or historically

A summary of the relationship between name types and individuals/non-individuals for current and historical information is shown below.

#### Name types for Individuals

| Name Type |Current| Historical|
|---|---|---|
|Legal name|0 or 1|0 - many|
|Main name|n/a|n/a|
|Business name|0 – many|0 – many|
|Main trading name|0 – 1|0 – many|
|Other trading name|0 – many|0 – many|
|DGR fund Name|0 - many|0 - many|
|PBIE Name|0 - many|0 - many|
|AWEFName|0 - many|0 - many|

#### Name types for Non-individuals

|Name Type|Current|Historical|
|---|---|---|
|Legal name|n/a|n/a|
|Main name|0 - 1|0 - many|
|Business name|0 - many|0 - many|
|Main trading name|0 – 1|0 – many|
|Other trading name|0 – many|0 – many|
|DGR fund Name|0 - many|0 - many|
|PBIE Name|0 - many|0 - many|
|AWEFName|0 - many|0 - many|

## Search results list

The *Search By Name* methods return zero or more matching names ordered by relevance. Each match is awarded a score based on how closely it matches the search criteria. The best results are achieved by entering as much of the name as you know. For more information see [Search tips](http://abn.business.gov.au/Help/SearchTips).

A matching record contains:

- ABN and status
- Matching name, current/historical indicator and match score
- State and postcode of the main business location

## ABN list

The _Search with Filters_ methods return zero or more ABNs. As only the ABN is returned through the _Search with Filters_ methods you will need to call the *Search by ABN* methods to retrieve the ABN details.

## ABN test cases

The following table of ABNs are useful test cases as they represent some of the more extreme examples in the database.

#### ABN test cases

|Description|ABN|
|---|---|
|Suppressed ABN|34 241 177 887|
|Replaced ABN|30 613 501 612|
|Re-issued ABN|49 093 669 660|
|Multiple addresses|33 531 321 789|
|Multiple GST status|76 093 555 992|
|Multiple ABN status|53 772 093 958|
|Many name types|85 832 766 990|
|Main DGR status|56 006 580 883|
|DGR funds with historical names|78 345 431 247|
|Tax concession information|48 212 321 102|
|Superannuation fund|12 586 695 715|

# Exceptions

Exceptions returned by the ABN Lookup web services fall into a number of categories:

- http errors such as:
 - Page not found (404)
 - Internal server error (500)
- Application errors such as:
 - Invalid ABN number
 - No records found
 - Timeout
 - Too many records found
 - Unrecognised user

If an Application error occurs, the payload will contain an exception consisting of an exceptionCode and an exceptionDescription. Codes and descriptions that may be returned include:

#### Exception Codes and Descriptions

|Code|Description examples|
|---|---|
|GENERAL|Unknown name search error|
|GENERAL|Error starting search|
|GENERAL|Error finishing search|
|GENERAL|Error opening session|
|GENERAL|Error closing session|
|GENERAL|Error opening IDS System|
|GENERAL|Error closing system|
|GENERAL|Error retrieving matching records from the Search|
|GENERAL|Error retrieving IDT field count|
|GENERAL|Error retrieving IDT field information|
|SEARCH|Unknown search error|
|SEARCH|Could not determine search type|
|SEARCH|Search text is invalid for name search|
|SEARCH|Unknown|
|SEARCH|Problem accessing ABN Lookup database.|
|SYSTEM|System problems have prevented this request from completing|
|WEBSERVICES|Unknown web services error|
|WEBSERVICES|Object to be transformed was not recognised - i.e not a Party, SearchResult or Request|
|WEBSERVICES|The Include History flag must be 'Y' or 'N'|
|WEBSERVICES|The GUID entered is not recognised as a Registered Party|
|WEBSERVICES|The Include History flag must be 'Y' or 'N'|
|WEBSERVICES|The party identified by this GUID has not been registered for the requested service|
|WEBSERVICES|Requested File does not exist either because it has not been published, it has been deleted, or the filename is incorrect|
|WEBSERVICES|At least one Name Type must be selected (Y). Blank defaults to Y|
|WEBSERVICES|At least one State must be selected (Y). Blank defaults to Y|
|WEBSERVICES|A GUID must be supplied to access service|
|WEBSERVICES|A Filename must be entered|
|WEBSERVICES|No name search criteria entered|
|WEBSERVICES|The flag must be 'Y' or 'N'|
|WEBSERVICES|Year must be between 1999 and 2018|
|WEBSERVICES|Month must be between 1 and 12|
|WEBSERVICES|The party identified by this GUID has not been registered to access the superannuation download file|
|WEBSERVICES|The party identified by this GUID has not been registered to access the superannuation web services|
|WEBSERVICES|The registration for the party identified by this GUID has been cancelled|
|WEBSERVICES|State must be NSW, VIC, QLD, SA, TAS, ACT, NT, WA. For all states, leave blank|
|WEBSERVICES|Error logging to the statistical database|
|WEBSERVICES|Name search type cannot be determined|
|WEBSERVICES|External request is not valid according to scheme|
|WEBSERVICES|The service requested is not known|
|WEBSERVICES|The search method is not known|
|WEBSERVICES|An ABN is invalid in Name Search|
|WEBSERVICES|An ASIC number is invalid in Name Search|
|WEBSERVICES|An ACN is invalid in ABN Search|
|WEBSERVICES|An ABN is invalid in ACN Search|
# Reference data

Codes and descriptions used in the ABN Lookup web services are defined below.

## Charity Types

#### Charity type codes and descriptions

|Code|Description|
|--- |--- |
|CF|Charity|
|PBI|Public Benevolent Institution|
|HPC|Health Promotion Charity|
|CI|Charity|
|PBIE|Public Benevolent Institution Employer|
|TEF|Income Tax Exempt Fund|


## Charity Tax Concessions

#### Charity tax concession type codes and descriptions

|Code|Description|
|--- |--- |
|GSTC|GST Concessions|
|ITEC|Income Tax Exemption|
|FBTR|FBT Rebate|
|FBTE|FBT Exemption|

Codes and descriptions used in the ABN Lookup web services are defined below.

## Entity types

#### Entity type codes and descriptions

|Code|Description|
|--- |--- |
|ADF|Approved Deposit Fund|
|ARF|APRA Regulated Fund (Fund Type Unknown)|
|CCB|Commonwealth Government Public Company|
|CCC|Commonwealth Government Co-operative|
|CCL|Commonwealth Government Limited Partnership|
|CCN|Commonwealth Government Other Unincorporated Entity|
|CCO|Commonwealth Government Other Incorporated Entity|
|CCP|Commonwealth Government Pooled Development Fund|
|CCR|Commonwealth Government Private Company|
|CCS|Commonwealth Government Strata Title|
|CCT|Commonwealth Government Public Trading Trust|
|CCU|Commonwealth Government Corporate Unit Trust|
|CGA|Commonwealth Government Statutory Authority|
|CGC|Commonwealth Government Company|
|CGE|Commonwealth Government Entity|
|CGP|Commonwealth Government Partnership|
|CGS|Commonwealth Government Super Fund|
|CGT|Commonwealth Government Trust|
|CMT|Cash Management Trust|
|COP|Co-operative|
|CSA|Commonwealth Government APRA Regulated Public Sector Fund|
|CSP|Commonwealth Government APRA Regulated Public Sector Scheme|
|CSS|Commonwealth Government Non-Regulated Super Fund|
|CTC|Commonwealth Government Cash Management Trust|
|CTD|Commonwealth Government Discretionary Services Management Trust|
|CTF|Commonwealth Government Fixed Trust|
|CTH|Commonwealth Government Hybrid Trust|
|CTI|Commonwealth Government Discretionary Investment Trust|
|CTL|Commonwealth Government Listed Public Unit Trust|
|CTQ|Commonwealth Government Unlisted Public Unit Trust|
|CTT|Commonwealth Government Discretionary Trading Trust|
|CTU|Commonwealth Government Fixed Unit Trust|
|CUT|Corporate Unit Trust|
|DES|Deceased Estate|
|DIP|Diplomatic/Consulate Body or High Commissioner|
|DIT|Discretionary Investment Trust|
|DST|Discretionary Services Management Trust|
|DTT|Discretionary Trading Trust|
|FHS|First Home Saver Accounts Trust|
|FPT|Family Partnership|
|FUT|Fixed Unit Trust|
|FXT|Fixed Trust|
|HYT|Hybrid Trust|
|IND|Individual/Sole Trader|
|LCB|Local Government Public Company|
|LCC|Local Government Co-operative|
|LCL|Local Government Limited Partnership|
|LCN|Local Government Other Unincorporated Entity|
|LCO|Local Government Other Incorporated Entity|
|LCP|Local Government Pooled Development Fund|
|LCR|Local Government Private Company|
|LCS|Local Government Strata Title|
|LCT|Local Government Public Trading Trust|
|LCU|Local Government Corporate Unit Trust|
|LGA|Local Government Statutory Authority|
|LGC|Local Government Company|
|LGE|Local Government Entity|
|LGP|Local Government Partnership|
|LGT|Local Government Trust|
|LPT|Limited Partnership|
|LSA|Local Government APRA Regulated Public Sector Fund|
|LSP|Local Government APRA Regulated Public Sector Scheme|
|LSS|Local Government Non-Regulated Super Fund|
|LTC|Local Government Cash Management Trust|
|LTD|Local Government Discretionary Services Management Trust|
|LTF|Local Government Fixed Trust|
|LTH|Local Government Hybrid Trust|
|LTI|Local Government Discretionary Investment Trust|
|LTL|Local Government Listed Public Unit Trust|
|LTQ|Local Government Unlisted Public Unit Trust|
|LTT|Local Government Discretionary Trading Trust|
|LTU|Local Government Fixed Unit Trust|
|NPF|APRA Regulated Non-Public Offer Fund|
|NRF|Non-Regulated Superannuation Fund|
|OIE|Other Incorporated Entity|
|PDF|Pooled Development Fund|
|POF|APRA Regulated Public Offer Fund|
|PQT|Unlisted Public Unit Trust|
|PRV|Australian Private Company|
|PST|Pooled Superannuation Trust|
|PTR|Other Partnership|
|PTT|Public Trading trust|
|PUB|Australian Public Company|
|PUT|Listed Public Unit Trust|
|SAF|Small APRA Regulated Fund|
|SCB|State Government Public Company|
|SCC|State Government Co-operative|
|SCL|State Government Limited Partnership|
|SCN|State Government Other Unincorporated Entity|
|SCO|State Government Other Incorporated Entity|
|SCP|State Government Pooled Development Fund|
|SCR|State Government Private Company|
|SCS|State Government Strata Title|
|SCT|State Government Public Trading Trust|
|SCU|State Government Corporate Unit Trust|
|SGA|State Government Statutory Authority|
|SGC|State Government Company|
|SGE|State Government Entity|
|SGP|State Government Partnership|
|SGT|State Government Trust|
|SMF|ATO Regulated Self-Managed Superannuation Fund|
|SSA|State Government APRA Regulated Public Sector Fund|
|SSP|State Government APRA Regulated Public Sector Scheme|
|SSS|State Government Non-Regulated Super Fund|
|STC|State Government Cash Management Trust|
|STD|State Government Discretionary Services Management Trust|
|STF|State Government Fixed Trust|
|STH|State Government Hybrid Trust|
|STI|State Government Discretionary Investment Trust|
|STL|State Government Listed Public Unit Trust|
|STQ|State Government Unlisted Public Unit Trust|
|STR|Strata-title|
|STT|State Government Discretionary Trading Trust|
|STU|State Government Fixed Unit Trust|
|SUP|Super Fund|
|TCB|Territory Government Public Company|
|TCC|Territory Government Co-operative|
|TCL|Territory Government Limited Partnership|
|TCN|Territory Government Other Unincorporated Entity|
|TCO|Territory Government Other Incorporated Entity|
|TCP|Territory Government Pooled Development Fund|
|TCR|Territory Government Private Company|
|TCS|Territory Government Strata Title|
|TCT|Territory Government Public Trading Trust|
|TCU|Territory Government Corporate Unit Trust|
|TGA|Territory Government Statutory Authority|
|TGE|Territory Government Entity|
|TGP|Territory Government Partnership|
|TGT|Territory Government Trust|
|TRT|Other trust|
|TSA|Territory Government APRA Regulated Public Sector Fund|
|TSP|Territory Government APRA Regulated Public Sector Scheme|
|TSS|Territory Government Non-Regulated Super Fund|
|TTC|Territory Government Cash Management Trust|
|TTD|Territory Government Discretionary Services Management Trust|
|TTF|Territory Government Fixed Trust|
|TTH|Territory Government Hybrid Trust|
|TTI|Territory Government Discretionary Investment Trust|
|TTL|Territory Government Listed Public Unit Trust|
|TTQ|Territory Government Unlisted Public Unit Trust|
|TTT|Territory Government Discretionary Trading Trust|
|TTU|Territory Government Fixed Unit Trust|
|UIE|Other Unincorporated Entity|


## Superannuation compliance

#### Self-managed super fund (SMSF) compliance codes and descriptions

|Code|Description|
|--- |--- |
|P|Election to be regulated is being processed|
|R|Registered - status not determined|
|Y|Complying|
|N|Non-complying|
|E|Regulation details removed|
|I|Regulation details withheld|
|S|Regulation details withheld|


#### Other Unincorporated Entity

|Code|Description|
|--- |--- |
|NONREGULATED|Non-regulated|
|EXEMPT|Exempt from regulation|
|APRA|Registered|
|NOTAPRAREGISTERED|Not registered with APRA|


## Superannuation regulators

#### Superannuation regulator codes and descriptions

|Code|Description|
|--- |--- |
|ATOREGULATED|ATO Regulated Funds|
|APRAREGULATED|APRA Regulated Funds|
|NONREGULATED|Non Regulated Fund|
|EXEMPT|Exempt from regulation|

# Data dictionary

The tables below summarise the field lengths and data types of the ABN attributes returned by the ABN Lookup web services. For more information refer to the [ABN Lookup web services schema (18 KB ZIP)](/Downloads/AbnLookupSchema.zip).

**Important:** We recommend that you avoid use of Trading Names (mainTradingName and otherTrading Name) as these have not been updated since 2012 and will be retired in 2018.



#### request/identifierSearchRequest

|Element Name|Max Length|Type|
|--- |--- |--- |
|authenticationGUID|50|string|
|identifierType|4|string|
|identifierValue|11|numeric|
|history|1|string|

#### response

|Element Name|Max Length|Type|
|--- |--- |--- |
|dateRegisterLastUpdated|10|date|
|dateTimeRetrieved|33|datetime|

#### businessEntity

|Element Name|Max Length|Type|
|--- |--- |--- |
|recordLastUpdatedDate||date|
|ABN/identifierValue|11|numeric|
|ABN/isCurrentIndicator|1|string|
|ABN/replacedIdentifierValue|11|numeric|
|ABN/replacedFrom|10|date|
|entityStatus/entityStatusCode|9|string|
|entityStatus/effectiveFrom|10|date|
|entityStatus/effectiveTo|10|date|
|ASICNumber|9|numeric|
|entityType/entityTypeCode|4|string|
|entityType/entityDescription|100|string|
|goodsAndServicesTax/effectiveFrom|10|date|
|goodsAndServicesTax/effectiveTo|10|date|
|mainName/organisationName|200|string|
|mainName/effectiveFrom|10|date|
|mainName/effectiveTo|10|date|
|legalName/givenName|40|string|
|legalName/otherGivenName|100|string|
|legalName/familyName|40|string|
|legalName/effectiveFrom|10|date|
|legalName/effectiveTo|10|date|
|mainBusinessPhysicalAddress/stateCode|3|string|
|mainBusinessPhysicalAddress/postcode|12|string|
|mainBusinessPhysicalAddress/effectiveFrom|10|date|
|mainBusinessPhysicalAddress/effectiveTo|10|date|
|mainTradingName/organisationName|200|string|
|mainTradingName/effectiveFrom|10|date|
|mainTradingName/effectiveTo|10|date|
|otherTradingName/organisationName|200|string|
|otherTradingName/effectiveFrom|10|date|
|otherTradingName/effectiveTo|10|date|

#### businessEntity200506

|Element Name|Max Length|Type|
|--- |--- |--- |
|charityType/charityTypeDescription|100|string|
|charityType/effectiveFrom|10|date|
|charityType/effectiveTo|10|date|
|taxConcessionCharityEndorsement/endorsementType|100|string|
|taxConcessionCharityEndorsement/effectiveFrom|10|date|
|taxConcessionCharityEndorsement /effectiveTo|10|date|

#### businessEntity200709

|Element Name|Max Length|Type|
|--- |--- |--- |
|superannuationStatus/complyingCode|20|string|
|superannuationStatus/complyingDescription|50|string|
|superannuationStatus/regulator|15|string|
|mainPostalPhysicalAddress/stateCode|3|string|
|mainPostalPhysicalAddress/postcode|12|string|
|mainPostalPhysicalAddress/addressLine1|60|string|
|mainPostalPhysicalAddress/addressLine2|60|string|
|mainPostalPhysicalAddress/suburb|50|string|
|mainPostalPhysicalAddress/countryName|100|String|

#### businessEntity201205

|Element Name|Max Length|Type|
|--- |--- |--- |
|businessName/organisationName|200|string|
|businessName/effectiveFrom|10|date|
|businessName/effectiveTo|10|date|

#### businessEntity201408

|Element Name|Max Length|Type|
|--- |--- |--- |
|dgrEndorsement/itemNumber|15|string|
|ACNCRegistration/status|10|string|
|ACNCRegistration/effectiveFrom|10|date|
|ACNCRegistration/effectiveTo|10|date|

# Sample code and resources

Incorporating the ABN Lookup web services into your application is usually quite straight forward.

To help you get started, see:

- [Schema (18 KB ZIP)](http://abn.business.gov.au/Downloads/AbnLookupSchema.zip) for a definition of the data returned by the web services. Note: The root document is **abrPublicPayloadSearchResults.xsd**.
- [Sample code](http://abn.business.gov.au/Tools/SampleCode) for a number of different languages and platforms including Visual Basic, C#, Java, php, Microsoft Access and Excel.

## Service description (WSDL)

The Web services Description Language (WSDL) is a language for describing web services and how to access them.

The ABN Lookup WSDL and test pages can be found at:

- [ABN Lookup web services](http://abr.business.gov.au/abrxmlsearch/abrxmlsearch.asmx)
- [ABN Lookup web services (RPC)](http://abr.business.gov.au/abrxmlsearchRPC/abrxmlsearch.asmx) for RPC style encoding

# Change control

The ABN Lookup web services allow you to incorporate ABN information and search capabilities into your own applications.

Over time, the information recorded about an ABN may change due to amendments to the underlying legislation. To ensure existing ABN Lookup web service users are not adversely affected, changes to the structure of the information returned may be implemented through new web service methods

For example, in July 2005 new legislation was introduced that effected the information recorded about tax concessions. We implemented new methods for searching by ABN and ACN which extended the information returned to include tax concession information where it existed. The schema was updated to reflect these changes without invalidating the information already being returned through existing web service methods. Likewise in September 2007 we implemented a new method which extended the information returned to include superannuation funds information where it exists. Then in May 2012 a new method was released that extended the information returned to include registered business names.

If you are integrating the web services for the first time or upgrading your existing applications, we recommend that you use the latest web service method (eg: [Search by ABN](Search by ABN), [Search by ASIC number](Search by ASIC number), [Search by Name](Search by Name), [Search with Filters](Search with Filters)) available.
