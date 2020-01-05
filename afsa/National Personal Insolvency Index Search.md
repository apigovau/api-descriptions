# Getting Started

The National Personal Insolvency Index (NPII) Search is a SOAP/XML web service that for a fee, anyone may conduct a search of to determine whether a debtor is currently, or has previously been, subject to provisions of the Bankruptcy Act 1966. 


## Business Context
The NPII is a permanent electronic register of personal insolvency proceedings in Australia maintained and updated by the Australian Financial Security Authority. The NPII contains records from August 1928.

The purpose of the NPII is to provide publicly available information regarding the insolvency status of individuals. 


The NPII contains details of:
- Creditors Petitions
- Debt Agreements
- Personal Insolvency Agreements
- Bankruptcies
- Insolvent Deceased Estates
- Control Orders and Authorities

The information available includes:

 - The name, date of birth (if known), address (suburb/state) and occupation of the person as disclosed on documents accepted by AFSA when the proceeding started
- Previous names and aliases if known
- The type of proceeding, the date it started and the administration number
- The name and business address of the trustee or administrator of the proceeding
- The current status of the person and/or the proceeding (e.g. whether a person is discharged from bankruptcy or whether a Creditors Petition for a person’s bankruptcy is in progress).

## Key Information

The NPII service is accessible using SOAP and uses XML for the message payload.

## Fees

The fees charged for NPII Search usage are [here](https://www.afsa.gov.au/insolvency/how-we-can-help/fees-and-charges-0#service-fees).


An organisation must become an ‘On Account’ customer with AFSA in order for usage of the service to be billed to the organisation accessing the service. The organisation must additionally complete the B2G channel [registration process](https://www.afsa.gov.au/online-services/b2g-system-integration/afsa-b2g-registration-steps).


# Authentication

A device AUSkey is required for authentication of NPII Search operation requests, see the [AFSA Web Services Gateway Guide](https://www.afsa.gov.au/sites/g/files/net1601/f/itsa-web-service-gateway-guide.pdf) for more information.


# Using the API

The operations in this service are described in the [NPII Search MIG](https://www.afsa.gov.au/sites/g/files/net1601/f/mig-npiisearch_v1.4.20150429.pdf) on the AFSA website.

