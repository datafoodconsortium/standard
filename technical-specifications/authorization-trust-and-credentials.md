# 🚧 Authorization trust and credentials

**Status:** Draft proposal — detail page for the [v2 authorization strategy](authorization-strategy.md).
**Namespace:** `dfc-t: https://www.w3id.org/dfc/ontology/src/DFC_TechnicalOntology.owl#`, `dfc-b: https://www.w3id.org/dfc/ontology/src/DFC_BusinessOntology.owl#`.
**External terms referenced (informative):** `solid:` = `http://www.w3.org/ns/solid/terms#`.

This page answers four questions the base model leaves open: how a Resource Server verifies that a token issuer may speak for a WebID (§1), how it verifies a named client (§2), how Verifiable Credentials serve as relationship evidence (§3), and how a grant itself becomes a portable credential (§4). Credential issuance/presentation protocols (e.g. OIDC for VCI/VP) are out of scope — this page specifies the credential data model and PDP consumption, not transport.

The key words **MUST**, **MUST NOT**, **SHOULD**, **MAY**, and **OPTIONAL** are to be interpreted as described in RFC 2119 / RFC 8174.

## 1. WebID trust binding

*This section is normative.*

In a single-platform deployment the token issuer and Resource Server share an operator, so trusting a WebID claim is trivial. In decentralized deployments it is the central question: a token asserting `webid: https://alice.example/#me` is not by itself evidence that its issuer may speak for `alice.example`. A Resource Server accepting a WebID claim MUST establish, through one of the methods below, that the token issuer is one the WebID's controller designated as authoritative. A claim with no successful binding MUST NOT be treated as authenticated for semantic authorization, even if the OIDC signature itself is valid.

### Trust binding methods

*This section is normative.*

```turtle
dfc-t:TrustBindingMethod a rdfs:Class .
dfc-t:webIdTrustMethod a rdf:Property ;
    rdfs:domain dfc-t:AuthorizationDecision ;
    rdfs:range dfc-t:TrustBindingMethod .
```

| Method | Requires WebID profile | Cross-platform | Rule |
|---|---|---|---|
| `dfc-t:SolidOidcIssuerBinding` | yes | yes | Profile's `solid:oidcIssuer` triple matches token `iss` |
| `dfc-t:IssuerAllowlistBinding` | yes | limited (deployment-local allowlist) | WebID pattern → trusted issuers map |
| `dfc-t:CredentialTrustBinding` | no | yes | Registrar-issued credential binds WebID to issuer/key (§3) |
| `dfc-t:PlatformLocalBinding` | no | no | Degenerate OIDC case: subject is the `(iss, sub)` pair, no portability implied |

