# Practical Examples

## Get Data linked to Connected Users

* No specification for url syntax
* Requests have to fill Authorization Header with JWT OIDC Token obtained thanks to login.lescommuns.org application.
* Basic authentification
  * Server has to validate token thanks to signature and parse token to obtain preferred\_username.
  * This preferred\_username can be linked to the Platform User. To link OIDC User and Platform User, platforms can add OIDC authentification feature.
* Outdated Token
  * If token is outdated, platforms can refuse request.
  * If platforms have to request another \(DFC or Other\) and receive outaded input token, Platforms can remember refresh token \(when OIDC authentication features execution\) and ask new access with a specific request.

### Get examples from all format versions

As we developed the DFC Standard aside the DFC Prototype, we had to tweak the data format with the standard evolution.

* [version 1.4](practical-examples/version-1-4.md)
* [version 1.3](practical-examples/version-1-3.md)
* [version 1.2](practical-examples/version-1-2.md)
