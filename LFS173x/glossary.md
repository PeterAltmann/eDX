[Evernym's latest version link.](https://docs.google.com/document/d/1gfIz5TT0cNp2kxGMLFXr19x1uoZsruUe_0glHst2fZ8/edit?usp=sharing)

**Agent**

A software program or process used by or acting on behalf of an entity to interact with other agents or other distributed ledgers. Embedded within an agent is a Key Management Service (KMS) that performs cryptographic operations on behalf of the entity the agent represents.

**Agent Interface**

The agent interface is a component of an agent that allows the agent to establish and manage connections with other agents and exchange messages with those agents.

**Agency/ies**

A service provider that hosts cloud agents and may provision edge agents on behalf of entities. Agencies may be unaccredited, self-certified, or accredited.

**AnonCreds**

AnonCreds (or Indy AnonCreds) are the format of the verifiable credentials that are used with Hyperledger Indy ledgers. The Indy AnonCreds support Zero Knowledge Proofs by not automatically exposing a unique identifier for the holder or the verifiable credential they are holding, even if the credential may be revoked. Indy AnonCreds also support ZKP predicates, allowing a holder to prove, for example, that they are over a given age based on a claim in their credential about their date of birth without exposing their date of birth.

**Aries Agent Test Harness (AATH)**

The AATH is just a test facilitator that talks to two full agents to test their compatibility with each other. The AATH uses a backchannel to each of the agents to execute the tests.

**Aries Framework**

An Aries Framework is a code component that implements Aries functionality, including at least the ability to establish DIDComm connections, send Aries protocol messages and make available an interface for a controller to define the behavior of an agent.

**Aries Interop Profiles (AIPs)**

An Aries Interop Profile (AIP) version provides a clearly defined set of versions of RFCs for Aries agent builders to target their agent implementation when they wish it to be interoperable with other agents supporting the same Aries Interop Profile version. The Aries Interop Profile versioning process is intended to provide clarity and predictability for Aries agent builders and others in the broader Aries community.

**Aries Protocols**

Protocols that define back-and-forth sequence-specific messages sent between collaborating agents to accomplish some shared goal. Aries protocols messages are delivered in DIDComm Protocol messages.

**Aries Protocol Test Suite**

The approach taken by the Aries Protocol Test Suite (APTS) is that the test suite is itself a minimal agent that interacts with the agent-under-test (AUT) to execute test cases.

**Aries Toolbox**

The Aries Toolbox is a desktop tool (written in Electron using Vue) that allows a user to control the behavior of running Aries agents. It provides a graphical user interface tuned to Aries for developers and system administrators to control (in theory) any running Aries agent.

**Aries Working Group**

The purpose of the Aries Working Group, which involves participation from a global audience, is to discuss work updates and answer questions from the community.

**Claim**

An assertion about an attribute of a subject. Examples of a claim include date of birth, height, government ID number, or postal address—all of which are possible attributes of an individual. A credential is comprised of a set of claims.

**Cloud Agent**

An agent that is hosted in the cloud. It typically operates on a computing device over which the identity owner does not have direct physical control or access. Mutually exclusive with edge agent. A cloud agent requires a wallet and typically has a service endpoint. Cloud agents may be hosted by an agency.

**Connections**

In DIDComm, a connection is a relationship established between two (or more) agents through the exchange of peer DIDDocs. Each agent in the relationship keeps a record of the connection to track the DIDs and DIDDocs in the relationship. The connection is used when the agents in the relationship send and receive messages. A participant may unilaterally remove themselves from a relationship by deleting their record of the connection.

**Controller**

A controller provides the rules/business logic that define what actions an agent will initiate and how the agent will respond to events. There is a controller for every agent instance.

**Credential**

A digital assertion containing a set of claims made by an entity about itself or another entity. A credential is based on a credential definition. Examples of credentials include college transcripts, driver licenses, health insurance cards, and building permits.

**Credential Definition (CredDef)**

A machine-readable definition of the semantic structure of a credential based on one or more schemas. Credential definitions are stored on the ledger. Credential definitions must include an issuer public key. Credential definitions facilitate interoperability of credentials and proofs across multiple issuers, holders, and verifiers.

**Decentralized Identifier (DID)**

A special kind of identifier that is created by its owner, independent of any central authority. It is a globally unique identifier developed specifically for decentralized systems as defined by the W3C DID specification. DIDs enable interoperable decentralized self-sovereign identity management. A DID is associated with exactly one DID Document.

**Decentralized Key Management Systems (DKMS)**

An approach to cryptographic key management where there is no central authority. DKMS leverages the security, immutability, availability and resilience properties of distributed ledgers to provide highly scalable key distribution, verification and recovery.

**Device Authorization Registry**

A mechanism that has been prototyped in Indy that adds another step to the presentation of an Indy proof—proof that the agent on the device that created the proof is authorized to do so.

**DID Communication (DIDComm)**

A messaging communication mechanism secured using the keys and service endpoints stored within DIDs owned by the communicating parties.

**DIDComm Protocol**

The method by which Aries Protocol messages are delivered, irrespective of the message content.

**DID Exchange Protocol**

The method by which connections are established between agents with different roles (inviter, invitee), using a sequence of message types (invitation, request, response) to exchange data elements (IDs, DIDs, DIDdocs). See the Aries Request for Change (RFC) specification of the DID Exchange protocol here.

**DID Document**

The machine-readable document to which a DID points, as defined by the W3C DID specification. A DID document describes the public keys, service endpoints, and other metadata associated with a DID. A DID document is associated with exactly one DID.

**DID Method**

A specification that defines a particular type of DID conforming to the W3C DID specification. A DID method specifies both the format of the particular type of DID as well as the set of operations for creating, reading, updating, and deleting (revoking) it.

**DIF Universal Resolver**

The DIF Universal Resolver open source project aims to enable the resolution of all publicly accessible DIDs. Entities that specify, implement and register at W3C a DID method can contribute a resolver for their DID method to the Universal Resolver project. The DIF Resolver is a website on which runs an instance of the Universal Resolver, including all of the contributed DID method resolvers. Given a DID of any supported DID method, an instance of the Universal Resolver returns the DIDDoc and DID Metadata (if any) associated with the DID.

**Distributed Ledger Technology**

A distributed ledger (also called a shared ledger or distributed ledger technology or DLT) is a consensus of replicated, shared, and synchronized digital data geographically spread across multiple sites, countries, or institutions. There is no central administrator or centralized data storage. For the purposes of this course, we use the terms blockchain, ledger, decentralized ledger technology (DLT) and decentralized ledger (DL) interchangeably even though there are differences.

**Domain**

The group of agents that are working for a given entity.

**Edge Agent**

An agent that operates at the edge of the network on a local device, such as a smartphone, tablet, laptop, automotive computer, etc. The device owner usually has local access to the device and can exert control over its use and authorization. Mutually exclusive with cloud agent. An edge agent may be an app used directly by an identity owner, or it may be an operating system module or background process called by other apps. Edge agents typically do not have a publicly exposed service endpoint in a DID document, but do have access to a wallet.

**Endorser**

Specific to an Indy network, an endorser can write transactions for another person, or they can create a DID for another person so they can write transactions themselves. See also Transaction Endorser.

**Genesis File**

Contains information about the physical endpoints (IP addresses and ports) for some of the nodes in the ledger pool of nodes, and the cryptographic material necessary to communicate with those nodes. Each genesis file is unique to its associated ledger and must be available to an agent that wants to connect to the ledger.

**Governance Framework**

The set of business, legal, and technical definitions, policies, specifications, and contracts by which the members of a trust community agree to be governed in order to achieve their desired levels of assurance. A governance framework is also known as a trust framework.

**Holder**

A role played by an entity when it is issued a credential by an issuer. The holder may or may not be the subject of the credential. (There are many use cases in which the holder is not the subject, e.g., a birth certificate where the subject is a baby and both the mother and father may be holders. Another case is a credential registry.) If the credential supports zero-knowledge proofs, the holder is also the prover. Based on the definition provided by the W3C Verifiable Claims Working Group.

**Hyperledger**

An initiative of the Linux Foundation to develop open source distributed ledger and blockchain technology.

**Hyperledger Aries**

Hyperledger Aries is infrastructure for blockchain-rooted, peer-to-peer interactions. It includes shared cryptographic storage for blockchain clients as well as a communications protocol for allowing off-ledger interaction between those clients. This project consumes the cryptographic support provided by Hyperledger Ursa, to provide secure secret management and decentralized key management functionality.

**Hyperledger Indy**

An open source project under the Hyperledger umbrella for decentralized self-sovereign identity. The source code for Hyperledger Indy was originally contributed to the Linux Foundation by the Sovrin Foundation. Sovrin stewards run the Hyperledger Indy node software to operate their nodes.

**Hyperledger Ursa**

Hyperledger Ursa is a shared cryptographic library that enables people (and projects) to avoid duplicating other cryptographic work and hopefully increase security in the process. The library is an opt-in repository for projects (and potentially others) to place and use crypto.

**Issuer**

The entity that issues a credential to a holder.

**Issue Credential Protocol**

Is used to enable an issuer to provide a holder with a verifiable credential.

**Key Management Service (KMS)**

A software module, and optionally an associated hardware module, for securely storing and accessing private keys, link secrets, other sensitive cryptographic key material, and other private data used by an entity. A KMS is local to, and accessed by, an agent.

**Key Recovery**

The process of recovering access to and control of a set of private keys—or an entire wallet—after loss or compromise. Key recovery is a major focus of the emerging DKMS standard for cryptographic key management. See also Recovery Key.

**Ledger Interface**

The ledger interface enables reading and writing DIDs and other transactions to/from the ledger. It will likely consist of a: DID resolver, writing mechanism and verifiable credential handler.

**Link Secret**

An item of private data used by the prover to link a credential uniquely to the prover. A link secret is an input to zero-knowledge proofs (ZKPs) that enables claims from one or more credentials to be combined in order to prove that the credentials have a common holder (the prover). A link secret should be known only to the prover.

**Mediator**

A mediator is a participant in agent-to-agent message delivery that must be modeled by the sender. It has its own keys and will deliver messages only after decrypting an outer envelope to reveal a forward request. Many types of mediators may exist, but two important ones should be widely understood, as they commonly manifest in DID Docs:

1. A service that hosts many cloud agents a single endpoint to provide herd privacy (an "agency") is a mediator.
2. A cloud-based agent that routes between/among the edges of a sovereign domain is a mediator.

**NYM**

Short for "pseudonym" and an early name for what is now called a DID.

**OpenID Connect**

OpenID Connect (OIDC) is an authentication layer on top of OAuth 2.0, an authorization framework. The standard is controlled by the OpenID Foundation.

**Pairwise**

A direct relationship between exactly two entities. Business-to-business relationships are pairwise by default. A DID or a public key or a service endpoint is pairwise if it is used exclusively in a pairwise relationship.

**Plenum Byzantine Fault Tolerant Protocol**

Plenum is the heart of the distributed ledger technology inside Hyperledger Indy. As such, it provides features somewhat similar in scope to those found in Hyperledger Fabric.

**Pluggable**

A mechanism used in software that defines how a component is to be used, while allowing for different implementations (plugins) to be created and used. An example in Hyperledger Aries is the interface to secure storage for an agent is defined, but plugins for different databases (SQLite, Postgres) can be deployed in different Aries agent instances.

**Private Key**

The half of a cryptographic key pair designed to be kept as the private data of an entity. In elliptic curve cryptography, a private key is called a signing key.

**Proof**

Cryptographic verification of a claim or a credential. A digital signature is a simple form of proof. A cryptographic hash is also a form of proof. Zero-knowledge proofs enable selective disclosure of the information in a credential.

**Proof Request**

The data structure sent by a verifier to a holder that describes the proof required by the verifier.

**Prover**

A role played by an entity when it generates a zero-knowledge proof from a credential. The prover is also the holder of the credential.

**Public Key**

The half of a cryptographic key pair designed to be shared with other parties in order to decrypt or verify encrypted communications from an entity. In digital signature schemes, a public key is also called a verification key. A public key may be either public data or private data depending on the policies of the entity.

**Public DID**

A DID that is intended to be widely available—resolvable by anyone that is presented with the DID.

**Present Proof Protocol**

A protocol (usually) initiated by a verifier to request the presentation of a proof by a holder/prover.

**Private DID**

A DID shared only between parties that will use the DID, usually just one other party (pairwise). Instead of publishing private DIDs on the blockchain for anyone in the world to see, entities create DIDs and DIDDocs and then send them directly to the other party(ies) to hold. No one other than the parties involved can see or resolve the DIDs.

**Protocol State Object**

An object persisted by an Aries agent while an Aries protocol is in flight that holds all of the information about the state of the protocol, such as the type of protocol, its current state, the role of the agent, the connection being used, and so on. When a message is received about an existing instance of a protocol, the protocol state object is retrieved from storage. When processing of a message is complete and handed off (to the controller or the other agent) for further processing, the protocol state object is saved to storage.

**Recovery Key**

A special private key used for purposes of recovering the data of an agent after loss or compromise. In the DKMS key management protocol, a recovery key may be cryptographically sharded for secret sharing among multiple trustees.

**Relay**

A relay is an entity that passes along agent-to-agent messages, but that can be ignored when the sender considers encryption choices. It does not decrypt anything. Relays can be used to change the transport for a message (e.g., accept an HTTP POST, then turn around and emit an email; accept a Bluetooth transmission, then turn around and emit something in a message queue). Mix networks like TOR are an important type of relay.

**Resolver**

A software module that accepts an identifier as input, looks up the identifier in a database or ledger, and returns metadata describing the identified entity. The Domain Name System (DNS) uses a DNS resolver. Self-sovereign identity uses a DID resolver.

**Revocation**

The act of an Issuer revoking the validity of a Claim or a Credential. With the Sovrin Protocol and the Sovrin Ledger, Revocation is accomplished using a Revocation Registry.

**Schema**

A machine-readable definition of the semantics of a data structure. Schemas are used to define the attributes used in one or more credential definitions.

**Selective Disclosure**

A privacy-by-design principle of revealing only the subset of the data described in a claim, credential, or other set of private data that is required by a verifier. There are many techniques for achieving selective disclosure. One of the primary techniques used in Hyperledger Indy is zero-knowledge proof cryptography.

**Self-Sovereign Identity**

Lifetime portable identity for any person, organization, or thing that does not depend on any centralized authority and can never be taken away.

**Service Endpoint**

An addressable network location offering a service operated on behalf of an entity. As defined in the DID specification, a service endpoint is expressed as a URI (Uniform Resource Identifier).

**Sovrin Foundation**

The non-profit public trust organization chartered to administer Sovrin Infrastructure on behalf of the Sovrin Community. The Sovrin Foundation is the Governance Authority for the Sovrin Governance Framework and the Sovrin Web of Trust Framework.

**SSI**

Acronym for self-sovereign identity.

**Steward**

An organization approved by the Sovrin Foundation to operate a node. A steward must meet the qualifications defined in the Steward Business Policies and the technical requirements defined in the Steward Technical Policies.

**Transaction**

A record of any type written to the Sovrin Ledger.

**Transaction Author Agreement**

A controlled document that functions as a legal agreement between the Sovrin Foundation and any transaction author, which must be digitally signed or otherwise explicitly agreed to by the transaction author in order to write a transaction.

**Transaction Endorser**

An organization authorized under permissioned write access to authorize a transaction by digitally signing it so it will be accepted by a validator node. The Transaction Endorser role is only needed for permissioned write access. It is not needed for public write access. See also Endorser.

**Trustee**

An identity owner entrusted with specific identity control responsibilities by another identity owner or with specific governance responsibilities by a governance framework.

**Trust over IP (ToIP)**

ToIP is a set of protocols being developed to enable a layer of trust on the Internet, protocols embodied in Indy, Aries and Ursa. It includes self-sovereign identity in that it covers identity, but goes beyond that to cover any type of authentic data.

**Universal DID Resolver**

An online service that supports the resolution of all DID methods and that can be used to resolve a DID and return a result when it cannot be resolved locally.

**Verifiable Credential**

A credential that includes a proof from the issuer. Typically this proof is in the form of a digital signature.

**Verifiable Data Registry**

A system that facilitates the creation, verification, updating, and/or deactivation of decentralized identifiers and DID documents. A verifiable data registry might also be used for other cryptographically-verifiable data structures such as verifiable credentials. For more information, see the W3C Verifiable Credentials specification [VC-DATA-MODEL].

**Verifier**

An entity who requests a credential or proof from a holder and verifies it in order to make a trust decision about an entity.

**Zero-Knowledge Proof**

A proof that uses special cryptography and a link secret to support selective disclosure of information about a set of claims from a set of credentials. A zero-knowledge proof provides cryptographic proof about some or all of the data in a set of credentials without revealing the actual data or any additional information, including the identity of the prover.
