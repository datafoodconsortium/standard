# 🚧 Authorization strategy (v2)

**Status:** Draft proposal
**Intended audience:** DFC platform implementers, API implementers, identity/authorization providers, application developers
**Namespace:** `dfc-t: https://www.w3id.org/dfc/ontology/src/DFC_TechnicalOntology.owl#`, `dfc-b: https://www.w3id.org/dfc/ontology/src/DFC_BusinessOntology.owl#`
**Prefix:** `dfc-t:`
**Specification family:** DFC Technical Ontology / DFC Protocol

This authorization strategy extends the [authentication strategy](authentication-strategy.md). It replaces the transitional v1 scope-based model (kept as a [legacy appendix](#appendix-legacy-v1-scope-based-model) below) with a fine-grained, semantic authorization layer.

The full normative detail lives in two companion pages:

* [Authorization grants](authorization-grants.md) — vocabulary, grant model, delegation, expiry/revocation, management APIs, response filtering.
* [Authorization trust and credentials](authorization-trust-and-credentials.md) — DPoP, WebID trust binding, client identity, Verifiable Credentials, the merged normative algorithm, conformance tests.

The key words **MUST**, **MUST NOT**, **SHOULD**, **MAY**, and **OPTIONAL** are to be interpreted as described in RFC 2119 / RFC 8174.

## 1. Layer separation

*This section is normative.*

The v2 model deliberately separates five concerns that the v1 model conflated:

```text
WebID
   │ identifies
   ▼
Agent ── authenticated by ──▶ OIDC
   │
   │ produces
   ▼
Access Token (subject, client, scopes)
   │── coarse API capability (scope check)
   │── optional proof-of-possession (DPoP)
   ▼
DFC semantic authorization (grants, policies, relationships)
   │
   ▼
Resource + Property + Action → ALLOW / DENY → filtered response
```

The following distinctions are normative:

> **A WebID is an identity identifier, not an authorization grant.**
> **An OIDC scope is an API capability, not a complete fine-grained permission.**
> **An access token is evidence of authenticated/delegated authority, not necessarily the complete authorization policy.**
> **A valid DPoP proof is proof of key possession, not semantic authorization.**

An OIDC scope therefore establishes a necessary coarse-grained capability but does not, by itself, authorize access to a particular DFC resource or property.

## 2. Architecture

*This section is normative.*

A conformant implementation SHOULD conceptually implement these components (a single deployment MAY combine several):

| Component | Responsibility |
|---|---|
| **Agent** | Person, organization, application, device or other actor |
| **WebID** | Stable Web identifier of an Agent, canonical subject for cross-platform grants |
| **OIDC Provider** | Authenticates the Agent |
| **Authorization Server** | Issues OAuth/OIDC credentials and/or manages grants |
| **Resource Server** | Hosts or exposes DFC resources |
| **PEP** (Policy Enforcement Point) | Enforces authorization decisions |
| **PDP** (Policy Decision Point) | Evaluates policies and grants |
| **Policy Store** | Stores policies, grants and/or relationships |

The model MUST NOT require a Solid Pod, LDP, a particular RDF store, a `.acl` resource, or a particular HTTP resource hierarchy. A platform MAY use Solid/WAC internally, but the external authorization semantics MUST remain equivalent (see the [trust and credentials](authorization-trust-and-credentials.md) page for the Solid-OIDC relationship).

## 3. OIDC scopes: coarse API gate

*This section is normative.*

OIDC/OAuth scopes are the coarse authorization layer, e.g.:

```text
dfc:catalog.read
dfc:catalog.write
dfc:orders.read
```

A valid scope establishes permission to invoke the corresponding API capability, subject to semantic authorization. A valid scope MUST NOT by itself establish permission to access an individual DFC resource or property. Both directions hold:

```text
valid scope + no semantic grant = DENY
semantic grant + missing required scope = DENY
```

### Scope naming

*This section is normative.*

The canonical v2 scope form is dotted (`dfc:<domain>.<operation>`). Deployments migrating from v1 SHOULD map legacy names as follows:

| Legacy v1 scope pattern | v2 equivalent pattern |
|---|---|
| `ReadEnterprise` | `dfc:enterprise.read` |
| `ReadProduct` | `dfc:catalog.read` |
| `ReadPrice` | `dfc:catalog.read` (price properties are filtered semantically) |
| `ReadOrder` | `dfc:orders.read` |
| `Write*` | `dfc:<domain>.write` (see [action mapping](#8-action-mapping)) |
| `Delete*` | `dfc:<domain>.delete` |

Price-style distinctions that v1 encoded in scope names are expressed in v2 as property-level grants instead (e.g. `price` allowed while `supplierCost` denied under the same `dfc:catalog.read` scope).

## 4. DPoP: proof-of-possession gate (independent profile)

*This section is normative.*

DFC implementations MAY require **OAuth 2.0 Demonstrating Proof of Possession (DPoP)** for protected operations. DPoP binds the access token to a client-held key and binds each request (method + URI, with replay prevention) to that key, reducing the value of a stolen token.

DPoP is an **independent deployment/security profile**: it MUST NOT be required merely to claim the semantic conformance levels below. Full DPoP rules — including the subject/client/proof-key separation and the complete access conditions — are on the [trust and credentials](authorization-trust-and-credentials.md#3-dpop-proof-of-possession) page.

## 5. Core access rule

*This section is normative.*

For a deployment requiring DPoP, protected access requires:

```text
authentication valid
AND access token valid
AND required OIDC scope present
AND DPoP valid
AND semantic authorization ALLOW
AND no applicable explicit DENY
AND grant/policy valid
```

Without DPoP, the same rule minus the DPoP condition. Everything else in the v2 model is an extensibility mechanism around that core.

The central interoperability requirement: **two independent DFC implementations MUST produce semantically equivalent authorization decisions when supplied with equivalent WebID, scope, grant, policy, resource, property and context information** (plus valid proof-of-possession state where DPoP is required). Storage technology (RDF/SPARQL, SQL, document store, Solid server) MAY differ; semantics MUST NOT.

## 6. HTTP behavior

*This section is normative.*

### Unauthenticated request

*This section is normative.*

If authentication is required and no valid credentials are supplied, return `401 Unauthorized` and SHOULD advertise the Authorization Server:

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer, as_uri="https://auth.example/"
```

### Insufficient OIDC scope

*This section is normative.*

Authenticated but missing the scope required by the API operation:

```http
HTTP/1.1 403 Forbidden
```

```json
{ "error": "insufficient_scope", "scope": "dfc:catalog.read" }
```

### Insufficient semantic authorization

*This section is normative.*

Scope present but the semantic PDP denies (e.g. a denied property):

```http
HTTP/1.1 403 Forbidden
```

```json
{
  "error": "insufficient_authorization",
  "resource": "https://farm.example/products/123",
  "property": "https://www.w3id.org/dfc/ontology/src/DFC_BusinessOntology.owl#supplierCost"
}
```

The server SHOULD avoid disclosing sensitive policy information through error responses, and SHOULD avoid revealing whether a protected resource exists when the caller may not discover it.

## 7. Conformance levels

*This section is normative.*

| Level | Name | Adds |
|---|---|---|
| 1 | Resource Authorization | WebID, OIDC scopes, resource, action, allow/deny |
| 2 | Property Authorization | Property-level rules, response filtering, inheritance, explicit deny |
| 3 | Delegated Authorization | Grantor/grantee, delegation, expiration, revocation, relationship-based authorization |
| 4 | Portable Authorization | Verifiable Grants, credential-based evidence, status-list revocation, selective disclosure ([trust and credentials](authorization-trust-and-credentials.md)) |

A platform claiming full DFC Fine-Grained Authorization conformance MUST implement Level 3. VC support (Level 4) and DPoP MUST NOT be required for Level 1–3 claims. A server advertising `dfc_authorization_supported = true` MUST use the fine-grained model for resources covered by its authorization policy; otherwise it MAY operate in legacy compatibility mode (authentication + scope → access).

A DFC API SHOULD expose its authorization capabilities (supported dimensions, DPoP required or not, accepted trust bindings) through a discovery document — see the [trust and credentials](authorization-trust-and-credentials.md#7-capability-discovery) page.

## 8. Action mapping

*This section is normative.*

The Resource Server MUST map the HTTP operation to a DFC authorization action:

| HTTP operation | DFC action |
|---|---|
| `GET` (`HEAD`) | `dfc-t:Read` |
| `POST` | `dfc-t:Create` |
| `PUT` / `PATCH` | `dfc-t:Update` |
| `DELETE` | `dfc-t:Delete` |

`dfc-t:Delegate` (the right to re-delegate a grant) has no HTTP equivalent and is evaluated during delegation-chain validation (see [grants §7](authorization-grants.md#7-delegation)). An API MAY define more specific operations.

## Appendix: legacy v1 scope-based model

*This section is non-normative (superseded legacy model, preserved for migrating implementations).*

> This section preserves the pre-v2 transitional recommendation. It is superseded by the model above but remains supported for existing deployments migrating incrementally.

We proposed a scope-based approach to authorization. Based on OAuth2 authorization logic, these form part of the OpenID Connect protocol already adopted for Authentication. These scopes further restrict access to specific operations on certain endpoints within the DFC standard.

Scopes are managed by the OpenID Provider (DFC implementations often use Keycloak) as client scopes.

Scopes are formed of an action & a subject:

### Actions

*This section is non-normative (legacy v1 model).*

Actions define the permitted operations a request can perform, these are:
1. **Read** - authorised to HEAD + GET
2. **Write** - authorised to POST + PUT + PATCH (if supported)
3. **Delete** - authorised to DELETE

### Subjects

*This section is non-normative (legacy v1 model).*

Subjects define which endpoints authorization is granted to. Subjects are:

| **Subject** | **Endpoints accessible with subject** |
| --- | --- |
| Enterprise | Enterprise, Address, SocialMedia, PhoneNumber, CustomerCategory, Coordination, Place |
| Product | TechnicalProduct, LocalizedProduct, SuppliedProduct, Catalog, CatalogItem, Transformation, ConsumptionFlow, ProductionFlow |
| Price | Offer, Price |
| Order | Order, OrderLine, SaleSession, FulfilmentStatus, PaymentStatus, OrderStatus, Coordination, ShippingOption, Place, PhysicalPlace |

### Implementation of scopes by Relying Parties

*This section is non-normative (legacy v1 model).*

Relying Parties (RP's), typically platforms implementing the DFC standard, MUST implement security constraints on all DFC endpoints to ensure all requests are appropriately authenticated. They MAY also ensure tokens have the appropriate scope for the action/subject combination.

To support this it is recommended that RP's implement a data consent system, whereby data owners can grant (and revoke) access to these scopes for individual clients/users within the OIDC domain. For example a portal wishing to read data on a users Organization and Products, might request `ReadEnterprise` and `ReadProduct` access. The RP should record which Organizations have authorized a specific client or user to which scopes.

These authorizations can be communicated via properties within the dfc-t Technical Ontology. Specifically `dfc-t:assignedScope` gives details of which scopes have been granted to a client/user by an Organization, and `dfc-t:requiredScope` gives details of which scopes a client/user is requesting from an Organization.

[Startin' Blox](https://startinblox.com/) have supported members of the DFC community by providing a web component (funded by [coopcircuits](https://apropos.coopcircuits.fr/)) that can support RP's with this workflow: the [Data Sharing Module](https://github.com/startin-blox/data-sharing-module/) has full instructions on how to implement & manage scope permissions for users on your platform.
