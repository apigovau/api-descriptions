# Getting Started

Debt agreements is a SOAP/XML web service allows practitioners to propose a debt agreement (DA), and any subsequent variations and/or termination to the agreement. This service, provided by the Australian Financial Security Authority (AFSA), allows creditors to vote on these proposals and additionally allows both practitioners and creditors to retrieve relevant correspondence.

# Business Context

A debt agreement (DA) is an option to assist debtors with unmanageable debt. The DA is a legally binding agreement between a debtor and their creditors where creditors agree to accept a sum of money that the debtor can afford. The debtor is released from their debts when they complete all payments and obligations under the agreement. Creditors vote on whether to accept or reject a DA proposal.



# Operations

## Operations for Practitioners

- SearchCreditors
- GetCreditor
- CreateCreditor
- GetDocuments
- SubmitDebtAgreementProposal
- SearchCorrespondence
- GetCorrespondence
- SubmitDebtAgreementVariation
- SubmitDebtAgreementTermination
- SubmitDebtAgreementCompletion
- SubmitDebtAgreementDefault

The business processes and operations are further described in the [Debt Agreements Service Practitioners MIG](/artefacts/MIG-DAServicePractitioners_v1.2.pdf)

## Operations for Creditors
- SearchCorrespondence
- GetCorrespondence
- GetDebtAgreementProposalCreditor
- SubmitDebtAgreementClaimAndVote
- GetDocuments

The business processes and operations specific to creditors are further described in the [Debt Agreements Service Creditors MIG](https://www.afsa.gov.au/sites/g/files/net1601/f/mig-daservicecreditors_v1.0.pdf) 

## Web services onboarding packs

The onboarding pack for practitioners is [here](https://www.afsa.gov.au/sites/g/files/net1601/f/webservices-onboarding-pack-all-20180701.zip)

The creditors onboarding pack is located on [the AFSA website](https://www.afsa.gov.au/sites/g/files/net1601/f/creditor-onboarding-pack-20180701.zip)

# Fees

For practitioners there is a [fee](https://www.afsa.gov.au/insolvency/how-we-can-help/fees-and-charges-0) for proposing a DA, but not for lodgement of subsequent DA forms or retrieval of correspondence.


For creditors, no fees apply to using this any operations in this service.

# Authentication

A device AUSkey is required for authentication of DA Service operation requests, see the [AFSA Web Services Gateway Guide](https://www.afsa.gov.au/sites/g/files/net1601/f/itsa-web-service-gateway-guide.pdf) for more information.

