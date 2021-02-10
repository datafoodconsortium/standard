# Protocol specification

The overall strategy is to implement the prototype in roughly two phases:

* Phase 1: implementing an MVP of the prototype, to showcase the potential of the project and help raise additional funds.
* Phase 2: the full implementation of the prototype. It will complete phase 1 to enable industrialization and professionalization of the standard through improvement of the tools, of the technical architecture, and of the possibilities of integration with external actors.

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Phase 1</th>
      <th style="text-align:left">Phase 2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Protocole</b>
      </td>
      <td style="text-align:left">
        <p>Data must be expressed in a semantic way in the API and must respect the
          OWL DFC model</p>
        <p>User-friendly and easy to implement</p>
      </td>
      <td style="text-align:left">Phase 1 + Full Semantic protocol</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Stateless or stateful</b>
      </td>
      <td style="text-align:left">Stateless</td>
      <td style="text-align:left">Stateless</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Granularity</b>
      </td>
      <td style="text-align:left">low granularity service</td>
      <td style="text-align:left">Phase 1+ high granularity via queries</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>URL</b>
      </td>
      <td style="text-align:left">REST resource driven s&#xE9;mantic, without parameter</td>
      <td style="text-align:left">Phase 1 + parameters enabling queries (SPARQL or HyperGraphQL)</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Service specifications</b>
      </td>
      <td style="text-align:left">OpenAPI except for the input/output data structure, for which use OWL.
        LDP specification compliant.</td>
      <td style="text-align:left">
        <p>- Complete OpenAPI specifications for standard services. LDP specification
          compliant.</p>
        <p>- SPARQL spec for high granularity service by Query.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Serialization</b>
      </td>
      <td style="text-align:left">
        <p>JSON-LD</p>
        <p>LDP specification compliant.</p>
      </td>
      <td style="text-align:left">
        <p>JSON-LD (JSON-LD in the data attribute if HyperGraphQL)</p>
        <p>LDP specification compliant.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Transport layer</b>
      </td>
      <td style="text-align:left">HTTP + LDP</td>
      <td style="text-align:left">HTTP + LDP
        <br />HTTP + SPARQL or HyperGraphQL</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Single or multi-source</b>
      </td>
      <td style="text-align:left">Simple access to one logical source</td>
      <td style="text-align:left">Query on multiple sources</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Right delegation</b>
      </td>
      <td style="text-align:left">No (all or nothing, all decided by the platform)</td>
      <td style="text-align:left">Yes (web ACL if SOLID used)</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Identification and authentication</b>
      </td>
      <td style="text-align:left">OIDC (h&#xE9;berg&#xE9; par les communs)</td>
      <td style="text-align:left">OIDC (or web-idOIDC if mature via Solid)</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Data storage</b>
      </td>
      <td style="text-align:left">Distributed</td>
      <td style="text-align:left">Phase 1 + centralized cache to improve performances</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>User data</b>
      </td>
      <td style="text-align:left">ID centralized by user</td>
      <td style="text-align:left">Phase 1 or web-id/OIDC if mature enough in order to achieve a decentralized
        authentication</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Product data</b>
      </td>
      <td style="text-align:left">ID provided by Open Food Facts</td>
      <td style="text-align:left">Decentralized ID management using the semantic web and SOLID</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Federation or syndication</b>
      </td>
      <td style="text-align:left">
        <p>Ontology: Federation</p>
        <p>Taxonomy: Federation</p>
        <p>Storage: Syndication</p>
        <p>Identification and authentication : Federation</p>
        <p>Validation: ?</p>
        <p>Synchronisation / caching: Federation</p>
        <p>Notification: ?</p>
        <p>Serialisation : Federation</p>
        <p>Others aspects of the protocol: Federation</p>
      </td>
      <td style="text-align:left">
        <p>Ontology: Federation</p>
        <p>Taxonomy: Federation</p>
        <p>Storage: Syndication</p>
        <p>Identification and authentication : Federation</p>
        <p>Validation: ?</p>
        <p>Synchronisation / caching: Federation</p>
        <p>Notification: ?</p>
        <p>Serialisation : Federation</p>
        <p>Others aspects of the protocol: Federation</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Interface &amp; server</b>
      </td>
      <td style="text-align:left">Native web components &amp; NodeJs</td>
      <td style="text-align:left">Phase 1 but Interface could be React OR Startin&#x2019;blox</td>
    </tr>
  </tbody>
</table>

Federation: all entities follow the same protocol

Syndication: entities may have different protocols

