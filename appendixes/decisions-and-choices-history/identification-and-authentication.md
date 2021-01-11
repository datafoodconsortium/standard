# Identification and authentication

Rights management covers the issues of authentication, identification, and rights management granted by an agent to another agent.

* **Identification** = we know who is the user trying to query the data. **Who we are**.
* **Authentication** = a technical way to check that the person is who he pretends to be and not a hacker. **It's the validation of who we are**.
* **Authorization = what data and functionality we have access to.**

In our case we need a way to perform these steps in an universal way and using the same credentials across platform, aka [SSO](https://en.wikipedia.org/wiki/Single_sign-on). We need it because if we take the case of the prototype, a producer who wants to visualize his overall catalog will have to request data from all the platforms he uses. Without SSO, he would have to connect from the prototype to each platform specifically which in terms of UX is not satisfactory.

Who performs these steps? Is there a single or several servers that know who is who and how to verify it? This is the difference between centralized and decentralized identification and authentication.

**1- Centralized**, it is possible to use a secure access to the server that hosts the data as a certificate \(private key, public key\). This is not the case for this project.

**2- Delegation to a centralized entity**. Protocols exist that enable multiple servers to perform identification/authentication, sharing a common OIDC or OAuth or SAML standard. This solution involves technical difficulties to become a reliable and trusted server. If an unreliable server is used, it becomes a risk to the entire system. The most known current servers of identification and authentication are, Google, Facebook, Github, the French State in France. They are recognized as authentication providers and used by many sites.
This is the famous "connect with Facebook".

**3- Decentralized**, we can move towards fully distributed identification and authentication. The [DID](https://w3c-ccg.github.io/did-spec/) specification is a standard on the subject. We can identify and authenticate ourselves on any SOLID server, thanks to a solution combining webID and OIDC: [Webid-OIDC](https://en.wikipedia.org/wiki/WebID#WebID-OIDC). It is an associated identification \(Webid\) and authentication \(OIDC\) protocol. The WebId carries the url of the server on which to make the OIDC. Webid-OIDC does not claim to respect DID.

But how to match producer accounts from different platforms if the system is totally decentralized? The reality is that today producers have one account per platform.
In the spirit of SOLID, a producer does not have accounts on two platforms. His account is on a single SOLID server. eg if the producer has a SOLID server, he can create his identity on his own SOLID server, and it is valid on the other servers too. Other servers will ask his server "Do you authenticate this person?"

If you are connected to a SOLID server, you can connect to any other SOLID server. But how do we know if this server is in the community or not?
It's a matter of ergonomics, UX: The user will have a list of servers validated by DFC for example on which he chooses to identify himself / authenticate. When we arrive in the application, unlike a traditional login with password, we ask with which server we identify. Like Oauth! But with Solid any server can be server of reference, it is possible to propose to the producer to identify with any of the platforms in the network. **A new server / authentication node means a manual addition to the list of servers on which authentication is possible - this is a political decision to be made by the consortium. The maintenance of the list of servers can be automated by crawling.**

If we want to decentralize authentication via federation, it means that an actor who does not wish to federate on authentication could not join. It's easy to validate the authentication once in the federation, but it's hard to enter it. Others have to trust the trustworthiness of the new entrant, because if he mistakenly authenticates himself, he poses a risk to the entire federation. The choice to decentralize the identification and authentication would imply the choice of the use of SOLID servers or an equivalent that manages a DID protocol, and thus the "transfer" of a certain amount of information from the platforms to their SOLID server or equivalent. Knowing that today producers already have multiple accounts on multiple platforms it would mean that they are left with several unique identifiers unless they are forced re-create an account in the new system and migrate the data.

**Conclusion: the vision shared by the consortium is towards decentralized identification, authentication and authorization, via federated SOLID servers.** But there is still uncertainty about how this can be implemented. In particular, decentralized right management require to have a consistent autorisation semantics across platforms, which in itself is a significant work.
**We will use OIDC for the prototype.**
