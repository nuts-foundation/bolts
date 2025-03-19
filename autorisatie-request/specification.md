# Technical specification authorization request

![](../.gitbook/assets/by-sa.png)

_This work is licensed under a_ [_Creative Commons Attribution-ShareAlike 4.0 International Licence_](https://creativecommons.org/licenses/by-sa/4.0/)_._

## 1. Introduction

The [functional description (nl)](overzicht.md) describes the process around an authorization request. This document aims to be a complete specification for developers to implement an authorization request.

### 1.1 Terminology

* Nuts — A community in Dutch healthcare for creating an open, decentralized infrastructure for the transfer of data in the medical domain. See [www.nuts.nl](http://www.nuts.nl).
* Bolt — A practical application of Nuts ideology and open standards to facilitate a use-case in healthcare.
* Custodian — The care organization that has patient data
* Subject — The patient giving consent.
* Actor — The care organization that requests access to patient data.

## 2. Service registration

A care organization that wants to process an authorization request MUST register a service according to [RFC006](https://nuts-foundation.gitbook.io/drafts/rfc/rfc006-distributed-registry) of the Nuts specification:

```javascript
{
  "id": "did:nuts:organization_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
  "type": "authorization-request", 
  "serviceEndpoint": {
    "oauth": "https://nuts.example.com/n2n/auth/v1/accesstoken",
    "consent": "https://api.example.com/requests"
  }
}
```

The `type` must be `authorization-request`. The `id` must be set according to RFC006. The `serviceEndpoint` must contain the fields `oauth` and `consent`. 
The `oauth` field must refer to an endpoint where the `serviceEndpoint` points to the `accesstoken` endpoint of an OAuth authentication server. The `consent` field must refer to an endpoint where the `serviceEndpoint` points to a URL that accepts the http POST message as specified in [§4](specification.md#4-consent-processing). Service references may be used instead of absolute endpoints as specified in [§4.1 of RFC006](https://nuts-foundation.gitbook.io/drafts/rfc/rfc006-distributed-registry#4.1-service-references).

## 3. Consent registration

### 3.1 NutsConsentCredential
The actor MUST create a Verifiable Credential containing the requested scope and proof of the consent. The exact contents of the credential is described in RFC018. Below is an example of a `NutsConsentCredential`.

```javascript
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://nuts.nl/credentials/v1"
  ],
  "id": "did:nuts:EgFjg8zqN6eN3oiKtSvmUucao4VF18m2Q9fftAeANTBd#90382475609238467",
  "type": ["VerifiableCredential", "NutsConsentCredential"],
  "issuer": "did:nuts:EgFjg8zqN6eN3oiKtSvmUucao4VF18m2Q9fftAeANTBd",
  "issuanceDate": "2010-01-01T19:53:24Z",
  "expirationDate": "2010-02-01T19:53:24Z",
  "credentialSubject": {
    "id": "did:nuts:EgFjg8zqN6eN3oiKtSvmUucao4VF18m2Q9fftAeANTBd",
    "custodian": "did:nuts:SjkuVHVqZndMVVJwcnUzbjhuZklhODB1M1M0LW9LcWY0WUs5S2",
    "purposeOfUse": "zorginzage",
    "subject": "123456780"
  },
  "proof": [
    {
      "proof": {
      "type": "EcdsaSecp256k1Signature2019",
      "created": "2010-01-01T19:53:24Z",
      "verificationMethod": "did:nuts:EgFjg8zqN6eN3oiKtSvmUucao4VF18m2Q9fftAeANTBd#234906587jfglout",
      "proofPurpose": "assertionMethod",
      "proofValue": "z58DAdFfa9SkqZMVPxAQpic7ndSayn1PzZs6ZjWp1CktyGesjuTSwRdoWhAfGFCF5bppETSTojQCrfFPP2oumHKtz"
    },
    {
      "type": "NutsPaperProof2022",
      "created": "2010-01-01T19:53:24Z",
      "expires": "2010-02-01T19:53:24Z",
      "description": "client signed document, filed under #803465."
    }
  ]
}
```

The DID of the actor MUST both be present in the `issuer` and `credentialSubject.id` fields. The `credentialSubject.custodian` field contains the DID of the custodian, which is also the endpoint owner the request will be sent to. The `purposeOfUse` field MUST correspond to one of the purposes of use stated by Bolts. The `subject` MUST contain the citizen service number. The credential contains two proofs. One is a regular proof from the issuer, the other is a `NutsPaperProof2022` which contains the `description` on how consent is given by the patient.  

The credential MAY NOT be published on the Nuts network. It stays at the Nuts node of the issuer.

### 3.2 VerifiablePresentation

The credential from the previous paragraph can be used by the actor to create a verifiable presentation. This presentation may contain additional credentials if needed. The presentation will be the body of the request. The request MUST be sent to the URL registered under the `consent` field of the `authorization-request` service of the `custodian`. 

The endpoint MUST be protected with mTLS as described by [RFC008](https://nuts-foundation.gitbook.io/drafts/rfc/rfc008-certificate-structure) of the Nuts specification. It also requires an access token as described in [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization). When requesting an access token, the `usi` and `vcs` fields are not needed. `authorization-request` MUST be entered in the `purposeOfUse` field. The access token request endpoint is registered under `oauth` in the  `authorization-request` service registration.

## 4. Consent processing

The custodian that receives the request MUST check the access token and the verifiable presentation validity. It can use the information in the `NutsConsentCredential` to identify the correct dossier and use the available information to determine if the request should be granted. This could also be a human process.

If the request is granted, the custodian MUST create a `NutsAuthorizationCredential` like the one below:

```javascript
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://nuts.nl/credentials/v1"
  ],
  "id": "did:nuts:SjkuVHVqZndMVVJwcnUzbjhuZklhODB1M1M0LW9LcWY0WUs5S2#903824934578638467",
  "type": ["VerifiableCredential", "NutsAuthorizationCredential"],
  "issuer": "did:nuts:SjkuVHVqZndMVVJwcnUzbjhuZklhODB1M1M0LW9LcWY0WUs5S2",
  "issuanceDate": "2010-01-01T19:54:24Z",
  "expirationDate": "2010-02-01T19:54:24Z",
  "credentialSubject":
  {
    "id": "did:nuts:EgFjg8zqN6eN3oiKtSvmUucao4VF18m2Q9fftAeANTBd",
    "legalBase": {
      "consentType": "explicit",
      "consentRef": "did:nuts:EgFjg8zqN6eN3oiKtSvmUucao4VF18m2Q9fftAeANTBd#90382475609238467"
    },
    "purposeOfUse": "zorginzage",
    "subject": "123456780"
  },
  "proof": {
    "type": "EcdsaSecp256k1Signature2019",
    "created": "2010-01-01T19:73:24Z",
    "verificationMethod": "did:nuts:SjkuVHVqZndMVVJwcnUzbjhuZklhODB1M1M0LW9LcWY0WUs5S2#234906587jfglout",
    "proofPurpose": "assertionMethod",
    "proofValue": z58DAdFfa9SkqZMVPxAQpic7ndSayn1PzZs6ZjWp1CktyGesjuTSwRdoWhAfGFCF5bppETSTojQCrfFPP2oumHKtz"
}
```

The following table shows how fields from the `NutsAuthorizationCredential` are mapped from the `NutsConsentCredential`:

| NutsAuthorizationCredential             | NutsConsentCredential          |
|-----------------------------------------|--------------------------------|
| issuer                                  | credentialSubject.custodian    |
| expirationDate                          | expirationDate                 |
| credentialSubject.id                    | issuer                         |
| credentialSubject.legalBase.consentType | explicit                       |
| credentialSubject.legalBase.consentRef  | id                             |
| credentialSubject.purposeOfUse          | credentialSubject.purposeOfUse |
| credentialSubject.subject               | credentialSubject.subject      |

The issuer of the `NutsAuthorizationCredential` may also limit the resources the credential gives access to using the `credentialSubject.resources` field. This is specific for a Bolt on how to do this.

The resulting credential is published by the issuer, the actor identified by `credentialSubject.id` MUST be one of the recipients. The node of the actor will receive the credential and will then be able to notify users.
