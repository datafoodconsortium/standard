# 🚧 Authorization grants

**Status:** Draft proposal — detail page for the [v2 authorization strategy](authorization-strategy.md).
**Namespace:** `https://www.datafoodconsortium.org#` (`dfc-t:`). Business-ontology terms below use `dfc-b:`; both prefixes resolve against the DFC ontology namespaces — do not conflate the two in new Turtle/JSON-LD.

The key words **MUST**, **MUST NOT**, **SHOULD**, **MAY**, and **OPTIONAL** are to be interpreted as described in RFC 2119 / RFC 8174.

## 1. Vocabulary

*This section is normative.*

### Classes

*This section is normative.*

| Class | Meaning |
|---|---|
| `dfc-t:AuthorizationPolicy` | A reusable authorization policy |
| `dfc-t:AuthorizationGrant` | A grant of authority from one Agent to another |
| `dfc-t:AuthorizationRequest` | A request submitted to a PDP |
| `dfc-t:AuthorizationDecision` | Result of authorization evaluation |
| `dfc-t:AuthorizationCondition` | A condition limiting a grant or policy |
| `dfc-t:AuthorizationObligation` | An action required when access is permitted |
| `dfc-t:AuthorizationFilter` | A restriction applied to returned data |
| `dfc-t:AuthorizationAction` | An operation being authorized |
| `dfc-t:AuthorizationResource` | An authorization target |
| `dfc-t:AuthorizationRelationship` | A relationship used during authorization |

Classes SHOULD be defined as subclasses of appropriate existing DFC technical concepts where such exist, and released through the normal DFC ontology release process (no separate `dfc-auth` namespace).

### Core properties

*This section is normative.*

| Property | Domain | Range | Meaning |
|---|---|---|---|
| `dfc-t:authorizationSubject` | `AuthorizationGrant` | Resource (SHOULD be a WebID) | Agent to whom authorization applies |
| `dfc-t:grantor` | `AuthorizationGrant` | Resource | Agent delegating authority |
| `dfc-t:grantee` | `AuthorizationGrant` | Resource | Agent receiving delegated authority |
| `dfc-t:authorizationAction` | — | `AuthorizationAction` | Operation: `dfc-t:Read`, `Create`, `Update`, `Delete`, `Delegate` (extensible) |
| `dfc-t:authorizationResource` | — | Resource | Protected individual resource |
| `dfc-t:authorizationResourceType` | — | Class | Class of resources covered (e.g. `dfc-b:CatalogItem`) |
| `dfc-t:authorizationProperty` | — | Property | Individual RDF property covered (e.g. `dfc-b:price`) |
| `dfc-t:requiredScope` | — | string | OIDC scope associated with the policy/grant (e.g. `"dfc:catalog.read"`). Scopes MUST NOT be interpreted as replacing semantic authorization. |

## 2. Grant model

*This section is normative (the Turtle and JSON-LD illustrate the normative model).*

A `dfc-t:AuthorizationGrant` represents an authorization delegation. The minimal grant contains grantor, grantee, action, and a resource or resource type:

```turtle
@prefix dfc-t: <https://www.datafoodconsortium.org#> .
@prefix dfc-b: <https://www.datafoodconsortium.org#> .

<https://auth.example/grants/7f31>
    a dfc-t:AuthorizationGrant ;
    dfc-t:grantor <https://alice.example/#me> ;
    dfc-t:grantee <https://market.example/client> ;
    dfc-t:authorizationAction dfc-t:Read ;
    dfc-t:authorizationResource <https://farm.example/products/123> ;
    dfc-t:authorizationProperty dfc-b:price .
```

> Alice grants the marketplace client permission to read the `price` property of product `123`.

The equivalent JSON-LD representation is RECOMMENDED for HTTP APIs:

