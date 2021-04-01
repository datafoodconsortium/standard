# Protocols specifications

The overall strategy is to implement the prototype in roughly two phases:

* Esential: implementing an MVP of the prototype \(DFC\), to showcase the potential of the project and help raise additional funds.
* Target : the full implementation of the prototype \(DFC\). It will complete phase 1 to enable industrialization and professionalization of the standard through improvement of the tools, of the technical architecture, and of the possibilities of integration with external actors.

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Essential Platorm</th>
      <th style="text-align:left">Essential DFC</th>
      <th style="text-align:left">Target Platform</th>
      <th style="text-align:left">Target DFC</th>
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
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential </em>
        </p>
        <p><em>Platorm </em><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential </em>
        </p>
        <p><em>Platorm</em> + LDP</p>
        <p><b>IN PROGRESS</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential </em>
        </p>
        <p><em>DFC + </em>Full Semantic protocol (LDP + SPARQL)</p>
        <p><b>DONE</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Stateless or stateful</b>
      </td>
      <td style="text-align:left">
        <p>Stateless</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left">Stateless<em><br /></em><b>DONE</b>
      </td>
      <td style="text-align:left"><em>Target Platform<br /></em><b>DONE</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Granularity</b>
      </td>
      <td style="text-align:left">
        <p>low granularity service (get all data of an authentified user)</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential platorm</em>
        </p>
        <p><b>ABANDONED</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential platorm </em>+ high granularity via LDP</p>
        <p><b> IN PROGRESS</b>
        </p>
      </td>
      <td style="text-align:left">
        <p>high granularity via LDP</p>
        <p><b>DONE</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>URL</b>
      </td>
      <td style="text-align:left">
        <p>REST resource driven s&#xE9;mantic, without parameter</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left">
        <p><em>Esential </em>
        </p>
        <p><em>Platorm</em> 
          <br /><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Target DFC </em>+</p>
        <p>parameters enabling queries (SPARQL or HyperGraphQL)</p>
        <p>SPARQL : <b>DONE</b>
          <br />HyperGraphQL : <b>OUTLOOK</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Service specifications</b>
      </td>
      <td style="text-align:left">
        <p>OpenAPI except for the input/output data structure, for which use OWL.
          LDP specification compliant.</p>
        <p><b>OUTLOOK</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>OUTLOOK</b>
      </td>
      <td style="text-align:left">
        <p>Complete OpenAPI specifications for standard services. LDP specification
          compliant.</p>
        <p><b>OUTLOOK</b>
        </p>
      </td>
      <td style="text-align:left">
        <p>SPARQL spec for high granularity service by Query</p>
        <p><b>OUTLOOK</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Serialization</b>
      </td>
      <td style="text-align:left">
        <p>JSON-LD</p>
        <p>LDP specification compliant.
          <br /><b>DONE</b>
        </p>
        <p></p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left">
        <p><em>Esential </em>
        </p>
        <p><em>Platorm</em> 
          <br /><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential DFC</em> + JSON-LD in the data attribute if HyperGraphQL</p>
        <p><b>OUTLOOK</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Transport layer</b>
      </td>
      <td style="text-align:left">
        <p>HTTP</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left"><em>Esential DFC<br /></em><b>DONE</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Single or multi-source</b>
      </td>
      <td style="text-align:left">
        <p>Simple access to one logical source</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left">
        <p>Query on multiple sources</p>
        <p><b>OUTLOOK</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Right delegation</b>
      </td>
      <td style="text-align:left">
        <p>Not standardized
          <br />OIDC authentification + platform manage acces to data using authentification
          : authentified user data or more</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>IN PROGRESS</b>
      </td>
      <td style="text-align:left">
        <p>Yes (web ACL Compliance)</p>
        <p><b>OUTLOOK</b>
        </p>
      </td>
      <td style="text-align:left">
        <p>Yes (web ACL implementation)</p>
        <p><b>OUTLOOK</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Identification and authentication</b>
      </td>
      <td style="text-align:left">
        <p>OIDC (hosted by lescommuns.org)</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left">
        <p>webId-OIDC (decentralised authentification serveur : need platform impl&#xE9;mentation)
          <br
          /><b>OUTLOOK</b>
        </p>
        <p></p>
      </td>
      <td style="text-align:left"><em>Target Platform<br /></em><b>OUTLOOK</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Data storage</b>
      </td>
      <td style="text-align:left">
        <p>Distributed data on each platform</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p>linked uri directory</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential platorm</em>
        </p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential DFC</em> +semantic cache</p>
        <p><b>IN PROGRESS</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>User data</b>
      </td>
      <td style="text-align:left">
        <p>ID centralized by lescommuns.org</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left">webId-OIDC (decentralised authentification serveur : need platform impl&#xE9;mentation)
        <br
        /><b>OUTLOOK</b>
      </td>
      <td style="text-align:left"><em>Target Platform<br /></em><b>OUTLOOK</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Product data</b>
      </td>
      <td style="text-align:left">
        <p>Decentralized ID management reconcile thanks to linked uri directory</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left"><em>Esential platorm<br /></em><b>DONE</b>
      </td>
      <td style="text-align:left">
        <p><em>Esential platorm</em>
        </p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><em>Esential DFC</em>
        </p>
        <p><b>DONE</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Interface &amp; server of prototype</b>
      </td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left">
        <p>Native web components &amp; NodeJs</p>
        <p><b>DONE</b>
        </p>
      </td>
      <td style="text-align:left">NA</td>
      <td style="text-align:left">
        <p>Semantic Serveur (Semapps)</p>
        <p>Interface could be React OR Startin&#x2019;blox</p>
        <p>server : <b>DONE</b>
          <br />interface : <b>OUTLOOK</b>
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Federation vs Syndication Summary

Federation: all entities follow the same protocol

Syndication: entities may have different protocols

Ontology: Federation

Taxonomy: Federation

Storage: Syndication

Identification and authentication : Federation

Validation: ?

Synchronisation / caching: Federation

Notification: ?

Serialisation : Federation

Others aspects of the protocol: Federation

