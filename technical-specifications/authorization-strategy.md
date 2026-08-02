# Authorization Strategy Recommendation

This authorization strategy extends the [authentication strategy](./authentication-strategy.md) to allow platforms to grant permissions to individual endpoints and Organizations within the DFC Standard.

To manage authorization of requests across the DFC standard, it is necessary to apply authorization scopes to each request. These scopes determine whether an authenticated request can access certain data from DFC endpoints.

**These are transitional recommendations, and are intended to be superceded by a more granular permissioning protocol in a future version of the DFC standard.**


## Scope based authorization

We propose a scope-based approach to authorization. Based on OAuth2 authorization logic, these form part of the OpenID Connect protocol already adopted for Authentication. These scopes further restrict access to specific operations on certain endpoints within the DFC standard.

Scopes are managed by the OpenID Provider (DFC implmentations often use Keycloak) as client scopes.

Scopes are formed of an action & a subject:

### Actions

Actions define the permitted operations a request can perform, these are:
1. **Read** - authorised to HEAD + GET
2. **Write** - authorised to POST + PUT + PATCH (if supported)
3. **Delete** - authorised to DELETE

### Subjects

Subjects define which endpoints authorization is granted to. Subjects are:

| **Subject** | **Endpoints accessible with subject** |
| --- | --- |
| Enterprise | Enterprise, Address, SocialMedia, PhoneNumber, CustomerCategory, Coordination, Place |
| Product | TechnicalProduct, LocalizedProduct, SuppliedProduct, Catalog, CatalogItem, Transformation, ConsumptionFlow, ProductionFlow |
| Price | Offer, Price |
| Order | Order, OrderLine, SaleSession, FulfilmentStatus, PaymentStatus, OrderStatus, Coordination, ShippingOption, Place, PhysicalPlace |

## Implementation of scopes by Relying Parties

Relying Parties (RP's), typically platforms implementing the DFC standard, must implement security constraints on all DFC endpoints to ensure all requests have the appropriate scope for the action/subject combination.

Furthermore it is recommended that RP's implement a data consent system, whereby data owners can grant (and revoke) access to these scopes for individual clients/users within the OIDC domain. For example a portal wishing to read data on a users Organization and Products, might request `ReadEnterprise` and `ReadProduct` access. The RP should record which Organizations have authorized a specific client or user to which scopes.

These authorizations can be communicated via properties within the dfc-t Technical Ontology. Specifically `dfc-t:assignedScope` gives details of which scopes have been granted to a client/user by an Organization, and `dfc-t:requiredScope` gives details of which scopes a client/user is requesting from an Organization.

[Startin' Blox](https://startinblox.com/) have supported members of the DFC community by providing a web component (funded by [coopcircuits](https://apropos.coopcircuits.fr/)) that can support RP's with this workflow: the [Data Sharing Module](https://github.com/startin-blox/data-sharing-module/) has full instructions on how to implement & manage scope permissions for users on your platform.
