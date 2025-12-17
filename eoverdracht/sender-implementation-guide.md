# eOverdracht Sender Implementation Guide

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Conventions](#conventions)
4. [Architecture Overview](#architecture-overview)
5. [Setup & Configuration](#setup--configuration)
6. [Implementation Steps](#implementation-steps)
7. [Testing & Validation](#testing--validation)
8. [Troubleshooting](#troubleshooting)
9. [References](#references)

---

## Introduction

This guide provides step-by-step instructions for implementing the **Sender** side of the eOverdracht (electronic
handover) use case.
As a sender, your system will initiate patient handovers and securely provide medical data to receiving organizations.

The handover process
follows [Nictiz eOverdracht V4.0](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht).

### What You'll Build

- Create and publish FHIR Task resources
- Generate authorization credentials
- Send notifications to receivers
- Serve handover FHIR resources via FHIR endpoints
- Manage handover lifecycle and credential revocation

### Key Responsibilities

As a sender system, you are responsible for:

- **Creating** FHIR resources to initiate handovers
- **Notifying** receivers about new handovers
- **Authorizing** receivers to access specific resources
- **Serving** FHIR resources (Task, Composition, Patient, etc.)
- **Revoking** access after handover completion

---

## Prerequisites

- Nuts node (at least v6)
- PKIoverheid certificates (used as server- and client certificates)
- FHIR server capable of handling STU3

---

## Conventions

### Parameterized Values

Throughout this guide, the following parameterized values are used in API examples:

| Parameter                   | Description                                                 |
|-----------------------------|-------------------------------------------------------------|
| `{senderOrgDID}`            | The DID of the sender organization (your organization)      |
| `{receiverOrgDID}`          | The DID of the receiver organization                        |
| `{vendorDID}`               | The DID of your vendor                                      |
| `{vendorSubjectID}`         | The subject ID of your vendor (used in DID management)      |
| `{organizationSubjectID}`   | The subject ID of the organization (used in DID management) |
| `{taskID}`                  | The handover FHIR Task resource ID                          |
| `{accessToken}`             | OAuth 2.0 access token                                      |
| `{notificationEndpointURL}` | The resolved notification endpoint URL                      |
| `{oauthEndpointURL}`        | Your OAuth endpoint URL                                     |
| `{fhirEndpointURL}`         | Your FHIR endpoint URL                                      |

### Nuts Node API Endpoints

All Nuts node API calls in this guide use `http://localhost:8081` as the base URL. This refers to the **Nuts node's
internal API** (port 8081 by default).

| API        | Base URL                                   | Description                             |
|------------|--------------------------------------------|-----------------------------------------|
| VDR API    | `http://localhost:8081/internal/vdr/v2`    | DID document management                 |
| VCR API    | `http://localhost:8081/internal/vcr/v2`    | Verifiable Credential management        |
| Auth API   | `http://localhost:8081/internal/auth/v1`   | Access token requests and introspection |
| Didman API | `http://localhost:8081/internal/didman/v1` | DID and service discovery               |

**Note**: In production, configure the Nuts node's internal API to only be accessible from your application server, not
from the public internet.

---

## Architecture Overview

### Sender Flow

1. **Create handover data** - Build FHIR Composition and Task resources
2. **Create authorization credentials** - Issue credentials for Task and handover data
3. **Discover receiver endpoint** - Resolve receiver's notification endpoint via DID
4. **Send notification** - Request access token and POST to receiver's notification endpoint
5. **Serve FHIR resources** - Handle GET requests for FHIR resources
6. **Handle Task status update** - Handle PUT requests to update Task status
7. **Revoke credentials** - Revoke handover data credentials when Task is completed

### Components

1. **Your Application**: Creates handovers and manages business logic
2. **FHIR Server**: Stores and serves FHIR resources
3. **Nuts Node**: Provides DIDs, credentials, and authorization
4. **mTLS Layer**: Ensures secure communication

---

## Setup & Configuration

### Step 1: Nuts Node Requirements

Before implementing the sender, ensure your Nuts node meets these requirements:

- **DID Method**: `nuts` DID method must be enabled
- **Network Connection**: Node must be connected to a Nuts network
- **Vendor DID**: A vendor DID must be created and registered
- **Organization Credentials**: NutsOrganizationCredential(s) must be issued for your organization(s)

For instructions on setting up a Nuts node, see the [Nuts Node documentation](https://nuts-node.readthedocs.io/).

### Step 2: Register Vendor Services

#### 2.1 Register OAuth Endpoint

```http request
POST http://localhost:8081/internal/vdr/v2/subject/{vendorSubjectID}/service
Content-Type: application/json

{
  "type": "production-oauth",
  "serviceEndpoint": "{oauthEndpointURL}"
}
```

#### 2.2 Register FHIR Endpoint

```http request
POST http://localhost:8081/internal/vdr/v2/subject/{vendorSubjectID}/service
Content-Type: application/json

{
  "type": "eOverdracht-sender-fhir",
  "serviceEndpoint": "{fhirEndpointURL}"
}
```

### Step 3: Register eOverdracht Sender Service

For each organization that will act as a sender, register the eOverdracht-sender service:

```http request
POST http://localhost:8081/internal/vdr/v2/subject/{organizationSubjectID}/service
Content-Type: application/json

{
  "id": "{senderOrgDID}#eoverdracht-sender",
  "type": "eOverdracht-sender",
  "serviceEndpoint": {
    "oauth": "{vendorDID}/serviceEndpoint?type=production-oauth",
    "fhir": "{vendorDID}/serviceEndpoint?type=eOverdracht-sender-fhir"
  }
}
```

---

## Implementation Steps

### Step 1: Prepare Handover Data

#### 1.1 Create FHIR Composition

Build a composition according to
the [Nictiz eOverdracht profile](https://informatiestandaarden.nictiz.nl/wiki/vpk:V4.0_FHIR_eOverdracht):

```json
{
  "resourceType": "Composition",
  "id": "handover-123",
  "meta": {
    "profile": [
      "http://nictiz.nl/fhir/StructureDefinition/eOverdracht-NursingHandoff-Adults"
    ]
  },
  "status": "final",
  "type": {
    "coding": [
      {
        "system": "http://snomed.info/sct",
        "code": "11171000146100",
        "display": "Nursing handoff report"
      }
    ]
  },
  "subject": {
    "reference": "Patient/patient-456"
  },
  "date": "2024-01-15T10:30:00Z",
  "author": [
    {
      "reference": "Practitioner/practitioner-789"
    }
  ],
  "title": "Nursing Handoff for Transfer",
  "section": [
    {
      "title": "Patient Demographics",
      "code": {
        "coding": [
          {
            "system": "http://snomed.info/sct",
            "code": "302147001"
          }
        ]
      },
      "entry": [
        {
          "reference": "Patient/patient-456"
        }
      ]
    },
    {
      "title": "Allergies",
      "code": {
        "coding": [
          {
            "system": "http://snomed.info/sct",
            "code": "722446000"
          }
        ]
      },
      "entry": [
        {
          "reference": "AllergyIntolerance/allergy-001"
        }
      ]
    }
  ]
}
```

**Important**: Store the Composition and all referenced resources in your FHIR server.

#### 1.2 Create Task Resource

```json
{
  "resourceType": "Task",
  "id": "task-123",
  "meta": {
    "profile": [
      "http://nictiz.nl/fhir/StructureDefinition/eOverdracht-Task"
    ]
  },
  "status": "requested",
  "intent": "order",
  "code": {
    "coding": [
      {
        "system": "http://snomed.info/sct",
        "code": "308292007",
        "display": "Transfer of care"
      }
    ]
  },
  "for": {
    "identifier": {
      "system": "http://fhir.nl/fhir/NamingSystem/bsn",
      "value": "999999990"
    }
  },
  "authoredOn": "2024-01-15T10:00:00Z",
  "owner": {
    "identifier": {
      "system": "http://fhir.nl/fhir/NamingSystem/ura",
      "value": "12345678"
    }
  },
  "input": [
    {
      "type": {
        "coding": [
          {
            "system": "http://nictiz.nl/fhir/CodeSystem/eOverdracht-TaskInput",
            "code": "nursingHandoff"
          }
        ]
      },
      "valueReference": {
        "reference": "Composition/handover-123"
      }
    }
  ]
}
```

**Privacy Note**: The Task should NOT contain directly identifiable information until there's a formal treatment
relationship.

### Step 2: Create Authorization Credential

Create a single authorization credential that allows the receiver to access both the Task and handover data:

```http request
POST http://localhost:8081/internal/vcr/v2/issuer/vc
Content-Type: application/json

{
  "issuer": "{senderOrgDID}",
  "type": "NutsAuthorizationCredential",
  "credentialSubject": {
    "id": "{receiverOrgDID}",
    "purposeOfUse": "eOverdracht-sender",
    "resources": [
      {
        "path": "/Task/{taskID}",
        "operations": ["read", "update"],
        "userContext": false
      },
      {
        "path": "/Composition/1234",
        "operations": ["read", "document"],
        "userContext": true
      },
      {
        "path": "/Patient/456",
        "operations": ["read"],
        "userContext": true
      },
      {
        "path": "/AllergyIntolerance/987",
        "operations": ["read"],
        "userContext": true
      }
    ]
  },
  "publishToNetwork": true,
  "visibility": "private",
  "expirationDate": "2026-01-20T10:30:00Z"
}
```

**Key Points**:

- Task resource: Set `userContext: false` (no user authentication required)
- Handover data resources: Set `userContext: true` (user authentication required)
- Set appropriate expiration date
- **List ALL resources** referenced in the Composition
- Include `document` operation for the Composition resource

### Step 3: Discover Receiver's Notification Endpoint

Use the didman convenience API to resolve the receiver's notification endpoint:

```http request
GET http://localhost:8081/internal/didman/v1/did/{receiverOrgDID}/compoundservice/eOverdracht-receiver/endpoint/notification
```

Response:

```json
{
  "endpoint": "https://receiver-vendor.example.com/notifications/eoverdracht"
}
```

The `endpoint` field contains the endpoint URL where you'll send the notification. The didman API automatically resolves
any DID references to their actual URLs.

### Step 4: Send Notification

#### 4.1 Request Access Token

```http request
POST http://localhost:8081/internal/auth/v1/request-access-token
Content-Type: application/json

{
  "authorizer": "{receiverOrgDID}",
  "requester": "{senderOrgDID}",
  "service": "eOverdracht-receiver"
}
```

Response:

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 900
}
```

#### 4.2 Send Notification POST

```http request
POST {notificationEndpointURL}/{taskID}
Authorization: Bearer {access_token}
```

**Important**:

- Use empty body (POST with no data)
- Append Task ID to endpoint URL
- Use PKIOverheid Private Services client certificate
- Expect `202 Accepted` response

### Step 5: Implement FHIR Endpoints

#### 5.1 Security & Authorization

##### Token Introspection

Validate incoming access tokens using the introspection endpoint:

```http request
POST http://localhost:8081/internal/auth/v1/accesstoken/introspect
Content-Type: application/x-www-form-urlencoded

token={accessToken}
```

Response:

```json
{
  "active": true,
  "sub": "{senderOrgDID}",
  "iss": "{receiverOrgDID}",
  "service": "eOverdracht-sender",
  "vcs": ["nuts-authz-credential-id-1"],
  "exp": 1640000000
}
```

##### Authorization Verification

Verify that access tokens contain the required authorization credentials:

```
FUNCTION verifyAccess(introspection, resourcePath, operation, requireUserContext):
  credentials = introspection.vcs OR empty list
  
  FOR EACH credentialId IN credentials:
    credential = resolveCredential(credentialId)
    resources = credential.credentialSubject.resources OR empty list
    
    FOR EACH resource IN resources:
      IF resource.path == resourcePath AND
         operation IN resource.operations:
        
        // If user context is required, verify it matches
        IF requireUserContext == true:
          IF resource.userContext == true AND
             introspection.initials IS NOT NULL AND
             introspection.family_name IS NOT NULL:
            RETURN true
        ELSE:
          RETURN true
  
  RETURN false
```

**Usage examples:**
- Task access: `verifyAccess(introspection, "/Task/123", "read", false)`
- Task update: `verifyAccess(introspection, "/Task/123", "update", false)`
- Composition document: `verifyAccess(introspection, "/Composition/456", "document", true)`
- Patient resource: `verifyAccess(introspection, "/Patient/789", "read", true)`

##### mTLS Configuration

Configure your HTTPS server to require and validate client certificates.

Make sure it only accepts certificate from the PKIOverheid Private Services chain.

TODO: Add CA certificate chain.

#### 5.2 GET /Task/{id}

Handle requests to retrieve the Task:

```
ENDPOINT: GET /fhir/Task/{id}

1. Extract access token from Authorization header
2. Introspect token using Nuts node API
3. If token is not active:
   - Return 401 Unauthorized
4. Verify access: verifyAccess(introspection, "/Task/{id}", "read", false)
5. If not authorized:
   - Return 403 Forbidden
6. Retrieve Task from FHIR server
7. Return Task as JSON
```

#### 5.3 PUT /Task/{id}

Handle requests to update the Task status:

```
ENDPOINT: PUT /fhir/Task/{id}

1. Extract access token from Authorization header
2. Introspect token using Nuts node API
3. If token is not active:
   - Return 401 Unauthorized
4. Verify access: verifyAccess(introspection, "/Task/{id}", "update", false)
5. If not authorized:
   - Return 403 Forbidden
6. Retrieve existing Task from FHIR server
7. Parse updated Task from request body
8. Validate that only 'status' and 'lastModified' fields changed
9. If other fields changed:
   - Return 400 Bad Request with error "Only status updates are allowed"
10. Update Task in FHIR server
11. If new status is 'completed':
    - Revoke handover data credentials (see Step 6)
12. Return updated Task as JSON
```

#### 5.4 GET /Composition/{id}/$document

Serve the complete handover document:

```
ENDPOINT: GET /fhir/Composition/{id}/$document

1. Extract access token from Authorization header
2. Introspect token using Nuts node API
3. If token is not active:
   - Return 401 Unauthorized
4. Verify access: verifyAccess(introspection, "/Composition/{id}", "document", true)
5. If not authorized:
   - Return 403 Forbidden
6. Retrieve Composition from FHIR server
7. Build FHIR Bundle of type 'document':
   - Add Composition as first entry
   - For each section in Composition:
     - For each entry reference in section:
       - Retrieve referenced resource from FHIR server
       - Verify access: verifyAccess(introspection, "/{resourceType}/{id}", "read", true)
       - If authorized, add resource to Bundle
8. Log access to audit log (NEN7513):
   - Action: 'read'
   - Resource: Composition/{id}
   - User: from token (initials, family_name)
   - Organization: from token (iss)
   - Timestamp: current time
9. Return Bundle as JSON
```

### Step 6: Revoke Credentials After Completion

When the Task status is updated to `completed`, revoke the handover data credentials:

#### 6.1 Search for Handover Credentials

```http request
POST http://localhost:8081/internal/vcr/v2/search
Content-Type: application/json

{
  "query": {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://nuts.nl/credentials/v1"
    ],
    "type": ["VerifiableCredential", "NutsAuthorizationCredential"],
    "credentialSubject": {
      "purposeOfUse": "eOverdracht-sender",
      "resources": {
        "path": "/Composition/123"
      }
    }
  }
}
```

Response contains the credentials that match the query.

#### 6.2 Revoke Each Credential

For each credential found, revoke it:

```http request
DELETE http://localhost:8081/internal/vcr/v2/issuer/vc/{credentialID}
```

**Note**: URL-encode the credential ID if it contains special characters.

---


## Testing & Validation

### Testing Checklist

- [ ] Vendor DID created and registered
- [ ] Organization DID created
- [ ] Organization credential issued
- [ ] eOverdracht-sender service registered
- [ ] FHIR endpoints accessible
- [ ] Task resource follows Nictiz profile
- [ ] Composition follows Nictiz profile
- [ ] Task authorization credential created
- [ ] Handover authorization credential created (with all resources)
- [ ] Notification successfully sent
- [ ] GET /Task/{id} returns valid Task
- [ ] PUT /Task/{id} accepts status updates only
- [ ] GET /Composition/{id}/$document returns complete bundle
- [ ] User context validated for patient data
- [ ] Credentials revoked after completion
- [ ] Audit logs generated
- [ ] mTLS properly configured

### Unit Tests

```
TEST SUITE: Task Endpoint
  
  TEST: should return task with valid token
    - Get test access token
    - Send GET request to /fhir/Task/test-123
    - Set Authorization header with Bearer token
    - Expect response status: 200
    - Expect response body.resourceType to be 'Task'
  
  TEST: should reject request without token
    - Send GET request to /fhir/Task/test-123
    - Do NOT set Authorization header
    - Expect response status: 401
  
  TEST: should only allow status updates
    - Get test access token
    - Retrieve existing Task test-123
    - Modify task.code.coding[0].display field
    - Send PUT request to /fhir/Task/test-123
    - Set Authorization header with Bearer token
    - Send modified task in body
    - Expect response status: 400
```

### Integration Tests

```
TEST SUITE: eOverdracht Sender Flow
  
  TEST: should complete full sender flow
    STEP 1: Create handover
      - compositionId = createComposition()
      - taskId = createTask(compositionId)
    
    STEP 2: Create credentials
      - createTaskCredential(receiverOrgDID, taskId)
      - createHandoverCredential(receiverOrgDID, compositionId)
    
    STEP 3: Send notification
      - endpoint = getReceiverEndpoint(receiverOrgDID)
      - sendNotification(endpoint, taskId)
    
    STEP 4: Verify credentials exist
      - credentials = searchCredentials(compositionId)
      - Expect credentials.length > 0
    
    STEP 5: Simulate task completion
      - updateTaskStatus(taskId, 'completed')
    
    STEP 6: Verify credential revocation
      - remainingCreds = searchCredentials(compositionId)
      - Expect remainingCreds.length == 0
```