```json
{
  "@context": {
    "dfc-t": "https://www.datafoodconsortium.org#",
    "grantor": "dfc-t:grantor",
    "grantee": "dfc-t:grantee",
    "action": "dfc-t:authorizationAction",
    "resource": "dfc-t:authorizationResource",
    "resourceType": "dfc-t:authorizationResourceType",
    "property": "dfc-t:authorizationProperty",
    "validUntil": "dfc-t:validUntil",
    "Read": "dfc-t:Read"
  },
  "@id": "https://auth.example/grants/7f31",
  "@type": "dfc-t:AuthorizationGrant",
  "grantor": "https://alice.example/#me",
  "grantee": "https://market.example/client",
  "action": "Read",
  "resource": "https://farm.example/products/123",
  "property": "https://www.datafoodconsortium.org#price",
  "validUntil": "2026-12-31T23:59:59Z"
}
```

## 3. Grant levels

*This section is normative.*

A grant MAY omit `dfc-t:authorizationProperty` — this grants access to the resource as a whole. A property-level grant MUST be more specific than its resource-level equivalent, and a resource-level `ALLOW` MUST NOT automatically expose properties subject to a more-specific `DENY`. Grants MAY also apply to all resources of a class via `dfc-t:authorizationResourceType` (optionally narrowed to one property).

## 4. Inheritance

*This section is normative.*

The following precedence applies (most specific first):

```text
property/resource-specific > resource > resource collection
    > resource type > relationship/policy > default
```