A Resource Server MUST declare which methods it accepts via [capability discovery](#7-capability-discovery) and MAY accept several. Subjects bound only through `PlatformLocalBinding` MUST NOT be treated as equivalent to the same string appearing as a WebID elsewhere. Because a profile's `solid:oidcIssuer` triple controls who may authenticate as that WebID, servers relying on it SHOULD treat the profile document's own access controls as part of the trust boundary.

Example profile fragment satisfying Solid-OIDC issuer binding:

```turtle
@prefix solid: <http://www.w3.org/ns/solid/terms#> .
<https://alice.example/#me>
    solid:oidcIssuer <https://idp.alice-coop.example> .
```

Normatively: validate `iss` by ordinary OIDC signature verification, dereference the WebID profile, require a matching `solid:oidcIssuer` triple — otherwise the claim is unauthoritative and the request is denied. The successful method SHOULD be recorded as `dfc-t:webIdTrustMethod` in the decision record for audit.

## 2. Client identity resolution

*This section is normative.*

The model distinguishes **subject** (who authorized: Alice) from **client** (which application acts: marketplace). A grant naming a specific client in `dfc-t:grantee` is only as strong as the mechanism binding that name to the requesting party. A server evaluating such a grant MUST verify the client through one of:

```turtle
dfc-t:ClientIdentityMethod a rdfs:Class .
dfc-t:clientIdentityMethod a rdf:Property ;
    rdfs:domain dfc-t:AuthorizationDecision ;
    rdfs:range dfc-t:ClientIdentityMethod .
```

* `dfc-t:RegisteredOAuthClient` — identity via the Authorization Server's client registration/authentication (secret, `private_key_jwt`, mTLS); the `client_id` claim is trusted to the degree the token issuer is trusted.
* `dfc-t:ClientIdDocument` — Solid-OIDC-style dereferenceable client URI; the server MUST dereference it and confirm it declares the redirect URI/origin actually used. Same worth as a registered client — the mechanism changes how identity is established, not what it is worth.

An unverifiable client MUST be treated as absent (not as the raw `client_id` string): grants naming a specific client MUST NOT match an unresolved client. A grant MAY name the subject, the client, or require both.

## 3. DPoP proof-of-possession

*This section is normative.*

DFC implementations MAY require OAuth 2.0 DPoP for protected operations. When required, the Resource Server MUST: constrain the access token to a public key; require a valid DPoP proof bound to the HTTP method + URI with replay prevention; verify the proof key matches the token-bound key. DPoP failure when required is DENY — a failed proof MUST NOT become an `ALLOW`, and a valid proof MUST NOT by itself constitute semantic authorization (`valid DPoP ≠ permission to access a resource`).

The model MUST keep three values distinct: `authorizationSubject` (WebID/principal), `client` (OAuth client), proof key (DPoP JWK thumbprint) — they MAY differ and MUST NOT be conflated. The semantic grant remains the authorization source; DPoP only binds the token to the requesting key. The semantic model MUST remain usable with plain bearer tokens where DPoP is not required; deployments requiring DPoP MUST advertise it through OAuth/HTTP mechanisms and discovery.

## 4. Verifiable Credentials as authorization evidence

*This section is normative.*

A Verifiable Credential is **evidence a PDP may consume**, not itself an authorization mechanism: the grant/policy/relationship model stays authoritative on ALLOW/DENY, and a credential only supplies a fact the PDP would otherwise fetch by live dereference. Deployments with no use for portable evidence MAY ignore this section and satisfy relationship authorization through live triples.

Rules: offered credentials MUST carry a verifiable proof (Data Integrity, JOSE/COSE, SD-JWT VC, …) validated against the issuer's key material — unsecured assertions MUST NOT be accepted. The relationship link is:

```turtle
dfc-t:assertedBy a rdf:Property ;
    rdfs:domain dfc-t:AuthorizationRelationship ;
    rdfs:range rdfs:Resource .
```

identifying the credential asserting the relationship instead of a live triple (e.g. Alice's `memberOf` the cooperative, issued and signed by the cooperative). Trust in the issuer is deployment policy: validating a proof establishes only that the issuer said it, so a deployment MUST configure which issuers it accepts for which relationship types / trust bindings, and MUST treat credentials from unaccepted issuers as absent (not as a distinct error).

## 5. Verifiable Grant

*This section is normative.*

A `dfc-t:AuthorizationGrant` MAY be issued as a Verifiable Credential ("Verifiable Grant"), letting a Resource Server that never contacted the grantor's Authorization Server verify authenticity directly. It MUST carry `VerifiableCredential` and `dfc-t:AuthorizationGrant` together in `type` and be secured by a verifiable proof. Non-VC grants remain valid bare RDF resources — this adds a form, it does not withdraw one.

Property mapping within a Verifiable Grant (VC-native properties replace their `dfc-t:` equivalents):

| Base grant property | Verifiable Grant equivalent |
|---|---|
| `dfc-t:grantor` | `issuer` (MUST be used instead) |
| `dfc-t:grantee` | `credentialSubject.id` |
| `dfc-t:validFrom` / `dfc-t:validUntil` | VC-native `validFrom` / `validUntil` |
| `dfc-t:AuthorizationRevocation` | `credentialStatus` (Bitstring Status List SHOULD replace the revocation resource) |

`authorizationAction`, `authorizationResource`, `authorizationProperty` are unchanged and MUST appear within `credentialSubject`. Example:

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    { "dfc-t": "https://www.w3id.org/dfc/ontology/src/DFC_TechnicalOntology.owl#" }
  ],
  "id": "https://auth.example/grants/7f31",
  "type": ["VerifiableCredential", "dfc-t:AuthorizationGrant"],
  "issuer": "https://alice.example/#me",
  "validFrom": "2026-10-01T00:00:00Z",
  "validUntil": "2026-12-31T23:59:59Z",
  "credentialSubject": {
    "id": "https://market.example/client",
    "dfc-t:authorizationAction": "dfc-t:Read",
    "dfc-t:authorizationResource": "https://farm.example/supplied-products/123",
    "dfc-t:authorizationProperty": "https://www.w3id.org/dfc/ontology/src/DFC_BusinessOntology.owl#description"
  },
  "credentialStatus": {
    "id": "https://auth.example/status/3#94567",
    "type": "BitstringStatusListEntry",
    "statusPurpose": "revocation",
    "statusListIndex": "94567",
    "statusListCredential": "https://auth.example/status/3"
  },
  "proof": {
    "type": "DataIntegrityProof",
    "cryptosuite": "eddsa-rdfc-2022",
    "verificationMethod": "https://alice.example/#key-1",
    "proofPurpose": "assertionMethod",
    "proofValue": "z58DAdFfa9SkqZMVPxAQpic7ndSayn1PzZs6ZjWp1CktyGesjuTSwRdoWhAfGFCF5bppETSTojQCrfFPP2oumHKtz"
  }
}
```

Revocation via the (cacheable, batchable) status list replaces per-grant live introspection; "revoked ⇒ no ALLOW" is unchanged. Verification order for a candidate Verifiable Grant: proof → issuer resolution → status check → issuer-is-grantor check → the ordinary grant checks (validity window, grantee identity, delegation constraints); failure at any step discards the grant.

**Selective disclosure caveat**: where the resource representation is itself a signed credential, redacting a property invalidates an ordinary signature. A server MUST then use a selective-disclosure-capable mechanism (SD-JWT VC, DI selective-disclosure suite) or fall back to an all-or-nothing resource-level decision — never return a tampered credential.

## 6. Normative authorization algorithm (merged)

*This section is normative.*

This merges the base 17-step algorithm with the trust/client/credential amendments into a single step numbering:

1. **Authenticate** — validate credentials; failure → `DENY`, HTTP 401.
2. **Resolve WebID** — determine accepted trust binding methods; no WebID claim + `PlatformLocalBinding` accepted → bind `(iss, sub)`; else attempt each accepted method (Solid-OIDC issuer check, allowlist, credential binding) until one succeeds; none succeeds → `DENY`, HTTP 401. Record the method in the decision.
3. **Resolve client** — determine the client identity method (registered OAuth client or Client ID Document) and verify; failure → client unresolved, and grants naming a specific client MUST NOT match.
4. **Validate DPoP when required** — proof validity, method+URI binding, freshness/replay, token-key match; any failure → `DENY`.
5. **Determine action** — map the HTTP operation per the [action table](authorization-strategy.md#8-action-mapping).
6. **Validate OIDC scope** — missing required scope → `DENY`; the PDP MUST NOT override this.
7. **Resolve target resource** — specific resource, else resource type/collection target.
8. **Resolve requested properties** — what would be exposed (read) or modified (write); each is independently authorizable.
9. **Retrieve applicable grants/policies** — direct, inherited, type-level, relationship, denies, delegations, conditions, revocations.
10. **Validate grants** — for Verifiable Grants first: proof, issuer resolution, `credentialStatus`, issuer-is-grantor; then for all: validity window, expiration, revocation, grantor authority, grantee identity, delegation constraints. Invalid → discard.
11. **Evaluate relationships** — resolve live triples; where evidenced by `dfc-t:assertedBy`, verify credential proof + issuer acceptance, else treat as absent.
12. **Evaluate conditions** — against request context (time, client, purpose, tenant, origin…); unevaluable ⇒ unsatisfied.
13. **Apply deny-overrides** — any applicable explicit deny → `DENY`.
14. **Evaluate allows** — at least one applicable allow → `ALLOW`, else `DENY`.
15. **Apply property-level decisions** — per property, independently.
16. **Apply obligations/filters** — mask, remove, transform, audit, restrict-export; the PEP MUST enforce them.
17. **Serialize** — only the authorized representation.

Conceptually: `Authorize(subject, client, action, resource, property, scope, context)` denies on invalid authentication, absent scope, invalid required DPoP, explicit deny, or no applicable allow — and the response is `{ p ∈ R | P(subject, client, action, resource, p) = ALLOW }`.

## 7. Capability discovery

*This section is normative.*

A DFC API SHOULD expose its authorization capabilities, extended with trust bindings:

```json
{
  "dfc_authorization": {
    "supported": true,
    "resource_level": true,
    "property_level": true,
    "resource_type": true,
    "inheritance": true,
    "delegation": true,
    "relationship_based": true,
    "explicit_deny": true,
    "revocation": true,
    "dpop": { "supported": true, "required": true },
    "accepted_trust_bindings": [
      "dfc-t:SolidOidcIssuerBinding",
      "dfc-t:CredentialTrustBinding"
    ]
  }
}
```

Standard OAuth/OIDC discovery SHOULD also be exposed, and protected servers MAY advertise their Authorization Server via `WWW-Authenticate: Bearer, as_uri="…"` (see [strategy §6](authorization-strategy.md#6-http-behavior)).

## 8. Conformance tests

*This section is normative (minimum test suite for conformance claims).*

Minimum test suite (extends the base table with the trust/credential cases):

| Test | Expected |
|---|---|
| Valid WebID + valid scope + grant | ALLOW |
| Invalid authentication | 401 |
| Valid authentication + missing scope | 403 |
| Valid scope + no grant | DENY |
| Resource / property / class grant | ALLOW (property only where narrowed) |
| Property deny; explicit deny + inherited allow | DENY property |
| Expired / revoked grant | DENY |
| Valid delegation; delegation exceeding parent authority | ALLOW / DENY |
| Unauthorized collection member / property / nested property | omitted |
| Relationship grant; broken relationship | ALLOW / DENY |
| Unknown condition; valid grant + wrong client | DENY |
| Required DPoP missing / invalid | DENY |
| Valid DPoP + missing semantic grant | DENY |
| WebID claimed, no trust binding satisfied | DENY, 401 |
| Solid-OIDC token, matching / non-matching `solid:oidcIssuer` | proceed / DENY 401 |
| Client named by grant, presented identity unresolved; Client ID Document with matching redirect | grant does not match / client bound |
| Verifiable Grant, valid proof + active status + issuer = owner | ALLOW (subject to remaining checks) |
| Verifiable Grant, tampered proof / revoked status / issuer ≠ owner without authority | DENY |
| Relationship only via untrusted-issuer credential | treated as absent |
| Property filter on non-selective-disclosure credential | resource-level decision only |

## 9. Recommended implementation sequence

*This section is non-normative (sequencing guidance).*

```text
v0.1  WebID, OIDC scopes, subject, client, resource, action, ALLOW/DENY
v0.2  property, resource type, inheritance, filtering, explicit deny
v0.3  grantor, grantee, expiration, revocation
v0.4  relationship authorization, delegation chains, conditions
v0.5  DPoP-bound access tokens, request validation, discovery
v1.0-a  WebID trust binding (§1), client identity resolution (§2)
v1.0-b  VC relationship evidence (§4)
v1.0-c  Verifiable Grants (§5), status-list revocation, selective disclosure
v1.0    inter-platform authorization, discovery, conformance suite
```

## Appendix: informative references

*This section is non-normative.*

```text
[VC-DATA-MODEL-2.0](https://www.w3.org/TR/vc-data-model-2.0/)  Verifiable Credentials Data Model v2.0. W3C Recommendation, 15 May 2025.
[VC-BITSTRING-STATUS-LIST](https://www.w3.org/TR/vc-bitstring-status-list/)  Bitstring Status List v1.0. W3C Recommendation, 15 May 2025.
[SOLID-OIDC](https://solidproject.org/TR/oidc)  Solid-OIDC. Solid Community Group Report (actively maintained;
    pin a revision date when citing normatively, as the text is still evolving).
```
