---
description: 'Disclaimer: This documentation has a DRAFT status!'
---

# Technical specification

![](../.gitbook/assets/by-sa.png)

_This work is licensed under a_ [_Creative Commons Attribution-ShareAlike 4.0 International Licence_](https://creativecommons.org/licenses/by-sa/4.0/)_._

## 1. Introduction

The [functional description (nl)](functionele_beschrijving.md) described how the different eTransfer concepts are mapped on Nuts concepts. This document aims to be a complete specification for developers to implement the eTransfer using Nuts components.

Where applicable, this document will refer to the [functional](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_Ontwerp_eOverdracht) and [technical](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht) design of the eTransfer as specified by Nictiz. It'll also refer to Nuts RFCs when required.

All chapters are normative unless otherwise stated. 

### 1.1 Terminology

* Nuts — A community in Dutch healthcare for creating an open, decentralized infrastructure for the transfer of data in the medical domain. See [www.nuts.nl](http://www.nuts.nl).
* Bolt — A practical application of Nuts ideology and open standards to facilitate a use-case in healthcare.
* Nursing handoff — The administrative process of transferring care for a patient from one care organization to another.
* eTransfer — The process of transfer of patient data to facilitate the nursing handoff.
* Custodian/sending party — The care organization from which the patient is being transferred. From an eTransfer perspective it's the organization that holds the requested patient data.
* Source system — The software system that holds the custodians data.
* Subject — The patient being transferred.
* Requesting/receiving party — The care organization that receives the patient and therefore has an information need from the custodian.
* Target system — The software system that holds the requesting parties data.
* Legal base — The legal base for breaking doctor-patient privilege.

## 2. Registration

Before any eTransfer can be made, each care organization and the software systems that support it must be registered. Registration allows other systems to find the correct endpoints.
A registration is required for both the source as the target system. Services are registered according to [RFC006](https://nuts-foundation.gitbook.io/drafts/rfc/rfc006-distributed-registry) of the Nuts specification. Services can be registered for vendors and/or care organizations as defined in [chapter 4](https://nuts-foundation.gitbook.io/drafts/rfc/rfc006-distributed-registry#4-services) of RFC006.

### 2.1 eOverdracht-sender service registration

The sending party must register a `eOverdracht-sender` service:

```javascript
{
  "id": "did:nuts:organization_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
  "type": "eOverdracht-sender", 
  "serviceEndpoint": {
    "oauth": "did:nuts:vendor_identifier/serviceEndpoint?type=production-oauth",
    "fhir": "did:nuts:vendor_identifier/serviceEndpoint?type=eOverdracht-sender-fhir"
  }
}
```

The `type` must be `eOverdracht-sender`. The `id` must be set according to RFC006. The `serviceEndpoint` must contain the fields `oauth` and `fhir`.
Both values must contain a reference to a service endpoint. 
The `fhir` field must refer to an endpoint where the `serviceEndpoint` points to the FHIR `[base]` endpoint as described by the Nictiz TO.
The `oauth` field must refer to an endpoint where the `serviceEndpoint` points to the `accesstoken` endpoint of an OAuth authentication server. 
The type in tje query field is free to choose. The reference from the previous example could point to the following examples:

```javascript
{
    "id": "did:nuts:vendor_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
    "type": "eOverdracht-sender-fhir", 
    "serviceEndpoint": "https://fhir.example.com/base"
}
```

```javascript
{
    "id": "did:nuts:vendor_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
    "type": "production-oauth",
    "serviceEndpoint": "https://nuts.example.com/n2n/auth/v1/accesstoken"
}
```

Note that the `eOverdracht-sender` service is registered per care organization whereas the `fhir` and `oauth` services are registered for the vendor. 

### 2.2 eOverdracht-receiver service registration

The receiver must register a `eOverdracht-receiver` service:

```javascript
{
    "id": "did:nuts:organization_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
    "type": "eOverdracht-receiver", 
    "serviceEndpoint": {
        "oauth": "did:nuts:vendor_identifier/serviceEndpoint?type=production-oauth",
        "notification": "did:nuts:vendor_identifier/serviceEndpoint?type=eOverdracht-receiver-notify"
    }
}
```

With the referenced `notification` service as:

```javascript
{
    "id": "did:nuts:vendor_identifier#F1Dsgwngfdg3SH6TpDv0Ta1aOE",
    "type": "eOverdracht-receiver-notify",
    "serviceEndpoint": "https://notify.example.com/specific/path"
}
```

This last service must point to the `[base]` path where notification are to be received for the `Tak` resource.
The next chapter will describe the notification mechanism. The path must not be registered with a trailing `/`.

## 3. Security

Any patient data is requested in a certain context. This context is essential for security and acts as input for the access policy:

1. What is the identity of the user?
2. From which party is the request coming?
3. From which target system in the request coming?
4. Which custodian makes the data available?

These point are covered by [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization).

### 3.1 Secured connection

All requests must be done over a mTLS connection. We'll make use of PKIoverheid certificates according to [RFC008](https://nuts-foundation.gitbook.io/drafts/rfc/rfc008-certificate-structure). This includes both FHIR and authorization requests.

### 3.2 Authentication & authorization

The source system has the responsibility to protect the data of the custodian. It's therefore important to have a decent authentication and authorization
policy. This chapter describes the OAuth flow to be used in context of this Bolt \[[RFC6749](https://tools.ietf.org/html/rfc6749)\]. The complete Nuts authorization model is specified in [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization). The mandatory access policy is described in chapter TODO.

To obtain an access token, OAuth 2.0 offers several flows. OAuth flows typically redirect a user to a login portal where he/she can enter its credentials. This can't be used in a Nuts context since the source system does not have user accounts for users of the target system. Nuts uses a cryptographic signed identity of the user that can be checked by the other system. To pass this information in the access token request, the flow from [RFC7523](https://tools.ietf.org/html/rfc7523) is used.

The request contains:

* The identity of the user
* A list of verifiable credentials that prove access is granted to certain resources
* A limitation in time
* A limited scope
* A digital signature from the requesting system

The information in the request combined with upfront knowledge on organizational public keys proves that the combination of user, source system and target system are related and authorized to exchange resources. Upon verification, an access token is returned. This access token can be used in requests for actual resources. 

The list of verifiable credentials passed in the access token request are issued by the source system and prove a legal base is present for exchanging data. Multiple of these credentials can be combined to provide access to a larger set of resources.

The diagram below shows the access token request in a sequence diagram. The individual steps are described below the diagram.

![](../.gitbook/assets/etransfer-oauth.png)

**1-2** The custodian searches for a particular service in the Nuts node. The service contains multiple endpoints of which one would be `oauth`. The `oauth` endpoint is used to request the access token.

**3-6** The custodian requests an access token from the receiving system. In the diagram this is done by the Nuts node. This does not need to be the case.
The sender system could do the request itself and the receiver could implement its own authorization server. Steps **4-5** are described in [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization). Steps **3,6** are specific for the node implementation. The access token request does not require the presence of a user identity (`usi` field) and verifiable credentials (`vcs` field). This requirement depends on the request being made.

### 3.3 Logging

The access token represents a specific user and requesting party. These identities are either captured within the access token or stored at the authorization server. In the case the Nuts node is used, the identities are stored within the access token. The availablility of these identities allows the source system to conform to the NEN7513 logging requirements. 

#### 3.4 Service authorization

[RFC014](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential) states that each Bolt must define an access policy. The access policy describes how the authorization credentials must be constructed and how these give access to resources. Only the source system is required to create authorization records. The `purposeOfUse` for these credentials must be `eOverdracht-sender`. Chapter TODO further describes how access to resources should be granted.

## 4. Notification

Notifying target systems is the first step in the data exchange process. Notifying the receiving party triggers their users to retrieve new or updated data for a nursing handoff.
Notifications are required for both the _AdvanceNotice_ and _NursingHandoff_ messages. The notification for the _AdvanceNotice_ should trigger users to evaluate the request for care.
The notification for the _NursingHandoff_ tells users additional data is available.

It's also required to track the state of the process. Each offer must therefore have its own status: is it new, under evaluation or declined? The eTransfer would eventually be assigned to a single party.

### 4.1 Task resource

The [task resource](https://www.hl7.org/fhir/task.html) is used to track the progress of an eTransfer. The custodian is responsible for registering the task.
If a custodian offers an eTransfer to multiple parties, it must create a task for each party. The custodian is responsible for the assignment of the eTransfer to a single party.
Because of this responsibility and becuase of the principle to keep data at the source, the source system will store the task resources.
The target system will get notified and will be able to retrieve the task. The source system must send a notification for each change to the task resource.

### 4.2 Task state change

[§3.4](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht) of the Nictiz technical design describes the basic workflow and state changes of the task resource.
The diagrams only specifies the happy flow. Some state changes are missing in the design. The diagram belows shows the complete set of state changes.

<img src="../.gitbook/assets/etransfer-states.png" width="400">

The task resource can contain a reference to the _AdvanceNotice_ as well as the _NursingHandoff_. If the state changes to `in-progress`, the _NursingHandoff_ can be added to the task.
This eliminates the requirement for an additional task resource.

The different process step in the nursing handoff require certain access to data. In parallel different state of the task resource shall grant access to different resources:

* Requested    → access to AdvanceNotice
* Accepted    → access to AdvanceNotice
* Rejected    → all access revoked
* Cancelled    → all access revoked
* In-progress → access to NursingHandoff
* Completed    → all access revoked

### 4.3 Notification protocol

The FHIR STU3 notification and [subscriptions](http://hl7.org/fhir/STU3/subscription.html) model do not support the requirements for this bolt.
Without a notification for a specific Task, a FHIR *search* operation will have to be performed.
Task resource in a FHIR database can only be related to FHIR *Organizations*.
The security context from which a request is made does not contain these resources.
Therefore, the choice has been made to include the Task identifier in the notification path.
This shall be reevaluated when FHIR version 5 is adopted.
The Task notification endpoint is specific for this bolt and should not be reused for other purposes.

When a task is added to the source system, a notification \(empty POST according to FHIR documentation\) is sent to the registered endpoint of the target system.
This signals the target system to fetch the FHIR task resource. This resource may be fetched without an authenticated user. 
This means that the task resources may not contain any personal information (or references that can uniquely identify a person).

The endpoint must be secured according to [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization).

The diagram below shows the notification process in a sequence diagram. The individual steps are described below the diagram.

![](../.gitbook/assets/etransfer-notification.png)

**1-2** The custodian searches for the `eOverdracht-receiver` service in the Nuts node. The service contains two endpoints: `notification` and `oauth`. The `oauth` endpoint is used to request the access token. The `notification` endpoint is used to send the notification to.

**3** The custodian requests an access token from the receiving system. Details are described in §3.2. The access token request does not require the presence of a user identity (`usi` field) and verifiable credentials (`vcs` field).

**4** The sender system sends the notification to the notification endpoint using an empty HTTP POST message. The base endpoint is listed under the `notification` field of the `eOverdracht-receiver` service of the receiving organization. The complete path for the notification consists of the _base_ endpoint and the task identifier. For example: when an organization has published the notification endpoint under `https://prod.example.com/fhir/notification` and the Task identifier is `7EA74998-6A5F-4455-B35E-B6D36B9A0EA3`, the complete call would be:

```
POST `https://prod.example.com/fhir/notification/7EA74998-6A5F-4455-B35E-B6D36B9A0EA3`
Authorization: bearer 029345u8dksfg20...423509uifrlfkj==
Accept: Application/fhir+json
```

The access token from step 3 is added to the `Authorization` header as bearer token. The `Accept` header must be set correctly for a FHIR API call.

**5-6** The receiving system validates the call. It checks the access token and makes sure the notification is intended for one of its customers. It may do additional validations if needed. In the diagram the validation is handled by the Nuts node.

**7** If all is well, the receiving system must answer with a `202 Accepted` HTTP status code. If something went wrong, the receiving system may return a `40x` or `50x` HTTP status code. When returning a `400` status code, a body may be included. If a body is included, this must be a FHIR STU3 [OperationOutcome](http://hl7.org/fhir/STU3/operationoutcome.html). The `Content-Type` header must be set accordingly.

### 5. Task resource

A task resource is used to track the workflow state between the sending and receiving party. For each potential receiving party a task resource is created. [§4.2](specification.md#42-task-state-change) describes the possible state changes. For each change to the task resource, the sending system will send a notification. The receiving system may update the state of the task by sending an update to the sender system.

#### 5.1 Retrieval

After the receiving system has received the notification, it can retrieve the task resource. In the access token request, the correct [Nuts Authorization Credential](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential) needs to be included. This credential can easily be found by searching on the following fields:

* The `issuer` must be equal to the DID of the sending party. This DID was included in the access token used for the notification.
* `credentialSubject.id` must contain the DID of the receiving party.
* `credentialSubject.purposeOfUse` must equal `eOverdracht-sender`.
* `credentialSubject.resources.#.Path` must contain the specific task: `/Task/[id]`. Where the `id` is taken from the notification URL.

The URL for retrieving the task resource will then be:

```text
GET [base]/Task/[id]
```

Where `[base]` needs to be replaced with the path as registered under the `fhir` field in the `eOverdracht-sender` service of the sending party.

The sequence diagram below shows the flow followed by the receiving system after a notification has been received.

![](../.gitbook/assets/etransfer-task-retrieval.png)

**1-2** The receiving system searches for the `eOverdracht-sender` service in the Nuts node. The service contains two endpoints: `fhir` and `oauth`. The `oauth` endpoint is used to request the access token. The `fhir` endpoint is used to retrieve the task. Note that the flow is started by the receiving system, not the user. This means that no user authentication is required to retrieve the task.

**3-4** The receiving system finds the relevant Nuts authorization credential using the information described previously. If the sending system created the authorization credential correctly and if it has been received by the receiving node, it'll be found. The notification and synchronization of credentials are done in parallel so it's possible that the notification arrives before the credential has arrived.

**5** The found credential is used in requesting an access token.

**6-9** After receiving the access token, the receiving system van fetch the task resources. The sender system will validate the access token and check if the requested resources is listed in the provided authorization credential. If there's a match on resource and if the access token is valid, the task may be returned. If something went wrong, the receiving system may return a `40x` or `50x` HTTP status code. When returning a `40x` status code, a body may be included. If a body is included, this must be a FHIR STU3 [OperationOutcome](http://hl7.org/fhir/STU3/operationoutcome.html). The `Content-Type` header must be set accordingly.

#### 5.2 Update

As described earlier, the task resource is used to track the workflow state between sender and receiver. It's expected from the receiving system to update the task resource. There's a limit to what the receiving system may update on the task. The [Nictiz TO §3.4](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht#FHIR_profiles) lists the expected changes. The receiving system will change the state of the task from `requested` to `on-hold`, `accepted` or `rejected` and from `in-progress` to `completed`. The other state changes can only be made by the sending system. The receiving system may only update the `status` field and the `output:alternativeDate` field when changing state to `on-hold`, no other changes may be made or accepted by the sender system. The complete sequence diagram is below. It's almost identical to the retrieval diagram, instead of retrieving the task a **7** the task is updated through a PUT operation. After the update succeeded, the sending system is also required to send a new notification (**11**).

![](../.gitbook/assets/etransfer-task-update.png)

### 6. Data retrieval

### 7. Advance Notice

### 8. Nursing handoff

### 4.2 Data uitwisselingen


#### 4.2.2 Inhoud

Voor de eOverdracht is door Nictiz een lijst met zorg-informatie-bouwstenen \(ZIBs\) gedefinieerd. Deze bouwstenen zijn een feitelijke omschrijving van alle informatie benodigd voor een overdracht.

De ZIBs beschrijven de benodigde data, maar beschrijven niet hoe deze door computersystemen kunnen worden uitgewisseld. Daarvoor maken we gebruik van HL7 FHIR. De profielen voor de eOverdracht worden door Nictiz beheerd en worden gepubliceerd op de website van [Simplifier](https://simplifier.net/Nictiz-STU3-eOverdracht). De mapping van een ZIB naar FHIR is vaak niet een-op-een. Zo wordt de ZIB Persoonsgegevens gemapt op de FHIR profielen: Patient, Coverage en Related Person. Zie voor de complete mapping van ZIBs op FHIR profielen voor de eOverdracht [deze tabel](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht#HCIMs). De set van FHIR resources die samen de overdracht vormen worden gebundeld in een [FHIR Composition](https://www.hl7.org/fhir/STU3/composition.html).

Volgens de specificatie is het mogelijk om persoonsgegevens op te nemen in het aanmeldbericht. Dit is niet altijd toegestaan vanuit de wetgeving. De verzendende partij moet hier rekening mee houden. Zie ook [§2.2.2](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_Ontwerp_eOverdracht) van het Nictiz functioneel ontwerp.



## 5. Implementatie

Het [functioneel](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_Ontwerp_eOverdracht) en [technisch](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht#FHIR_profiles) ontwerp van Nictiz, de Nuts specificaties en hoofdstuk 4 bevatten alle benodigde informatie voor het implementeren van de eOverdracht. De informatie is echter verspreid waardoor het moeilijk is om te bepalen wanneer nu welke actie vereist is. Dit hoofdstuk probeert daar zo goed mogelijk antwoord op te geven.

### 5.1 Aanbieden

Aan dit deel wordt gewerkt.

### 5.2 Accepteren/Verwerpen

Aan dit deel wordt gewerkt.

### 5.3 Ophalen overdrachtsbericht

Het ophalen van het overdrachtsbericht begint wanneer de verpleegkundige overdracht toegewezen is aan een enkele ontvangende partij. Het proces van de voorgaande paragrafen is afgerond of er is sprake van een rechtstreekse overdracht waarbij een cliënt bijvoorbeeld weer terugkeert in zorg na bijvoorbeeld een tijdelijke opname.

Voordat de ontvangende partij genotificeerd kan worden is het de taak van de bronhouder om zowel het overdrachtsbericht als de bijbehorende Task resource te prepareren. Het is de bronhouder vrij om deze van tevoren klaar te zetten of ze realtime te genereren. De inhoud van het overdrachtsbericht moet een FHIR Composition resource zijn volgens de definities van het [Nictiz TO](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht#FHIR_profiles). De Task resource zal gevuld moeten worden volgens het **eOverdracht-Task** FHIR profiel. Een verwijzing hiernaar is te vinden op de website van [Nictiz](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht#Task). Belangrijk hierbij is dat de `Task.status` en `Task.input:nursingHandoff` velden gevuld zijn.

Onderstaand flow diagram toont alle stappen van notificeren tot ophalen. Wanneer er gegevens uitgewisseld worden tussen _Sender_ en _Receiver_ gebeurt dit altijd over een verbinding die is beveiligd met mutual TLS. Zie ook [RFC008](https://nuts-foundation.gitbook.io/drafts/rfc/rfc008-certificate-structure) voor het gebruik van certificaten. De verschillende stappen worden in de volgende paragrafen uitgelegd.

![](../.gitbook/assets/etransfer.png)

### 5.3.1 Registreren autorisatie

**2-3** In één van de eerste stappen wordt het Nuts autorisatie record geregistreerd. Hiervoor dient [RFC014](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential) gevolgd te worden. Zie §6.2.2 voor de invulling van het autorisatie record.

**4.** Het Nuts netwerk zorgt voor de aflevering van het autorisatie record bij de juiste ontvangende Nuts node.

### 5.3.2 Notificatie

**5-6** De bronhouder zoekt in de Nuts node naar het endpoint om de Task notificatie naar toe te sturen. Het base endpoint bevindt zich in het `notification` veld van de `eOverdracht-receiver` service van de ontvangende organisatie. Het endpoint waar de notificatie heen moet is een combinatie van het _base_ endpoint en de task identifier, bijv: `base/7EA74998-6A5F-4455-B35E-B6D36B9A0EA3`. Zie ook [§4.1.2](leveranciersspecificatie.md#412-organisatie-endpoint-discovery)

**7.** De bronhouder vraagt een access token aan de Nuts node. Het token wordt aangevraagd in de context van de beide partijen en de bolt.

**8-10** De node vraagt volgens [RFC003](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization) een access token aan bij de authorization server van de ontvangende partij.

**11.** Er wordt een notificatie gestuurd middels een lege POST naar het endpoint vanuit stap 5. Het security token uit stap 10 wordt hierbij als autorisatie header meegestuurd. De `Accept` header moet gezet worden conform de eisen van een FHIR API call.

**12.** Het doelsysteem valideert het access token bij de Nuts node.

**14.** Bij een correct verwerking zal de ontvangende partij een `202 Accepted` teruggeven met een lege body. Bij een incorrecte verwerking kan de ontvangende partij een `40x` of `50x` HTTP status code teruggeven. Bij een `400` status code mag de ontvangende partij een body meegeven, indien dit gedaan wordt dan moet dit een FHIR STU3 [OperationOutcome](http://hl7.org/fhir/STU3/operationoutcome.html) zijn.

### 5.3.3 Ophalen Task

Het doelsysteem is nu op de hoogte van een nieuwe of gewijzigde Task resource. Het security token dat in stap 11 gebruikt is, bevat het bronsysteem.

**15-21** De notificatie uit stap 11 bevatte een autorisatie header met daarin oa de identifiers van de bronhouder en de ontvangende partij. Het doelsysteem kan hiermee bepalen waar de Task opgehaald moet worden. Het endpoint van de Task FHIR URL is te vinden in het `fhir` veld van de `eOverdracht-sender` service van de bronhouder. Dit is de `base` URL van de FHIR service. Daar moet nog het relatieve pad van de Task resource (`Task`) en de specifieke identifier aan toegevoegd worden. Het access token gaat net zoals bij stap 11 mee in een header. Het bronsysteem is verantwoordelijk voor het vinden van de juiste Task resource. Omdat het security token geen gebruikersinformatie bevat, mogen er nooit persoonsgegevens meegestuurd worden in de Task.

**22-24** Het bronsysteem controleert het access token en bepaalt aan de hand daarvan de bronhouder en de opvragende partij. Samen met de gegeven query parameters bevat dit voldoende informatie om de juiste Task resource\(s\) terug te geven.

### 5.3.4 Authenticatie

**26-28** Het ontvangende systeem zal op verzoek van de gebruiker een authenticatie flow starten met een door de gebruiker gekozen authenticatiemiddel. We nemen in deze beschrijving IRMA als authenticatiemiddel. Andere authenticatiemiddelen zullen een soortgelijke flow gebruiken. Er wordt een authenticatie sessie gestart op de Nuts node. Deze geeft een _contract_ en _sessie ID_ terug. [RFC002](https://nuts-foundation.gitbook.io/drafts/rfc/rfc002-authentication-token) bevat de details.

**29-31** Het resultaat van stap 10 wordt als QR code weergegeven voor de gebruiker. De gebruiker scant de QR code met de camera van zijn/haar device. Het _contract_ dat in stap 27 is aangemaakt wordt op het device van de gebruiker getoond. De gebruiker ondertekent dit _contract_ met de gevraagde attributen. Het device stuurt het ondertekende contract naar de Nuts node waarna het ontvangende systeem het resultaat zal ophalen. Het ondertekende contract dient opgeslagen te worden in de sessie van de gebruiker. Het contract is voor langere tijd geldig zodat deze stap niet elke keer doorlopen hoeft te worden.

### 5.3.5 Ophalen overdrachtsbericht

Het ontvangende systeem zoekt in de Nuts node naar het endpoint waar het overdrachtsbericht opgehaald kan worden. Het base endpoint bevindt zich in het `fhir` veld van de `eOverdracht-sender` service van de bronhouder. De URL waar het overdrachtsbericht opgehaald kan worden staat in de opgehaalde Task onder `Task.input:nursingHandoff`. Deze URL zou een combinatie van het `base` path uit stap 15 en het relatieve pad voor het overdrachtsbericht onder `Task.input:nursingHandoff` moeten zijn.

**32.** Nu bekend is wat het bronsysteem is kan er een access token worden aangevraagd. Dit is gelijk aan de stappen 15 t/m 20 m.u.v. dat er nu ook gebruikersinformatie en een autorisatie record aanwezig moeten zijn. Het autorisatie record, het service ID, de bronhouder, de ontvangende partij en het ondertekende contract zullen allemaal onderdeel zijn van de access token aanvraag.

**33.** Met het access token in de juiste header kan het overdrachtsbericht opgehaald worden bij de URL zoals aangegeven in de Task resource.

**34-37** Het bronsysteem laat het access token controleren door de Nuts node. Deze zal o.b.v. de gebruikte autorisatie records ook de mogelijke resources teruggeven die geraadpleegd mogen worden. Het bronsysteem kijkt of de gevraagde resource, in dit geval het overdrachtsbericht, onderdeel is van die resources.

### 5.3.6 afronden overdracht

Voor het wijzigen van de Task is een access token nodig. Hiervoor kan hetzelfde access token gebruikt worden die ook bij stap 32 is gebruikt indien de geldigheid van het access token dit toelaat. Indien het updaten van de Task op een later tijdstip plaats vindt of als het token verlopen is, zullen stap 15 t/m 20 nogmaals doorlopen moeten worden. Als ook het contract van de gebruiker niet meer geldig is, dan zullen stappen 26 t/m 31 ook doorlopen moeten worden.

**41.** Het ontvangende systeem wijzigt de `Task.status` van `in-progress` naar `completed`. De Task wordt middels een `PUT` request gestuurd naar het juiste pad volgens het [Nictiz](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht) TO §3.2.2. Het `base` path is afkomstig vanuit het endpoint wat in stap 15 is opgehaald.

**42-44** Het bronsysteem laat het access token controleren door de Nuts node. Deze zal o.b.v. de gebruikte autorisatie records ook de mogelijke resources teruggeven die geraadpleegd mogen worden. Het bronsysteem kijkt of de gevraagde resource, in dit geval de Task resource, onderdeel is van die resources.

**45-46** Als de autorisaties het toestaan kan het bronsysteem de task updaten. Het controleert hierbij of alleen de status is aangepast.

**47-49** Nu de overdracht voltooid is kan het bronsysteem de relevante autorisatie records intrekken. De Nuts node zorgt er dan voor dat de intrekkingen bekend zijn bij andere nodes.

**50** Het updaten van de Task zorgt er tenslotte voor dat er een notificatie naar het ontvangende systeem gaat.

## 6. Access policy

Een onderdeel van een Bolt is de beschrijving van de autorisaties tot bepaalde resources.  De autorisaties worden beschreven in een access policy. Het is de taak van een resource server om de policy te volgen wanneer resources worden opgevraagd.

Deze eOverdracht Bolt omvat twee verschillende access policies. De belangrijkste is de `eOverdracht-sender` policy aangezien die toegang biedt tot daadwerkelijke gegevens. Daarnaast is er nog de `eOverdracht-receiver` policy die toegang regelt voor de notificaties.

### 6.1 eOverdracht-receiver policy

De `eOverdracht-receiver` policy beschrijft alleen de toegang tot het Task notificatie endpoint. Bij het aanvragen van een access token op de authorization server zijn geen [Nuts Authorization Credentials](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential) nodig. Ook is er geen gebruikersinformatie nodig. De `vcs` en `usi` velden in de [JWT grant](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization#4-2-2-payload) mogen dus leeg zijn. Het `purposeOfUse` veld moet in ieder geval `eOverdracht-receiver` bevatten.

De resource server van de ontvangende partij moet controleren dat er voor het notificatie endpoint een `POST` request wordt gedaan zonder body. Het gaat hierbij om een relatief pad t.o.v. wat er geregistreerd staat onder het `notification` veld in de `eOverdracht-receiver` service. Achter dit pad mag alleen nog een resource identifier staan.

### 6.2 eOverdracht-sender policy

De `eOverdracht-sender` policy bevat zowel regels voor persoonsgebonden als niet-persoonsgebonden resources. Voor de niet-persoonsgebonden resources geldt hetzelfde als bij §6.1: de `vcs` en `usi` velden in de [JWT grant](https://nuts-foundation.gitbook.io/drafts/rfc/rfc003-oauth2-authorization#4-2-2-payload) mogen leeg zijn.

#### 6.2.1 Niet-persoonsgebonden resources

**6.2.1.1 Ophalen task**

Het ophalen van de gewijzigde task valt onder de niet-persoonsgebonden resources.
Omdat het hierbij gaat om de toegang tot een enkele resource, is er een [Nuts Authorization Credential](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential) nodig. Het credential moet voldoen aan de volgende eisen:

* De `issuer` moet het DID bevatten van de versturende partij.
* `credentialSubject.id` moet het DID van de ontvangende partij bevatten.
* `credentialSubject.purposeOfUse` moet gelijk zijn aan `eOverdracht-sender`.
* `credentialSubject.legalBase.consentType` moet gelijk zijn aan `implied`.
* `credentialSubject.resources` moet de specifieke Task bevatten: `/Task/[id]`.

De resource server moet controleren of het gevraagde request gelijk is aan:

```text
GET [base]/Task/[id]
```

Waarbij `[base]` vervangen moet worden door het pad zoals deze is geregistreerd onder het `fhir` veld in de `eOverdracht-sender` service. De resources server moet ook de Task selecteren o.b.v. het access token. Bij de aanvraag voor het access token is meegestuurd welke organisatie de aanvraag doet en voor welke organisatie deze bedoeld is.

**6.2.1.2** **Updaten task resource**

Een onderdeel van de eOverdracht is dat de ontvangende partij de status van de Task aanpast. Hiervoor dient het een request te doen zoals beschreven in [§3.2.2 van het Nictiz TO](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht#Task_invocations). Omdat het hierbij gaat om de toegang van een enkele resource, is er een [Nuts Authorization Credential](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential) nodig. Het gaat hierbij om hetzelfde credential als beschreven in de vorige paragraaf.

#### 6.2.2 Persoonsgebonden resources

Het ophalen van het overdrachtsbericht en alle aanverwante gegevens vereisen een geregistreerde autorisatie in de vorm van een [Nuts Authorization Credential](https://nuts-foundation.gitbook.io/drafts/rfc/rfc014-authorization-credential). Het credential moet voldoen aan de volgende eisen:

* De `issuer` moet het DID bevatten van de versturende partij.
* `credentialSubject.id` moet het DID van de ontvangende partij bevatten.
* `credentialSubject.purposeOfUse` moet gelijk zijn aan `eOverdracht-sender`.
* `credentialSubject.legalBase.consentType` moet gelijk zijn aan `implied`.
* `credentialSubject.subject` moet het BSN bevatten als OID: `urn:oid:2.16.840.1.113883.2.4.6.3:999999990`.

De `eOverdracht-sender` policy geeft geen toegang tot gegevens anders dan die onder `resources` vermeld staan in het credential. `resources` moet in ieder geval de volgende waardes bevatten:

* path: `/task/[ID]`, operations: `["read", "update"]`, userContext: `false`. `[ID]` moet hierbij vervangen worden door een echt ID. Deze waarde geeft lees en update rechten op de task resource.
* path: `/compositon/[id]`, operations: `["read", "document"]`, userContext: `true`. `[ID]` moet hierbij vervangen worden door een echt ID. De `document` operatie wordt in FHIR vertaald naar het pad: `/Compositon/[id]/$document`. Dit betreft lees rechten op het overdrachtsbericht.
* path: `/[path]`, operations: `["read"]`, userContext: `true`. Voor elke FHIR reference die voorkomt in het overdrachtsbericht moet een waarde worden opgenomen. `/[path]` moet daarbij vervangen worden door de FHIR reference.

Bij de aanvraag van het access token moet het credential volgens bovenstaande eisen meegestuurd worden in het `vcs` veld. Daarnaast moet er gebruikersinformatie meegestuurd worden in het `usi` veld. Het `purposeOfUse` veld moet de waarde `eOverdracht-sender` bevatten.

### 6.3 Intrekken autorisatie

Wanneer de overdracht voltooid is, is het niet langer nodig voor de ontvangende partij om gegevens op te halen. Het autorisatie credential kan dan ingetrokken worden. Wanneer de status van de Task geupdate wordt naar completed, dan moet de verzendende partij het credential intrekken. Indien de ontvangende partij vergeet om het proces af te ronden dan staat het de verzendende partij vrij om het credential na een redelijke periode in te trekken.

## Appendix A. Integratie voorbeelden

Deze bijlage geeft een aantal voorbeelden hoe de concepten uit de eerdere hoofdstukken te vertalen zijn naar verschillende integratie mogelijkheden. Er worden slechts enkele uiteenlopende voorbeelden gegeven om een idee te krijgen van de flexibiliteit van de gekozen richting. Deze bijlage is niet-normatief.

Voor zowel de ontvangende als de versturende kant zijn drie voorbeelden opgenomen, deze dragen dezelfde namen, maar zijn geen afhankelijkheid van elkaar. De ontvangende staat volledig los van de versturende kant.

Legenda:

* AAA —  Authenticatie, Autorisatie en Auditing. De security gateway die zorgt voor de gegevensbeveiliging. De autorisatie gebeurt o.b.v. de aanwezige grondslagen. Het uitwisselen van de grondslagen staat niet expliciet in de modellen, maar is wel noodzakelijk voor dit component.
* Broker — Andere dan de XIS/ECD leverancier die functionaliteit beschikbaar stelt voor de verpleegkundige overdracht.
* API — Een specifieke gegevensdienst voor het betreffende XIS/ECD, hoeft niet gebaseerd te zijn op enige standaard. Beveiligd.
* Data proxy — Communicatie module om met specifieke API van XIS/ECD te communiceren.
* Medische data — Logische component/systeem waar de medische gegevens staan opgeslagen. Dit kan meerdere systemen betreffen.
* Medisch professional — De professional die gegevens in het dossier registreert.
* Medisch secretariaat — Of vergelijkbare afdeling die de zorgvraag voor de overdracht kan beoordelen.
* Resource server — De FHIR resource server. Stelt gegevens beschikbaar via ZIBs.
* Tasks — Logisch component/systeem voor opslag van gegevens voor volgen overdrachtsprocedure.
* Transfer professional — De professional die die overdracht klaar zet of beoordeelt.
* Transfer UI — Gebruikersinterface van de transfer professional.
* UI — Gebruikersinterface van de medisch professional.

De blauwe pijlen geven interactie van de gebruiker weer en de rode pijlen geven gegevensstromen weer. Het slotje bij een gebruiker wil zeggen dat deze geauthenticeerd is volgens de oauth beschrijving.

### A.1 Aanbiedende partij

#### A.1.1 Embedded scenario

In het embedded scenario is één leverancier verantwoordelijk voor het bijhouden van het overdrachtsproces en voor het beschikbaar stellen van de benodigde gegevens. Dit is het geval wanneer een XIS/ECD leverancier de benodigde functionaliteit voor de verpleegkundige overdracht implementeert in het XIS/ECD.

![](https://lh5.googleusercontent.com/O3od8KgnBNyw47gRCXxk_vLf_d8UJkU3heJTq-TuDb7xnhL-uVz7NZPz6fv0VazflwG64hMeDolrnHarvE0_poG0sORWHSRfN8RFDt3qj0spaqWQTGJHzx2mspeoYuxYuOqIFlGd)

#### A.1.2 Hybride scenario

In het hybride scenario is er een scheiding aangebracht in het beschikbaar stellen van de gegevens en het afhandelen van de verpleegkundige overdracht. Het XIS/ECD beschikt in dit scenario over een resource server die ZIBs over FHIR beschikbaar stelt. In dit scenario komen dergelijke gegevens rechtstreeks uit het XIS/ECD. De broker handelt in dit geval het overdrachtsproces af. De broker maakt het mogelijk via de transfer UI om de ontvangende zorginstellingen te selecteren en om de juiste gegevens te selecteren. Ook het aanmeldbericht en het overdrachtsbericht zijn dan de verantwoordelijkheid voor de broker. Deze hebben echter wel verwijzingen naar resources die bij het XIS/ECD opgeslagen zijn. Om de juiste gegevens te selecteren moet er wel een koppeling zijn tussen broker en XIS/ECD. Hoe die koppeling er uit ziet valt buiten deze specificatie. Bij het selecteren van de juiste gegevens moet de broker ook de juiste grondslag vastleggen zodat de AAA service de juiste autorisatie kan toepassen.

![](https://lh4.googleusercontent.com/1h7jRP1_heZAwAeRhVNLwfudzuT8aiSk8U38N-0tXwEBnhBuv18CyWHWsDHXmNh9QxHLzgfM_N5a_aSCLGeU2dr3uQMljTt068auyLXuX4Hu8gELfqHG20NGvH9A0_ChOK5-WuRG)

#### A.1.3 Delegate scenario

Het delegate scenario gaat er van uit dat het XIS/ECD geen enkele ondersteuning heeft en ook niet zal gaan bieden voor het ontsluiten van ZIBs over FHIR. In dit geval handelt de broker en het proces af en voorziet het in de gegevensontsluiting. In de praktijk komen de gegevens wel vanuit het XIS/ECD, maar zal de broker deze omvormen naar ZIBs. De data-proxy handelt dit voor de broker af. Het kan zijn dat het hier gaat om het kopiëren van gegevens, maar ook dat gegevens real-time omgezet worden naar ZIBs. Ook in dit scenario zal de broker bij de gegevens uit het XIS/ECD moeten kunnen om de transfer professional te kunnen ondersteunen.

![](https://lh5.googleusercontent.com/uqASB5V_px0piuRh_FV59tmeY2F9MaYkjBgmi2KzwUBrwCI3aQhH6Ikc_MKd4E6jWLARHE-GZEULkmEYyxCbKk0vKyYFyYqji5C39YyYRTHHyosWNm6W46HQQpRcqgj_9q86afiY)

### A.2 Ontvangende partij

#### A.2.1 Embedded scenario

Dit is de evenknie van het proces aan de versturende kant. Alle functionaliteit wordt door het XIS/ECD geïmplementeerd. Zowel de professionals die de aanvraag beoordelen als de medische professionals die de zorg opstarten zullen geauthenticeerd moeten zijn volgens de in H4 vermelde methode. Een verschil is dat de beoordelend professional alleen het aanmeldbericht zal hoeven in te zien, terwijl de medisch professional het volledige overdrachtsbericht nodig heeft om dossiervorming te starten. Het synchroniseren van de tasks kan gebruikt worden om de beoordelend professional een signaal te geven dat er een nieuwe overdracht wordt aangeboden. Het updaten van de tasks gaat nog altijd via de versturende kant. Het XIS/ECD kan zelf ZIBs over FHIR verwerken en tonen aan de gebruiker.

![](https://lh5.googleusercontent.com/igwPdEzndJH1o8HSCTmL7oCEzDDqOAEajbMHUAUp3D5n3V1RsWGWF2xTmXuNISv1llXzkcXGIufoEAbjitRfHrsUZpIlS78FPGvhFaFlIefmknIe6JTNTq5AXCZEJZVP2XbxnubV)

#### A.2.2 Hybride scenario

Het hybride scenario is voor de ontvangende klant bijna exact hetzelfde als het embedded scenario. Er zou een verschil kunnen zitten in hoe de medisch professional op de hoogte wordt gebracht dat er een nieuwe patiënt is. In het embedded scenario kan het XIS/ECD dit via interne logica regelen, terwijl er in het hybride scenario twee mogelijkheden zijn: De broker notificeert het XIS/ECD via een custom koppeling of het XIS/ECD reageert op het aanwezig zijn van een grondslag omtrent een overdracht waarbij een overdrachtsbericht beschikbaar is gekomen.

![](https://lh3.googleusercontent.com/kGZEamyRjm__WjL5adWk7AzqhdXgCc8vXfHNp1eNznasS1CiRBtV06rAJSvmLGeQxUK4dxK9beb0kmZ4w41pKDG_kw1MYAD9wcEu06s8RMqHvLQVk4M9lZeRutMLb7Xvf_8gnk-Q)

#### A.2.3 Delegate scenario

In dit scenario neemt de broker ook het verwerken van de ZIBs over FHIR voor zijn rekening en levert dit aan het XIS/ECD op een verwerkbare manier.

![](https://lh3.googleusercontent.com/Gop-QxG9C0IEJ8tQSfO4Ak-T-fAo17Kiex-SGKDkquNKNsSo2e_izGWyo2t0OpY67UmeruTRCSPCImuXrwwTPDDHkVmqhJdB-l_hShSo9PCOnvYoFYtC57p0BRi4nUk3W1hd9s5r)