An implementation MUST support inheritance at least for resource hierarchies or equivalent logical collections (e.g. a grant on an Organization's Catalog MAY apply to its Products). The mechanism MUST NOT require URL hierarchy — SQL, graph, or document implementations MAY represent the relationship differently.

## 5. Explicit deny and default

*This section is normative.*

A rule MAY explicitly deny an operation:

```turtle
<https://auth.example/policies/no-cost>
    a dfc-t:AuthorizationPolicy ;
    dfc-t:authorizationSubject <https://market.example/client> ;
    dfc-t:authorizationAction dfc-t:Read ;
    dfc-t:authorizationResourceType dfc-b:Product ;
    dfc-t:authorizationProperty dfc-b:supplierCost ;
    dfc-t:effect dfc-t:Deny .
```

Effective precedence (deny-overrides): **explicit DENY > specific ALLOW > inherited ALLOW > default DENY**. The default decision MUST be `DENY`; absence of a policy MUST NOT be interpreted as unrestricted access.

## 6. Relationship-based authorization

*This section is normative.*

Authorization MAY be based on relationships between Agents and resources (`memberOf`, `owns`, `manages`, `represents`, `employedBy`, `supplierOf`, `customerOf`, …). The relationship vocabulary SHOULD reuse existing DFC business ontology properties wherever possible. A policy can then express e.g. `memberOf(subject, resource.owner) → ALLOW read`. Relationships evidenced by Verifiable Credentials are covered on the [trust and credentials](authorization-trust-and-credentials.md#4-verifiable-credentials-as-authorization-evidence) page.

## 7. Delegation

*This section is normative.*

Delegation is distinct from authentication: Alice delegates to the marketplace application, which then acts on her behalf at the Resource Server. A delegation MUST identify grantor, grantee, action, resource/resource type, and optionally property and expiration. A grant MAY itself be re-delegated only when it explicitly permits delegation (`dfc-t:Delegate`); a grantee MUST NOT delegate authority exceeding what it received (`effective(child) = intersection(parent, child)`); a chain MUST terminate if any parent authorization is revoked or expired. Implementations SHOULD impose a maximum delegation depth.

## 8. Expiration and revocation

*This section is normative.*

A grant MAY carry `dfc-t:validFrom` / `dfc-t:validUntil`; outside its validity interval it MUST NOT produce an `ALLOW`. Every grant SHOULD have a globally unique identifier, and revocation SHOULD be represented independently of deleting the grant (e.g. a `dfc-t:AuthorizationRevocation` resource with `dfc-t:revokes` / `dfc-t:revokedAt`). A revoked grant MUST NOT produce an `ALLOW`, and resource servers MUST account for revocation when caching decisions (short TTLs or explicit invalidation; fresh evaluation for high-risk writes).

## 9. Grant management API

*This section is normative.*

An Authorization Server or grant-management service MAY expose:

* `POST /authorization/grants` (JSON-LD body) → `201 Created` with `Location` of the grant.
* `GET /authorization/grants/{id}` — a caller MUST only receive a grant it is itself authorized to view; grant documents MUST NOT be publicly readable.
* `DELETE /authorization/grants/{id}` or `POST /authorization/grants/{id}/revoke` (RECOMMENDED when auditability is required) — revocation MUST prevent subsequent decisions from relying on the grant.
* `POST /authorization/introspect` — PDP evaluation endpoint returning `allow`/`deny` (optionally with the granting URI and per-property decisions); MUST NOT expose policy information to unauthorized callers.

## 10. Enforcement: filtering before serialization

*This section is normative.*

Authorization MAY produce a response filter; the PEP MUST enforce it, and authorization MUST occur **before** serialization or transmission — never retrieve-then-hide, and never rely on the client to hide data. Rules:

* **Properties**: each protected property is evaluated independently (e.g. `name`/`description`/`price` ALLOW, `supplierCost` DENY → the property is omitted from the response).
* **Nested resources**: authorization is evaluated against the actual resource/property pair; `Product/123 → producer → name` MUST NOT inherit the authorization of `Product/123 → producer` unless a policy explicitly permits it.
* **Collections/queries**: unauthorized members MUST be excluded server-side (`find Product where subject is authorized`, not `find all then hide`); authorization predicates SHOULD be pushed into the query engine (SQL/SPARQL/document equivalent). Enumeration resistance applies: avoid revealing whether a protected resource exists to callers not authorized to discover it.

## 11. Complete example

*This section is non-normative (worked example).*

Grants (marketplace may read descriptions and prices of catalog items; supplier cost is denied):

```turtle
<https://auth.example/grants/catalog-read>
    a dfc-t:AuthorizationGrant ;
    dfc-t:grantor <https://alice.example/#me> ;
    dfc-t:grantee <https://market.example/client> ;
    dfc-t:authorizationAction dfc-t:Read ;
    dfc-t:authorizationResourceType dfc-b:CatalogItem ;
    dfc-t:authorizationProperty dfc-b:description ;
    dfc-t:requiredScope "dfc:catalog.read" .

<https://auth.example/grants/catalog-price>
    a dfc-t:AuthorizationGrant ;
    dfc-t:grantor <https://alice.example/#me> ;
    dfc-t:grantee <https://market.example/client> ;
    dfc-t:authorizationAction dfc-t:Read ;
    dfc-t:authorizationResourceType dfc-b:CatalogItem ;
    dfc-t:authorizationProperty dfc-b:price ;
    dfc-t:requiredScope "dfc:catalog.read" .

<https://auth.example/policies/no-supplier-cost>
    a dfc-t:AuthorizationPolicy ;
    dfc-t:authorizationAction dfc-t:Read ;
    dfc-t:authorizationResourceType dfc-b:CatalogItem ;
    dfc-t:authorizationProperty dfc-b:supplierCost ;
    dfc-t:effect dfc-t:Deny .
```

Request `GET /products/123` with token claims `sub = webid = https://alice.example/#me`, `client_id = https://market.example/client`, `scope = "openid webid dfc:catalog.read"` yields `description → visible, price → visible, supplierCost → absent` — even though the OIDC scope is present. The scope is the coarse API gate; the grants are the fine-grained data gate.

## 12. Implementation notes

*This section is normative.*

* **Storage independence**: the semantic model is normative, the physical representation is not. Relational (`authorization_grant` table), graph, or Solid (mapped onto WAC/ACP) implementations MUST produce equivalent semantics; WAC/UMA mappings are implementation options, not requirements.
* **Security**: the Resource Server MUST distinguish user from client application (confused-deputy); delegated authority MUST NOT exceed the grantor's own authority (privilege escalation); grants exchanged between platforms SHOULD be integrity-protected (signatures or authenticated transport — see [Verifiable Grants](authorization-trust-and-credentials.md#5-verifiable-grant)); grant metadata MUST itself be access-controlled.
* **Caching**: decisions MAY be cached keyed by subject, client, action, resource, property, and policy/grant state; a cache MUST NOT return `ALLOW` after a known revocation.
