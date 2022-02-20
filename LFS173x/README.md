*Learn how to develop blockchain-based, production-ready, decentralized identity applications using verifiable credentials with Hyperledger Aries in this free course.*

# Chapter 0. Welcome!

Data is driving our world today. However, we hear about data breaches and identity thefts all the time. Trust on the Internet is broken, and it needs to be fixed. As such, it is imperative that we adopt a new approach to identity management, and ensure data security and user privacy through tamper-proof transactions and infrastructures.

Blockchain-based decentralized identity management is revolutionizing this space. The three Hyperledger open source projects, Aries, Indy and Ursa, provide the foundation for distributed applications built on authentic data, applications that implement the concept of Trust over IP. Together, the three projects provide tools, libraries, and reusable components for creating and using independent digital identities rooted on blockchains or other distributed ledgers that are interoperable across jurisdictions, applications and other data silos. While this course will cover what you need to know about Indy and Ursa, the main focus is Aries and how you can use it to quickly build your own applications on a solid digital foundation of trust. This focus will be explained further in the course but for now, rest assured: if you want to start developing decentralized identity applications rooted on the blockchain, Aries is where you need to be.

## Learning objectives

This course will get you from (pretty much) zero to developing code for issuing, holding and verifying credentials with your own production-ready Aries agents. On the way, you'll look at how Aries agents use Hyperledger Indy ledgers (you’ll even run your own ledger instances), dig into the architecture and components of an Aries agent, and learn about its underlying messaging protocols. Most importantly, you’ll get started building applications that address your Trust over IP use cases, whether they involve COVID-19 proof-of-vaccination credentials, digital driver's licenses, proof of employment, climate change, or anything else. The possibilities are endless!

## Terminology

We use the terms blockchain, ledger, decentralized ledger technology (DLT) and decentralized ledger (DL) interchangeably in this course. While there are precise meanings of those terms in other contexts, in the context of the material covered in this course the differences are not meaningful.

For more definitions, take a look at our course [Glossary](https://github.com/PeterAltmann/eDX/blob/main/LFS173x/glossary.md) section.

## Labs and Demos

Throughout this course there will be labs and demos that are a key part of the content and that you are strongly advised to complete. The labs and demos are hosted on GitHub so that we can maintain them easily as the underlying technology evolves. Many will be short interactions between agents. In all cases, you will have access to the underlying code to dig into, run and alter.

For some labs, you won’t need anything on your laptop except your browser (and perhaps your mobile phone). For others, you have the option of running the labs in your browser or locally on your own system. For the "in browser" option, we use a service called "Play with Docker" that allows you to access a terminal command line environment in your browser so you don’t have to install everything locally. The downside of using Play with Docker is that you don’t have all the code locally to review and update in your local code editor. If you want that, stick with running the labs locally.

When you run the labs locally, you need the following prerequisites installed on your system:

* A terminal command line interface running bash shell. This is built-in for Mac and Linux, and on Windows, the `git-bash` shell comes with git (see installation instructions below).
* Docker, including Docker Compose (Community Edition is fine). If you do not already have Docker installed, open [Docker Documentation](https://docs.docker.com/get-docker/) and then click the link for the installation instructions for your platform. Instructions for installing docker-compose for a variety of platforms can be found [here](https://docs.docker.com/compose/install/).
* Git. This [link](https://www.linode.com/docs/guides/how-to-install-git-on-linux-mac-and-windows/) provides installation instructions for Mac, Linux (including if you are running Linux using VirtualBox) and native Windows (without VirtualBox).

All of the labs that you can run locally use Docker. You can run the labs directly on your own system without Docker, but we don’t provide instructions for doing that, and we highly recommend you not try that until you have run through them with Docker. Because of the differences across systems, it’s difficult to provide universal instructions; we’ve seen many developers spend too much time trying to get everything installed and working right.

**NOTE**: *The teams we work with only use Docker/containers for development and production deployment. In other cases, developers unfamiliar with (or not interested in) Docker set up their own native development environment. However, doing so is outside the scope of the labs in this course.*

# Chapter 1. Overview.

## Introduction
Data breaches. Identity theft. Companies exploiting our personal information. We read about these Internet issues all the time. Simply put, trust on the Internet is broken and it needs to be fixed.

This is where the Hyperledger Indy, Aries and Ursa projects come in and, we assume, the main reason you are taking this course. The Indy, Aries and Ursa tools, libraries and reusable components provide a foundation for distributed applications built on authentic data using a distributed ledger, purpose-built for decentralized trust.

**NOTE**: *This course is called "Becoming a Hyperledger Aries Developer" because it focuses on building applications on top of Hyperledger Aries components—the area where you, a (soon to be!) verifiable credentials application developer, can have the most impact. Aries builds on Indy and Ursa. While you need to have a good understanding of Indy (and other ledger/verifiable credential technologies) and a basic idea of what Ursa (the underlying cryptography) is and does, Aries is where you need to dig in.*

By the end of this chapter you should:

* Understand why this course focuses on Aries (and not Indy or Ursa).
* Understand the issues that Aries is trying to address.
* Know the core concepts behind self-sovereign identity.

## Why Focus on Aries Development?

Hyperledger Indy, Aries and Ursa make it possible to:

* Establish a secure, private channel with another person, organization, or IoT thing—like authentication plus a virtual private network—but with no central server and no login.
* Send and receive arbitrary messages with high security and privacy.
* Prove things about yourself; receive and validate proofs about other parties.
* Create an agent that proxies/represents you in the cloud or on edge devices.
* Manage your own identity:
 - Authorize or revoke devices.
 - Create, update and revoke keys.

However, you will have relatively little interaction with Indy—and almost none with Ursa—as the vast majority of those working with the Hyperledger identity solutions will build on top of Aries; only those contributing code directly into Indy, Aries and Ursa (e.g., fixing a flaw in a crypto algorithm implementation), will have significant interaction with Indy and Ursa. And here’s another big takeaway: while all three projects are focused on decentralized identity, Indy is a specific blockchain, whereas Aries is blockchain-agnostic. With Aries, you don’t have to be limited to Indy.

<img src="https://courses.edx.org/assets/courseware/v1/08b138f04ba61db6ce5a2c97aa8640db/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-21.png" alt="drawing" width="400"/>

## Why We Need Identity Solutions

In today's world, we are issued **credentials** as paper documents (for example, our driver's license). When we need to prove things from those paper documents, such as who we are, we hand over (or present) the document. The **verifier** looks at the document and attempts to ascertain whether it is valid and whether it satisfies the purpose for which it was requested. The **holder** of the document cannot choose to only hand over a certain piece of the document but must hand over the entire thing.

A typical paper credential, such as a driver’s license, is issued by a government authority (an **issuer**) after you prove to them who you are (usually in person using your passport and/or birth certificate) and that you are qualified to drive. You then hold this credential (usually in your wallet) and can use it elsewhere whenever you want—for example, when renting a car, in a bank to open up an account or in a bar to prove that you are old enough to drink. When you do that you've proven (or presented) the credential. That’s the paper credential model.

The paper credential model (ideally) proves:

* Who issued the credential.
* Who holds the credential.
* The claims have not been altered.

The caveat "ideally" is included because of the possibility of forgery in the use of paper credentials. As many university students know, it’s pretty easy to get a fake driver’s license that can be used when needed to "prove" those same things.

## The Verifiable Credential (VC) Model

Enter the VC model, the bread and butter behind authentic data and decentralized identity. The VC model brings about the possibility of building applications with a solid digital foundation of trust.

The verifiable credentials model is the digital version of the paper credentials model. That is:

* An authority decides you are eligible to receive a credential and issues you one.
* You hold your credential in your (digital) wallet.
* At some point, you are asked to prove the claims from the credential.
* You provide a verifiable presentation to the verifier, proving the same things as with a paper credential.
* In some cases, you can prove one more thing—that the issued credential has not been revoked.

As we’ll see, verifiable credentials and presentations are not simple documents that anyone can create and use. They are cryptographically constructed so that a presentation proves the four key attributes of all credentials:

1. Who issued the credential.
2. The credential was issued to the entity presenting it.
3. The claims were not tampered with.
4. The credential has not been revoked.

Unlike a paper credential, those four attributes are evaluated not based on the judgment and expertise of the person looking at the credential, but rather online using cryptographic algorithms that are extremely difficult to forge. When a verifier receives a presentation from a holder, they use information from a decentralized source, often a distributed ledger (shown as the **verifiable data registry** in the image below) to perform the cryptographic calculations necessary to prove the four attributes. Forgeries become much (MUCH!) harder with verifiable credentials!

<img src="https://courses.edx.org/assets/courseware/v1/e6878fa7000fb538a7e8b9dec066d0b1/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS172x_CourseGraphics_V1-04.png" alt="VC data model" width="500"/>
<br>
<br>
The prerequisite course, "Introduction to Hyperledger Sovereign Identity Blockchain Solutions: Indy, Aries and Ursa" (LFS172x), more than covers the reasons why we need a better identity model on the Internet so we won’t go into it too much here. Suffice to say, the availability of distributed ledgers (such as those based on Hyperledger Indy) has enabled a better way to build solutions using authentic data, and in doing so, enables the creation of a more trusted Internet.

## Key Concepts

Let’s review other key concepts that you’ll need for this course, such as:

* self-sovereign identity
* trust over IP
* decentralized identifiers
* zero-knowledge proofs
* selective disclosure
* revocation
* verifiable credential formats
* secure storage
* agent

If you are not familiar or comfortable with these concepts and terminology from the following short summaries, we suggest you review them in greater depth in this course: "Introduction to Hyperledger Sovereign Identity Blockchain Solutions: Indy, Aries and Ursa" (LFS172x).

### SSI

Self-sovereign identity is one of the most important concepts discussed in the prerequisite course and it is what you should keep in mind at all times as you dig into the Aries world of development. SSI is the idea that you control your own data and you control when and how it is provided to others; when it is shared, it is done so in a trusted way. With SSI, there is no central authority holding your data that passes it on to others upon request. You hold your own data. And because of the underlying cryptography and distributed ledger technology, SSI means that you can present claims about your identity and others can verify it with cryptographic certainty.

### ToIP

Along with SSI, another term we use in this course is Trust over IP (ToIP). ToIP is a Linux Foundation organization that (per the ToIP website) is defining a complete architecture for Internet-scale digital trust that combines both cryptographic trust at the machine layer and human trust at the business, legal and social layers. The technical capabilities embodied in Indy, Aries and Ursa enable the "trust at the machine layer." ToIP includes self-sovereign identity in that it covers identity, but also goes beyond identity (attributes about you) to cover any type of authentic data.

Authentic data, in this context, is *a set of claims (assertions) stated by the issuer to the holder (in the form of a verifiable credential), that the holder can later prove were said by the issuer to the holder, were not tampered with and have not been revoked*. ToIP’s dual focus on both the technical and governance aspects of authentic data is encapsulated in the "Trust over IP Stacks," as represented in this image from the [Trust over IP Foundation](https://trustoverip.org/):

<img src="https://miro.medium.com/max/1400/1*DgPBpnT_RxDEnd_ptdynkQ.png" alt="drawing" width="800"/>

Image [source](https://www.dizme.io/).

As shown, there are two stacks in ToIP: a governance stack and a technology stack. The technology stack is a typical layered stack, driven by technical components at each level. In parallel, the governance stack defines the information to govern each layer of the stack. For example, at Layer 3, where verifiable credentials are exchanged, governance defines how a verifier might come to trust the issuer of a credential—not the cryptography (that can be verified using technology), but what the information in the credential means. That is, under whose authority and using what processes did the issuer come to issue the credential being proven?

The core of Aries implements Layer Two (peer-to-peer protocol) of the ToIP stack, enabling both Layer One (DIDs) and Layer Three (Data Exchange, including verifiable credentials) capabilities. Aries implements DIDComm, a secure, transport-agnostic mechanism for sending messages based on DIDs (decentralized identifiers). Hyperledger Indy provides both a Layer 1 DID utility mechanism and Layer 3 verifiable credential format called **AnonCreds** (aka, Anonymous Credentials). Although Aries works really well with Indy, it can also be used with other Layer One implementations, as well as other verifiable credential formats, including W3C Standard Verifiable Credentials.

### DIDs

A decentralized identifier is like a universally unique identifier (uuid), but with some added power—it is backed by a cryptographic key owned by the controller of the key. When needed, an Aries agent generates DIDs to either publish on a distributed ledger (such as an instance of Indy) or for exclusive use by another agent (a "peer DID"). A DID looks like this:

`did:sov:AKRMugEbG3ez24K2xnqqrm`

An Indy DID is created by generating an Ed25519 cryptographic public/private key pair. We’re assuming in this course that you know about key pairs and elliptical curve cryptography, but if you need a quick refresher, here is the [Wikipedia definition](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography). The identifier for the DID (such as above) is derived from the first instance of the public verification key (the verkey). An Aries agent will publish an issuer’s public DID on a ledger, including the DID, the verkey and a physical endpoint (such as a URL) to which messages can be sent to the agent. Aries holder/provers and verifiers generally do not publish a DID on a ledger because they don’t need to play those roles. On most public Indy ledgers, such as Sovrin, a DID for a person may **not** be published, blocked by GDPR-related legal agreements that a ledger writer must accept before using the ledger. Such agreements prevent a person from issuing verifiable credentials rooted in a public Indy network.

**NOTE**: *While early (2020) Aries agents could only read and write published DIDs on an Indy ledger, newer implementations can use other ledgers.*

Peer DIDs are created and exchanged peer-to-peer between pairs of messaging agents. Such DIDs are shared directly with the paired agent and so do not need to be published on a distributed ledger. Such DIDs are used only for messaging; they are not used in the signing and issuing of verifiable credentials or in presenting proofs of claims from verifiable credentials.

The DID specification can be found in the W3C document titled ["Decentralized Identifiers (DIDs) v1.0."](https://w3c.github.io/did-core/)

### ZKP

A zero-knowledge proof ([ZKP](https://en.wikipedia.org/wiki/Zero-knowledge_proof)) is a cryptographic method of proving to someone that you know the value of an attribute without exposing the value of the attribute itself. In verifiable presentations, a ZKP is used to prove that the verifiable credentials were issued to the holder without exposing a unique, correlable identifier for the holder to the verifier. A presentation using a ZKP still exposes the claims asked for by the verifier (which may uniquely identify the prover), but a unique identifier for the holder is not automatically provided as part of the process of presenting a proof.

A second (super cool) use of a ZKP in verifiable presentations is a "predicate ZKP." A predicate ZKP is a true/false expression proven (with cryptography) to a verifier based on a claim from an issued credential without exposing the underlying claim—for example, proving based on a "date of birth" claim that a person is older than a given age (e.g., older than 18) without providing the date of birth itself. The cryptography is pretty neat, although left up to you to dig into. In this course, we’ll just cover enough to show how it is used.

**NOTE**: *Although powerful and certainly privacy preserving, the application of predicates in Indy AnonCreds is limited to situations where the underlying data is a number. As such, to prove "an older than" predicate, the "date of birth" in the credential MUST be a number (such as the integer 20210413 for April 13, 2021), not a string (such as “2021-04-13”), and the expression from the verifier must also use a date represented in the same way.*

### Selective disclosure

A key capability of some verifiable credential formats is support for selective disclosure, meaning it is possible to prove a subset of the claims issued in a verifiable credential. With selective disclosure support, an issuer can put all of the claims that might be needed for a range of use cases, and the holder/prover and the verifier can limit the information shared for a specific presentation. In the example pictured below, a holder with a “driver’s license” type verifiable credential can share just their picture and that they are old enough to drink (using a ZKP predicate) to a bartender at a pub. The rest (name, address, driver’s license number, etc.) can be held back—it’s not needed by the pub. Selective disclosure is an important privacy capability with verifiable credentials!

### Revocation

Verifiable credential revocation is the capability for an issuer to publish that an issued verifiable credential is no longer active. This action is done unilaterally by the issuer, although they might inform the holder that the credential is revoked. While many think that revocation is the result of something "bad" done by the holder (e.g., the "revoking a driver’s license" scenario), there are many other business reasons for revoking a credential, the most common being that information in the credential is changed (e.g., change of address), and a new verifiable credential is available with updated information. Once revoked, the verifiable presentation process should make it easy for a verifier to know if a prover uses a credential that has been revoked, and hence (in most use cases), should not be accepted.

For verifiable credential formats that support ZKP capabilities, revocation is a bit more complex than one might assume. In a simple revocation approach, the issuer might include a unique ID for the credential, and to revoke it, add the ID to a published list of revoked credential IDs. The holder/prover and verifier can use the credential ID to check the registry and see if the credential has been revoked. The problem with that approach for a ZKP-enabled verifiable credential is that the credential ID is given to the verifier—a unique, correlatable identifier. That’s exactly what we want to avoid by using a ZKP verifiable credential!

A ZKP revocation generally uses a credential ID that is shared by the issuer to the holder, and a published revocation registry. However, the prover does not give the credential ID to the verifier. Instead, the prover generates a ZKP "proof of non-revocation" that the verifier can check using the data in the published revocation registry.

### Verifiable Credential Formats

Aries agents support one or more of a limited set of verifiable credential formats. At the time this course is being updated (mid-2021), verifiable credential formats are an active area of focus in the Aries community.

Any Aries agent that is built on Hyperledger Indy supports the Indy AnonCreds format. AnonCreds includes a number of verifiable credential capabilities that are outlined in this section, including support for ZKPs, ZKP predicates, selective disclosure and ZKP revocation based on CL Signature cryptography. While simple to use in Aries and functionally complete, the Indy AnonCreds data format predates the publishing of the W3C Verifiable Credential Standard, and as such, does **not** meet that standard.

To align with other verifiable credential communities that don’t use Indy AnonCreds, some Aries projects have implemented support for the W3C Standard Verifiable Credentials data format, which is commonly based on JSON-LD. Those implementations use a couple of types of cryptographic signatures:

* LD Signatures (JSON-LD signed with an Ed25519 key), or
* BBS+ Signatures.

With LD Signatures, a verifiable presentation includes the entire verifiable credential, lacking selective disclosure and any form of ZKP. Using BBS+ Signatures, a verifiable presentation supports selective disclosure. BBS+ Signatures can be implemented to support basic ZKPs—e.g., not exposing a unique identifier for a prover as part of a verifiable presentation. However, the BBS+ ZKP capability is not part of the mid-2021 Aries implementations (although it’s being worked on). Further, there is not yet a ZKP revocation capability for BBS+ Signatures, and ZKP predicates, while theoretically possible with BBS+ Signatures, are unlikely to be supported.

When looking at different Aries agents, verifiable credential format support is a differentiating factor. Want all the privacy-preserving features available today? Indy AnonCreds is the way to go. Want W3C Standard Verifiable Credentials? JSON-LD credentials and signatures are your best bet, ideally using BBS+ so you get selective disclosure. It’s an either/or right now (mid-2021), but we expect it will become less of an issue over time, as support for one, the other or both become the norm across all Aries agents.

Additional reading here:
* https://www.lfph.io/wp-content/uploads/2021/04/Verifiable-Credentials-Flavors-Explained-Infographic.pdf
* https://www.lfph.io/wp-content/uploads/2021/02/Verifiable-Credentials-Flavors-Explained.pdf

### Secure storage

Every Aries agent includes some kind of secure storage to hold its secrets. Most important of those secrets are the private keys that the agent creates and uses to sign data and decrypt messages from other agents. The creation and use of private keys is generally handled within a **key management service** (KMS) that is within or associated with the agent’s secure storage. The KMS makes sure that the keys are used appropriately, and are not ever available to the Aries application code. Ideally, the KMS is a hardware component or a software secure enclave. For an Aries developer (like you!) that is good news as it means that you don’t have to write any software that accesses those important secrets.

Along with the Aries agent created keys, the secure storage holds other data that you don’t want others to access but need as an operational agent, such as connections with other agents (peer DIDs and connection metadata), verifiable credentials issued to you, cached ledger objects, and the state of protocols that are currently "in flight" (e.g., while you are being issued a credential). All of the data in Aries secure storage is encrypted, with the decryption keys carefully managed. While this is absolutely necessary for the security of Aries, at some point when you are debugging your Aries application, you will likely become annoyed that all secure storage is encrypted, inaccessible even in a developer sandbox.

Most Aries implementations provide a way for "other data" to be stored in the Aries agent secure storage, but we usually discourage such use of Aries’ persistence. Our recommendation is that any business data not needed for the operation of the agent itself be stored in a separate "business" database. We’ll talk more about this when we talk about Aries controllers, starting in Chapter 4.

### Agent

Hyperledger Aries uses the term agent to mean the software that interacts with other entities (via DIDs and more). For example, a person might have a mobile agent wallet application on their smart device, while an organization might have an enterprise agent running on a server, perhaps in the cloud. All agents (with rare exceptions) have secure storage for securing identity-related data including DIDs, keys and verifiable credentials. As well, all agents are controlled to some degree by their owner, sometimes with direct control (and comparable high trust) and sometimes with minimal control, and far less trust. We’re going to be talking a lot about agents in this course. As an Aries developer, you’ll spend most of your time building agents for specific use cases.

## Knowledge Check (4 Questions)

1. Aries is blockchain-agnostic. **True** or False?
2. Using zero-knowledge proofs (ZKPs) guarantees your private data is never shared. True or **False**?
3. What layer(s) of the Trust over IP technical stack does Aries implement? Select all answers that apply. **{2,3}**
4. What is a capability enabled by the Indy AnonCreds ZKP model? **Selective Disclosure**

## Summary

This chapter has largely been a review of the concepts introduced in the previous course. Its purpose is to provide context for why you want to become an Aries developer and recaps some of the terminology and concepts behind decentralized identity solutions that were discussed in the prerequisite course: "Introduction to Hyperledger Sovereign Identity Blockchain Solutions: Indy, Aries and Ursa" (LFS172x).

# Chapter 2. Exploring Aries and Ares Agents

## Introduction

As you learned in the prerequisite course ("Introduction to Hyperledger Sovereign Identity Blockchain Solutions: Indy, Aries and Ursa" (LFS172x))—which you took, right?—Hyperledger Aries provides a shared, reusable, interoperable tool kit designed for initiatives and solutions focused on creating, transmitting and storing verifiable digital credentials. It provides the infrastructure for distributed ledger-rooted, peer-to-peer interactions as the basis for applications using verifiable credentials. Aries agents are software components that act on behalf of entities—people, organizations and things. Aries agents enable decentralized, self-sovereign identity based on a secure, peer-to-peer communications channel.

In this chapter, we’ll try to convert all the buzzwords in the previous paragraph into practical knowledge about Aries that you can use to develop your own applications. We’ll look at the architecture of an Aries agent through some hands-on labs. Specifically, we look at what parts of an agent come from Aries, and what parts you, an Aries developer-to-be, are going to have to build. We will also look at the interfaces that exist to allow Aries agents to talk to one another and to distributed ledgers such as instances of Hyperledger Indy.

By the end of this chapter you should:

* Be familiar with the Aries ecosystem consisting of wallet agents for people and organizations, server-based agents for enterprises and agents for IoT devices.
* Know the concepts behind issuing, holding/proving and verifying agents.
* Understand the internal components of an Aries agent.
* Know what Aries interoperability means and how it is achieved.

## Aries Agents

Let’s first look at a couple of examples to remind us what Aries agents can do. We’ll also use this as a chance to introduce some characters that the Aries community has adopted for many Aries proof-of-concept implementations. You first met these characters in the LFS172x course.

* Alice is a person who has a mobile wallet that uses Aries protocols running on her smartphone. She uses it to receive credentials from various entities, and uses those credentials to prove things about herself online.
* Alice’s smartphone wallet application connects with a specialized Aries agent that does nothing except routes messages to her. It too is Alice’s agent (called a mediator agent, as we’ll see), but it’s one that is (most likely) run by a vendor. We’ll learn more about these cloud services when we get to the Aries mobile agents chapter.
* Alice is a graduate of Faber College (of Animal House fame), where the school slogan is "Knowledge is Good." (We think the slogan should really be "Zero Knowledge is Good.") Faber College has an Aries agent that issues verifiable credentials to the college’s students. It issues student ID cards as verifiable credentials to students, and digital diplomas to graduates.
* Faber has agents that verify presentations of claims from students when their student ID is needed around campus—joining classes, writing exams, getting food at campus eateries and so on.
* Businesses in and around Faber College have agents that verify presentations of claims from students and staff at the college to allow them to easily offer discounts to students. For example, Alice proves the claims from her "Faber College Student ID" credential to get a discount every Tuesday night when she goes to (and wins) Trivia Night at a nearby pub.
* Faber also has an Aries agent that receives, holds and proves claims from credentials about the college itself. For example, Faber’s agent might hold a credential that it is authorized to grant degrees to its graduates from the jurisdiction (perhaps the US state) in which it is registered.
* ACME is a company for whom Alice wants to work. As part of their application process, ACME’s Aries agent requests proof of Alice’s educational qualifications. Alice’s Aries agent can provide proof using the credential issued by Faber to Alice.
* Since Alice is the first Faber College student to ever apply to ACME, ACME doesn’t know if they can trust Faber. An ACME agent might connect to Faber’s agent (using the DID in the verifiable credential that Alice presented) to get a proof from Faber that it is a credentialed academic institution.

### Lab: Issuing, Holding, Proving, and Verifying

In this first lab, we’ll walk through the interactions of three Aries agents:

* A mobile agent to hold a credential and prove claims from that credential.
* An issuing agent.
* A verifying agent.

The instructions for running the lab can be found on [GitHub](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/IssuingHoldingProving.md).

Additional sources:
* [Verifiable Credential Authentication via OpenID Connect](https://github.com/bcgov/vc-authn-oidc/blob/main/docs/README.md)
* [IAM Authentication using VC](https://docs.google.com/presentation/d/150n2PikoshbQB46QDMO3xpWFVLdcsoJNey0MVtCbGvk/edit#slide=id.p)


## An Aries Ecosystem

All of the examples of Aries agents in the previous section can be built and operated independently by different organizations because of the common protocols upon which Aries agents communicate, as pictured below.

* Some of the Aries agents are run by large enterprises (such as Faber College) that have a publicly accessible endpoint to which other agents can initiate connections.
* Other agents are owned by small businesses (such as the pub where Alice goes for Trivia Night) that might not be sufficiently IT-savvy to run their own agents. They might use an Agency-as-a-Service offered by a vendor that allows them to easily configure their agent to, for example, verify certain types of credentials.
* Mobile wallet applications, such as the one you used in the first lab, run on smartphones.
* Since mobile applications cannot have their own physical endpoint, it is not possible for an enterprise agent (such as Faber’s agent) to send a message directly to a mobile wallet. Rather, each mobile wallet must have routing agents that give other agents a physical endpoint to which they can send messages destined for the wallet. We’ll cover this in detail in the course chapter on mobile wallets and routing.

An Aries ecosystem consists of both the Aries agents that message one another to exchange verifiable credentials and an understanding of the verifiable credential types that are being issued, held, proven and verified. The off-campus store that is providing discounts to students because they have a student ID verifiable credential must know that Faber College is issuing the credential and the meaning of the various data elements ("claims") in the credential. To put that into Trust over IP (ToIP) terms, all the participants have to know both the technical and governance elements of the ecosystem—the ToIP Dual Stack.

The agents in an Aries ecosystem share many attributes:

* They all have secure storage for keys.
* They all have some secure storage for other data related to their role as an agent.
* Each interacts with other agents using secure, peer-to-peer messaging protocols.
* Most connect with ledger(s) to write (issuers) and read (holders, provers and verifiers) decentralized identifiers (DIDs) and verifiable credential metadata.
* Most process (issue, hold, prove and verify) verifiable credentials (although routing agents may not).
* They all have an associated mechanism to provide "business rules" to control the behavior of the agent:
 - Often a person (via a user interface) for phone, tablet, laptop, etc.-based wallet agents.
 - Often a backend enterprise system for enterprise agents. The backend system may have request handling workflows that include people.
 - For routing agents the "rules" are usually limited to the forwarding of messages to other agents. Such routing agents are usually fully automated.


 While there can be many agent variations, the most common ones are:

* Agents for people.
* Agents for organizations.
* Agents for routing messages to and from agents for people and organizations.

A significant emerging use case that we’ve not covered in this section are agents embedded within or associated with IoT devices. In the common IoT case, IoT device agents are just variants of other agents, connected to the rest of the ecosystem through a server-based agent. All the same principles apply. For example, IoT devices might include a sensor to measure something (such as greenhouse gas emissions at a factory) and emitting the data by issuing verifiable credentials, ensuring that the data cannot be tampered with after generation. This provides a foundation of trust about the captured data—if the device itself can be trusted. Such trust might be accomplished by third-party auditors or government regulators certifying the operation of the device, and hence the validity of the issued verifiable credentials.

## Ledgers and Verifiable Credential Formats in an Aries Ecosystem

The vast majority of Aries agents connect with a distributed ledger (and sometimes several) to read and write the data necessary to share verifiable credentials and presentations. In the diagram below, the issuer, holder and verifier use the verifiable data registry (as defined in the W3C Verifiable Credential standard), which is often implemented as a distributed ledger as the basis for issuing and presenting verifiable data. Early Aries agents used both Hyperledger Indy ledgers (such as the Sovrin MainNet) and the Indy verifiable credential format, called Anonymous Credentials (AnonCreds). As of the updating of this course (mid 2021), a significant shift has begun to building Aries agents that can use other types of verifiable credentials, specifically those based on the W3C Verifiable Credential standard, as well as use data from other ledgers. Interestingly, the architecture and operation of an Aries agent is the same regardless of the verifiable credential format or the ledger used. As such, in this course, we will mostly use the more mature Aries+Indy combination in the labs as we establish the foundational concepts. However, we’ll also highlight the differences in using other verifiable credential formats and point out where other ledgers can be used. There is also a lab or two where you can get hands-on with some non-Indy-based use cases.

<img
  src="https://miro.medium.com/max/1838/1*39dEx0Ta2LvmQuPUclqOMA.png"
  alt="VC model"
  width="600"
/>

## The Logical Components of an Aries Agent

All Aries agent deployments have two logical components: a **framework** and a **controller**.

The framework contains the standard capabilities that enable an Aries agent to interact with its surroundings—ledgers, storage, verifiable credentials, presentations and other agents. As an Aries application developer, a framework is an artifact of Aries that you don’t have to create or maintain, you just embed it in your solution. An Aries framework knows how to initiate connections, respond to requests, send messages, manage secure storage and more. However, a framework needs to be told *when* to initiate a connection or to send a request. It doesn’t know *what* response should be sent to a given request. A deployed framework just sits there until it’s told what to do.

The controller is the component that, well, controls the behaviour of an instance of an Aries framework—the business rules for that particular agent instance. The controller is the part of a deployment that you, an Aries developer, build to create an Aries agent that handles your use case. For example:

* In a mobile application, the controller is the user interface and how the person interacts with the user interface. As events come in, the user interface shows the person their options, and after input from the user, tells the framework how to respond to the event.
* An issuer, such as Faber College’s agent, has a controller that integrates agent capabilities (requesting proofs, verifying them, issuing credentials and so on) with enterprise systems, such as a "Student Information System" that tracks students and their grades. When Faber’s agent is interacting with Alice’s, and Alice’s requests an "I am a Faber Graduate" credential, it’s the controller that figures out if Alice has earned such a credential, and if so, what claims should be put into the credential. The controller also directs the agent to issue the credential.

## Aries Agent Architecture (ACA-Py)

The diagram below is an example of an Aries agent architecture, as exemplified by Aries Cloud Agent-Python (ACA-Py):

<img src = "https://courses.edx.org/assets/courseware/v1/625bf4cc7d88cbb4a65143c4fa426933/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-01.png" alt = "Aries Agent Architecture ACA-Py" width = "500"
/>

The framework provides all of the core Aries functionality such as interacting with other agents and the ledger, managing secure storage, sending event notifications to, and receiving instructions from the controller. The controller executes the business logic that defines how that particular agent instance behaves—how it responds to the events it receives, and when to initiate events. The controller might be a web or native user interface for a person or it might be coded business rules driven by an enterprise system.

Between the two is a pair of interfaces:

* When the framework receives a message (an event) from the outside world, it sends a webhook (a notification) about the event to the controller so the controller can decide what to do.
* In turn, the controller sends a request to the framework to tell the framework how to respond to the event.
 - The same controller-to-framework request interface is used when the controller wants the framework to initiate an action, such as sending a message to another agent.


What that means for an Aries developer is that the framework you use is a complete dependency that you include in your application. You don’t have to build it yourself. It is the controller that gives your agent its unique personality. Thus, the vast majority of Aries developers focus only on building controllers, while paying little attention to the internals of the Aries framework they are using.

Further easing the learning curve for controller development is that its event driven processing is almost identical to web development. The controller sits waiting for an event. When received, the event type is determined, it is dispatched for processing, and back into the event loop we go.

Of course, since Aries frameworks are both evolving and open source, if your agent needs a feature that is not in the framework you are using, you are welcome to do some Aries framework development and contribute the feature to Hyperledger. We’d really like it if you did!

## Agent Terminology Confusion

In many places in the Aries community, the "agent framework" term we are using here is shortened to "agent." That creates some confusion as you can say "an Aries agent consists of an agent and a controller." Ugh… Throughout the course we have tried to make it clear when we are talking about the whole agent versus just the framework. Often we will use the name of a specific framework (e.g., Aries Cloud Agent Python or ACA-Py) to make it clear the context of the term. However, as a developer, you should be aware that in the community, the term "agent" is sometimes used just for the agent framework component and sometimes for the combined framework+controller. Naming is hard...

## Aries Agent Internals and Protocols

In this section, we’ll cover, at a high level, the internals of Aries agents and how Aries agent messaging protocols are handled.

The most basic function of an Aries agent is to enable (on behalf of its controlling entity) secure, encrypted messaging with other agents. Here’s an overview of how that happens:

* Faber and Alice have running agents.
* Somehow (we’ll get to that) Faber’s agent discovers Alice’s agent and sends it an invitation (yes, we’ll get to that too!) to connect.
 - The invitation is in plaintext (often presented as a QR code) and includes information on how a message can be encrypted and securely sent by Alice’s agent to Faber’s.
* Alice’s agent (after conferring with Alice—"Do you want to do this?") creates a private DID for the relationship and embeds it in a "request to connect" message it sends to Faber’s agent.
 - This message is not plaintext. It uses information from the invitation to securely send the encrypted message back to Faber’s agent.
* Faber’s agent associates the message from Alice’s agent with the invitation that was sent to Alice and confers with Faber’s backend system about what to do with the request.
* Assuming Faber agrees, its agent stores Alice’s connection information, creates a private DID for the relationship, and sends an encrypted response message to Alice’s agent.
 - Whoohoo! The agents are connected.
* Using its agent, Faber can now send a message to Alice (via their agents, of course) and vice versa. The messages use the newly established encrypted communication channel and so are both private and secure.

### Lab: Agents Connecting

Let’s run through a live code example of two Aries agents using a protocol to connect and send messages to one another.

Follow this [link](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/agentsConnecting.md) to try it yourself and explore the code in the example.

## Aries RFCs, Interoperability and Aries Interop Profiles

While we’ll be digging into Aries protocols a lot more in this course (especially in Chapters 4 and 5), we want to cover something right up front that is crucial in any software component built to run protocols: interoperability. A core part of Aries are the protocols that have been defined to exchange messages between agents to accomplish shared goals, such as issuing a verifiable credential and presenting a proof. Inherent in the use of protocols is that they evolve over time as implementations are completed ("wait, we missed this piece of data!"), as new use cases come up ("if we add this to the protocol, we can do these other use cases"), and as the need for new protocols are discovered. In Aries, the "[Aries RFCs](https://github.com/hyperledger/aries-rfcs)" GitHub repository contains the full set of community-created protocols that enable software components to be Aries agents, and PRs are constantly being submitted, reviewed and merged (or rejected). Aries protocols are ever changing. How can a developer build something that is interoperable with things being built by others in the face of all this change?

The challenge of independently building components based on shared, changing protocols is enabling interoperability. How can we get all of the teams of developers using a common set of protocols? What version of the protocols should they be using? Are all the teams interpreting the protocols in the same way? This is a common problem in many technical communities building products based on protocols—Wi-Fi makers, Bluetooth, OAuth and more have all faced this challenge.

The Hyperledger Aries project encountered this challenge after the first few Aries agents were successfully demonstrated at a hackathon in Provo, Utah in early 2020. Leading up to, and at the hackathon, the teams building Aries implementations regularly tested with one another and things worked pretty well—the hackathon was a success! However, once the hackathon ended, the teams went back to focusing on their own use cases, their own deployments, and cross implementation testing and communication fell away. New teams joined the community and built their own implementations without the benefit of the hackathon interop focus. And with a lack of focus on interoperability came frustration… one could never be certain that the agents that worked together yesterday would still be working together tomorrow.

The solution for the Aries community was the definition of Aries Interop Profiles (AIPs). The concept of Aries Interop Profiles is formally defined in Aries [RFC 302](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0302-aries-interop-profile). Here is a quick summary of the key points:

* Each AIP has a version number, starting with 1.0.
 - AIP versions are much like the various published Wi-Fi standards (e.g., 802.11a, 802.11b/g/n and 802.11ac and so on).
* Each AIP has a mission, goals that Aries implementations will be able to accomplish if they are compliant with the AIP version.
* Each AIP defines a set of Aries RFCs that all compliant implementations will support.
* For each included RFC, a link in the AIP goes to a specific version (a literal GitHub commit) of the RFC.
* An RFC-driven suite of tests enables demonstrating interoperability across Aries implementations.

The Aries community defined AIP 1.0 in February 2020, selecting 19 RFCs, and (per the AIP process) selecting a specific version of each of those RFCs. The AIP 1.0 mission was based on the de facto standard Aries agents of the time, particularly as it related to the use of establishing connections and the use of Indy AnonCreds verifiable credentials. One thing that AIP 1.0 did not include at the time it was created was an effective test suite for demonstrating either compliance or interoperability. As we’ll see in Chapter 6, such a test capability came later.

AIP 1.0 has proven to be quite successful in enabling (pretty) reliable interoperability across many Aries implementations. It has been demonstrated repeatedly that it is relatively easy to build interoperable Aries agents from scratch or based on existing frameworks.

In early 2021, an effort was begun to define AIP 2.0, with final approval occurring on May 26, 2021. AIP 2.0’s mission is much the same as AIP 1.0, but extended to add support for both ledgers and verifiable credential formats other than those in Hyperledger Indy. Throughout the course, we’ll highlight places where the differences between AIP 1.0 and 2.0 are most relevant. As we’ve mentioned before, the core concepts of Aries remain the same across implementations, and that’s also true with AIP 1.0 and 2.0.

### Lab: Agents Connecting, AIP 2.0-Style

Let’s do the "agents connecting" lab from earlier, but this time using AIP 2.0 protocols. Not surprisingly, you won’t find much of a difference in the user experience.

Follow this [link](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/agentsConnectingAIP2.md) to try it yourself and explore the code in the example.

## Current Agent Frameworks

There are currently (mid-2021) five major Aries general purpose open source agent frameworks that are (more or less) ready to go out-of-the-box. The following provides a brief summary of the frameworks, including key features and links to their associated repos:

* [aries-cloudagent-python](https://github.com/hyperledger/aries-cloudagent-python) (ACA-Py) is suitable for all non-mobile agent applications and has production deployments. As noted in the previous section, ACA-Py and a controller run together, communicating across an HTTP interface. Your controller can be written in any language that can send and receive HTTP requests, which is pretty much any language. While initially built on top of the indy-sdk for use with Indy ledgers and verifiable credentials, in early 2021, ACA-Py maintainers added support for JSON-LD format verifiable credentials and added a multi-ledger resolver to support other ledgers. As well, in mid-2021, support was added for an Aries-based secure storage module (called aries-askar), eliminating the dependency on the "indy-wallet" part of the indy-sdk. We’ll spend a fair amount of time in this course running labs based on ACA-Py.
* [aries-framework-dotnet](https://github.com/hyperledger/aries-framework-dotnet) (AF-.NET) can be used for building mobile (via Xamarin) and server-side agents and has production deployments. Many of the current (mid-2021) Aries agents in the Google and Apple app stores are built on AF-.NET. The controller for an aries-framework-dotnet app can be written in any language that supports embedding the framework as a .NET library in the controller. The framework embeds the indy-sdk. The Trinsic Studio from Trinsic is a powerful verifiable credentials "as-a-service" platform built on AF-.NET.
* [aries-framework-go](https://github.com/hyperledger/aries-framework-go) (AF-Go) is a pure Golang Aries framework that implements a similar architecture to that of ACA-Py, exposing an HTTP interface for its companion controller. Since it is a pure Golang implementation, it does not embed or use the indy-sdk at all, and has read-only support for Indy ledgers. AF-Go supports W3C Standard Verifiable Credentials, including JSON-LD credentials, and its primary maintainers (SecureKey) have defined their own W3C standard DID method for publishing DIDs called did:orb. AF-Go uses a pluggable remote secure storage mechanism that provides various options for deploying storage for Aries agents.
* [aries-framework-javascript](https://github.com/hyperledger/aries-framework-javascript) (AFJ) is a fairly new framework currently being developed primarily to support the creation of mobile applications based on a JavaScript mobile framework such as React Native. That said, AFJ is also being built to run server-side with Node-JS. The initial version of AFJ is being built to work with Hyperledger Indy ledgers and verifiable credentials, with an aggressive roadmap to add support for other ledgers and verifiable credential formats.
* [aries-vcx](https://github.com/hyperledger/aries-vcx) (AVCX) is an evolution of the VCX (Verifiable Credential eXchange) component written in Rust that was added to the indy-sdk just before the Aries project started. Since it makes more sense for a verifiable credential exchange component to be at the Aries level, VCX was extracted from Indy in early 2021 and a full Aries framework built around it. AVCX is deployed as a library with a C callable interface, meaning that it can be wrapped in and used by a wide variety of programming languages. Like AF-.NET, as of mid-2021, AVCX is focused on use cases involving Indy ledgers and AnonCreds verifiable credentials, although that is likely to change over time.

Of the five frameworks, all except AF-Go support AIP 1.0, and as of mid-2021, only ACA-Py and AF-Go support (much of) AIP 2.0. As you look into the question of what open source Aries framework you will use in your application, check on the current status and roadmap for supporting AIP 2.0.

In addition to the open source Aries frameworks, there are a number of other ways to take advantage of Aries capabilities:

* As of mid-2021, there are two significant Hyperledger Aries mobile wallet applications that we’ll talk about later in the course (wait for it!).
* In addition to the sub-projects within Aries, there are also a number of private companies that have built their own Aries components. For example, the company that started working on the software that became Hyperledger Indy, Evernym, has its own closed source Aries Verity server platform and Connect.Me mobile application SDK.
* A number of companies, such as [Trinsic](https://trinsic.id/), [esatus](https://esatus.com/?lang=en) and [idRamp](https://idramp.com/), have built platforms on top of Aries that in turn allow organizations to deploy verifiable credential-based solutions with minimal internal effort.
* Open source applications built on Aries are starting to appear. The Aries ["Business Partner Agent"](https://labs.hyperledger.org/labs/business-partner-agent.html) (BPA) Hyperledger Labs project, started by Bosch, is a great example. BPA is an Aries agent that holds, issues and verifies organizational credentials, allowing businesses to share verifiable data amongst partner organizations, such as in a supply chain.
* As well, there are countless applications that have been deployed on top of the Hyperledger Aries open source platforms.

There’s a lot going on in Aries! With all that is going on in and around Hyperledger Aries, how do you decide what to use for your project? Well, you are certainly in the right place to figure that out! In this course, we’re covering the core concepts of Aries, things that are consistent across all the frameworks. With that knowledge, combined with your ideas about what you are trying to achieve for your use case, you’ll be ready to decide what is the best platform to build upon. You might decide to go with a commercial approach that does a lot of the heavy lifting for you, leaving you with (mostly) configuration work to support the issuing, holding/proving and/or verifications that your app needs to do. Alternatively, you might deploy your own solution, built on top of the Aries open source framework that fits your organization and the ecosystem in which you want to participate.

Whatever your use case, you’ll have the knowledge you need at the end of this course to make the right call.

## Knowledge Check 2

1. Developers who want to solve business problems should start with an Aries agent framework.
  - True. Agent-based applications are created by adding controllers, application-specific code that controls an Aries agent framework.
2. Which statement(s) is/are correct:
  1. The controller knows how to initiate connections, respond to requests, send messages and nothing more
  2. **The framework is dumb; it just sits there until the controller tells it what to do**
  3. **The controller is the part of a deployment that you build to create an Aries agent**
  4. The framework handles your use case for responding to requests from other agents and for initiating requests
  - A and D are inaccurate. The framework knows how to initiate connections, respond to requests, send messages and nothing more. And it is the controller that handles your use case for responding to requests from other agents. The controller "controls" and the framework merely coordinates the actions.
3. An Aries Interop Profile ensures that protocols never change once they are defined. True or False?
  - False. Within the context of an AIP version, the version of an Aries protocol is locked in. However, the protocol itself may continue to evolve, and in a future AIP, the later version of the protocol may be used.
4. Aries is a core component that is embedded in each language specific instance of an Aries framework. True or False?
  - False. Each Aries framework is (for the most part) an independent implementation of the Aries protocols. While some Aries frameworks share some dependencies (particularly, the Indy SDK and Ursa), that is not a requirement.

## Chapter Summary

This chapter focused on the Aries ecosystem (the way Aries is used in the real world), the Aries agent architecture (the components that make up an Aries agent), and how an Aries agent functions. We looked at examples of Aries agents, namely Alice and Faber College, and you stepped through a demo to verify, hold and issue verifiable credentials. We mentioned, but didn’t spend a lot of time on routing agents (mediators and relays). We’ll cover them in more detail later in the course. Next, we described the Aries agent architecture, discussing an agent framework, its controller and how the two work together.

We next looked at the core of Aries, the protocols, and how Aries Interop Profiles are used to provide certainty for developers looking to build Aries agents that will interoperate with Aries agents built by others. We surveyed the current landscape of Aries frameworks (open and closed source), showing the range of approaches that are available to Aries developers. There are lots of options!

The main takeaway from this chapter is that as a developer, you will most likely be building your own controller, which will give your agent the business rules it needs to follow depending on your agent implementation. This is true regardless of the Aries framework upon which you plan to build your controller.

You might have noticed in the Agents Connecting lab, there was no mention of a distributed ledger. That’s right, Aries agents can provide messaging capabilities without any ledger at all! Of course, since the main reason for using an Aries agent is to exchange verifiable credentials, and verifiable credential exchange requires a ledger, we’ll look at ledgers in the next section.

# Chapter 3: Running a Network for Aries Development

## Introduction

In the last chapter, we learned all about the Aries agent architecture and the key components of an agent: the controller and framework. We also discussed common setups of agents and some basics about messaging protocols used for peer-to-peer communication. The labs in Chapter 2 demonstrated connecting two agents that didn’t use a ledger. Now that you’re getting comfortable with what an agent does and how it does it—and have seen agents at work off-ledger—let’s set the groundwork for using a distributed ledger for your development needs. We’ll even look at some alternatives to using a ledger.

In this chapter, we will describe:

* What you need to know and *not know* about ledgers in order to build an SSI application.
* How to get a local Indy network up and running, plus other options for getting started.
* The ledger’s genesis file—the file that contains the information necessary for an agent to connect to a specific ledger.

## Ledgers: What you do not need to know

Many people come to the Indy and Aries communities thinking that because the projects are "based on blockchain," that the most important thing to learn first is about the underlying blockchain. Even though their goal is to build an application for their use case, they dive into Indy, finding the guide on starting an Indy network and off they go—bumping their heads a few times on the way. Later, when they discover Aries and what they really need to do (build a controller, which is really, just an app with APIs), they discover they’ve wasted a lot of time.

Don’t get us wrong. The ledger is important, and the attributes (robustness, decentralized control, transaction speed) are all key factors in making sure that the ledger your application will use is sufficient. It’s just that as an Aries agent application developer, the details of the ledger are someone else’s problem. There are three situations in which an organization producing self-sovereign identity solutions will need to know about ledgers:

1. If your organization is going to operate a ledger node (for example, a steward on the Sovrin network), the operations team needs to know how to install and maintain that node. There is no development involved in that work, just the operation of a software artifact.
2. If you are a developer who is going to contribute code to a ledger project (such as Indy) or contribute an interface to a ledger not yet supported by Aries, you need to know about the development details of that ledger.
3. If you are building a product for a specific use case, the business team must select a ledger that is capable of supporting that use case. Although there is some technical knowledge required for that, there is little developer knowledge needed—it’s more of a business question.

So, assuming you are here because you are building an application, the less time you spend on ledgers, the sooner you can get to work on developing that application. For now, skip diving into Indy and use the tools and techniques outlined here. We’ll also cover some additional details about integrating with ledgers in Chapter 8: Planning for Production, later in this course.

With that, we’ll get on with the minimum you have to know about ledgers to get started with Aries development. In the following, we assume you are building an application for running based on a Hyperledger Indy ledger.

## Why use a DLT with Aries?

Before we go further in this chapter, let’s go back to the basics and cover the purpose of a distributed ledger when using Aries. For Aries agents that comply with Aries Interop Profile (AIP) 1.0 and AIP 2.0, the primary purpose of the ledger is to be a place for a verifiable credential issuer to publish cryptographic keys and credential metadata so that a prover can produce a presentation that a verifier can cryptographically verify. In theory, such information could be digitally published in other ways, but the attributes of a ledger are ideal for this purpose:

* Data written to a distributed ledger (such as Indy) is immutable—it can’t ever be changed.
* Ledger data can’t be removed, so, for example, the issuer cannot remove the data (public keys and credential metadata) and "break" (make unverifiable) the credentials issued to holders.
* Multiple parties (that is, validators or miners) reach consensus on what is to be written to a ledger, preventing the data from being maliciously altered before writing.
* The data is replicated across a set of independent parties and as such is highly available.

**NOTE**: *Always remember that with verifiable credentials in general, and specifically with Aries, no private data goes on the ledger, and the data written to the ledger is extremely limited to the verifiable credential use case.* **Credentials are NEVER written to the ledge**

That discussion covers why distributed ledgers are commonly used for verifiable credential implementations such as Aries. However, publishing DIDs need not always be to a distributed ledger. There are DID methods that enable publishing DIDs that can be used with Aries agents if you are using other than the Indy AnonCreds verifiable credential format. The limitation about requiring an Indy ledger for Indy AnonCreds is because of the additional ledger objects (e.g., schema, credential definitions, etc.) involved that require the use of an Indy ledger.

The following are some DID methods that you might run into when doing Aries development:

* The "did:web" DID method is a non-ledger alternative for an organization to publish DIDs using their domain name’s DNS record—the same place where they publish, for example, the location of their email and web server. It’s easy to update a DNS record, and we trust the DNS record for that data, why not for the organization’s DID?
  - The problem with this approach is that it is easy for the organization to remove the data at any time, breaking the "immutability" goal for verifiable credentials.
  - On the other hand, (almost) every organization has a domain name and DNS record, so it’s public and really easy to publish DIDs, so it might be a good stopgap measure.
* A developer might use the "did:github" DID method as a quick’n’dirty way to publish a DID for development purposes.
  - Like "did:web," the DID is not immutable and implies even less trust than "did:web," but for development purposes it might be useful.
* A new DID method being developed by SecureKey, the maintainers of Aries Framework Go, is the "did:orb" DID method which uses a protocol called ActivityPub (and other technologies) to enable an entity to publish their DIDs in a trusted way without requiring a distributed ledger.

There are a lot of other DID methods defined in the W3C DID Standard registry. In theory, for non-Indy verifiable credential purposes, any of them could be used. If you plan to use other than Indy, we recommend you do some research on the DID methods to gain confidence that they will still be around in a few years. Some of the more interesting DID methods are ones that are rooted in the major permissionless blockchains such as Bitcoin and Ethereum, as well as ones built on private blockchains such as Hyperledger Fabric.

While the published DIDs and other objects on a ledger are mostly used for the processing of verifiable credentials and verifiable presentations, the public DIDs can also be used for connecting and messaging with an organizational Aries agent. Some organizations use a public DID as the basis for all their connections with other agents, instead of using peer DIDs for that purpose.

We’ll touch on the use of other DID methods later in this chapter. For now though, we’ll focus on running (or not) an Indy network that we’ll use for labs in the later chapters.

## Running a Local Indy Network

The easiest way to get a local Indy network running is to use von-network, a pre-packaged Indy network built by the Government of British Columbia’s (BC) VON Team. In addition to providing a way to run a minimal four-node Indy network using docker containers with just two commands, von-network includes:

* A well maintained set of versioned Indy container images.
* A web interface allowing you to browse the transactions on the ledger.
* An API for accessing the network’s genesis file (see below).
* A web form for registering DIDs on the network.
* Guidance for running a network in a cloud service such as Amazon Web Service or Digital Ocean.

The VON container images maintained by the BC Gov team are updated with each release of Indy, saving others the trouble of having to do that packaging themselves. A lab focused on running a von-network instance will be provided at the end of this chapter.

## Or do not run a network for Aries dev

What is easier than running a local network with von-network? How about not running a local network at all!

Another way to do development with Indy is to use a public Indy network sandbox. With a sandbox network, each developer doesn’t run their own local network, they access one that is running remotely. With this approach, you can run your own, or even easier, use the BC Government’s BCovrin (pronounced "Be Sovereign") networks (dev and test). As of writing this course, the networks are open to anyone to use and are pretty reliable (although no guarantees!). They are rarely reset, so even long lasting tests can use a BCovrin network.

An important thing that a developer needs to know about using a public sandbox network is to make sure you create unique credential definitions on every run by making sure issuer DIDs are different on every run. To get into the weeds a little:

* Indy DIDs are created from a seed, an arbitrary 32-character string. A given seed passed to Indy for creating a DID will always return the same DID, public key and private key.
* Historically, Indy/Aries developers have configured their apps to use the same seeds in development so that the resulting DIDs are the same every time. This works if you restart (delete and start fresh) the ledger and your agent storage on every run, but causes problems when using a long lasting ledger.
 - Specifically, **a duplicate credential definition (same DID, name and version) to one already on a ledger will fail to be written to the ledger**.
* The best solution is to configure your application so a randomly generated seed is used in development such that the issuer’s DID is unique on every run so that the credential definition name and version can remain the same on every run.

**NOTE**: *This is important for development. We’ll talk about some issues related to going to production in Chapter 8: Planning for Production, where the problem is reversed—we MUST use the same DID and credential definition every time we start an issuer agent.*

In the labs in this course, you will see examples of development agents running against both local and remote sandbox Indy networks.

## Proof of Concept networks

When you get to the point of releasing a proof of concept (PoC) application "into the wild" for multiple users to try, you will want to use an Indy network that all of your PoC participants can access, especially if the PoC includes a third party mobile wallet application. As well, you will want that environment to be stable such that it is always available when it’s needed. We all know about how mean the demo gods can be!

Some of the factors related to production applications (covered in Chapter 8: Planning for Production) will be important for a PoC. For such a test, a locally running network is not viable and you must run a publicly accessible network. For that, you have three choices:

* The BCovrin sandbox test network is available for long term testing and is supported by many of the better known mobile wallet applications.
* The Sovrin Foundation, operates two non-production networks:
  - Builder Net: For active development of your solution.
  - Staging Net: For proofs of concept, demos, and other non-production but stable use cases.
  <br>**NOTE**: *Non-production Sovrin networks are permissioned, which means that you have to do a bit more to use those—get a special "Endorser" DID that allows writing to the ledger and agreeing to a "Transaction Author Agreement" as you write to the ledger. We’ll cover these issues a bit in the next section of this chapter and in Chapter 8: Planning for Production.*
* You may choose to run your own network on something like Amazon Web Service or Azure. Basically, you would be running your own version of "BCovrin."

## The Indy Genesis File

In working with an Indy network, the ledger’s **genesis file** contains the information necessary for an agent to connect to that ledger. Developers new to Indy, particularly those who try to run their own network, often have trouble with the genesis file, so we’ll cover it here.

The genesis file contains information about the physical endpoints (IP addresses and ports) for some or all of the nodes in the ledger pool, as well as the cryptographic material necessary to securely communicate with those nodes. Each genesis file is unique to its associated ledger and must be available to an agent that wants to connect to the ledger. It is called the genesis file because it has information about the genesis (first) transactions on the ledger. Recall that a core concept of blockchain is that the blocks of the chain are cryptographically tied to all the blocks that came before it, right back to the first (genesis) block on the chain.

The genesis file for Indy **sandbox** ledgers is (usually) identical, with the exception of the endpoints for the ledger—the endpoints must match where the nodes of the ledger are physically running. The cryptographic material is the same for sandbox ledgers because the genesis transactions are all the same, generated from the same seeds. Those transactions:

* Create a **trustee endorser** DID on the ledger that has full write permission on the ledger.
* Permission the nodes of the ledger to process transactions.

Thus, if you get the genesis file for a ledger and you know the "magic" seed for the DID of the [trustee](https://docs.google.com/document/d/1gfIz5TT0cNp2kxGMLFXr19x1uoZsruUe_0glHst2fZ8/edit#heading=h.xs7z6weav1fw) (the identity owner entrusted with specific identity control responsibilities by another identity owner or with specific governance responsibilities by a governance framework), you can access and write to that ledger. That’s great for development and it makes deploying proof of concepts easy. Configurations such as von-network take advantage of that consistency, using the "magic" seed to bootstrap the agent. For agents that need to write to the network (at least credential issuers, and possibly others), there is usually an agent provisioning step where the endorser DID is used to write a DID for the issuer that has sufficient write capabilities to do whatever else it needs to do. This is possible because the seed used to create the endorser DID is well known. In case you are wondering, the magic seed is:

`000000000000000000000000Trustee1`

**NOTE**: *For information about this, see [this](https://stackoverflow.com/questions/59089178/hypelerdger-indy-node-seed-value) great answer on Stack Overflow about where it comes from.*

As we’ll see in Chapter 8: Preparing for Production, the steps are conceptually the same when you go to production—you use a transaction endorser that has network write permissions to create your DID. However, you’ll need to take additional steps when using a permissioned test or production ledger (such as the Sovrin Foundation’s StagingNet or MainNet) because you won’t know the seed of any endorsers on the network. Connecting to and reading from a production ledger is just as easy as a sandbox ledger—you get the network’s genesis file and pass that to your agent. However, being able to write to the network is more complicated because you don’t know the "magic DID" that enables full write access.

As we’ll see in *Chapter 8: Preparing for Production*, the steps are conceptually the same when you go to production—you use a transaction endorser that has network write permissions to create your DID. However, you’ll need to take additional steps when using a permissioned test or production ledger (such as the Sovrin Foundation’s StagingNet or MainNet) because you won’t know the seed of any endorsers on the network. Connecting to and reading from a production ledger is just as easy as a sandbox ledger—you get the network’s genesis file and pass that to your agent. However, being able to write to the network is more complicated because you don’t know the "magic DID" that enables full write access.

By the way, the typical problems that developers have with genesis files is either they try to run an agent without a genesis file, or they use a default genesis file that has not had the endpoints updated to match the location of the nodes in their network. It’s part of why using von-network is so helpful; it takes care of those details for you, dynamically making the genesis file available to you based on how the network was started.

## Genesis File Handling in Aries Frameworks

Most Aries frameworks make it easy to pass to the agent the genesis file for the network to which it will connect. For example, we’ll see from the labs in the next chapter that to connect an instance of an ACA-Py agent to a ledger you use command line parameters to specify either a file name for a local copy of the genesis file, or a URL that is resolved to fetch the genesis file. The latter is often used by developers that use the von-network because each von-network instance has a web server deployed with the network and provides the URL for the network’s genesis file. The file is always generated after deployment, so the endpoints in the file are always accurate for that network.

### Lab: Running a von-network Instance

Please follow this [link](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/vonNetwork.md) to run a lab in which you will start a von-network, browse the ledger, look at the genesis file and create a DID.

Here is a report on the network start logs: [Google docs](https://docs.google.com/document/d/1XpI8Z4ISg5Y8E-x0Bm7QVle2Wj64qcUC_3KDagEKjpM/edit?usp=sharing)

Running some cli commands to get familiar with the process ([guide source](https://github.com/bcgov/von-network/blob/master/docs/Indy-CLI.md)):

```bash
$ sudo ./manage generateSecrets

Using: docker-compose --log-level ERROR

Seed: QXQ/lXdaCX5gnwk0j3D58LluA9G9SALE
Key: PJD1ufhtWz7A9KzVRQfRtP049iMoVb/yGgtVCKJ1GwN2XeM3DPHk3ID6kNOPdX/1
$ sudo ./manage generateDid QXQ/lXdaCX5gnwk0j3D58LluA9G9SALE

Using: docker-compose --log-level ERROR

Creating von_client_run ... done

Seed: QXQ/lXdaCX5gnwk0j3D58LluA9G9SALE
DID: LtANYCPKaomEGFUkeXyjVR
Verkey: BqWiDpqsgbLL5NYBRVEAKesQ6zZzqyuRuPvkXbHjX9cG
```

Then I navigated to the web interface and authenticated the new DID, which created the following tx on the domain.

```json
{
  "auditPath": [
    "Hf2vXibDGJUFB2sMyhEPZZNKEPEiY4iLFxaxckeXwgKx",
    "D52hsZf4iH4Kp4x4eEp18FicbPNdirG9TL2cvat1eKvL"
  ],
  "ledgerSize": 6,
  "reqSignature": {
    "type": "ED25519",
    "values": [
      {
        "from": "V4SGRU86Z58d6TV7PBUe6f",
        "value": "TA8thqnFM2iAq7a7CCHLZ4tqVXZJgzmzyAUBpBn8fUPK3hjq4vBpBX8jU89xFpHGCsP8KupxoG5DvNDSw8YKyzW"
      }
    ]
  },
  "rootHash": "5DoRdzQnhNGiE12TEbJg2qEKuztFu2LEkmJiD4vrBoL9",
  "txn": {
    "data": {
      "dest": "LtANYCPKaomEGFUkeXyjVR",
      "role": "101",
      "verkey": "BqWiDpqsgbLL5NYBRVEAKesQ6zZzqyuRuPvkXbHjX9cG"
    },
    "metadata": {
      "digest": "1bd4e38e12baa23a1a48820b489b4e899bdd34714e145902c7abc99bd36cea07",
      "from": "V4SGRU86Z58d6TV7PBUe6f",
      "payloadDigest": "32aee0af1a3a0a4e54e71d08702006cea97cca5577b42729e99a955e15d4e1f9",
      "reqId": 1641856234052722700
    },
    "protocolVersion": 2,
    "type": "1"
  },
  "txnMetadata": {
    "seqNo": 6,
    "txnId": "666a9ca8e34a1e997e4fb7594788837e84072eb70434c5da22d5c1af54416427",
    "txnTime": 1641856234
  },
  "ver": "1"
}
```

Let us now create a wallet for the new DID
```bash
$ sudo ./manage dockerhost

DockerHost: 172.17.0.1

$ sudo ./manage cli init-pool local_net http://172.17.0.1:9000/genesis

Creating von_client_run ... done
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3076  100  3076    0     0  3003k      0 --:--:-- --:--:-- --:--:-- 3003k
$ sudo ./manage indy-cli create-wallet walletName=local_net_trustee_wallet

Creating von_client_run ... done
load-plugin library=libindystrgpostgres.so initializer=postgresstorage_init
Plugin has been loaded: "libindystrgpostgres.so"

wallet create local_net_trustee_wallet key storage_type=default storage_config={} storage_credentials={}
Enter value for key:
#123
Wallet "local_net_trustee_wallet" has been created

wallet open local_net_trustee_wallet key storage_credentials={}
Enter value for key:
#123
Wallet "local_net_trustee_wallet" has been opened

did new seed
Enter value for seed:
#QXQ/lXdaCX5gnwk0j3D58LluA9G9SALE
Did "LtANYCPKaomEGFUkeXyjVR" has been created with "~HFNkaCUutq57WtcB1XPbHv" verkey

local_net_trustee_wallet:indy> did list
+------------------------+-------------------------+----------+
| Did                    | Verkey                  | Metadata |
+------------------------+-------------------------+----------+
| LtANYCPKaomEGFUkeXyjVR | ~HFNkaCUutq57WtcB1XPbHv | -        |
+------------------------+-------------------------+----------+
local_net_trustee_wallet:indy> wallet close
Wallet "local_net_trustee_wallet" has been closed
indy> wallet detach local_net_trustee_wallet
Wallet "local_net_trustee_wallet" has been detached
indy> exit
Goodbye...
```

And now we can play around with the cli and the local ledger

```bash
$ cd tmp && sudo touch cliconfig.json && cd ..
# Add the following to cliconfig.json
$ cat cliconfig.json
{
  "taaAcceptanceMechanism": "for_session"
}
$ sudo ./manage indy-cli --config /tmp/cliconfig.json start-session walletName=local_net_trustee_wallet poolName=local_net useDid=LtANYCPKaomEGFUkeXyjVR

Creating von_client_run ... done
"for_session" is used as transaction author agreement acceptance mechanism
load-plugin library=libindystrgpostgres.so initializer=postgresstorage_init
Plugin has been loaded: "libindystrgpostgres.so"

pool connect local_net
Pool "local_net" has been connected

-wallet attach local_net_trustee_wallet storage_type=default storage_config={}
Wallet "local_net_trustee_wallet" has been attached

wallet open local_net_trustee_wallet key storage_credentials={}
Enter value for key:

Wallet "local_net_trustee_wallet" has been opened

did use LtANYCPKaomEGFUkeXyjVR
Did "LtANYCPKaomEGFUkeXyjVR" has been set as active

pool(local_net):local_net_trustee_wallet:did(LtA...jVR):indy> ledger get-nym did=LtANYCPKaomEGFUkeXyjVR
Following NYM has been received.
Metadata:
+------------------------+-----------------+---------------------+---------------------+
| Identifier             | Sequence Number | Request ID          | Transaction time    |
+------------------------+-----------------+---------------------+---------------------+
| LtANYCPKaomEGFUkeXyjVR | 6               | 1641858280995555311 | 2022-01-10 23:10:34 |
+------------------------+-----------------+---------------------+---------------------+
Data:
+------------------------+------------------------+----------------------------------------------+----------+
| Identifier             | Dest                   | Verkey                                       | Role     |
+------------------------+------------------------+----------------------------------------------+----------+
| V4SGRU86Z58d6TV7PBUe6f | LtANYCPKaomEGFUkeXyjVR | BqWiDpqsgbLL5NYBRVEAKesQ6zZzqyuRuPvkXbHjX9cG | ENDORSER |
+------------------------+------------------------+----------------------------------------------+----------+
```

## Accessing Multiple Indy Networks

In the "genesis file" discussions before the lab we talked about loading *the* genesis file for an Aries agent deployment, allowing the agent to connect with, read and possibly write to a single Indy ledger instance. And, until recently, that has been sufficient. With small use cases each with relatively few participants, one network was (sort of) enough (if we ignore developers that were using development, test and production ledgers…). However, as more production networks become available, and Aries agents (especially Aries mobile wallet apps) are used in different use cases, it has become obvious that Aries agents need to access more than one ledger at a time. A person might be issued verifiable credentials from a university using the [Indicio MainNet](https://indicio.tech/indicio-mainnet/), and a government using the Sovrin MainNet and it all has to work smoothly.

Early Aries wallets allowed the user to manually switch ledgers in the settings, but that’s a pretty bad user experience, especially if they have to keep switching back and forth. Worse, there is no way to construct a presentation that uses the two credentials at the same time.

There is a "fix" for this issue in some Aries implementations, with more on the way as this course is updated (mid 2021). Aries agents can be configured to load genesis files for, and connect to, all needed Indy networks. While an issuer agent will likely only write to one of the loaded networks, when resolving a given Indy identifier (a DID or ledger object ID), the agent will check for the object on each of the connected networks. The process has proven to work pretty well, with the caveat that the resolver might find the same object on multiple networks and so need some form of collision handling if the object is on more than one ledger. A future evolution will embed in all Indy identifiers a reference to the ledger on which the object resides, eliminating the possibility of a collision.

The good news for you, the Aries developer, is that for the most part, the complexity of connecting to and resolving objects on multiple Indy ledgers will be handled by the Aries framework you are using.

## Resoving DIDs

As discussed earlier in this chapter, Indy is not the only ledger around and Indy DIDs are not the only DIDs used in Aries. In this section we’ll cover how Aries handles other ledgers and DID methods.

As you (should) know by now, DIDs are resolved in a similar fashion to web URLs, returning a DID document (DIDDoc) instead of a web page. Rather than a single DNS-based process for finding the resource associated with a URL, DID identifiers embed a reference to a DID method in each DID. In turn, each **DID method** has an associated specification that is implemented in software to resolve the DID and find the associated DIDDoc. This picture below shows the DID resolution process.

An application (such as an Aries agent) needs to resolve a DID. They call a resolver ("Universal Resolver" in the picture below), which figures out what DID method is being used. Remember that the DID method is part of the DID, right after "`did:`". From that the resolver decides on the driver to use—there’s usually a driver per DID method, and the DID method knows how to interact with the distributed ledger (or other storage location) to get and return the DIDDoc associated with the DID.

<img src = 'https://courses.edx.org/assets/courseware/v1/72882e343de1a8d766f1039eef0fe10a/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-10.png' alt = 'The DID resolution process' width = 500>

<br><br>
This all sounds good, until you realize that the current (June 2021) [W3C DID Registry](https://w3c.github.io/did-spec-registries/#did-methods) contains 104 DID methods and the list is still growing. With each having their own DID method specification, that means a **universal DID resolver** that can resolve every DID method must support 104 drivers. This is exactly what the open source **DIF universal resolver** does (more or less). Each organization that adds a DID method to W3C DID Method Registry also implements a driver for their DID method and adds it to the DIF universal resolver. An instance of the universal resolver is deployed by DIF as a central web service with APIs that can be used for resolving (in theory, at least) any DID that has been published. And anyone else can deploy their own version of a universal resolver.

That paragraph leads us to a couple of editorial comments:

* First, not all 104 (and counting!) DID methods will last. The general expectation of the community is that a handful of DID methods will survive and the rest will quietly fade away.
* Second, while an interesting and useful tool for developers, a centralized web service such as the DIF universal resolver is not a particularly strong foundation on which to build a scalable, production layer of trust for the Internet. There’s a safer way.

Let’s look at how DID resolution is handled in Aries.

## Aries DID resolvers

In early AIP 1.0 Aries implementations, only a couple of DID methods were supported (specifically Indy and peer DIDs) and they were used only in a few specific scenarios. As a result, DID resolution was handled wherever needed via direct calls to each method. It was workable for the use cases we had, but was not really meeting the goal of Aries being "ledger agnostic."

The "new" approach to Aries DID resolution is a generic "resolveDID" call in each Aries deployment that takes DIDs of any method and returns (if available) a DIDDoc for the DID via a set of resolver plugins. The call determines the DID’s method and checks if there is a resolver plugin available for that DID method. If so, the resolver plugin is called and the result returned; if not, the resolution fails. The term "plugin" is used loosely here; plugins might be either added at compile time or loaded at runtime depending on the framework. Either way, a generic interface to the resolver is used. Each supported DID method is handled in one of two ways:

* The resolver may be a local plugin run within the Aries deployment.
* The resolver might be part of an external "universal resolver."

<img src = 'https://courses.edx.org/assets/courseware/v1/ded47353d42b7948e436db835eb25ceb/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-07.png' alt = 'Aries agent DID resolution: local and external' width = 600>

The idea (shown in the diagram above) is that an Aries implementation will use faster, local instances for commonly used DIDs (in this case, "did:sov", "did:peer", "did:key" and "did:web") and fallback to a universal resolver for "obscure" DIDs that it might need to support. The external "universal resolver" could be the public DIF universal resolver (although that’s not recommended for production), or more likely, a local deployment of the DIF universal resolver, including only the DID methods of interest.

Using such an approach, an Aries deployment can have an easily configured policy on what DIDs to resolve (or not) and how—via a built-in plugin or external resolver.

The built in DID resolvers in Aries are likely "did:peer," "did:key" and the ledger that the Aries deployment writes to, such as an Indy ledger. Aries implementations that don’t support Indy, such as those based on Aries Framework Go, will likely have favored DID methods and use an external DID resolver for Indy DIDs.

## The `did:key` DID method

A special DID method that we haven’t talked about yet but that deserves mention is `did:key`. `did:key` is similar to `did:peer` in that it is not a DID that is published on a ledger. Rather, it is a way to represent a single, public key as a DID. It’s not as capable as a `did:peer` (for example, a did:key cannot have an endpoint, and its key cannot be rotated), but it is useful in a number of Aries RFCs.

The details of using the did:key method are provided in the [`did:key` specification](https://w3c-ccg.github.io/did-method-key/), but the quick version is as follow:

* Create a key pair of a known key type (e.g., Ed25519).
* Encode (using standards "multibase" and "multicodec" per the did:key specification) the public key and its type, and then prefix the result with `did:key:` to create a DID that looks like this: `did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH`
* To "resolve" the DID, reverse the encoding (e.g., remove the "`did:key:`" prefix and decode the multibase/multicodec string) to get back the public key and its type.
* Generate a DIDDoc by merging the three data values (the DID, public key and signature type) into a fixed template.
 - Where possible, such as with an Ed25519 verification key, generate a key agreement key (for encryption) from the verification key and include in the DIDDoc a key agreement block.
 - Here’s a link to a [sample did:key DIDDoc](https://w3c-ccg.github.io/did-method-key/#example-2-a-did-document-derived-from-a-did-key).

 ```JSON
 {
  "@context": "https://w3id.org/did/v1",
  "id": "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH",
  "publicKey": [{
    "id": "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH#z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH",
    "type": "Ed25519VerificationKey2018",
    "controller": "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH",
    "publicKeyBase58": "B12NYF8RrR3h41TDCTJojY59usg3mbtbjnFs7Eud1Y6u"
  }],
  "authentication": [ "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH#z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH" ],
  "assertionMethod": [ "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH#z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH" ],
  "capabilityDelegation": [ "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH#z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH" ],
  "capabilityInvocation": [ "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH#z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH" ],
  "keyAgreement": [{
    "id": "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH#zBzoR5sqFgi6q3iFia8JPNfENCpi7RNSTKF7XNXX96SBY4",
    "type": "X25519KeyAgreementKey2019",
    "controller": "did:key:z6MkpTHR8VNsBxYAAWHut2Geadd9jSwuBV8xRoAnwWsdvktH",
    "publicKeyBase58": "JhNWeSVLMYccCk7iopQW4guaSJTojqpMEELgSLhKwRr"
  }]
}
 ```

Although the did:key template process is based on some pretty simple text processing, the representation is powerful, allowing an otherwise plain old public key string to be handled in the same way as any other DID.

### Lab: Aries and Universal DID resolvers

Please follow this [link](https://github.com/cloudcompass/ToIPLabs/blob/main/docs/LFS173xV2/didResolvers.md) to run a lab in which you try out a couple of DID resolvers, including one built into an instance of ACA-Py.

**NOTE**: There is a `did:ebsi` resolver. Most did methods are broken or no longer supported. I am not sure how to create a did and DIDDoc without simply implementing the specs. I will try https://github.com/sicpa-dlab/acapy-resolver-universal later when using ACA-Py


## Knowledge Check 3
1. Which of the following is **not** true about von-network?
  1. von-network is an easy way to get a local Indy network running
  2. von-network is a ledger that can be used for development, POC and production implementations
  3. von-network is a pre-packaged Indy network built by a team from the Government of British Columbia
  4. von-network is a minimal four-node Indy network running in docker containers
  - B. von-network should not be used as the basis of a production implementation. You can learn how to deploy your own network by looking at VON Network, but don’t use a single docker-compose Indy network for production.
2. A developer starting to build an Aries agent-based application must learn a lot about the underlying distributed ledger/blockchain. True or False?
  - False. While some developers in the community must know the details of the blockchain, developers building Aries agents can get going without knowing all of those details.
3. All public DIDs must be published on a distributed ledger. True or False?
  - False. While many DIDs are published on a distributed ledger, there are good ways to publish DIDs, such as "did:web" (on a website) and "did:github" (on GitHub), that are useful in some situations.
4. Which statement about an Indy genesis file is false?
  1. A genesis file contains information about the physical endpoints (IP addresses and ports) for some of the nodes in the ledger pool of nodes
  2. An agent can use any Indy genesis file to connect to a ledger
  3. A genesis file contains cryptographic material
  4. A genesis file contains information about a trustee DID that has write permission to the ledger
  - B. A genesis file specific to the ledger to which the agent is connecting must be available to an agent.

## Summary

The main point of this chapter is to get you started in the right spot: you don’t need to dig deep into distributed ledger technology in order to develop SSI applications. By now, you should be aware of the options running an Indy network and know the importance of the genesis file for your particular network. We also covered some of the new options for publishing DIDs to other than a Hyperledger Indy network when not using Indy AnonCreds verifiable credentials. If you are using AnonCreds, you will want to stick to using an Indy network. Finally, we went over a powerful DID method that doesn’t use a ledger at all and makes plain public keys way more useful, the "did:key" DID method.

In the last chapter, we covered the architecture of an agent and demonstrated connecting two agents that didn’t use a ledger. In this chapter, we covered running a ledger. So, in the next chapter, let’s combine the two and look at running agents that connect to a ledger.

# Chapter 4 Developing Aries controllers

## Overview

In the second chapter, we ran two simple command line agents that connected and communicated with one another. In the third chapter, we went over running a local ledger. In this chapter, we’ll go into details about how you can build a controller by running agents that connect, communicate and use a ledger to enable the issuing of credentials and the presentation of proofs. For developers wanting to build applications on top of Aries and Indy, this chapter is the core of what you need to know and is presented mostly as a series of labs.

We will learn about controllers by looking at [aries-cloudagent-python (ACA-Py)](https://github.com/hyperledger/aries-cloudagent-python) in all the examples. Don’t worry if you are not a Python guru as with ACA-Py, a controller can be written in any language. ACA-Py is not suitable for use as a mobile wallet app, so we’ll leave the wallet discussion until Chapter 7. In the last section of this chapter, we’ll cover controllers for other Aries open source frameworks. As you’ll discover, the concepts are similar across the frameworks.

In this chapter, you will learn:

* What an agent needs to know at startup.
* How protocols impact the structure and role of controllers.
* About the aries-cloudagent-python (ACA-Py) framework versus other Aries frameworks (such as aries-framework-dotnet and aries-framework-go).

The goal of this chapter is to give you the knowledge you’ll need to build your own controller using the framework of your choice. Succeed and you’ll be an Aries developer!

## The Wallet

In the prerequisite course, "Introduction to Hyperledger Sovereign Identity Blockchain Solutions: Indy, Aries and Ursa" (LFS172x), we talked about the term "wallet" as a mobile application that is like a physical wallet, holding money, verifiable credentials and important papers. This is the definition the Aries community would like to use. Unfortunately, the term has historically had a second meaning in the Indy developer community, and it’s a term that developers in Aries and Indy still use. In Indy, the term "wallet" is also used for the secure storage (introduced in Chapter 1) part of an Indy agent, the place in the agent where DIDs, keys, ledger objects and credentials reside. That definition is still in common use in the source code that developers see in Indy and Aries.

So, while the Indy meaning of the term wallet is being eliminated in Aries, because of the use of the existing Indy components in current Aries code (at least at the time of writing this course), we’re going to have to use the Indy meaning for the term wallet: *an agent’s local, secure storage*. We’ll also use the term "secure storage" to mean the same thing as we do in the rest of the course.

<img src= 'https://courses.edx.org/assets/courseware/v1/cbe95bfbd74ccf88b905b7b6d57e1adb/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-27_v2.png' alt = 'The term wallet' width=400>

## Agent start up

An Aries agent needs to know a lot of configuration information as it starts up. For example, it needs to know:

* The location of the genesis file(s) for the ledger(s) it will use (if any).
* If it needs objects (DIDs, schema, etc.) on the ledger, checking that they exist on ledger and in secure storage, and creating those objects if they don’t exist.
* Transport (such as HTTP or web sockets) endpoints for messaging other agents.
* Storage options for keys and other data.
* Interface details between the agent framework and the controller for events and requests.

These are concepts we’ve talked about earlier and that you should recognize. Need a reminder? Checkout the "Aries Agent Architecture" section of Chapter 2. We’ll talk about these and other configuration items in this chapter and the next.

## Command Line Interface

All Aries agent frameworks will have a number of configuration options that must be set to run an instance in a specific scenario. At minimum, the agent must know about things such as how to access a database for secure storage, how to connect to a ledger, how to interact with the controller, and so on. So, one of the first things you’ll need to know is how to set the configuration options for your agent. Let’s look at how ACA-Py handles agent configuration.

Most of the options for an ACA-Py instance are configured using command line options in the form of `--option <extra info>`. For example, to specify the name of the genesis file the agent is to use, the command line option is `--genesis-file <genesis-file>`. The number of startup options and the frequency with which new options are added are such that ACA-Py is self-documenting. In fact, plugins to ACA-Py (added via command line options!) might add new configuration options at runtime. A `--help` option is used to get the list of options for the instance of ACA-Py you are using.

In addition to specifying options on the command line, ACA-Py provides two other ways to set configuration options:

* Each command line option has a corresponding environment variable that can be used to set options. For example, instead of using the command line option, `--genesis-file <genesis-file>`, the environment variable `ACAPY_GENESIS_FILE` can be set to `<genesis-file>` before running the command to have the same effect. The `--help` information lists the environment variable to use for each option.
* A YAML configuration file can be used to set any and all of the command line options. The following is a minimal YAML file that sets the genesis file for an instance of ACA-Py, including a comment to document why the option is being used. YAML files are a great way to manage and document the settings being used by an ACA-Py instance.

```bash
# Use the Indy BCovrin Test genesis file:
genesis-file: http://test.bcovrin.vonx.io/genesis
```

**NOTE**: *If you specify the options in multiple ways, the order of precedence is command line options override environment variables override YAML file values override code defaults. ACA-Py uses the (pretty cool!) ConfigArgParse library to handle arguments.*

<img src='https://courses.edx.org/assets/courseware/v1/e2ad0c181b5d68e0781df349b7455915/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-19.png' alt='Override order when specifying options' width=300>

## Provision and start options

An agent is a stateful component that persists data in its wallet and optionally, to a ledger. When an agent starts up for the first time, it has no persistent storage and so it must create a wallet and any ledger objects it will need to fulfill its role. When we’re developing an agent, we can do that over and over: start an agent, create its state, test it, stop it and delete it. However, when an agent is put into production, we only initialize its state once. We must be able to stop and restart it such that it finds its existing state, without having to recreate its wallet and all its contents from scratch.

Because of this requirement for a one time "start from scratch" and a many times "start with data," ACA-Py provides two major modes of operation: "provision" and "start." Provision is intended to be used one time per agent instance to establish a wallet and the required ledger objects. This mode may also be used later when something new needs to be added to the wallet and ledger, such as an issuer deciding to add a new type of credential they will be issuing. Start is used for normal operations and assumes that everything is in place in the wallet and ledger. If not, it should error and stop—an indicator that something is wrong.

The provision and start separation is done for security and ledger management reasons. Provisioning a new wallet often (depending on the technical environment) requires higher authority (for example, root) database credentials. Likewise, creating objects on a ledger often requires the use of a DID with more access permissions. By separating out provisioning from normal operations, those higher authority credentials do not need to be available on an ongoing basis. As well, on a production ledger such as Sovrin, there is a cost to write to the ledger. You don’t want to be accidentally writing ledger objects as you scale up and down ACA-Py instances based on load. We’ve seen instances of that and it’s no fun!

We recommend the pattern of having separate provisioning and operational applications, with the operational app treating the ledger as read-only. When initializing the operational app, it should verify that all the needed objects are available in the wallet and ledger, but should error and stop if they don’t exist. At minimum, care must be taken to make sure that the same object is not created by multiple parallel running instances. During development, we recommend using a script to make it easy to run the two steps in sequence whenever they start an environment from scratch (a fresh ledger and empty wallet).

The only possible exception to the "no writes in start mode" method is the handling of credential revocations, which involve ledger writes. However, that’s pretty deep into the weeds of credential management, so we won’t go further with that here. Better yet, in ACA-Py at least, revocation object handling is done entirely within ACA-Py, and controller developers don’t really have to worry about it.

## Startup option groups

The ACA-Py startup options are divided into a number of groups, as outlined in the following:

* *Debug*: Options for development and debugging. Most (those prefixed with "auto-") implement default controller behaviors so the agent can run without a separate controller. Several options enable extra logging around specific events, such as when establishing a connection with another agent.
* *Admin*: Options to configure the connection between ACA-Py and its controller, such as on what endpoint and port the controller should send requests. Important required parameters include if and how the ACA-Py/controller interface is secured.
* *General*: Options about extensions (external Python modules) that can be added to an ACA-Py instance and where non-Indy objects are stored (such as connections and protocol state objects).
* *Ledger*: Options that provide different ways for the agent to connect to a ledger.
* *Logging*: Options for setting the level of logging for the agent and to where the logging information is to be stored.
* *Mediation*: Options related to configuring an ACA-Py instance that is being used as an Aries mediator agent. We’ll cover mediators in Chapter 7, when we talk about mobile wallets.
* *Multi*-tenant: Options for configuring ACA-Py to run in multi-tenant mode, where a single instance of ACA-Py serves as an agent for multiple entities.
* *rotocol*: Options for special handling of several of the core protocols. We’ll be going into a deeper discussion of protocols in the next chapter.
* *Start*-up: Options about profiles, a topic we’ll talk about (briefly) in the advanced ACA-Py section in the next chapter.
* *Transport*: Options about the interfaces (such as HTTP and web sockets) that are to be used for connections and messaging with other agents.
* *Wallet*: Options related to the storage of keys, DIDs, Indy ledger objects and credentials. This includes the type of database (e.g., SQLite or PostgreSQL) and credentials for accessing the database.

**NOTE**: *While the naming and activation method of the options are specific to ACA-Py, few are exclusive to ACA-Py. Any agent, even those from other Aries frameworks, likely provide many of these options.*

### Lab Agent startup options

Here is a [short lab](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/ACA-PyStartup.md) to show you how you can see all of the ACA-Py startup options.

The many ACA-Py startup options can be overwhelming. We’ll address that in this course by pointing out which options are used by the different sample agents.

## How Aries Protocols Impact Controllers

Before we get into the internals of controllers, we need to talk about Aries protocols, introduced in Chapter 5 of the prerequisite course ("Introduction to Hyperledger Sovereign Identity Blockchain Solutions: Indy, Aries and Ursa" (LFS172x)) and a subject we’ll go deep into next chapter. For now, we’ll cover just enough to understand how protocols impact the structure and role of controllers.

As noted earlier, Aries agents communicate with each other via a message mechanism called DIDComm (DID Communication). DIDComm uses DIDs (specifically private, pairwise DIDs—usually) to enable secure, asynchronous, end-to-end encrypted messaging between agents, including the option of routing messages through a configuration of mediator agents. Aries agent’s DIDs used for DIDComm commonly use the did:peer DID method, which uses DIDs that are not published to a distributed ledger, but that are only shared privately between the communicating parties, usually just two agents.

The caveats in the above paragraph:

* An enterprise agent may use a public DID for all of its peer-to-peer messaging.
* Agents may directly message one another without any mediator agents.
* Most current Aries implementations using did:peer do not support rotating keys of the DID.
* Aries AIP 1.0 and 2.0 agents use what has become known as “DIDComm V1”. DIDComm Messaging V2 is currently (mid-2021) being specified at the Decentralized Identity Foundation.

Given the underlying DIDComm secure messaging layer (routing and encryption are covered in Chapter 7), Aries protocols are standard sequences of messages communicated on the messaging layer to accomplish a task. For example:

* The **connection** (or **DID Exchange**) protocol enables two agents to establish a connection through a series of messages—an invitation, a connection request and a connection response.
* The **issue credential protocol** enables an agent to issue a credential to another agent.
* The **present proof protocol** enables an agent to request and receive a proof from another agent.

Each protocol has a specification that describes and defines the protocol (an Aries RFC). Included in the specification are:

* a series of messages
* one or more roles for the different participants
* a series of named states for each role
* a state machine per role that defines the state transitions triggered by messages/events

For example, the following table shows the messages, roles and states for the connection protocol. Each participant in an instance of a protocol tracks its state based on the messages they've seen. An agent’s state depends on its role and the most recent message received or sent by/to that agent. The details of the components of all protocols are covered in Aries [RFC 0003 (RFC 0003)](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0003-protocols).

| Message            | Role of sender | State of sender |
|--------------------|----------------|-----------------|
| invitation         | inviter        | invited         |
| connectionRequest  | invitee        | requested       |
| connectionResponse | inviter        | connected       |
| ack                | invitee        | complete        |

In ACA-Py, the code for protocols is implemented as (possibly externalized) Python modules. All of the core (AIP 1.0 and AIP 2.0) protocols are in the [ACA-Py code base](https://github.com/hyperledger/aries-cloudagent-python/tree/main/aries_cloudagent/protocols) itself. External modules are not in the ACA-Py code base, but are written in such a way that they can be loaded by an ACA-Py instance at run time, allowing organizations to extend ACA-Py (and Aries) with their own protocols. Protocols implemented as external modules can be included (or not) in any given agent deployment. All protocol modules include:

* The definition of a state object for the protocol.
 - The protocol state object gets saved in secure storage while an instance of a protocol is in flight and waiting for the next event.
* The handlers for the protocol messages.
* The events sent to the controller on receipt of messages that are part of the protocol.
* Administrative endpoints that are available to the controller to direct ACA-Py on what to do next for a given protocol.
* Optional command line options for configuring the protocol.

## Protocol versions AIP 1.0 and AIP 2.0

As we start to get into Aries protocols, we need to talk about instances of protocols in terms of version and whether or not the protocol is part of AIP 1.0 or 2.0. In fact, in mentioning "connections," issuing credentials and presenting proofs in the last section, we hit on some of the biggest changes between AIP 1.0 and 2.0.

All Aries protocols are versioned, generally starting with 1.0. Although a protocol is versioned, you can tell the version in a running Aries agent by the "type" field in messages, which includes the version, as shown in these examples:

* `v1.0: https://didcomm.org/issue-credential/1.0/offer-credential`
* `v2.0: https://didcomm.org/issue-credential/2.0/offer-credential`

Protocols are versioned using the "semver" scheme that allows for major and minor version changes based on whether the changes are only additions (minor) or removals and/or changes in meaning (major). In Aries, major protocol versions are generally treated as entirely new protocols, usually with a new RFC and, within implementations, by copying and pasting the code for the old version as a starting point for the new version. Clarifications to an RFC that don’t require a change in the technical implementation of a protocol are permitted without changing the protocol version.

As a reminder from Chapter 2, each AIP version is:

* a specific list of RFCs (many of which are protocols)
* a link for each RFC to a specific GitHub commit of the RFC

In transitioning from AIP 1.0 to AIP 2.0, RFCs were dropped, kept and added, resulting in some protocols being updated to new major protocols versions, including the most fundamental protocols—those used for connecting agents, issuing credentials and presenting proofs.

The impact on this course is that we’ll have a number of places in the rest of the content where we’ll need to indicate whether we’re using/talking about AIP 1.0 or AIP 2.0 RFCs. At key points, we’ll add a paragraph that references the RFC(s) from the other AIP version and the differences between the protocols. As you’ll see, for the protocols that changed from AIP 1.0 to 2.0, the changes are relatively subtle. The concepts are (for the most part) still the same.

## Aries protocols: the controller perspective

We’ve defined all of the pieces of a protocol as well as where the code lives in an Aries framework such as ACA-Py. Let’s take a look at a protocol from the controller’s perspective.

Our agent has started up (an ACA-Py instance and its controller), and everything has been initialized. Some connections have been established but nothing much is going on. The controller is bored, ACA-Py is bored. Suddenly, a DIDComm message is received from another agent. It’s a credential offer message! ACA-Py starts a new instance of the "issue credential" protocol ([RFC 0036](https://github.com/hyperledger/aries-rfcs/tree/main/features/0036-issue-credential) V1/AIP 1.0 [RFC 0453](https://github.com/hyperledger/aries-rfcs/tree/main/features/0453-issue-credential-v2) V2/AIP 2.0) and we’re off and running!

Here’s what happens:

* The ACA-Py instance figures out what connection is involved and creates a protocol state object. It sets some data in the object based on what it received in the message and sends a webhook event (HTTP request) to the controller with information about the message and the protocol state object.
 - This assumes that the ACA-Py instance is not set up to automatically process the offer (e.g., the option `--auto-respond-credential-offer` is not set). If that option is set, the ACA-Py instance would just handle the whole thing and the controller would just get an "all done!" webhook event, but not have to do anything else. Sigh... back to being bored.
 - Since the ACA-Py instance doesn’t know how long the controller will take to tell it what to do next, the ACA-Py instance saves the protocol state object in its secure storage and moves on to do other things—like waiting for more agent messages.
* The controller panics (OK, it doesn’t…). The controller code (that you wrote!) figures out what the business rules are for handling credential offers. Perhaps it just accepts (or rejects) them. Perhaps it puts the offer into a queue for a legacy app to process and tell it what to do. Perhaps it opens up a GUI and waits for a person to tell it what to do.
 - Depending on how long it will take to decide on the answer, the controller might also need to persist the state of the in-flight protocol in a database of its own.
* When the controller decides (or is told) what to do with the credential offer, it retrieves (if necessary) its information about the in-flight protocol and constructs an HTTP request to send the answer to the appropriate ACA-Py administrative endpoint.
 - In this example, the controller might use the "credential_request" endpoint to request the offered credential.

<img src = 'https://courses.edx.org/assets/courseware/v1/e1784b93a2dcf23295008beadfef5c61/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-23.png' alt = "Aries protocols in ACA-Py from the controller's perspective" width = 500>

* Once it has sent the request to the ACA-Py instance, the controller might persist the data about the interaction and then lounge around for a while, waiting for something else to do.
* The ACA-Py instance receives the request from the controller and gets busy. It retrieves the corresponding protocol state object from its secure storage and constructs and sends a properly crafted "credential request" message in a DIDComm message to the other agent, the one that sent the credential offer.
 - The ACA-Py agent then saves the protocol state object in its wallet again as it has no idea how long it will take for the other agent to respond. Then it returns to waiting for more stuff to do.

 That describes what happens when a protocol is triggered by a message from another agent. A controller might also decide it needs to initiate a protocol. In that case, the protocol starts with the controller sending an initial request to the appropriate endpoint of the ACA-Py instance’s HTTP API. In either case, the behavior of the controller is the same:

* Wait for an event to happen.
* Get notified of an event either by the ACA-Py instance (via a webhook) or perhaps by some non-Aries event from, for example, a legacy enterprise app.
* If necessary, retrieve saved information about the related business process.
* Figure out what to do with the event:
 - Perhaps the event ends the business process.
 - Perhaps by asking some other software or a person to do something.
 - Perhaps by sending a response to the event to the ACA-Py instance via the administrative API.
* If necessary, save the state of the business process.
* Return to waiting for the next event to come along.

### Lab: Alice Gets a Credential

In this section, we’ll start up two command line agents, much as we did in Chapter 2. However, this time, one of the participants, Faber College, will carry out the steps to become an issuer (including creating objects on the ledger) and issue a credential to the other participant, Alice. As a holder/prover, Alice must also connect to the ledger. Later, Faber will request a proof from Alice for claims from the credential and Alice will oblige.

We’ll do a couple more versions of this interaction in subsequent labs. There is not much content to see in this chapter of the course, but there is lots in the labs themselves. Don’t skip them!

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/AliceGetsCredential.md) to access the lab.

## Links to code

There are a number of important things to highlight in the code from the previous lab—and in the ones to come. Make sure when you did the previous lab, that you followed the links to the key parts of the controller code. For example, in the previous lab, Alice and Faber ACA-Py agents were started and it’s helpful to know for each what ACA-Py command line parameters were used. Several of the labs that follow will include a comparable list of links that you can use to inspect and understand the code.

## Learning the ACA-Py controller API using OpenAPI

Now that you have seen some examples of a running controller, let’s get minimal. As noted earlier, ACA-Py has been implemented to expose an HTTP interface to the controller—the "Admin" interface. To make it easy (well...easier) for you to understand the Admin interface, ACA-Py automatically generates an industry standard [OpenAPI](https://www.openapis.org/) (also called Swagger) configuration. In turn, that provides a web page that you can use to see all the calls exposed by a running instance of ACA-Py, with examples and the ability to "try it"—that is, execute the available HTTP endpoints. Having an OpenAPI/Swagger definition for ACA-Py also lets you do cool things such as generate code (in your favorite language) to create a skeleton controller without any coding. If you are new to OpenAPI/Swagger, here’s a [link](https://swagger.io/docs/specification/about/) to what it is and how you can use it. The most important use: being able to quickly test something out just by spinning up an agent and using OpenAPI/Swagger.

With ACA-Py, the exposed API is dynamic for the running instance. If you start an instance of ACA-Py with one or more external Python modules loaded (using the `--plugin <module>` command line parameter), those modules must add administrative endpoints to the OpenAPI/Swagger definition so that they are visible in the OpenAPI/Swagger interface.

### Lab: Using ACA-Py’s OpenAPI/Swagger Interface

In this lab, we’ll introduce you to the OpenAPI/Swagger interface for interacting with a running ACA-Py instance, covering how to do some querying of ACA-Py state information. In the next chapter, after we learn a little more about protocols, we’ll use the OpenAPI/Swagger interface to carry out the Faber and Alice use case.

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/OpenAPIIntroduction.md) to run the OpenAPI/Swagger introduction lab.

### Lab: Help Alice get a job THIS IS THE LAB THAT SHOWS YOU HOW TO ADD A SCHEMA!!!

Time to do a little Python controller development. The next assignment is to extend the command line lab with Alice and Faber to include ACME Corporation. Alice wants to apply for a job at ACME. As part of the application process, Alice needs to prove that she has a degree. Unfortunately, the person writing the ACME agent’s controller quit just after getting started building it. Your job is to finish building the controller, deploy the agent and then walk through the steps that Alice needs to do to prove she’s qualified to work at ACME.

Alice needs your help. Are you up for it? Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/HelpAliceGetAJob.md) to run the lab.

### Lab: Python not for you? THIS DOES NOT WORK BUT HAS GREAT GUI SO WE SHOULD TRY TO MAKE IT WORK

The last lab in this chapter provides examples of controllers written in other languages. GitHub user [amanji](https://github.com/amanji) (Akiff Manji) has taken the agents that are deployed in the command line version of the demo and written a graphical user interface (GUI) controller for each participant using a different language/tech stack. Specifically:

Alice’s controller is written in Angular.
Faber’s controller is written in C#/.NET.
ACME’s controller is written in NodeJS/Express.
To run the lab, go to the instructions [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/AriesACAPyControllers.md).

As an aside, Akiff decided to make these controllers based on an [issue posted in the ACA-Py repo](https://github.com/hyperledger/aries-cloudagent-python/issues/93). The issue was labelled "Help Wanted" and "Good First Issue." If you are looking to contribute to some of the Aries projects, look in the repos for those labels on open issues. Contributions are always welcome!

## Building your own controller

Want to go further? This is optional, but we recommend doing this as an exercise to solidify your knowledge. Build your own "Alice" controller in the language of your choice. Use the pattern found in the two other Alice controllers ([command line Python](https://github.com/hyperledger/aries-cloudagent-python/blob/master/demo/runners/alice.py) and [Angular](https://github.com/petridishdev/aries-acapy-controllers/tree/master/AliceFaberAcmeDemo/controllers/alice-controller)) and write your own. You might start by generating a skeleton using the OpenAPI/Swagger tools, or just by building the app from scratch. No link to a lab or an answer for this one. It’s all up to you!

If you build something cool, let us know by [clicking here and submitting an issue](https://github.com/cloudcompass/ToIPLabs/issues). If you want, we might be able to help you package up your work and create a pull request (PR) to the [aries-acapy-controllers](https://github.com/hyperledger/aries-acapy-controllers) repo.

## Controllers for other frameworks

In this chapter we have focused on understanding controllers for aries-cloudagent-python (ACA-Py). The Aries project has several frameworks other than ACA-Py. In this section, we’ll briefly cover how controllers work with those frameworks.

### aries-framework-dotnet

The oldest of the other Aries frameworks supporting AIP 1.0 is aries-framework-dotnet, written in Microsoft’s open source C# language on the .NET development platform. The architecture for the controller and framework with aries-framework-dotnet is a little different from ACA-Py. Instead of embedding the framework in a web server, the framework is built into a library, and the library is embedded in an application that you create. That application is equivalent to an ACA-Py controller. As such, the equivalent to the HTTP API in ACA-Py is the API between the application and the embedded framework. This architecture is illustrated below.

<img src = 'https://courses.edx.org/assets/courseware/v1/23995c77a7ce8f2e36ea17d0a8a90ebe/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-02.png' width = 400>

As we’ll see in the chapter on mobile agents, aries-framework-dotnet can be embedded in Xamarin and used as the basis of a mobile agent. It can also be used as a full enterprise agent, or as a more limited cloud routing agent. A jack-of-all-trades option!

### Trinsic studio

The easiest way to get started with aries-framework-dotnet is with the Trinsic Studio platform from [trinsic.id](https://trinsic.id/), the primary maintainers of the aries-framework-dotnet repository. Although you can build and host your own aries-framework-dotnet agent, the Trinsic Studio let’s you launch and run agents on their platform. That relieves you from having to deploy your application—Trinisic takes care of running your agent for you. Further, the platform provides several pre-built types of agents, such as an issuer and a verifier that you can deploy with just some basic configurations. No coding needed!

The Trinsic platform currently gives you a great tradeoff between being able to get started quickly and easily with verifiable credentials, and having full control over your Aries agent.

In this optional lab, we’ll use the Trinsic Studio to create a verifiable credential verifier agent. Once we have it running, we’ll collect a verifiable credential from a deployed ACA-Py issuer, store it in our wallet app and then verify a presentation from the credential with our new verifier.

To run the lab, go to the instructions [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/TrinisicStudio.md).

### aries-framework-go

The aries-framework-go takes the same approach to the controller/framework architecture as ACA-Py—an HTTP interface between the two. In fact, as the team building the framework has created the implementation, they have tried to use the same Admin interface calls as ACA-Py. As such, a controller written for ACA-Py should work with an instance of aries-framework-go.

What’s unique about this framework is that it is written entirely in Golang without any non-Golang dependencies. That means that the framework can take advantage of the rich Golang ecosystem and distribution mechanisms. That also means the framework does not embed the indy-sdk (libindy) and as such does not support connections to Indy ledgers or the Indy verifiable credentials exchange model. Instead, the team is building support for other ledgers and other verifiable credential models.

In this optional lab, we’ll run the aries-framework-go repository’s "Getting Started" exercise to learn a bit about aries-framework-go. To run the lab, go to the instructions [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/AFGoGettingStarted.md).

## Knowledge check 4

1. When setting the parameters of an ACA-Py framework, YAML file values take precedence over command line options. True or **False**?
 - False. The order of precedence is command line options override environment variables override YAML file values override code defaults.
2. What are the reasons for ACA-Py having separate "provision" and "start" modes? Select all answers that apply:
  1. To split the parameters into groups to make them easier to understand
  2. **For security, by limiting higher authority operations to be limited to provisioning mode**
  3. To allow ACA-Py to automatically handle some protocols without a controller
  4. **To prevent a full agent from inadvertently writing to a ledger that charges per write**
  5. To specify the genesis file
  - B and D. Increasing security and saving money are important.
3. What does the aries-cloudagent-python (ACA-Py) framework do that the aries-framework-dotnet does not?
 1. **Exposes an HTTP interface to the controller**
 2. Builds the framework into a library
 3. Embeds a library into a controller application
 - A. The architecture for the controller and framework with aries-framework-dotnet is a little different from ACA-Py. Instead of embedding the framework in a web server, the framework is built into a library, and the library is embedded in an application that you create. That application is the controller, therefore, the equivalent to the HTTP API in ACA-Py is the API between the application and the embedded framework.
4. The controllers built using the Aries frameworks with a programming language must use that language. True or **False**?
  - False. The language in the name of a framework indicates what is used in building the framework. A controller using a framework can be in pretty much any language.

## Chapter 4 summary

We’ve covered an awful lot in this chapter! As we said at the beginning of the chapter, this is the core of the course, the hands-on part that will really get you understanding Aries development and how you can use it in your use cases. The rest of the course will be lighter on labs, but we’ll refer back to the labs from this chapter to put those capabilities into a context with which you have experience.

# Chapter 5

## Introduction

In the last chapter we focused on how a controller injects business logic to control the agent and make it carry out its intended use cases. In this chapter we’ll focus on what the controller is really controlling—the messages being exchanged by agents. We’ll look at the format of the messages and the range of messages that have been defined in the Aries protocols. Later in the course, when we’re talking about mobile agents, we’ll look deeper into how messages get from one agent to another.

Some of the topics in this chapter come under the heading of "DIDComm Protocol," where pairs (and perhaps in the future, groups) of agents connect and securely exchange messages using DIDs each has created and shared, usually privately. The initial draft and implementations of the DIDComm V1 protocol described here was incubated first in the Hyperledger Indy Agent Working Group, and later the Hyperledger Aries Working Group. Work on a DIDComm Messaging V2 specification is currently happening in the [DIDComm Working Group](https://identity.foundation/working-groups/did-comm.html) within the [Decentralized Identity Foundation (DIF)](https://identity.foundation/).

**NOTE**: *The work on DIDComm V2 was transferred to DIF (another Linux Foundation project) because Hyperledger is an open source code creating organization, not a standards creating body. Since part of DIF’s charter is to define standards, DIF was deemed a more suitable steward for this work. Of course, open source implementations of the standards remain an important part of the Hyperledger Aries project.*

As we’ve just explained, this chapter is all about messaging and the protocols that allow messaging to happen. We will discuss:

* The aries-rfcs repository—the home of all the documents!
* "DIDComm 101," covering the two layers of Aries messaging protocols.
* Which protocol layer a developer needs to worry about (hint: it’s the Aries protocols layer).
* The format of protocol messages.
* Message processing in general.
* More about the differences between AIP 1.0 and AIP 2.0.

## The aries-rfc repository

The concepts and features that make up the Aries project are documented in the Hyperledger [aries-rfcs](https://github.com/hyperledger/aries-rfcs) GitHub repository. RFC stands for "request for comments" and is a type of text document used by a wide range of technical groups to create understanding towards defining and evolving communication standards. RFCs are the documents that tell developers how they MAY, SHOULD and MUST do things to meet a particular standard. With Aries, the RFCs are defined into two groups, concepts (background information that crosses all protocols) and features (specifications of specific protocols).

The aries-rfcs repository is full of extremely detailed information about exactly how each protocol is to be implemented. While the repo is a crucial resource for developers, it is overwhelming for newcomers. It’s definitely not the best place to get started—kind of like learning a new language by reading the dictionary. It’s important that you are aware of the aries-rfcs repo so that when you are deep in the weeds and need to know exactly what a certain protocol does, you know where to look. For now though, follow along here and we’ll get you going!

Through the remainder of the course, we’ll provide pointers into the aries-rfcs repo for things we mention. That way, when you are on your own, you’ll know how to navigate the repo when you need to know the crucial details it holds. Here’s a first example: the [index](https://github.com/hyperledger/aries-rfcs/blob/main/index.md) markdown file in the root of the repo provides a generated list of all of the RFCs in the repo ordered by their current maturity status, proposed, demonstrated, accepted, etc. Check it out!

**NOTE**: *Hyperledger Indy has a comparable repository to aries-rfcs called indy-hipe (for Hyperledger Indy Project Enhancement). Despite the name difference, the purpose of the repository is the same. Likewise, the DIDComm Messaging protocols that moved from Hyperledger Aries to the Decentralized Identity Foundation (DIF) have a comparable home there—although the format of the documents is a bit different. Regardless of where the specification document resides, if it is relevant to Aries protocols and interoperability, it will be covered in aries-rfcs.*

## Basic Concepts of DIDComm Messaging

The core capability of an Aries agent is peer-to-peer messaging—connecting with other agents and sending and receiving messages to and from those agents to accomplish some interaction. The interaction might be as simple as sending a text message, or more complex, such as negotiating the issuing of a verifiable credential or the presentation of a proof.

Enabling both the exchange and use of messaging to accomplish higher level transactions requires participants interact in pre-defined ways, to interact by following mutually agreed upon protocols. Aries protocols are just like human protocols, a sequence of events that are known by the participants to accomplish some shared outcome. For example, going out to a restaurant for a meal is a protocol, with both the guests and the restaurant staff knowing the sequence of steps for their roles—greeting, being seated, looking at menus, ordering food, etc. Unlike human protocols (etiquette), Aries protocols need to be carefully specified and then implemented by multiple participants.

With Aries, there are two levels of messaging protocols.

<img src = 'https://courses.edx.org/assets/courseware/v1/2ad59841be10d060c156f5592556e834/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-28.png' alt = 'DIDComm' width = '400'>
<br>
<br>

At the lower level is the **DIDComm protocol** (version 1.0), the method by which messages are exchanged, irrespective of the message content. The participants exchange DIDs, and the attributes of the DIDs—signing and encryption keys, and service endpoint details—enable end-to-end secure messaging. You can think of the DIDComm protocol as being like the postal service. You write a letter, place it into an envelope, address it to a recipient, give it to the postal service and magically, it appears at the recipient’s door. And, just like the postal service, there is a lot of complexity in the sending and receiving of a message. As an aside, it’s [Version 2](https://identity.foundation/didcomm-messaging/spec/) of this protocol that was moved from Hyperledger to the [DIDComm Working Group](https://identity.foundation/working-groups/did-comm.html) at the [Decentralized Identity Foundation](https://identity.foundation/).

At a higher level are the **Aries protocols** we talked about in the last chapter, the many protocols that define back-and-forth sequences of specific messages to accomplish some shared goal. The simplest of these is one agent sending another agent a text message ("Hi, how are you?"). A much more complex protocol is issuing a verifiable credential, where it takes three messages back and forth to offer, request and issue a credential. The set of messages, the roles of the participants, the state of the protocol and some extra details (such as acknowledgments and error handling), define each of the many Aries protocols.

With the DIDComm protocol we don’t care about the content of the message, just the method by which the message gets delivered. With the Aries protocols, it’s the reverse—we know that the messages get delivered (we don’t care how), and we only care about what to do with the content of each message.

## Aries protcols that matter for devs

As we’ve discussed, there are two levels of protocols at play with DIDComm messaging: the DIDComm protocol (aka "envelope") and the Aries protocols (aka "content"). While it’s important to understand that both layers of protocols exist, for the most part, <mark>Aries application developers (those building Aries controllers) are really not exposed to the lower DIDComm (envelope) layer at all</mark> The DIDComm (envelope) layer is handled pretty much entirely in code provided by the Aries frameworks.

On the other hand, Aries application developers do need to understand a lot about the Aries protocols (content). While the mechanics of receiving Aries protocol messages and preparing and sending messages is handled by the code in the Aries frameworks, the semantics (business processing) of what to do with those messages is up to the controller. For example, when an enterprise agent wants to issue a credential to another agent, the controller doesn’t have to get into the nitty-gritty of preparing a verifiable credential. However, the controller does have to pass in (via the controller-to-framework API) the claims data that will be put into the credential.

As such, for the remainder of this chapter, we will focus only on the upper-level, Aries protocols (content). We will cover a bit about the lower level DIDComm (envelope) protocol in Chapter 7 when we talk about message routing in relation to mobile agents.

## The Format of Aries Protocol Messages

As we saw in the labs in the previous chapter, the format of Aries protocol messages is JSON. Let’s start our look into Aries protocols by considering an example of a simple message and going through each of the JSON items. The following is the only message in the “basic message” protocol defined in Aries [RFC 0095](https://github.com/hyperledger/aries-rfcs/tree/master/features/0095-basic-message).

```JSON
{
    "@id": "caec7c09-e0ef-4e2a-9708-e69316a0b73f",
    "@type": "https://didcomm.org/basicmessage/1.0/message",
    "~l10n": { "locale": "en" },
    "sent_time": "2019-01-15 18:42:01Z",
    "content": "Your hovercraft is full of eels."
}
```

The purpose of the message is simple: to send some content in a message from one agent to another.

While the format is standard JSON, some conventions are used in messages such that they are compatible with [JSON-LD](https://json-ld.org/) (JSON Linked Data)—notably the `@type` and `@id` items that are in every Aries protocol message. For those unfamiliar, JSON-LD is based on JSON and enables semantic information (what the JSON data items mean) to be associated with JSON. It is a W3C-standard way to embed in the JSON itself a link to metadata about the items in the JSON structure so that an application developer can know exactly how the data is to be processed. "Compatible with JSON-LD" means that while the JSON does not support all the features of JSON-LD, it respects JSON-LD requirements such that JSON-LD tools can process the messages. It’s not crucial to know more about Aries messaging and JSON-LD at this point, but if you are curious, you can read Aries [RFC 0047](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0047-json-ld-compatibility) about support for JSON-LD in Aries messages.

Back to the message content. <mark>Every message in every protocol includes the first two fields `@id` and `@type`.</mark>

* `@id` is a unique identifier for the message, generated from a globally unique ID (GUID) library.
* `@type` is the type of the message. It is made up of the following parts:
  - Namespace (e.g., https://didcomm.org). See details below.
  - Protocol (e.g., basicmessage). The name of protocol to which the message type belongs.
  - Message version (e.g., 1.0). Aries uses the [semver](https://semver.org/) versioning system, as defined here, although with just major.minor version components.
  - Message type (e.g., message). The actual text of the message.

Namespace is defined so that when appropriate, different groups (for example, companies, standards groups, etc.) can independently define protocols without conflicting with one another. All of the protocols defined by the Aries Working Group (and hence, documented in the aries-rfcs repo) use the same namespace, the one listed above (https://didcomm.org). By using a URL as the namespace, the Aries community hopes to one day make the message type a link to the human (and perhaps machine readable) specification for the message.

**NOTE**: *When AIP 1.0 was defined, the “Namespace” prefix was a specific DID (specifically this DID:* `did:sov:BzCbsNYhMrjHiqZDTUASHg;spec`). *The Aries community decided that since that DID was on a specific Indy network, it was inappropriate to use for a "blockchain-agnostic" project, and the transition was made to the new didcomm.org namespace. Even now (mid-2021) you may find some production Aries agent still using the old-style DID message type namespace. Message types in all AIP 2.0 RFCs require using the didcomm.org-style namespace.*

The remaining items in the JSON example above are specific to the type of the message. In our example, these are the items `~l10n`, which we’ll talk about in the next section, `sent_time` (when the message was sent, as defined by the sender) and `content` (the text of the message). For each message type in each protocol, the protocol specification defines all of the relevant information about the message type specific item. This includes which items are required and which are optional, the data format (such as the time format in the `sent_time` field above), and any other details necessary for a developer to implement the message type. That overview of message types should be all you need to know. However, it’s a good idea to get used to going to the aries-rfcs repository to get all the details on a topic, so please check out [RFC 0020](https://github.com/hyperledger/aries-rfcs/blob/main/concepts/0020-message-types/README.md) to see the accepted information about message types.

## Message decorators

In addition to protocol specific data elements in messages, messages can include "decorators," standardized message elements that define cross-cutting behavior. "Cross-cutting" means the behavior is the same when used in different protocols. Decorator items in messages are indicated by a "`~`" prefix. The `~l10n` item in the previous section is an example of a decorator. It is used to provide information to enable the localization of the message—translations to other languages. Decorators all have their own RFCs. In this case, the localization decorator is specified in [RFC 0043](https://github.com/hyperledger/aries-rfcs/blob/master/features/0043-l10n/README.md).

The most commonly used decorator is the `~thread` ([RFC 0008](https://github.com/hyperledger/aries-rfcs/tree/master/concepts/0008-message-id-and-threading)) decorator, which is used to link the messages in a protocol instance. As messages go back and forth between agents to complete an instance of a protocol (for example, issuing a credential), `~thread` decorator data lets the agents know to which protocol instance the message belongs, and the ordering of the message in the overall flow. It’s the `~thread` decorator that will let Faber College issue thousands of credentials in parallel without losing track of which student is supposed to get which verifiable credential. We’ll talk later in this chapter about the `~thread` decorator when we go over the processing of a message by an agent. You’ll note that the example message in the previous section doesn’t have a `~thread` decorator. That’s because it is the first message of that protocol instance. When a recipient agent responds to a message, it takes the `@id` of the first message in the protocol instance and makes it the `@thid` (thread-id) of the `~thread` decorator. Want to know more? As always, the place to look is in the aries-rfcs. Other currently defined examples of decorators include `~attachments` ([RFC 0017](https://github.com/hyperledger/aries-rfcs/tree/master/concepts/0017-attachments)), `~tracing` ([RFC 0034](https://github.com/hyperledger/aries-rfcs/tree/master/features/0034-message-tracing)) and `~timing` ([RFC 0032](https://github.com/hyperledger/aries-rfcs/blob/master/features/0032-message-timing/README.md)).

Decorators are often processed by the core of the agent, but some are processed by the protocol message handlers. For example, the `~thread` decorator is processed by the core of the agent to connect an incoming message with the state of a protocol that is mid-flight. On the other hand, the `~l10n` decorator (enabling text display in multiple languages) is more likely to be handled by a specific message type, as its purpose can’t be understood except by the message handler.

## Special Message Types: Ack and Problem Report

The Aries Working Group has defined two message types that deserve special attention: `ack` (acknowledge) and `problem_report`. Much like decorators, these are cross-cutting messages, used in the same way across many protocols. In particular, these messages are for error handling.

In executing an instance of a protocol, there are often times when one agent wants to say to the other "yup, I got your last message and all is well." Likewise, if something goes wrong in a protocol, an agent will want to notify the other "uh-oh, something bad has happened!" Rather than having each protocol define their own special purpose messages, the common `ack` and `problem_report` messages were defined so that any protocol can *adopt* those messages. When a protocol adopts these messages it means that there is a message type of `ack` (and/or `problem_report`) in the protocol, even though there is not a formal definition of the message type in the specification. The definition is found in the respective RFCs: [RFC 0015](https://github.com/hyperledger/aries-rfcs/tree/main/features/0015-acks) for ack, and [RFC 0035](https://github.com/hyperledger/aries-rfcs/tree/main/features/0035-report-problem) for problem report.

Remember that unlike HTTP requests, all DIDComm messages are asynchronous. An agent sends off the message, doesn’t immediately get a response, and continues doing other things until it receives the next message of the protocol. Thus, a common use case for `problem_report` is when an agent sends off a message and doesn’t hear back in a time it thinks is reasonable, it can send a problem report message asking if the previous message was received.

## Protocols and Messages: The Controller’s Perspective

Since protocols enable the interactions between Aries agents, it’s important for you, the Aries developer, to know what the protocols do and how to use them when coding your controller. However, as we’ll see soon, the controller does not handle all the nitty-gritty details of the protocol messages directly. Rather, each Aries framework exposes a way to use the protocols that minimizes what the controller has to do, thus limiting the mistakes the controller can make. In ACA-Py, the endpoints made available via the Admin HTTP API requires the controller to provide the absolute minimum information necessary for ACA-Py to carry out an action.

For example, let’s look at the ["request" message in the RFC 0160 Connection protocol](https://github.com/hyperledger/aries-rfcs/tree/main/features/0160-connection-protocol#1-connection-request). The message has a number of explicitly defined fields, plus some implicit ones such as the `~thread` decorator. Fortunately, the controller doesn’t have to construct the actual message. All the controller does is call the `/connections/request` Admin API endpoint, pass in a single parameter (the ID of the connection, which it got from an earlier webhook event), and ACA-Py takes care of the rest!

So, while it is important to know what’s going on with the protocols your controller needs to use, the technical details about what information the controller needs to supply to the framework is in the API it provides to controllers.

## Framework messaging protocol

Messages are sent between agents. Let’s talk about the processing of messages from the perspective of the controller. We’ll start with a controller initiating a message, and then move onto the receipt of a response to the message.

You should have noticed in looking at the ACA-Py startup parameters and in the labs in the last chapter that ACA-Py can handle a number of common interactions without involving a controller. For example, when ACA-Py is started with the `--auto-accept-invites` parameter, ACA-Py will not wait on the controller to tell it what to do when it receives an invitation—it will just accept it.

The `--auto*` parameters are to simplify getting started and debugging ACA-Py itself. It is rare that those parameters would be used in a production agent implementation. **Don’t rely on those parameters in your application!** In this section we’ll make sure those options are off.

## Initiating a protocol

We’ll start with a protocol initiated by the controller. Assume that Faber College and Alice have established a DIDComm connection and now Faber College wants to offer Alice a verifiable credential about her accomplishments at Faber. The Faber controller uses the administration API to initiate the process by requesting the agent framework send a message identifying the:

* type of message to be sent
* content of the message
* connection to which the message is to be sent

In ACA-Py (and likely other Aries frameworks) the type of message to be sent is implied by the API endpoint used by the controller. In general, there is one API endpoint per message type. The content is provided as JSON data as defined by the API endpoint. Sometimes, the JSON provided by the controller corresponds to the content items of the message that is to be sent—the JSON message fields except `@id` and `@type`. If the controller request is to start a new protocol, it might need to pass in the ID of an existing connection to use. Or, if the controller is sending a request that tells ACA-Py what to do next in an in-flight protocol, it might pass in the ID of the protocol state object.

**NOTE**: *In order to know the connection on which to initiate a protocol, the controller must keep a record of the connections as they are established, perhaps storing the relationship as an attribute of an account it holds. For example, Faber College might keep the correspondence between Alice’s record and the IDs of the DIDComm connections it has established with students in its "Student Information System."*

Armed with that information, agent framework code carries out the details of:

* Finding the connection for Alice.
* Determining how to route messages to Alice’s agent.
* Generating an `@id` for the message.
* Packaging (including encrypting) the message.
* Sending the credential offer message.

If the message requires any special processing, such as getting information from a public ledger, or using a cryptographic primitive to encrypt the message, the agent framework code handles that as well. Since usually a first message is not going to be the only message of the protocol (for example, Alice’s agent is expected to reply to the message), the agent framework also persists a protocol state object in anticipation of the response to come. We’ll talk more about that in the next section.

## Receiving a Response

After the message is sent, the agent handles other work it needs to do. For an enterprise agent such as Faber’s, that might be hundreds or hundreds of thousands of messages being exchanged with the agents of other users. The message from Alice might take a long time to come back. For example, the message might go to Alice’s mobile agent while her phone is not connected to the Internet because she is climbing a mountain in the Andes. Alice won’t receive the message until her phone is back online, and even then, she won’t respond until she’s had a long sleep to recover from her amazing mountain adventure. Plus she needs to catch up on Instagram...

Eventually, a response from Alice’s agent is received—a credential request message because Alice wants to be issued a credential about her degree from Faber College. The agent framework code processes the message (with help from the controller) as follows:

* It uses the `~thread` information to find the message’s corresponding protocol state object and retrieves it from storage. In our example, the state object from the initial credential offer message.
  - If it doesn’t find the protocol state, there is an error in the message metadata, or if the message is found to be out of order, it will likely send an event to the controller to determine if it should send a problem report message back to Alice’s agent.
* It processes any other generic decorators, ones not intended to be processed by the message type handler, such as tracing.
* It hands the message and the protocol state object to the appropriate message type handler. In this case, the credential request handler that is part of the issue credential protocol is invoked.
* The message type handler processes the message, updates the protocol state object appropriately and (if needed) sends an HTTP request to the controller (a webhook) to provide info to the protocol state object.
* The controller receives the information via the event webhook.
* Since the agent framework code doesn’t know how long it will take for the controller to respond to the event, it again persists the protocol state object and carries on with its other duties. Back to waiting for stuff to do...
  - Even some enterprise controllers might take a long time to respond. For example, perhaps the Faber College Student Information System is not connected to their Accounting System. Before they issue a credential, they have to email Accounting to see if Alice is fully paid up to the college and wait on a response. That could take hours…
* The controller uses the information from the protocol state to figure out how it wants to respond to the message. In this case, it checks Alice’s data in the Student Information System and decides if it’s going to issue the credential and the claims that are to go in the credential.
* The controller issues another call to the Aries administration API with information on how to respond to the previous message. It passes all the same types of information as with the first message (above), with the content including the claims to be put into Alice’s verifiable credential. It also passes a reference to the protocol state object.
* The agent framework code receives the request, retrieves the protocol state object, prepares and sends the message to Alice and updates and saves the protocol state object.

You’re right—there’s a lot going on there! The good news is <mark>the sequence described above is the same for almost every request/response transaction, and it is much like the processing of traditional web servers—one layer handles the mechanics of receiving requests and responding, and another layer figures out what the request is for and how to respond</mark>. If you have done any web development, you’ve seen this pattern before.

## Lab: Executing a protocol

We only have one lab in this chapter, but it’s a doozy! We’re going to use the OpenAPI/Swagger interface for ACA-Py that was introduced in the last chapter to manually walk through a connection and credential issuance process from Faber to Alice. There’s no controller other than you and OpenAPI/Swagger to receive and respond for each of the agents

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/OpenAPIController.md) to jump into the lab.

## Aries Interop Profile (AIP) Versions

In Chapter 2, we talked about a key driver in the Hyperledger Aries community—the Aries Interop Profile (AIP) ([RFC 0302](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0302-aries-interop-profile)), and introduced the two AIP versions that have been defined to date, [AIP 1.0](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0302-aries-interop-profile#aries-interop-profile-version-10) and [AIP 2.0](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0302-aries-interop-profile#aries-interop-profile-version-20). Let’s talk about how AIP versions impact you, an Aries developer.

* AIP versions reduce the number of protocols that you need to learn. AIP 1.0 includes just 19 RFCs and only six are Aries messaging protocols. AIP 2.0 increases the total RFC count to 34, but still includes just 10 protocol RFCs. In both versions, many of the non-protocol RFCs are addressed within the Aries framework and are not the concern of those building controllers. The bottom line? Despite the many RFCs in the aries-rfcs repo, the subset of "getting started" RFCs is really quite small.
* As you select and configure an agent framework to use, <mark>you must be aware of the framework’s AIP support and plans for supporting any upcoming AIP versions</mark>. As this course is being updated (mid-2021) that means knowing about support for both AIP 1.0 and AIP 2.0.
* Another factor to consider in selecting and configuring your agent is what AIP versions are supported by other agents in the ecosystem in which you are participating. Building a "latest and greatest" issuer agent when there are no mobile wallets that support the "latest and greatest" is going to be frustrating.
  - Of course, being stuck in "old tech" is also a problem. Encouraging and helping everyone move steadily forward is in all of our best interests.
* Finally, while most of the hard work in supporting an AIP version is in the agent framework, controllers will be impacted in the form of changes and additions to the administrative API. Pay attention to the API endpoints you are using!

As a controller developer you should be aware of when AIP version changes are coming and their potential impact on your controllers. That means that you need to be plugged into the [Aries Working Group](https://wiki.hyperledger.org/display/ARIES/Aries+Working+Group), as that is the group that defines when new versions of AIPs will be introduced. You also have to be aware of what’s happening with the Aries framework you are using. Chances are, there are community meetings and regular updates for that.

## Lab: Executing AIP 2.0 Protcols with JSON-LD

Remember when we said there is only one lab in this chapter? Well, technically, we were telling the truth... This lab is going to be (almost) identical to the one you just finished, but this time we’re going to use AIP 2.0 protocols. We’ll still be doing the connection, but using the Out of Band and DID Exchange protocols. We’ll still be issuing a verifiable credential, but we’ll be using the Issue Credential V2.0 protocol. Also, when we issue those credentials, we’ll be using W3C Standard JSON-LD verifiable credentials using the BBS+ signatures. So, the same lab, but completely different!

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/OpenAPIControllerAIP2.md) to jump into the lab.

All done? We think that’s a pretty important lab for a couple of reasons.

## Reflections from the labs New protocols and VC formats

Here are some things to reflect upon now that you’ve done the lab. Twice.

The lab shows that the controller’s job is pretty much the same, regardless of whether you’re using AIP 1.0 or AIP 2.0. Sure, there are some API differences that have to be accounted for, but the core logic is pretty much the same for the controller--get an event, decide what to do about it, do it, and go back to waiting. And the events that are processed for AIP 1.0 and 2.0 are pretty much the same, even though they are using different protocols.

The lab also shows some of the differences between the handling of Indy AnonCreds and W3C Standard JSON-LD verifiable credentials. AnonCreds are simple (just a list of claims), so managing them is pretty easy and they have some key powerful privacy-enhancing features. However, because their format is so simple, there is a lot of implied understanding between those issuing the credentials and those verifying them. The only technical information about how to use the claims in the credentials is the name of the claims. Conversely, JSON-LD credentials require the creation/use of a lot more semantic information around them. The verifier can know from the `@context` links precise information about the claims, their format, perhaps even localization information (how to present them in different languages). On the other hand, that detail makes them harder to construct and manage, and they lack some of the privacy features of AnonCreds. It’s an interesting tradeoff!

## Summary

We have covered a lot of information about the Aries protocols including the details of the protocols you will use as you code them into your controller. As you build your own application and need to know more about the underlying details of the protocols, you know where to look, right? Yup, the aries-rfcs repo!

Let’s highlight the key takeaways from this chapter:

* You need to know about the aries-rfcs repos but you don’t need to do a deep dive (or even a shallow one) there until you have something specific to look up (for example, handling a particular protocol).

**NOTE**: *Want to gain some points in the Aries community? If you find something in an RFC that needs clarification, submit an issue or a pull request to make things better. Such contributions are always welcome.*

* With Aries, there are two levels of messaging protocols. Since all of the messaging is based on the exchange and use of DIDs, the overall messaging mechanism is called **DIDComm** (for DID communication).
  - The DIDComm protocol handles the delivery of messages.
  - The Aries protocols define the content of the messages delivered.
* As an Aries application developer your focus is on the Aries protocols, the protocols that define back-and-forth sequences of specific messages to accomplish some shared goal. You don’t need to know much about how the messages get delivered, just about the content of each message.
* As an Aries application developer, you also have to pay attention more to the APIs provided by the Aries framework you are using than to the nitty-gritty details of the protocols. You need to understand the protocols, but to use them, it’s all in the API!
* As a developer, you need to be aware of the Aries Interop Profile (AIP) to keep track of the versions of the Aries protocols that other developers and organizations are using so that your applications will be able to interoperate with theirs.

Easy, peasy, right?! You’re well on your way to becoming a Hyperledger Aries developer!

## Knowledge Check 5

1. When it comes to developing an Aries controller, developers should focus more on the DIDComm protocol layer. True or <mark>False</mark>?
2. What two fields are included in every message in every Aries protocol?
  * `@type` and `namespace`
  * `sent_time` and `content`
  * <mark>`@id` and `@type`</mark>
  * `@id` and `content`
3. As a developer, you should be aware of an agent framework’s support for the current AIP version. <mark>True</mark> or False?
4. What are the two message types defined by the Aries Working Group that deserve special attention:
  * `rinse` and `repeat`
  * `auto_response` and `error_handler`
  * `thread` and `id`
  * <mark>`ack` and `problem_report`</mark>
5. In the basic message example below, what does the element `~110n` line tell us:
```JSON
{
    "@id": "123456780",
    "@type": "did:sov:BzCbsNYhMrjHiqZDTUASHg;spec/basicmessage/1.0/message",
    "~l10n": { "locale": "en" },
    "sent_time": "2019-01-15 18:42:01Z",
    "content": "Your hovercraft is full of eels."
}
```
  * The type of the message
  * <mark>A kind of cross-cutting behaviour to apply to the message</mark>
  * Where the message is to be sent
  * The content of the message

# Chapter 6

We’ll take a break in this chapter from diving into the weeds of controllers, APIs and protocols and talk about a couple of useful development tools available in the Aries community. This is a short chapter but it might spark some ideas about how to extend these tools, or what other tools might be helpful.

In this chapter, we will describe the current suite and state of the various Aries development tools. You will learn about:

* The Aries Agent Test Harness
* The Aries Toolbox

We also have some labs that let you take these tools for a test drive.

## Interoperability testing

We’ve talked several times about the Aries Interop Profiles (AIPs) and briefly mentioned that there is a need for a way for Aries agents to demonstrate interoperability and AIP-compliance. When AIP 1.0 first came out, agent and agent framework builders could claim (self-assert) they were compliant, but wouldn’t it be better if they could prove their support for a specific AIP version? As we know from all we’ve learned about verifiable credentials, self-asserted claims are not nearly as useful as claims issued by an authority. Further, wouldn’t it be even more useful to know that not only does an Aries component comply with AIP 1.0, but that it is demonstrably (and continuously) interoperable with other Aries components—that ACA-Py really does work with aries-framework-dotnet, aries-framework-go and so on?

To prove an agent or agent framework’s support for a given AIP version, we want to be able to run an instance of the agent through a set of tests aligned with the versions of the protocols included in the AIP. The test suite must generate, at minimum, a report on the tests executed and on success. Better yet, test suite execution should produce a verifiable credential with claims about the conformance results!


Aries RFC ([0270 Interop Test Suite](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0270-interop-test-suite)) outlines the requirements that should be provided by an Aries Interop Test Suite. The following list highlights some of the important features mentioned in the RFC that we want in a good test suite. Think about these features as you go through this section:

* Test cases should be built around protocols and the different roles supported by the protocols. For example, test cases should cover the agent both initiating and responding to a protocol.
* Tests should exercise both expected and unexpected inputs; agents should gracefully handle receiving bad data.
* The test suite should have test variations to exercise different features of agents.
* The test suite should allow the agent-under-test (AUT) configuration to define the set of tests to run.
* An execution of the test suite should report only on the results of the tests executed.
* The test suite should be reasonably efficient to run. This implies there is likely a need to load the state of participating agent(s) to a given starting state vs. executing actions to get to the desired starting state for every test.
* The test suite should be automated and able to be included in a [CI/CD pipeline](https://semaphoreci.com/blog/cicd-pipeline) as part of the code promotion process.

A couple of these requirements (e.g., having the agent-under-test (AUT) initiate tests and loading state) imply that <mark>the test suite has a way to control the AUT for some tests—to get it to do something versus waiting to be contacted via agent messaging. This is done using the concept of a "backchannel," an API between the test suite and the AUT.</mark> This is pictured below, in an image taken from [RFC 0270](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0270-interop-test-suite). The "frontchannel" is the normal DIDComm messaging path between agents, while the backchannel is used to tell the agent what to do in executing a test.

**NOTE**: *In order to execute the tests in a test suite, an AUT may have to build some custom code to handle the backchannel commands sent by the test suite. This is part of deploying an instance of the agent to run the tests in the suite.*

<img src = 'https://courses.edx.org/assets/courseware/v1/8a3258ca5844cbd48d47b317ec79c314/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-05.png' width = 500><br>

With that introduction into Aries interoperability, let’s take a look at the Aries Agent Test Harness.

## Aries Agent Test Harness

The vision for the Aries Agent Test Harness (AATH) comes from the physical labs that the big telecommunication companies use to test their products. In those labs (like the [UNH InterOperability Lab](https://www.iol.unh.edu/)), a telecom company brings their networking products, new and old, plugs them into the lab network and sees if they work with products from all of the other participating companies. Networking products that don’t interoperate with the rest of the network ecosystem create problems for everyone. Same with Aries components in the ToIP ecosystem!

AATH works by having each "component-under-test" packaged as a test agent (TA) that looks identical (from the outside) as all other TAs. <mark>In AATH, a component-under-test is generally an Aries framework (not an agent, hence not an "agent-under-test"), although in theory it could be an agent (e.g., a framework plus a controller)</mark>. Each TA implements a backchannel (as discussed above) that provides an API that can be called by the test harness to orchestrate test steps. The backchannels are controllers that for each API request, cause the component-under-test to execute the steps. Since all the TAs look the same (at least to the test harness), AATH tests can be run using any set of TAs.

Each AATH test involves four TAs (called ACME, Bob, Faber and Mallory), as shown in the picture below. The TAs are driven by a "Behavior Driven Development" ([BDD](https://www.agilealliance.org/glossary/bdd/)) engine (specifically, the [Behave](https://behave.readthedocs.io/en/latest/) engine) that orchestrates the test executions. Each test script implements some AIP capability—a set of protocols. As noted, since all of the TAs are (more or less) identical, any TA can play any role—limited of course by the capabilities of the "component-under-test." To make everything easy to run, the test agents are all packaged into Docker-type containers.

<img src = 'https://courses.edx.org/assets/courseware/v1/42e7c57386ffdfee64b630367c556c48/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics_v3.png' width = 700>

The names of the participants in the test scripts imply their usual role in a verifiable credential ecosystem, although they may perform activities other than their usual role in some tests.

* ACME is an issuer
* Bob is a holder/prover
* Faber is a verifier
* Mallory is holder/prover that is sometime malicious

When AATH tests are run, the TAs and BDD engine are deployed and a list of test scripts to execute is passed into the engine. As necessary, some supporting services are also started; perhaps a VON Network Indy instance, or an external universal resolver. Driven by the test scripts, the BDD engine makes calls (using HTTP) to the TA backchannels, triggering interactions between the components under test using the DIDComm agent-to-agent interface. Progress and results are reported from the backchannels to the BDD engine, and optionally, to a test suite execution report tool.

The BDD tests themselves are quite readable and so provide a good example of the business flows involved in Aries agent interactions. For example, the following is a test script that executes the "present proof" protocol:

```
Scenario Outline: Present Proof where the prover does not propose a presentation of the proof and is acknowledged
      Given "2" agents
         | name | role |
         | Faber | verifier |
         | Bob | prover |
      And "Faber" and "Bob" have an existing connection
      And "Bob" has an issued credential from When "Faber" sends a request for proof presentation to "Bob"
      And "Bob" makes the presentation of the proof
      And "Faber" acknowledges the proof
      Then "Bob" has the proof verified

      Examples:
         | issuer |
         | Acme |
         | Faber |
```

The AATH scripts are organized around a protocol that is part of a particular AIP. Tags are used on tests to both indicate the topic of the tests (e.g., what RFC, what AIP version, etc.) and to select what tests are to be executed on a given run.

The AATH approach of using a backchannel to control the component-under-test should be familiar to you. The backchannel is just another Aries controller! Each test agent is just a bundling into a Docker container of a controller (the backchannel) that responds to HTTP requests from the test harness, and an Aries component-under-test (usually an Aries framework) that the controller controls.

## Lab Aries Agent Test Harness

In this lab we’ll look at the Aries Agent Test Harness that can be used to test the interoperability of two Aries agents.

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/AriesTestHarness.md) to run the lab.

## Visualizing Aries Interoperability

While the Aries Agent Test Harness is interesting to know about, how does it help an Aries developer? Why is it important?

One obvious benefit is that the tests provide a good way to see how Aries agent interactions occur (by looking at the BDD test scripts) and how controllers are implemented (by looking at the backchannels). But other than that, the internals of the AATH is really focused on the developers of Aries frameworks. Helpful, but not too helpful.

A second thing you might consider is creating a backchannel for your full Aries agent. We’ve not had anyone in the community do this yet, but it has been considered. It would give you a whole lot of runnable tests and a way to run them against other implementations. Interesting, but if you are building on an existing Aries framework, there shouldn’t be any surprises.

However, there is one absolutely critical output of AATH that is a must-use resource for all Aries developers. The AATH repo has a series of automated test sets (via GitHub actions) that are executed daily for all of the available backchannels using (usually) the latest code merged into the "main" branch of each "component-under-test." The results of those runs are collected using a BDD test reporting tool ([Allure](https://github.com/allure-framework/allure2) — pretty neat!) and a summary report is generated that is published on the website https://aries-interop.info every few days (whenever the results change).

By monitoring the https://aries-interop.info website, you can track:

* Current state of interoperability across a number of Aries frameworks (most importantly, the current state of interoperability of the Aries framework that you are using).
* You can see what specific tests are passing and failing. Are they going to impact you?
* You can drill into the test results and see exactly where the failures are occurring. Perhaps you can contribute a fix?
* You can see areas that are not being tested. Perhaps you could contribute some test cases to AATH!
* You can see how new protocols and AIP versions are progressing across Aries implementations. This will tell you, for example, is AIP 2.0 ready to use in the wild?

## Lab The aries interop info website

In this lab, you will check out the Aries Interop Info website, learn how to navigate beyond the front page and use the Allure reports to drill into specific test cases. We’ll try to find some test failures and see exactly where and why they are failing.

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/AATHInfo.md) to run the lab.

## The mobile backchannel

The last thing we’ll mention about the AATH is a cool backchannel that can be used to test mobile wallet apps. The "mobile" backchannel operates the same as all of the other backchannels, except the "component-under-test" is intended to be an Aries mobile wallet. The backchannel is limited, as the only way the backchannel can "control" the mobile wallet is by printing directions for you (the person holding the phone) and displaying QR codes for connecting the mobile wallet to the other test agents being run. Once connections are made, the actions of the other agents drive the behavior of the mobile wallet. Only some of the AATH tests can be run, and the mobile agent is "Bob" in all the tests.

The beauty of the "mobile" backchannel is that you can download a mobile wallet app onto your phone and immediately run it against issuers and verifiers using different Aries frameworks, no muss, no fuss! It’s also pretty cool how the QR codes are displayed on a terminal screen. Of course, they are pretty low resolution, so if the mobile wallet’s scan feature can’t handle the QR code, you are pretty well stuck.

Next you will find a quick lab to give that a try.

## Lab The AATH Mobile Backchannel

In this lab, you will use AATH, the mobile backchannel and one of the other backchannels to test the interoperability of an Aries mobile wallet. Get out your favorite mobile wallet app and give it a try!

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/AATHMobileBackchannel.md) to run the lab.

That’s it for the Aries Agent Test Harness. Next up, the Aries Toolbox!

## The Aries Toolbox

The Aries Toolbox is a desktop tool (written in [Electron](https://www.electronjs.org/) using [Vue](https://github.com/SimulatedGREG/electron-vue)) that allows a user to control the behavior of running Aries agents. It provides a graphical user interface tuned to Aries for developers and system administrators to control (in theory) any running Aries agent. To enable an instance of the Aries Toolbox to control running agents, additional protocol message types are added to these agents. With that additional functionality, <mark>an ACA-Py agent can be controlled by an Aries Toolbox instance using the DIDComm channel, instead of by the HTTP interface used by a typical ACA-Py controller</mark>.

Conceptually, the Aries Toolbox enables controller functionality similar to what we saw with ACA-Py and its OpenAPI interface. However, unlike the generic OpenAPI interface, which is used with any HTTP interface, the Aries Toolbox has Aries-specific knowledge and capabilities, making it easier to accomplish certain agent-related functions. As such, <mark>while the OpenAPI interface we talked about in earlier chapters is strictly for developers to learn about the administrative API to ACA-Py</mark>, the Aries Toolbox can be used to connect to any running agent—even those in production—to accomplish administrative tasks.

During interactions with other agents, the Aries Toolbox shows connection, protocol state and debugging information. The image below is the Aries Toolbox running with connections to ACA-Py agents for Alice and Bob.

<img src = 'https://courses.edx.org/assets/courseware/v1/6986ad72bdefb5a725799feb48449874/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/AriesToolboxScreenshot.png' width = 700>

As shown in the image, an administrative interface for each agent connection is presented in its own window, along with a list of actions that the user can trigger the agent to perform. The list of actions is tuned to each connected agent by the Aries Toolbox executing the "Discover Features" protocol ([RFC 0031](https://github.com/hyperledger/aries-rfcs/tree/main/features/0031-discover-features)).

Architecturally, Aries Toolbox works quite differently from the combined ACA-Py/OpenAPI administrative interface we looked at earlier in the course. Rather than using the HTTP interface that ACA-Py exposes, Aries Toolbox uses the same DIDComm agent interface supported by every Aries agent. To work with a given agent, Aries Toolbox requires that the connected agent supports special administrative message types. It’s through these added message types that the Aries Toolbox is able to control the agent. For ACA-Py, a second repo, aries-acapy-plugin-toolbox, provides an external Python module that can be included in any ACA-Py instance at runtime (using the `--plugin ACA-Py` command line parameter) to add the necessary admin message types. The following contrasts the ACA-Py Admin API with the approach used by the Aries Toolbox:

| ACA-Py/OpenAPI | Aries Toolbox |
|---|---|
| Uses an HTTP interface to ACA-Py. | Uses the DIDComm agent interface. |
| Use when you are learning about the HTTP interface to ACA-Py in order to write your own controller. | Can (in theory) connect to any Aries Agent—provided it supports extended protocols needed by Aries Toolbox. |
|  | Use this for controlling an agent to establish connections and execute higher level protocols. |

The use cases for the two techniques are quite different. The ACA-Py/OpenAPI mechanism is a training tool intended to be used by developers learning how to use the administrative API exposed by an ACA-Py agent in order to write a controller application. Aries Toolbox is for interactively controlling Aries agents. However, since it does not expose a programmable API, Aries Toolbox cannot be used as the basis of a controller application (although it is a great example controller).

The Aries Toolbox implements a couple of interesting use cases:

* Users new to Aries agents can explore the interactions between agents (connections and protocols) at a higher-than-the-controller level, to understand how they work.
* Aries Toolbox can be used to perform one time agent actions that previously required the Indy Command Line Interface (CLI) or single purpose scripts.

For example, Aries Toolbox can be used for experimenting with creating and writing a new schema and credential definition to a test ledger, and then trying out issuing credentials using those objects. We’ll look at more such tasks in Chapter 8 when we talk about getting ready for production.

The special administrative message types used by the Aries Toolbox are not part of aries-rfcs and so are not supported by other agent frameworks. The ACA-Py capability to add Python external modules to an agent at runtime made it easy for Aries Toolbox developers to add the necessary messages to ACA-Py agent instances.

## Lab The Aries Toolbox

In this lab, we’ll spin up some Aries ACA-Py agents and the Aries Toolbox, and use the Toolbox graphical user interface to control the agents.

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/AriesToolboxLab.md) to run the lab.

## Knowledge check

1. What should be used by developers to learn how to use the administrative API exposed by an ACA-Py agent to write a controller application?
  * <mark>OpenAPI/Swagger</mark>
  * Aries Toolbox
  * ACA-Py
  * Aries Agent Test Harness
2. How is the Aries Toolbox different than the ACA-Py OpenAPI administrative interface?
  * <mark>It uses the DIDComm agent interface to control agents</mark>
  * It uses the HTTP interface to control agents
  * It uses the OpenAPI/Swagger interface to control agents
  * It uses an administrative interface to control agents
3. The backchannel in the Aries Agent Test Harness is used to tell the test agent what to do in executing a test. <mark>True</mark> or False?
4. Both the APTS and AATH implement agents that test the conformance of an agent to a given standard, such as the Aries Interop Profile (AIP). True or <mark>False</mark>?

# Chapter 7

So far in this course we have focused on how a controller injects business logic to control the agent and make it carry out its intended use cases. We have looked at how the controller manages the messages being exchanged by agents and the format and range of the messages that have been defined by the Aries Working Group. In this chapter, we’ll look at the architecture of mobile Aries agents. In doing that, we need to look more closely at message routing—how messages get from one agent to another. The routing part is applicable beyond mobile agents, but we’ve combined the two because routing is required from mobile agents. So, even if you aren’t interested in mobile agents, read on, because there are lots of other things to learn in this chapter.

In this chapter, you will learn about:

* Agent message routing, particularly the important roles of mediator and relay agents in mobile messaging, and especially in the Aries mobile wallet use case.
* The concept of agency—a collection of cloud agents that service mobile Aries agents.
* The role of the DIDDoc in message routing.
* Emerging open source mobile agent projects.

## Agent message routing

While this chapter is eventually going to be about mobile agents, there is an important digression we have to make in order to understand why mobile agents work the way they do. We’ll get into mobile agents soon, but let’s first talk about the general topic of Aries message routing—how a message gets from its sender agent to its recipient. In this section, we’ll use the term "edge agent" to mean the ultimate sender and recipient agents, to differentiate from the intermediate "routing agents" that may be used solely to get the message from one edge agent to another.

Based on what we have covered in the course so far, it’s easy to form a mental model of lots of edge agents interacting directly with one another; Faber College has its enterprise agent, Alice has her mobile wallet app on her smartphone and the two can message one another whenever the need arises. However, while two messaging agents appear to be directly connected, they often are not. Mediator and relay (terms we will formalize shortly) agents are often necessary to enable messages to be securely delivered from one agent to another because:

* Mobile agents do not have an endpoint (a physical address) that other agents can use for sending messages. Thus, it is impossible for other agents to send messages directly to a mobile wallet app.
* Entities may want to minimize correlation of their agent’s messages across relationships and so instead of having a unique endpoint per agent, many agents share a common endpoint (for example, https://agents-r-us.ca) such that their specific messages are "hidden in a crowd" of lots of other messages.
* Entities may not want their inbound messages to be correlated to their outbound messages so they use different paths for sending and receiving messages.
* An enterprise may want to have a single gateway for the use of the many enterprise agents they have in their organization.

Thus, when a DIDComm message is sent from one agent to another, it is routed per the instructions of the receiver to the sender, and for the outbound needs of the sender. For example, using the following picture, Alice might be told by Bob to send messages to his phone (agent 4) via agents 9 and 3, and Alice might always send out messages via agent 2.

In DIDComm, the term **domain** is used to indicate the group of agents that are working for a given entity. Alice’s domain has agents 1 and 2, and Bob’s agents 3, 4, 5 and 6. Agents 8 and 9 represent **agencies**—service providers that provide domain endpoints, host cloud agents and may provision edge agents on behalf of entities. Our concern in defining domain boundaries is how messages travel from one domain to another. What does Alice have to know to get a message both to Bob’s domain (the physical endpoint) and through to Bob’s edge agent (the sequence of agents)? Incidentally, there is a Cross-Domain Message [RFC (0094)](https://github.com/hyperledger/aries-rfcs/issues/239) that covers a lot of this material as well.

<img src='https://courses.edx.org/assets/courseware/v1/5c29c635e7e4360e22dec82c4271c3e0/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-20.png' width=500>

When Bob and Alice communicate, they want their message to be private—they don’t want the intermediate agents to see the contents of their message. So, when there are other agents between the two, Alice and Bob encrypt their messages with wrappers that provide only enough information for each intermediary agent to route (send) the message along on the next step of its end-to-end journey. These wrappers are the equivalent to a postal service envelope with just a "To" address on the outside of the envelope.

To carry the postal system analogy a bit further, suppose Alice and Bob work in different offices of a corporation. Alice might write her message (on paper) for Bob, put it into an interoffice envelope addressed to Bob, and then in a postal service envelope addressed to the office in which Bob works. She mails the letter via the postal service and it gets delivered to the mailroom at Bob’s office. The outer envelope is removed in the mailroom, and the inner envelope is delivered to Bob via the internal mail system. Bob takes the message out of that envelope, and reads Alice’s message. This matches the DIDComm world exactly, with Alice using encryption for the envelopes, and the postal service and mailroom as intermediary agents to facilitate delivery.

## Mediators and Relays

In DIDComm, the term mediator is used for the list of agents Bob provides Alice through which Alice’s message will be routed. If there is no list of agents, the message will go directly to Bob, so her agent needs only to encrypt the message for transport to Bob. For each mediator, she explicitly adds another envelope, another layer of encryption and a "To" address. Thus, Bob’s "list of agents" is really just a list of encryption keys for Alice to use.

**NOTE**: *As an aside, the folks designing the DIDComm spec have come up with some clever handling to prevent message bloat when there are multiple encryption envelopes. When you repeatedly encrypt the same content, the message can get quite large, and care has been taken to prevent that.*

The term **relay** is used in DIDComm to indicate that the message is being routed through one or more additional agents *without* the knowledge of the sender. The important difference is that the sender must know about all recipient’s mediators and explicitly add envelope wrappers for each. They don’t know (or care) about the recipient’s relays. To stretch our paper message analogy a bit more, the mailroom in Bob’s building might deliver all the messages for Bob’s floor to an assistant who then distributes the messages to their recipients. Alice doesn’t know about that process, and the mailroom handles the "encryption" of putting the messages for Bob’s floor into an envelope.

There is (of course!) an Aries RFC that covers mediators and relays in more detail—[RFC 0046](https://github.com/hyperledger/aries-rfcs/blob/main/concepts/0046-mediators-and-relays/README.md).

To get back to our Alice and Bob’s agents picture, agents 9 and 3 are mediators because Bob explicitly tells Alice, "please send your messages to me through those agents." If Alice is sending her outbound messages via agent 2, then agent 2 is acting as a relay agent for her.

**Mediators and relays are agents too!** An important item to underline here is that mediators and relays are themselves Aries agents. They may only do routing activities, or they may do other tasks on behalf of their controlling entity. They have a peer-to-peer relationship with other agents with which they interact, and they use that channel to coordinate the routing they will do on behalf of the edge agents, and to route the messages.

## Mediators, Relays, and Privacy

When messages pass through other agents there are some privacy implications that need to be considered. It is tempting to simplify the message handling as much as possible, but there are privacy reasons for being careful about how to do that. In this section, we’ll give you a taste of the threats to privacy that we are trying to mitigate with DIDComm. Keep these in mind as you think about deploying your DIDComm based services.

Each time a mediator and relay agent receive a message and pass it on, they can record information about the message—metadata. Even though the agent can’t see the content of the message, they know at least that the message was sent, how big it is, when it was sent and where it will go next. It is well known that with existing communications systems, the collection of metadata can lead to major invasions of privacy. Telephone metadata (who called who, when and for how long), routinely collected by the telcos, enables the detailed tracking of an individual’s social and business relationships. Facebook and Google tracking your logins to other services provides them with information on your interests and frequency of use of that service. ISPs can likewise learn about your interests by tracking the websites you visit from your home computer and mobile phone.

One of the goals in designing the DIDComm messaging protocol was to try to limit the metadata exposed in passing messages. The use of encryption for every wrapper and the minimal inclusion of information in the envelope (just a "To" address for the next step in the route) limits what metadata can be gathered. The use of multiple hops in the sequence limits the agents from knowing the early and later steps in the flow. From our Alice and Bob picture above, let’s consider what *could* be collected by the agents:

* Every agent in the flow could try to decrypt the inner messages being passed along.
  - As long as we are using updated, trusted encryption approaches this risk is mitigated—their decryption attempt would fail. That’s covered by DIDComm messaging.
* Agent 9 can see the physical address from which a message came (e.g., from 2). However, it can’t see from where agent 2 got the message, so unless all and only Alice’s messages come from agent 2’s physical IP address, it won’t know it’s from Alice.
  - This is a reason that Alice would likely not want to run her own agent on her own hardware at home as it would be easier to know when Alice was sending and receiving messages. Enterprises, on the other hand, might not be so concerned.
* Agent 2 can see that messages are going to the physical address of agent 9. Again, as long as people/organizations other than just Bob are using agent 9 as a physical endpoint, agent 2 doesn’t know who the ultimate message recipient is.
  - This is the "lost in the crowd" idea. Many people receive their messages at common physical endpoints, and the senders (and other Internet observers) don’t learn anything about the ultimate sender or receiver (e.g., Alice or Bob).
* Agent 3 (Bob’s mobile agent mediator) knows every time Bob receives a message and when he picks up the messages.
  - This is the same for almost any mobile application operating in conjunction with a cloud service.
  - While not yet (as far as we know) implemented, in theory, Bob could have several mediators and use different ones for different connections.
* Agent 3 does not know from whom the messages were received. Further, if Bob sends out his messages via a different agent (say agent 6), agent 3 does not know anything about Bob’s outbound activities.
  - Of course, if agent 3 and agent 6 are operated by the same service, the service would know about all of Alice’s traffic. This leads to the question about how much you trust the service.
* For a mobile agent, the data sent between the mediator and the mobile agent flows through the mobile service provider that Bob is using. They likewise can know when all the messages are flowing (inbound and out), but cannot decrypt the content of the messages.
* The recipient agent (mobile or otherwise) can see the plaintext data of all of the messages so that data can be displayed for the user. Of course this is necessary and assumed, but from a privacy and security perspective, it should be considered as a risk. What if the Aries agent you are using is scraping the plaintext data presented on screen and sharing it with other parties?

Those are at least some of the privacy threats to consider. A security/privacy maven would likely be able to find others. In comparison with the examples we gave (telcos, "login with" services, ISPs), the amount of exposed metadata is far less with DIDComm.

In theory, by using different vendors for different agents you could further reduce the metadata each participant can gather. But that comes with its own challenges—for example, the need to engage with each of the different vendors. Other techniques have been suggested. These include:

* Randomly sending "no-op" messages to reduce the knowledge gained about when you send/receive messages
* Adding a chunk of throwaway data to messages to alter the size of the message.

However, these complicate the creation of controllers. It’s all about tradeoffs.

## Mobile agents and mobile agent mediators

We’re supposed to be talking about mobile agents in this chapter, so why the in-depth look into message routing? While routing is an extremely important thing to understand about DIDComm and Aries agents in general, it is especially important for mobile agents—hence the digression.

As noted in the previous section, mobile agents cannot be sent data directly. All data that gets to any mobile application (agents, games, email—any app) does so via the mobile app making a request to receive the data. <mark>As such, Faber’s agent cannot directly send a message to Alice’s mobile agent.</mark> It’s also impractical for Alice’s mobile agent to send requests to all of her connections to ask "Have you got a message for me?," on the off chance that they do.

"Wait!," you say, what about all those notifications I get on my phone? Aren’t those sent to my mobile apps? While there is some ability to send notifications to a mobile app, the use of notifications is restricted by mobile operating system vendors (for example, Google and Apple). Only registered services may send notifications to an app, so arbitrary agents can’t message a mobile agent that way. The app stores limit the volume of notifications that can be sent, and require that notifications have an associated message displayed to the user when sent. <mark>As such, notifications cannot be used as a way to send messages to an Aries mobile agent.</mark>

The bottom line is that in order to operate, <mark>a mobile agent **must** have a mediator agent through which all inbound (at minimum) messages must flow</mark>. As a result, if you are thinking about deploying a mobile agent, you also have to think about how you will deploy a cloud-based mediator agent, how it will be architected and what features it will provide.

The mediator agent serves other purposes as well. Since mobile agents are not online at all times, and are not constantly polling to see if they have any incoming messages (that consumes phone resources, particularly data and battery), the mediator provides a queue to hold messages until the mobile agent requests them. The mediator could be registered with the mobile OS (iOS or Android) notification mechanism and let the user know when a message arrives in the queue, triggering a check with the mediator. A mediator could provide backup and restore capabilities for the mobile agent, backing up the agent periodically, and enable a UI supporting the restoration of the agent’s storage. The mediator could provide other services as well, but that would come down to trust. How much does the mobile agent owner want to trust the mediator? Let’s look at that next.

## Mobile agent trust

In the previous section we mentioned a few other services that the mediator agent could provide for a mobile agent. The amount that such an agent could do on our behalf is largely based on how much we can trust the mediator, and the vendor that is providing it. Let’s go over what you as a developer of applications in this ecosystem should be thinking about with respect to the trust your clients must have in your products.

As mentioned in the earlier section on privacy, the mediator for a mobile agent will know more than any other about our messaging activity—timing of inbound (at least) messages, when messages are picked up, information about your mobile app (for example, the ISP and other information to connect with your mobile agent) and your use of any other services it provides. This is the same for any mobile application with an associated web service.

The difference between Aries agents and other mobile apps is that in many cases, Aries agents hold keys that must be tightly held by the owner of those keys. As such, where those keys reside and how they are accessed is paramount in the client trusting the agents. In particular, the private keys that enable Aries protocols such as verifiable credential exchange, should be under the complete control of the agent’s owner, and ideally protected by a secure enclave such as is provided on mobile phones. That gives the owner confidence (trust) that the only way to access the agent and all that it is protecting.

A suggestion we often hear from developers new to the community is about making a mobile agent that resides on the cloud as a web service and put only the user interface on the mobile phone. This would be extremely easy to do with ACA-Py since you would only be building the controller on the mobile device. The problem with that approach is that all of the owner’s keys would reside on the web service infrastructure and not in the direct control of the owner. The owner would have to trust that the web service would not do anything with the private keys they are holding. That’s a big leap compared to having the keys locally, and not one we would recommend except (perhaps) where there are no other options.

So, while it is tempting with Aries to build centralized components, as is often effective in other business domains, be careful in doing that. Make sure that the control, particularly of private keys and the use of those private keys is as close to the owner as possible. Using cloud services run by vendors that are just passing along encrypted messages is likely safe enough and necessary. Using cloud services run by vendors that are (for example) actually receiving credentials and proving claims about an entity (a person or an organization) must be considered with skepticism.

## Message encryption handling

The DIDComm encryption handling is performed within the Aries agent, and not really something a developer building applications using an agent needs to worry about. Further, within an Aries agent, the handling of the encryption is left to libraries—ultimately calling dependencies from Hyperledger Ursa. To encrypt a message, the agent code calls a `pack()` function to handle the encryption, and to decrypt a message, the agent code calls a corresponding `unpack()` function. The "encryption envelope" described in [RFC 0019](https://github.com/hyperledger/aries-rfcs/blob/main/features/0019-encryption-envelope/README.md) for AIP 1.0 and [RFC 0587](https://github.com/hyperledger/aries-rfcs/tree/main/features/0587-encryption-envelope-v2) for AIP 2.0 define variations for sender authenticated and anonymous encrypting. With authenticated messages, the keys of the sender and recipient are used together for the encryption, so the recipient knows exactly who the sender is. With anonymous encryption, only the public key of the recipient is used, so anyone who knows that key can send a message. In DIDComm, authenticated encryption is generally used for all "sender-to-recipient" message encryption, while anonymous encryption is generally used only for messages to and from mediators and relays. Much thought has also gone into repudiable and non-repudiable messaging, as described in [RFC 0049](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0049-repudiation).

## Establishing a connection with routing

To this point in the course when we’ve talked about establishing a connection, we’ve assumed the two agents are able to talk *directly* to one another. With the scenario described below, mediators are involved in the connection. As you will see, this process seems to be (Ok, it is) pretty complicated, and it’s useful to understand what’s happening. The good news is that for an Aries developer building and deploying an Aries application, the details of using mediators is a deployment and configuration issue, and not a coding issue. We’ll cover some of the details at the end, but once you configure your agent to use a mediator, and optionally, deploy your own mediator, everything else should be taken care of automatically by the Aries framework. If the agent your application is connecting to, whatever mediators that agent requires will definitely automatically be handled by the Aries framework.

## The scenario

We'll use the same Alice and Bob example we used earlier. Here’s that picture again.

<img src='https://courses.edx.org/assets/courseware/v1/5c29c635e7e4360e22dec82c4271c3e0/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-20.png' width=500>

Bob and Alice want to establish a connection so that they can communicate. Bob uses an agency endpoint (such as https://agents-r-us.ca), labelled as 9 and will have a mediator agent, labelled as 3. We'll also focus on Bob's messages to and from his main mobile agent, labelled as 4. We'll ignore Bob's other agents (5 and 6) and we won't worry about Alice's configuration (agents 1, 2 and 8). While the process below is all about Bob, Alice and her agents are doing the same kinds of interactions within her domain.

Note that this scenario is actually more complex than is generally expected in the current versions of agents. Usual practice today is just to have a mediator agent service run by the same organizations that make their mobile agent available in the app store. While a bit more complicated, the following really illustrates what’s going on.

A DID and DIDDoc are generated by each participant in each relationship. For Bob's agents (mobile agent and mediator), that includes:

* Bob and Alice
* Bob and his mediator agent
* Bob and the agency

That's a lot more than just the Bob and Alice relationship we usually think about!

From a routing perspective, the important information in the DIDDoc is the following (as defined in the DIDDoc Conventions [RFC 0067](https://github.com/hyperledger/aries-rfcs/blob/master/features/0067-didcomm-diddoc-conventions/README.md)).

* The public keys for agents referenced in the routing.
* A service element of type did-communication, including:
  - the one `serviceEndpoint`
  - the `recipientKeys` array of referenced keys for the ultimate target(s) of the message
  - the `routingKeys` array of referenced keys for the mediators

Recall that a DIDDoc is what you get when you resolve a DID, regardless of where that DID is published (and even if it is not published). Shown below is an example of these elements of a `service` definition section of a DIDDoc. We saw this in the mediators lab we did earlier. In this example, there is one recipient of the message and one mediator.

```JSON
{
  "service": [{
    "id": "did:example:123456789abcdefghi#did-communication",
    "type": "did-communication",
    "priority" : 0,
    "recipientKeys" : [ "did:example:123456789abcdefghi#1" ],
    "routingKeys" : [ "did:example:98490275222" ],
    "serviceEndpoint": "https://agent.example.com/"
  }]
}
```

Let's look at the did-communication service data in the DIDDocs generated by Bob's mobile and mediator agents for the set of relationships involved. Recall that there are three relationships, and with two DIDDocs per relationship, we have six(!) DIDDocs to look at. Note the term "key reference" all through the following tables. In most cases, that will be an instance of a "did:key." Recall from our discussion in Chapter 4 that "did:key" is a plain public key and key type that is formatted as a DID. An example of a "did:key" is in the JSON above.

| Bob's DIDDoc for Alice: |  |
|---|---|
| `serviceEndpoint` | The endpoint (often an HTTP URL) for the agency.  This could be empty if the `routingKeys` (below) contains a public DID for the agency. In that case, the public DID would contain the endpoint. |
| `recipientKeys` | A key reference for Bob's mobile agent specifically for Alice. |
| `routingKeys` | Key references to the public keys for the agency and mediator agent. Perhaps a public DID is used for the agency rather than a key reference. |

```JSON
{
  "service": [{
    "id": "did:peer:bob-for-alice#did-communication",
    "type": "did-communication",
    "priority" : 0,
    "recipientKeys" : [ "did:peer:bob-for-alice#keyagreement" ],
    "routingKeys" : [ "did:key:mediator123", "did:key:agency123" ],
    "serviceEndpoint": "https://agents-r-us.ca/"
  }]
}
```

| Alice’s DIDDoc for Bob: |  |
|---|---|
| `serviceEndpoint` | Depends on Alice’s inbound messages configuration. From our picture, it’s likely a URL for the Alice agency ("8" in the picture). |
| `recipientKeys` | A key reference for Alice’s mobile agent specifically for Bob. |
| `routingKeys` | Depends on Alice’s agent inbound messages configuration. From our picture, likely key references for agents "8" and "2". |

| Bob’s DIDDoc for his mediator agent: |  |
|---|---|
| `serviceEndpoint` | Null, because Bob's mobile agent has no endpoint (see the note below for more on this). |
| `recipientKeys` | A key reference for Bob's mobile agent specifically for the mediator agent. |
| `routingKeys` | Empty—there are no routing agents between Bob and his mediator. |

```JSON
{
  "service": [{
    "id": "did:peer:bob-for-mediator#did-communication",
    "type": "did-communication",
    "priority" : 0,
    "recipientKeys" : [ "did:peer:bob-for-mediator#keyagreement" ],
    "routingKeys" : [ ],
    "serviceEndpoint": null
  }]
}
```

**NOTE**: *The null serviceEndpoint for Bob's mobile agent is worth a comment. Mobile apps work by sending requests to servers. As we’ve covered earlier in the course, the server has no way to directly access the mobile app. A DIDComm mechanism called Transport Return Route [(RFC 0092)](https://github.com/hyperledger/aries-rfcs/tree/main/features/0092-transport-return-route) defines how a server can get messages to an agent to which messages cannot be sent. Instead of the mediator agent sending the message, it holds the message until the mobile agent makes an HTTP request (or a web socket connection) to the mediator. The mediator then transports the message back to the mobile agent using the HTTP response. While this approach doesn’t work well for a mobile agent getting direct messages from all it’s connections (e.g., Alice, Faber, Acme, etc.), it does work well for a mobile agent having a single mediator that handles all its connections. As well, cloud agents can use mobile platforms' (Apple and Google) notification mechanisms to trigger a user interface event so the person (and app) know there are messages queued.*

| Bob’s mediator’s DIDDoc for Bob: |  |
|---|---|
| `serviceEndpoint` | A physical endpoint for Bob's mediator agent. |
| `recipientKeys` | A key reference for the mediator agent specifically for Bob. |
| `routingKeys` | Empty, since the two agents are directly connected. |

```JSON
{
  "service": [{
    "id": "did:peer:mediator-for-bob#did-communication",
    "type": "did-communication",
    "priority" : 0,
    "recipientKeys" : [ "did:peer:mediator-for-bob#keyagreement" ],
    "routingKeys" : [ ],
    "serviceEndpoint": "https://agents-r-us/mediator123"
  }]
}
```

| Bob’s DIDDoc for the agency: |  |
|---|---|
| `serviceEndpoint` | The physical endpoint for Bob's mediator agent. |
| `recipientKeys` | A key reference for Bob's mobile agent specifically for the agency. |
| `routingKeys` | A key reference to Bob’s mediator agent. |

```JSON
{
  "service": [{
    "id": "did:peer:bob-for-agency#did-communication",
    "type": "did-communication",
    "priority" : 0,
    "recipientKeys" : [ "did:peer:bob-for-agency#keyagreement" ],
    "routingKeys" : [ "did:key:mediator123" ],
    "serviceEndpoint": "https://agents-r-us/mediator123"
  }]
}
```

| Agency DIDDoc for Bob's mediator: |  |
|---|---|
| `serviceEndpoint` | A physical endpoint for the agency. |
| `recipientKeys` | A key reference for the agency specifically for Bob's mobile agent. |
| `routingKeys` | Is empty—Bob’s agent sends messages directly to the agency. |

```JSON
{
  "service": [{
    "id": "did:peer:agency-for-bob#did-communication",
    "type": "did-communication",
    "priority" : 0,
    "recipientKeys" : [ "did:peer:agency-for-bob#keyagreement" ],
    "routingKeys" : [ ],
    "serviceEndpoint": "https://agents-r-us"
  }]
}
```

## Preparing Bob's DIDDoc for Alice

Given that background, let's go through the sequence of events and messages that occur when Bob and Alice first connect so that Bob's agent can construct the service part of the DIDDoc to send to Alice's agent.

### Bob and the mediator

Before Alice and Bob can start the process to establish a connection with one another, Bob must initialize his routing setup. To do that, Bob’s agent first establishes DIDComm connections with the mediator and agency agents, and then asks each if they will mediate for him. If the same vendor is providing all of Bob’s agents, it’s possible that process might be done in some proprietary way—that’s how it was done in AIP 1.0. However, with AIP 2.0, a new protocol called "coordinate-mediation" ([RFC 0211](https://github.com/hyperledger/aries-rfcs/tree/main/features/0211-route-coordination)) was added that enables generic mediators to be deployed and used. The protocol has a few messages ("mediate-request", "mediate-grant" and "keylist-update") that we’ll use in the following processes.

* mediate-request: an agent (e.g., Bob’s mobile wallet) asks a mediator to provide mediator services such as forwarding messages to the asking agent.
* mediate-grant: The mediator agrees to provide mediation services, and gives a key reference (usually a "did:key") that the asking agent can share with their connections in the `routingKeys` array.
* keylist-update: While establishing a connection, the asking agent sends a key reference to the mediator for the new connection to say in effect, "when you see a message for this key, please send it to me." The mediator maintains what amounts to a routing table of all the keys for which it might receive messages and for each, an agent to whom the message for that key should be sent.

For this example, we’ll assume that Bob’s agency and mediator are independent and using the "coordinate-mediation" protocol. First, Bob’s agent and the mediator agent establish their relationship, as follows:

* Bob’s mobile agent (somehow) connects to the mediator agent (agent 3 in our picture)—perhaps triggered by a link in the app.
* Once connected, Bob’s mobile agent sends a "mediate-request" message to the mediator agent.
* The mediator agent agrees, and sends back a "mediate-grant" message with a public key and endpoint for the mediator. Bob will use the public key in DIDDocs for his connections, such as to the agency and later, to other connections like Alice.

As mentioned before, the [RFC 0092](https://github.com/hyperledger/aries-rfcs/tree/main/features/0092-transport-return-route) Transport Return Route protocol will be used to allow Bob’s mobile wallet to get the messages from the mediator agent even though the wallet doesn’t have an endpoint.

### Bob and the agency

With Bob’s mobile agent now having a mediator, let’s add the agency (agent 9 in our picture) as a second mediator for Bob. In this case, the agency is just a "lost in the crowd" mediator, allowing many agents to share the same endpoint, so no outside observer can tell who is the ultimate receiver of each message.

Here’s the process:

* Bob’s mobile agent connects to the agency (agent 9 in our picture), likely triggered by a QR code from the agency that Bob scans. That generates a DID for the connection.
* Bob populates the the DIDDoc for the agency with the following:
  - The did-communication service endpoint is the mediator agent endpoint.
  - The recipientKeys array is populated with a DID containing Bob’s public key for the agency.
  - The routingKeys array populated with a DID containing the public key that Bob has from his mediator agent.
* Before sending the response to the agency, Bob first has to let the mediator agent know that messages using a new "to" agency DID should be sent to Bob.
  - To do this, Bob sends a "keylist-update" message to the mediator agent to say "when you see messages for this DID, send them to me." The mediator starts forwarding messages sent by the agency to Bob’s agent.
* Bob’s mobile agent sends a "mediate-request" message to the agency.
* The agency agrees and sends back a "mediate-grant" message containing a public key and endpoint for the mediator. Bob will use both in DIDDocs for his connections, such as Alice.

Initialization is done, and routing is established. The example would be a lot easier just to have one mediator, but what’s the fun in that! With two, you see all the details—adding more mediators is just the same again. In the vast majority (perhaps all so far?) real deployments of Aries mobile wallets, only a single mediator is used.

### Connecting to Alice

Let’s assume that Bob's mobile agent receives an out-of-band connection invitation message from Alice—perhaps via a QR code or a link in an email. With the initialization in place, the steps for Bob to follow to prepare his side of the connection is as follows:

* Bob's mobile agent generates a new DID for Alice and prepares and partially completes a DIDDoc, including the public key(s) that he will use when sending messages to Alice.
  - The did-communication service endpoint is the agency endpoint.
  - The `recipientKeys` array is populated with a DID containing Bob’s public key for Alice.
  - The `routingKeys` array is populated with: a DID containing the public key that Bob has from the agency and a DID containing the public key that Bob has from his mediator agent.
* Before sending the response to Alice, Bob first has to let the mediator agent know that messages using a new "to" Alice DID should be sent to Bob.
  - To do this, Bob sends another "keylist-update" message to the mediator agent to say "when you see messages for this DID, send them to me." The mediator is now forwarding messages sent by the agency and Alice to Bob’s agent.

Note that the agency doesn’t need to know about Alice at all. It will receive messages from Alice (although the agency won’t know who that is), check the "next hop" DID and use the information from Bob to figure out it must send the message to the mediator agent.

If there is a public DID used for the agency, Alice will have to resolve (look up on a public ledger) the DID of the agency to get the public key for the agency. Done that way, the agency, which presumably has many users, each with many relationships, can just update its public DID information to, for example, rotate its key, rather than having that done by every user of the agency, for every relationship they have. The downside of that is that Alice has to regularly check the public ledger for changes to the agency’s DIDDoc.

### Completing the connection

With the DIDDoc ready, Bob uses the path provided in the invitation to send a connection-request message to Alice with the new DID and DIDDoc. Alice now knows how to get any DIDComm message to Bob in a secure, end-to-end encrypted manner. Subsequently, when Alice sends messages to Bob's agent, she uses the information in the DIDDoc to securely send the message to the agency endpoint, from which it is sent to the mediator agent and on to Bob's mobile agent. And with that, Bob has the information he needs to securely send any DIDComm message to Alice in a secure, end-to-end encrypted manner. Connection established!

### Alice messages Bob

Based on the DIDDoc that Bob has sent Alice and the internal configuration that Alice uses to send messages, the following are the steps that Alice carries out in order to send a DIDComm message to Bob:

* Prepares the message for Bob's agent.
* Encrypts and places that message into a "forward" message for Bob's mediator agent.
* Encrypts and places that message into a "forward" message to Bob's agency endpoint.
* Encrypts and places that message into a "forward" message to Alice’s outbound relay.
* Sends that message to her outbound relay.

**NOTE**: *The first two "forward" messages are required because of what Bob put into his DIDDoc for Alice. The last is independent of what Bob’s agent requires and is needed because of how Alice’s agent has decided her outbound messages should be handled. Alice could have just skipped that last forward and sent the message straight to Bob’s agency endpoint, no relay required.*

## Lab: Using a mediator

The ACA-Py Aries framework repository includes functionality for a basic mediator agent using an instance of ACA-Py using the two AIP 2.0 mediator-related Aries RFCs. That means that in the repository is actually a controller implementation of a mediator agent. As well, the ACA-Py framework itself contains functionality to allow an ACA-Py-based Aries agent to be a client of a mediator. Let’s use those capabilities to create a mediator for Alice to use in her connection with Faber.

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/MediatorLab.md) to get started on the mediator lab.

## Open source mobile wallets

It’s obvious that particularly for the self-sovereign identity use cases we’ve been talking about, mobile wallets are crucial. To date there have been a number of good closed source agents deployed to the Google and Apple App stores. These wallets, built on AIP 1.0 for interoperability, have enabled the success of many production and proof-of-concept applications. Although the wallets are closed source, a number of them have been built on the open source Aries Framework .NET.

For several reasons, it is desirable to have open source wallets that can be deployed by appropriate organizations. An open source wallet in this context includes core Aries functionality (a framework) and as much of the user experience as possible. Let’s look at why open source wallets are likely to be important as Aries agents go mainstream.

Perhaps the most important reason to have an open source wallet is to ensure that the code and functionality in the wallet is transparent and auditable. Users are trusting their wallet apps to manage their keys and credentials. No one wants a wallet that (for example) shares private data about the user, or exploits that information for profit, such as using the information to market to the user ("Hey, I see you presented your Driver’s License to the police. Do you want me to connect you to a good lawyer?"). We’ve seen this type of exploitation with web browsers, and we definitely don’t want a repeat of that, especially with an app whose explicit purpose is to manage a user's private data. Important issuers and verifiers, such as businesses and governments, want to be certain that their users have wallets that are known to be safe and secure. Open source is a great way to achieve such a certainty.

Most vendors to date have built wallets not to make money on the wallet itself, but rather, to enable use cases for the issuer/verifier applications they are building to sell to enterprises. For the reasons outlined in the previous paragraph, the wallet "business model" is tricky, with end users not wanting to pay for the service, and issuers, verifiers and privacy regulators extremely leary of any sort of "exploitation"-based business model. As such collaborating on a wallet, sharing the one-time engineering costs, can result in a more capable wallet, while enabling the powerful, verifiable credential-based use cases we all want to see.

Next, since many of the wallet vendors are only creating wallets for their specific use cases, they often may not be able to afford to investigate and implement features that work beyond their use cases. For example, features such as internationalization, staying up to date on evolving Aries protocols, messaging, adding new verifiable credential formats, using the wallet as a verifier, and defining new protocols to enable fantastic wallet user interfaces are often not top of mind. Open source wallet projects allow multiple organizations to collaborate on those issues versus each spending the same money working independently on their wallets.

A final reason we’ll cover that argues for open source collaborations for wallets is the tendency for early players in the field to embed wallet functionality in their existing apps, so their users have a familiar (and branded) place to use their credentials. While this may work for a user’s first experience with verifiable credentials, it precludes the ability to combine credentials into a single presentation, and could make things confusing for users ("which app is holding credential x???"). While we need ways to enable great user experiences for the clients of businesses, we’d really like to do that with a single credential and keys store.

Our advice if you are looking to provide a wallet for your business is to join one of the open source wallet communities, and contribute to what is being created there. Don’t go it alone!

Let’s look at the open source wallets that are the most promising as this course is updated in mid-2021.

### Aries Mobile Agent - Xamarin ("Aries MAX")

The Aries Mobile Agent-Xamarin ("Aries MAX") was started as a combined effort of teams from [trinsic.id](https://trinsic.id/) and [Mattr Global](https://mattr.global/) in 2019 (eons ago in SSI time!). The repo for Aries MAX can be found here. Aries-MAX is based on the [aries-framework-dotnet](https://github.com/hyperledger/aries-framework-dotnet)—the same library that powers the Trinsic, estatus, LISSI and IdRamp commercial mobile agents, and the Trinsic Studio that we looked at earlier in the course. Aries MAX wraps the framework code with [Xamarin](https://dotnet.microsoft.com/apps/xamarin), Microsoft’s open source development platform for creating mobile iOS and Android applications.

Aries MAX provides a mobile agent and a basic cloud mediator agent suitable for local deployments. Further, given the maturity of its aries-framework-dotnet foundation, Aries-MAX is a complete open source component for mobile wallet deployments, handling AIP 1.0 connections, receiving credentials and proofing claims and more.

A stumbling block for some teams getting started with a mobile agent is Aries MAX’s C#/.NET and Xamarin basis. That’s a non-starter for some mobile teams looking to use a framework such as [React Native](https://reactnative.dev/) for mobile development. And that’s where our next open source mobile option comes into play.

### Aries Mobile Agent - React Native ("Bifold")

A second Aries open source wallet project is known by its long name (the one used for its GitHub repository) "[Aries Mobile Agent React Native](https://github.com/hyperledger/aries-mobile-agent-react-native)," and its short name "Aries Bifold" (as in the name for a type of physical wallet). In this course, we’ll go with the short name. As with Aries-Max, Aries Bifold, is built on a major (although quite a bit newer) open source Aries Framework called Aries Framework JavaScript. Given the JavaScript basis of the [React Native](https://reactnative.dev/) mobile framework in Aries Bifold, it’s no surprise that the JavaScript Aries framework underlies Bifold. As well as Aries Framework JavaScript, a second dependency of Bifold is the React Native Indy SDK ([rn-indy-sdk](https://github.com/AbsaOSS/rn-indy-sdk)), providing the Indy ledger and AnonCreds capabilities. The following are a brief summary of each of the Aries/Indy repositories that make up Aries Bifold.

The Aries Framework JavaScript project has been around since late 2019, evolving slowly, at first powered by a small team of maintainers. That pace picked up significantly when the Aries Bifold project took off and its team realized that Bifold could really benefit from the work already completed on Aries Framework JavaScript. At the time this course was updated (mid-2021), development on Aries Framework JavaScript is fast-paced, with build tags being applied about every second day. Major short-term roadmap items (full AIP 1.0 support, standard mediator protocols) are being delivered and attention is moving on to cover both the needs of Aries Bifold (enabling a great mobile experience) and adding AIP 2.0 functionality, such as support for holding and proving W3C standard JSON-LD verifiable credentials.

Another nice benefit of using Aries Framework JavaScript is that it has an Aries Agent Test Harness backchannel, and so is being tested daily against the other Aries frameworks.

The second Aries Bifold dependency open source project is a React Native library for Indy ledger and AnonCreds functionality—[rn-indy-sdk](https://github.com/AbsaOSS/rn-indy-sdk). This project is a contribution from Sovrin Steward and longtime Aries contributor [Absa Bank](https://www.absa.co.za/personal/) in South Africa.

The React Native Indy SDK is an implementation of an indy-sdk wrapper for React Native, much like there are for other languages—Python, C#/.Net, Java and so on. While that is a step back from an Aries component, it addresses one of the more challenging parts of building an Aries mobile agent—getting the low-level cryptography dependencies packaged for use in a React Native app. With an Indy SDK in place, the Indy-related agent capabilities (key generation, signing/verifying, encrypting/decrypting, etc.) in Aries Framework JavaScript can be invoked in a React Native environment. Another key building block!

Which of course brings us to the Aries wallet part of the project, the Aries Bifold repository itself. The repository pulls in the dependencies (Aries Framework JavaScript and the React Native Indy SDK), adds in the React Native framework, and then a wallet mobile experience. From just the components in the Aries Bifold repository, a developer can run a simulation of an Android version of the app on their laptop/desktop, and can build and load onto mobile devices Android and iOS versions of the wallet.

## Lab: Open Source Mobile agent projects

In this lab, we’ll take a look at the state of open source mobile agents in Aries. The lab involves running a development instance of the wallets on a simulator on your development system (a laptop or desktop). Since the simulator can’t be run using Docker, it’s a little more effort to set up a development environment—especially if you are new to developing mobile apps.

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/MobileAgentLab.md) to go to the lab.


## Knowledge check 7

1. Mobile agents can use notifications as their endpoints for messages from other agents. True or <mark>False?</mark>
2. Mobile agents need mediator agents to receive messages sent by other agents. <mark>True</mark> or False?
3. From a routing perspective, the important information in the DIDDoc is (Select all answers that apply):
  * <mark>The public keys for agents referenced in the routing</mark>
  * <mark>The service element of type did-communication</mark>
  * Message encrypting
  * <mark>`routingKeys`</mark>
4. A DIDDoc is established when Alice and Bob agents connect. True or <mark>False</mark>?
  * False. When Alice’s and Bob’s agents establish a connection, two DIDDocs are created, one by each of Alice and Bob (i.e. two DIDDocs per relationship). *Stupid question...*

## Summary chapter 7

Routing is a pretty intense topic, eh?! There is a lot going on when it comes to Aries mobile agents. The key take-aways from this chapter are:

* Agents are often not directly connected, and in fact, sometimes cannot be directly connected (for example, mobile agents).
* Mediator agents play a key role in Aries agent routing and are required for Aries mobile agent apps. If you are developing mobile apps, you will need to consider how you will deploy your cloud-based mediator agent.
* An agent provides instructions (in the DIDDoc) for how another agent should be routed to it during connection establishment.
* While you might be tempted to build your own mobile agent, before doing so consider instead sharing the effort, by contributing to and deploying an open source wallet.
* There are several open source mobile wallets that can be used as starting points for deploying your own mobile agent.

# Chapter 8

Now that you have explored building your own Aries controller or mobile wallet, let’s talk about actually getting it into production. This chapter describes some of the things you need to be aware of in the production context, things that are important to the Aries environment. While the vast majority of the content of this chapter will be in the context of enterprise versus mobile agents, we do touch on mobile agent production as well. Important stuff!

In this chapter, you will learn about:

* Mobile agent deployment challenges such as pushing an app to the app store.
* Keeping ledgers and agent storage in sync.
* Writing to a sandbox ledger versus a production one.
* Managing writes to production ledgers, especially permissioned ledgers and those that charge for writes.
* Considerations for the management of your agent, such as backup and restore.
* Horizontal scaling of high-volume, enterprise issuer agents.
* Running many logical agents in a single deployment—multi-tenant deployments.
* Some advanced Aries framework capabilities that are important for some use cases.

## Production challenges - mobile wallet apps

The bulk of this chapter will be about production issues around enterprise agents, particularly issuer agents. But we’ll first touch briefly on mobile wallets and the challenges they bring.

**NOTE**: *We are not experts in mobile development so these are based on our observations and experiences only.*

The biggest challenge with Aries mobile wallets is likely the same with all mobile apps—agent distribution. In particular, there are two issues related to the distribution of new versions:

* Dealing with the app stores’ release processes—getting each release of your app through the gates that Apple and Google define.
* Getting users to upgrade to the latest version so that you can drop support for deprecated features in older versions.

For those of us that have grown comfortable (and complacent) in deploying web services that have but a single deployment, having to distribute and upgrade apps on (hopefully) millions of mobile phones creates a much bigger backwards compatibility problem. Sure, with a web service we have to make sure that the web API provides backward compatibility, but that’s a more manageable problem. As well, while Google and Apple are aggressive in pushing users to update their apps on a more or less continuous basis, it’s still a challenge to keep things working across a range of "stable" releases that users might be running.

## Community upgrade process

This backwards compatibility challenge is exacerbated in the Aries world because you want your mobile wallets integrating with a range of other agents. We’ve talked a lot about interoperability and the Aries Interop Profile (AIP) in Chapter 5. Having agents (mobile and server based) that align to AIP versions and are tested for interoperability is crucial for the community of agent builders. For these applications to succeed, they all have to be able to work together!

A second mechanism that is important for managing upgrades in Aries is the "Community Coordinated Update," as described in [RFC 0345](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0345-community-coordinated-update). <mark>This mechanism is used when the Aries community agrees on the need to make a breaking change to the protocols agents are using</mark>. We want such changes to be made carefully so that each agent maker can make the changes independently, and all agents will continue to interoperate throughout the transition. With RFC 0345, we have a template process that coordinates a breaking change using a series of steps that gives time for (hopefully) all deployed agents to be <mark>updated without breaking agent-to-agent interoperability</mark>.

Mobile wallets have to be particularly aware of these community coordinated updates because not only do they have to make the changes in their code, but they have to get a new version with that code change distributed to (again, hopefully!) millions of users. To enable global interoperability, it’s not sufficient that only creators of Aries components (especially frameworks) communicate about updates, but for mobile wallet operators to let the rest of the community know about their status—what protocol changes they have made, what protocol changes are in the works, and what’s their latest on interoperability.

## Mobile wallet interoperability testing

How can a wallet publisher let others know about their interoperability profile? We’ve talked earlier about the Aries Agent Test Harness (AATH, covered in Chapter 6), an automated tool for testing the interoperability of Aries Test Agents, such as Aries frameworks, and the special AATH Mobile Backchannel. Although a manual process, it does let the maintainers of the open source wallets determine their wallet’s level of interoperability, and lets users of deployed open source wallets see that for themselves. Two things that would be really nice to have in AATH are the following:

* An extension to the Mobile Backchannel to allow formal reporting of tests that were run (what wallet was tested, and what tests passed/failed).
* A way to run the tests automatically for a given mobile wallet on a periodic basis (e.g., daily)

If you have ideas on how to make that happen, please connect (on Hyperledger Chat) with the AATH developers!

## Deploying a mediator or not

Beyond deploying the mobile agent to the mobile OS App stores, each mobile wallet will need (as we’ve discussed in Chapter 7) a mediator to at least serve as the target physical endpoint for other agents sending messages to the wallet. The mediator may do other things as well for the wallet, such as act as an outbound relay, manage wallet backup and restore capabilities and so on. It’s up to you, the mobile wallet provider, what you want the mediator service to do—beyond the minimum of routing messages. You have several options for providing a mediator, so let’s look at what’s possible.

A (relatively) simple option is to deploy an instance of ACA-Py (or another Aries framework) that supports the Aries mediation protocols, notably [RFC 0211](https://github.com/hyperledger/aries-rfcs/tree/main/features/0211-route-coordination) Route Coordination and [RFC 0212](https://github.com/hyperledger/aries-rfcs/tree/main/features/0212-pickup) Pickup. Your mobile wallet app must support those same Aries mediation protocols, but as a client. Any other features you want to add to improve your app user’s experience would be added as part of the controller. Of course, while configuring an instance of ACA-Py to use for mediation is pretty easy for testing, setting it up to support millions of mobile wallet apps (because that’s your goal, right?) is a little more challenging. The good news is that we cover deploying scalable, server-side Aries agents (which is what a mediator agent is) in the next section of this chapter.

A second option is to write a custom mediator "agent" that works with the mobile wallet app you will deploy. Since a mediator doesn’t do a lot of agent-y things, using an Aries framework as the basis of a mediator may not be a huge advantage, and it may make more sense to write your own component that handles all the other features you want (perhaps some services for which you can charge), and includes the minimum Aries protocol support to be a mediator. The API between the wallet and the mediator can be custom between the two, or you can implement the two mediator protocol RFCs referenced above. This "custom mediator" approach was implemented successfully in the early AIP 1.0 Aries wallet apps and is definitely a viable strategy.

With the advent of the Aries mediation protocols RFCs linked above, a third option has become possible—use a third-party mediator. Anyone can set up a mediator that uses the mediation protocols and serve as a mediator for any mobile wallet app that supports those protocols. Indicio.tech has done just that. Check out https://indicio-tech.github.io/mediator/, where a public mediator has been made available that anyone can use. Your wallet app could use a specific public mediator, or you could even make it possible for your users to pick their own mediator.

Lots of options! We recommend using the easiest option you have first, learning more about the problem and then making a decision on the best approach for your use case. The easiest is probably the use of the public mediator if your wallet supports the mediator protocols, or a simple custom mediator if not.

## Mobile wrap up

That’s all we have for mobile agents. The rest of this chapter is largely focused around challenges with running enterprise agents in production. We’ve had our fair amount of experience (and battle scars) in dealing with production use cases and we hope the things that we’ve learned along the way will make it easier for you. Certainly this is not an exhaustive list, but it will give a good starting point. You’ll know a lot more than we did when we got started!

## Working with production Indy ledgers

This section mostly deals with working with Indy production ledgers, such as Sovrin MainNet. Most of this section also applies to the test networks made available by organizations such as Sovrin that operate production ledgers. While to this point in the evolution of Aries, the focus has been on the use of Indy ledgers, we’ll also touch briefly on impacts of using non-Indy ledgers.

### Sandbox versus production-secure storage handling

As you develop your first Aries controllers and deploy your first proof-of-concept using a sandbox ledger, you can get quite complacent in managing the ledger and your agent’s secure storage. If things aren’t working, it’s easy to just entirely reset a sandbox ledger, reset your agent(s) storage, and start again. In fact, particularly during development, it’s often the case that things unintentionally get out of sync between the objects on the ledger and what’s in agent secure storage. When that happens, you are forced to delete everything and start again. That’s OK during development, but in production, you won’t have that luxury—you can’t afford to lose your secure storage!

Some production ledgers (for example, Sovrin production MainNet) there is a fee for writing to the ledger and you don’t want to pay more than you have to. Even more important, if you are an issuer of credentials, if you lose access to your secure storage, you won’t be able to issue credentials based on the objects you have already put on the ledger. The simple reality is that once you are in production, you must remember that your storage is a production database that must be handled appropriately—back it up, be able to restore it and always, always keep it secure. On that last part, security: you don’t want someone to steal the keys in your secure storage and be able to act in your place!

### Agent Storage Backup and Restore

As was covered extensively in the prerequisite for this course ("Introduction to Hyperledger Sovereign Identity Blockchain Solutions: Indy, Aries & Ursa" ([LFS172x](https://www.edx.org/course/identity-in-hyperledger-aries-indy-and-ursa))), backup and restore of agent secure storage is crucial to the long term usefulness of this technology. While that may be less important in the early days for mobile wallets (users do not write to a public ledger and should be able to get credentials easily reissued), backup and restore for enterprise Aries agents is crucial from day one.

Fortunately, unlike the mobile use case, backup and restore for the enterprise use case is a well understood problem. Enterprise agent storage should use an enterprise database, such as PostgreSQL, and all of the tools and techniques that have been developed over the years to manage such an enterprise database. There should be nothing special about enterprise agent secure storage from a database perspective than any other enterprise database. As such, the following guidelines apply:

* Backup the data regularly, either fully or incrementally using logs, depending on the use case.
  - The specific approach taken depends on the answer to the question: how much data can you afford to lose if the operational database is lost and you must restore it from a backup?
  - Consider using a cluster database with replicated storage to have continuous data redundancy.
* Test the viability of the backups regularly.
  - Run frequent test restores to verify the integrity of the backup.
* Define and periodically execute a full agent disaster recovery plan.
* Ensure that the backups are maintained in a secure location, and protect both the database and encryption keys for the secure storage.

**NOTE**: *The vast majority of the data in the agent storage database is encrypted and the encryption keys for the database must be appropriately managed. <mark>This includes ensuring that the keys are stored separately from the data</mark>, they are protected from accidental disclosure, and they are accessible when necessary for starting the agent and after a database restore. Techniques using tools like [Hashicorp’s Vault](https://www.hashicorp.com/products/vault) for managing the agent storage keys might be a good approach. Again, none of these requirements are different from any other enterprise database system.*

Regarding the "how much data can you afford to lose" question, remember as you consider that question, that in addition to the ledger objects, agent secure storage holds information about the connections your agent has established (with your private key for each connection) and protocols that are "in-flight."

## Indy ledger writing

While Indy ledgers are pretty easy to write to (there's an API call for that!), there are a couple of added governance related complications that need to be handled in using most production and some test Indy ledgers, the "TAA" and "Endorsers." We’ll go through each of them next.

### The Indy Transaction Author Agreement (TAA)

The Indy Transaction Author Agreement (TAA) is an optional agreement between a party that is writing a transaction to an Indy instance and the operators of that instance. If the ledger is configured to use a TAA, the API call that is made to write to the ledger must include information that amounts to the transaction author (you) saying "I agree to the TAA"—just like the "End User License Agreement" (EULA) that most software and websites require you to acknowledge. In the case of an Indy write transaction, there are three fields that are added to each Indy write transaction that the ledger will check before processing the transaction if the TAA is enabled:

* The time the TAA was accepted.
* The hash of the current TAA.
* An enumerated value indicating how the TAA was accepted.

From a legal perspective, your organization must review and accept the TAA before writing to the ledger. That means, getting the TAA from the ledger operator and making sure your organization understands the legal implications of, and abides by, the agreement. The TAA can be retrieved from the ledger (for Sovrin MainNet it’s [here](https://indyscan.io/tx/SOVRIN_MAINNET/config/9364) on indyscan.io) and likely stored in a more readable form on the Indy ledger operator’s site, such as [here](https://docs.google.com/document/d/1wcSESHbqU6OCOG1g1q5TrIkRehJZ38PtjWkFkLxg-Dc/edit) for Sovrin MainNet.

From a technical perspective, an agent writing to the ledger that has an active (non-empty) TAA needs to get the hash of the current TAA from the ledger, and a list of configured values for how to accept the TAA. Once again, that information is stored on the ledger, this time in the `TXN_AUTHOR_AGREEMENT_AML` configuration transaction, such as here for the Sovrin MainNet. "AML" means "Acceptance Mechanism List," and it’s a list of the name/value pairs for how an organization can accept the TAA, plus the hash of the TAA. So, to write to the ledger, an agent must:

* Get the AML transaction from the ledger.
* Have the (human) controller select a method from the AML to use to accept the TAA.
* Include the method selected, the hash from the AML data, and the current timestamp in any ledger write transaction.

Fortunately, most Aries frameworks make it easy for you to (technically) accept the TAA on every write. For example, Aries Cloud Agent Python has an Admin API call to read the AML data, and another, `/ledger​/taa​/accept`, that lets you indicate how you accept the TAA from the list of options. After execution, ACA-Py automatically adds TAA data to every write. By adding that call to the controller initialization code, you no longer have to worry about the TAA from a technical perspective.

However, remember that’s only the technical handling. Your organization should definitely do a legal review of the TAA itself and make sure that it is willing to abide by the terms of the agreement before adding that call to the controller. And of course, you must also be certain that what you are writing to the ledger doesn’t break the agreement that you’ve agreed to.

## Lab: Handling the TAA In ACA-Py

In this lab, we’ll activate the TAA on a local instance of the VON-Network, and then run some API commands to retrieve the TAA Agreement AML, and use it to accept the TAA.

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/TAALab.md) to run the lab.

## Endorsing transactions

The second governance challenge in writing to a production ledger is having permission to do so. Not just any transaction author can write a transaction to an instance of a ledger, even if they have agreed to the Transaction Author Agreement. While the permission rules are configurable per Indy instance, on the Sovrin ledgers at least, writing requires the participation of a "Transaction Endorser." An endorser is an organization that has submitted and had accepted an application to be an endorser, has (physically/online) signed legal agreements about being an endorser, and has had a DID with the "endorser" role added for them on the ledger. Once an organization is a transaction endorser, they can:

* Write objects on the ledger with their endorser DID.
* Sign (with the private key from their endorser DID) write object transactions created by authors (non-endorsers) so that the transactions are processed by the ledger instance.

The ability to sign transactions of other authors is an interesting capability because it can be used in several ways.

* An organization with just one issuer wouldn’t use the signing capability, and it would just write any required transactions directly using its endorsed DID.
* For a large enterprise, like a government or multinational, the enterprise might have one endorser, and all other issuing agents in the organization are "authors" that request the endorser sign their transactions. That keeps both the control and billing for the writes centralized in the organization.
* An "SSI-as-a-Service" (SSIaaS) company might provide services for other organizations that include signing any write transactions needed for their customers to be verifiable credential issuers. The SSIaaS company would have to pay the ledger operator (for example, the Sovrin Foundation) for the objects written to the ledger, and could charge their clients whatever they want. The Trinsic Studio (that we’ve referenced before) Agent-as-a-Service handles endorsing transactions for their clients.

When transactions are written to the Sovrin ledger (for example), it’s not the author that has to pay for them, it’s the endorser; the organization that has the endorser legal agreement with the Sovrin Foundation. And, while the author must still adhere to the TAA (above), the endorser must ensure the endorser legal agreements are adhered to by both the endorser and the author, if those are different entities.

Let’s take a look at the details of signing a transaction manually in a lab, and then we’ll cover the automation available in Aries frameworks to handle endorser signing.

## Lab: Scripting Indy Production Writes

In this lab, we’ll look at the manual process (using the Indy CLI) for authoring, endorsing and writing transactions to a ledger that is fully permissioned, such as Sovrin MainNet.

Click [here](https://github.com/cloudcompass/ToIPLabs/blob/master/docs/LFS173xV2/ProductionWrites.md) to run the lab.

## Automating Indy Endorser Transaction Signing

In the first few times our team used Sovrin MainNet, we completed the legal steps to be an endorser and created an endorser DID to write any other ledger objects we needed. That meant that we didn’t need to use the author-endorser signing process. Even when we started using authors, we just used the Indy CLI (manual) process to create, get endorsed, and execute the few transactions we needed for issuing credentials. Who needs automation?

Well...that’s a lot of overhead at best, and unsustainable when you get to use cases requiring verifiable credential revocation. How do we make the process a lot easier for all parties involved? As of mid 2021, the process is becoming automated. Let’s go over what has been done so far, and what’s left to be done.

Work to automate the process started with the definition of an "endorser signing" protocol. Like all Aries protocols, it defined the roles of the participants, the messages and the states of the protocol. It allowed an author (a protocol role) to request a transaction be signed, and an endorser (another role in the protocol) to sign the transaction and either execute it (submit it to an Indy ledger) or pass it back to the author to be submitted to the ledger.

Next up was implementing the protocol, which was done by [AyanWorks](http://www.ayanworks.com/) as a contribution to Aries Cloud Agent Python. The implementation extended the data associated with connections to include the role of a connected agent and specifically, if it is the endorser for this agent. As well, the Admin API was extended to indicate when a "create" operation required an endorser’s signature.

As this course is being updated (mid 2021), two items remain on the roadmap for this capability in Aries Cloud Agent Python.

* While the current ledger write-by-write control of endorser signatures gives us the most flexibility, the vast majority of use cases will be all or nothing—either all the ledger writes have to be signed by an endorser, or none of them do. So, instead of the controller having to ask (or not) on every ledger write, we are changing the implementation to have a single (command line) configuration that controls if an endorser signature is needed or not, who the endorser is, and applying it on all transactions. That way, the controller won’t need to do anything beyond configuring the ACA-Py instance—the framework will take care of everything!
* On the endorser agent side, we need a controller to accept endorser requests and to decide if they are to be processed or not. Such a controller could do that all with business logic, such as a fixed "allow list" of agents that can have their transactions signed, or perhaps a UI that allows a person to decide to sign (or not) an endorsement request.

As this capability is evolving in Aries Cloud Agent Python and may be implemented differently in other Aries frameworks, be sure to look at the framework documentation to get the latest.

## Ongoing Ledger Writes: Handing Revocations

Before we complete this section, we want to cover revocation from an issuer perspective, and particularly Indy AnonCreds revocation. Issuer revocation handling in AnonCreds is pretty tricky, with several extra challenges. The good news (at least if you are using Aries Cloud Agent Python) is that the framework takes care of most of those extra challenges, leaving you with just some extra configuration.

We’re not going to cover much on how Indy AnonCreds works, but rather provide a little guidance on what extra things you, as an Aries developer, need to do when including support for revocation, and what support ACA-Py (and perhaps other Aries frameworks) provides for handling revocation. If you are interested in how revocation works, please checkout this [article](https://github.com/hyperledger/indy-hipe/tree/master/text/0011-cred-revocation) that is part of the Indy SDK repository.

The first complication with AnonCreds revocation is the need for the issuer to publish the (infamous) tails files. A tails file must be published by the credential issuer such that every credential holder (such as a mobile wallet app) can retrieve it. In theory, it can be posted anywhere (e.g., on a file server), but for it to be read on a mobile wallet app, the file must be retrieved from an SSL certificate protected (https:) server (e.g., it must be encrypted). This is enforced by iOS and Android. To make it (relatively) easy to get started, the BC Gov team has implemented a ["Tails Server" open source repository](https://github.com/bcgov/indy-tails-server) that you can deploy yourself, and they have deployed a public instance of it at https://tails.vonx.io/. The 404 you get from following that link is expected so that you can "just use it."

A second complication is that AnonCreds revocation registries are relatively small, limiting the number of credentials you can issue per registry. When you run out, you have to create a new registry. That’s pretty painful! Fortunately, Aries Cloud Agent Python takes care of that complexity around revocation registries for you. When the controller indicates that revocation will be used when creating a credential definition, ACA-Py immediately creates two revocation registries, and starts using one for issuing credentials. When that registry runs out of credentials, ACA-Py switches to use the other one, and automatically creates another registry. This means you, as a controller writer, don’t have to worry about revocation registries at all. ACA-Py makes sure there is always an active and ready-to-go registry on hand!

From an issuer controller perspective, here’s what you have to do when using AnonCreds revocation with ACA-Py:

* Deploy your own, or use a public tails server. When starting ACA-Py, pass in the URL of the tails server as a command line option. ACA-Py will take care of publishing the tails file for each revocation registry created and making sure that the location of the tails file is published as required to the Indy ledger you are using.
* When creating a credential definition, indicate that revocation is to be supported and the size (number of credentials) in each revocation registry. The size will be a tradeoff between the number of registries needed (fewer is better) and the tails file size (smaller is better). This is why tails files are infamous...
* <mark>When issuing a credential, save the ID of the credential (associating it with the holder) such that it can be used later for revoking the credential, if necessary.</mark>
* <mark>When required, revoke credentials using the saved ID for the credential. This may (or may not) trigger the publishing of the revocation to the Indy ledger.</mark>
* Trigger the publishing of revoked credentials. That can be done in conjunction with revoking a credential, or periodically, to publish all of the revoked credentials since the last publication.

By the way, all of this complication is on the shoulders of the issuers. A holder’s job is much easier. As long as the path to the tails file is accessible, <mark>all of the revocation handling is within the Indy AnonCreds code, so there is not a lot that a holder agent’s controller needs to do. Oh wait, maybe one thing. After creating a presentation that involves a revocable credential, the controller might want to check if the credential has been revoked.</mark> If it has, perhaps the presentation shouldn’t be sent to the verifier...

A last issuer concern to touch on is how often you should publish revocations to the ledger? On public Indy ledgers such as Sovrin’s MainNet there is a cost associated with writing a revocation entry (as they are called on Indy) to the ledger. It’s not much, $0.10US right now, but it’s something to consider. At the same time, your verifiers may need to know as quickly as possible if a credential has been revoked. What’s the best approach? Well, it depends...

Revocations could be written as needed (whenever they occur), or written periodically. For example, consider a government that issues driver’s licenses as verifiable credentials that needs to revoke existing credentials when driver’s license data changes—address, class of license, the right to drive, etc. In that use case, it’s likely acceptable to write an update to the revocation registry daily that includes all the revocations that have occurred since the last update. For a large population, the number of daily revocations might number in the thousands. Such an issuer might also define a class of "high importance" revocations (for example, loss of right to drive) for which the issuer wants to support immediate revocation. On the other hand, issuers that rarely revoke credentials (for example, monthly) could work either way, revoking credentials as they happen, or batching them into periodic writes to the ledger.

The most important thing you need to take from this section is that if you are using the Indy ledger, there will likely be legal and technical requirements around writing data to the ledger. That means that you have to investigate the governance around the ledger you are using to understand that governance, and the legal and technical requirements you will need to address. Once you know those requirements, look at the Aries framework you are using and understand the features of the framework that allow you to manage your Indy ledger writes.

We also covered revocation with Indy AnonCreds and the complications it brings—and the support that Aries frameworks provide to hide those complications from controller software. That makes your job a lot easier!

Currently, writing to ledgers (or just publishing DIDs) from Aries agents is mostly limited to Indy. As support in Aries expands to include writing to other types of ledgers beyond Indy, these types of governance issues will need to be addressed, leading to additional technical requirements on Aries agents.

## Horizontally Scaling Enterprise Aries Agents

For enterprise agents, an important production issue to consider is horizontal scaling—the ability to increase the capacity of an enterprise agent by adding more agent instances. While your use cases may start out small, it's a good idea to think about how you will scale up your capacity as demand grows for the services offered by your agent.

This section draws mainly on the experience of the Verifiable Organizations Network (VON) team at the Government of British Columbia. We won’t go into the business case behind OrgBook BC and its code base (you can read about it [here](https://vonx.io/), if you are interested), we’ll just look at its verifiable credential processing requirements.

The VON team has implemented two generations of agents in its OrgBook BC issuer and community holder. For the OrgBook initialization use case, an enterprise issuer must issue millions of credentials to a single enterprise holder as quickly as possible. That sheer volume of credential issue events requires the scaling of the solution to use as much processing power as is available. The architecture of the solution is pictured below.

<img src = 'https://courses.edx.org/assets/courseware/v1/73099ae7e59be9f966693668084bb8de/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-31.png' width = 350>
<br>
<br>
The following is a summary of the architecture:

* ACA-Py instances are the orange boxes—issuer and holder agent frameworks.
* The controllers are in green, each controlling one of the ACA-Py instances.
* The databases labelled "Secure Storage" are agent secure storage, holding the keys, ledger data, connections, credentials and protocol state objects.
* The issuer controller monitors the source system for "events" that trigger credential updates—issuing or re-issuing credentials. It invokes the issuer ACA-Py instance to issue the verifiable credentials to the Credential Registries agent using the "Aries Issue Credential V2" protocol ([RFC 0453](https://github.com/hyperledger/aries-rfcs/tree/main/features/0453-issue-credential-v2)). The controller keeps track of the events processed and credentials issued in the Event Tracker database.
* The holder ACA-Py agent framework instance receives issued credentials, stores them in its agent secure storage and notifies its controller about the credentials.
* The holder controller extracts out the claims from the credentials and stores some of the claims in the search database.
* HTTP is used for transport between all of the controllers and ACA-Py instances.
* All of the databases (both Secure Storage and the Search Database) are all PostgreSQL instances.

The design (hopefully) looks pretty straightforward, and the operation is pretty simple: use Aries protocols to connect two agents during initialization, and then issue credentials from the issuer to the holder. The challenge is the volume of credentials to be issued. For launching OrgBook BC, our requirement was to issue 2.5 million credentials as quickly as possible, ideally within hours, and then continue to issue credentials at a steady pace of thousands per week. Thus, we want to scale up for the initial load, using every bit of computing resource we can find, and then scale down to a pretty low steady state.

The OrgBook BC use case is somewhat different than might be seen for any high volume issuer. In a more typical case, instead of one holder, the issuer would be connecting with tens to hundreds to thousands of holders simultaneously, issuing credentials to each. Regardless, the need for horizontal scaling to adapt to loads as they happen is needed in each case.

For this scenario, we want to be able to run the agent on a platform that supports (ideally automated) scaling, such as [Kubernetes](https://kubernetes.io/). As the load on the agent increases (more requests), more processes (containers) are added so that we have more available issuing capacity. When requests drop, some of the containers stop and capacity decreases. To enable auto-scaling, [cloud native approaches](https://www.cncf.io/) must be applied. In the case of Aries, that means the controllers and agent framework components must be stateless—they must operate without holding state in memory. The multiple agent (framework and controller) instances operate as a single logical agent, with the combined capability of all of the instances looking to the outside world as a single agent.

<mark>All of the agent framework (ACA-Py in this case) instances persist data to a single logical secure storage instance</mark>; for example, a single PostgreSQL database. Ideally, that database would be implemented as a cluster, so it too supports high availability and scalability. Likewise, the scalable controllers likely have their own persistence, separate from the Aries secure storage.

As we have seen in this course, both Aries controllers and agent frameworks (such as ACA-Py) operate an event loop (waiting for an event, processing it, and responding to the event), just like any web service. To make Aries components stateless, event state must not be held in memory, but rather persisted to shared storage when completing the processing of an event. With that, any number of instances of the agent components can wait on events, retrieve the state information associated with the event and process it. Since it is likely in enterprise scenarios that all the transports to, from and within the agents are HTTP, load balancers can be run in front of the components to distribute the load across all available instances.

Agent frameworks such as ACA-Py have been built with this stateless requirement in mind. As you build your controller, you should also try to meet this requirement. Define a clean controller event loop that includes both retrieving state from shared storage at the start of processing an event and persisting state to that same shared storage at the end of processing. Do not maintain state outside of event processing.

## Agent Instance Initialization

With horizontal scaling, you will have instances of the agent starting and stopping regularly, and initializing those agents must be managed properly, particularly as it relates to objects written to the ledger. A naive approach to starting every instance of an agent might be to follow the pattern in the image below.

<img src = 'https://courses.edx.org/assets/courseware/v1/debd3c1e61d62e56652d53d150f3bc77/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-22.png' width = 300>

While that will work in most cases, since there can be multiple agent framework instances running concurrently, what happens if multiple instances start up in parallel, each sees in step 2 that some ledger objects don’t exist, and each tries to create them?

The maintainers of ACA-Py have attempted to address this and related initialization concurrency issues by having agents run in one of two modes: provisioning and operational (called "start"). In provisioning mode, the agent starts up, checks if all of the resources that an agent needs have already been provisioned and if not, it creates them. This includes creating and initializing secure storage, creating any ledger objects required (for issuers), and writing them to the ledger. Once that is done, the agent exits, its work done.

In ongoing operation, the operational mode is used. The agent starts up checks if all of the resources that an agent needs have already been provisioned and if not, exits immediately with an error. However, if the provisioning has been done, off it goes to do its work.

We recommend that you stick with this pattern as you deploy your agents. It allows you to limit the use of higher level permissions (e.g., to create the secure storage database, and write to the ledger) to provisioning mode and running agents with less authority in operational mode. Note that if you are using revocation, your operational agents likely will need the ability to write to the ledger, as new revocation registries will need to be created from time to time.

We don’t have a lab related to horizontal scaling, but if you think you will need to know more about this topic, we recommend that you keep handy this link to the [BC Gov’s OpenShift Deployment Configurations repository](https://github.com/bcgov/orgbook-configurations) to use when you need it. OpenShift is Red Hat’s distribution of Kubernetes, and the repository will give you some guidance as you deploy agents to a Kubernetes platform.

## Multi-Tenant Aries Agency

We’ve covered how to deploy a single agent (one framework with one controller) a number of times in this course—Faber the issuer and verifier, Alice the holder, Acme the verifier. That’s all good, but what if you want to operate an agent service with thousands (or millions!) of agents? For example, you want to empower many small businesses to become verifiable credential issuers. Or perhaps you want to create "custodial wallets" for lots of holders, wallets that aren’t stored on a mobile device, but that are accessed through a web app. As we’ve discussed earlier in the course, it’s not an ideal approach, but it may be the "best possible" approach in some cases.

In the early days of ACA-Py the answer to that need wasn’t great: "create a new deployment of a full agent using a different URL and use different ports." What a pain! Fortunately, a better answer was possible, and [SICPA](https://www.sicpa.com/) from Switzerland contributed the code and documentation in ACA-Py to provide multi-tenant mode.

In multi-tenant mode, a single instance of ACA-Py is deployed, usually using the horizontal scaling techniques talked about in the previous section. When the service starts, a base instance of secure storage is created with limited agent capabilities, but with the added administrative power to be able to dynamically create new "tenants," a new agent within the agency. A new tenant is really just three things:

* A JWT authentication token given to the controller for the new tenant agent.
* A new unit of secure storage ("sub-wallet"), a new database for the exclusive use of the new tenant.
  - This means that within the database server (e.g., Postgres) the `CREATE DATABASE` command is executed, and new tables are created within that new logical database.
* An entry in the base secure storage ("base wallet") with information about the new agent.

**NOTE:** *In the ACA-Py multi-tenant documentation, the term "wallet" is used for what we call secure storage in this course.*

Since the ACA-Py framework is stateless, those three elements are sufficient to make an entirely new agent framework within a single instance of ACA-Py. From outside of the agency, tenants look exactly like any other Aries agent. Likewise, the controller’s administrative API for the tenant framework is exactly the same as for a standalone agent framework, allowing the execution of all of the same Aries protocols. Since the overhead for each tenant is minimal, the solution can scale to the limits of the underlying platform (compute and storage), supporting as many tenants as needed.

The base secure storage plays an important role in multi-tenancy, identifying for each request the tenant to which it applies and providing access to the tenant’s storage. In most cases, the base storage holds the decryption key for the tenant storage, although the multi-tenant capability can be configured such that the decryption key must be passed in and thus never stored in the agency. The role of the base storage can be seen from the drawing below, which shows how messages from external agents are received by the base storage ("wallet" in the drawing) and, based on the intended recipient, passed on to the right tenant.

<img src = 'https://courses.edx.org/assets/courseware/v1/f61929368a596cc4cd6020de67deb5f2/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS173X_CourseGraphics-17.png' width = 700>

Requests from the controller must include the issued JWT for the tenant attached, allowing the ACA-Py instance to find and access the secure storage for the tenant to process the request. Webhooks from the tenants can be configured per tenant, allowing each to go to a different URL.

That’s all we’re going to cover about multi-tenant mode—a little taste just to let you know the feature is available should you need it for your use case. It’s a pretty advanced feature, and many Aries developers won’t need it—until they do! Want more information about it right now? Check out this [document](https://github.com/hyperledger/aries-cloudagent-python/blob/main/Multitenancy.md) in the ACA-Py repository.

## Advanced Capabilities

Skip to main content
In this final section of the chapter we’ll list a couple of pretty advanced deployment use cases. We’re just going to skim these topics so that you know about them in case you need them—most will not. But for some use cases, these are crucial features. Note that these are all capabilities that are part of Aries Cloud Agent Python. If you are using a different Aries framework, you’ll need to check if/how these capabilities are supported.

### Multiple controllers

While multi-tenant mode allows you to implement an agency where multiple agents can happily coexist, there are also occasions where we have discovered that being limited to a single controller per agent instance is not enough. As such, as of ACA-Py Release 0.6.0, a new capability has been added to support that requirement. Controllers are notified of events via webhooks, with the controller registering with the ACA-Py instance the URL to which webhooks are to be sent. The original single webhook per agent has been extended to support registering multiple webhooks. Each webhook URL receives notifications from ACA-Py as they occur. Of course, if multiple controllers are receiving the webhook notifications about events, they need to coordinate on the handling. For example, notifications might go to a "typical" controller to process, and a second controller that just monitors traffic and publishing metrics. The former responds to the events, while the latter just quietly listens.

### Persistent Queues

While ACA-Py has (mostly) been designed to support horizontal scaling based on a platform such as Kubernetes, there remains (as of mid-2021) a weakness in the implementation—some in-memory queues. As messages, webhooks and administrative requests are initially received, they are put onto a queue for processing. If an instance of ACA-Py is suddenly terminated for some reason (such as a Kubernetes node on which it is running crashes), the items in the queue will be lost.

An implementation of an external queue based on Redis has been implemented, so we’re part way there, but as of mid-2021, the supporting code to make it easy to deploy and use has not yet been added. Perhaps you might want to work on that!

### Plugins

While ACA-Py is pretty complete for general purpose, Trust over IP applications, there may be capabilities (such as protocols) that will need to be added for particular use cases. In some cases, you may have to take the long road to get those implemented: propose a protocol in an RFC, get it accepted in the community, get the RFC added to an AIP, get the AIP accepted, add the protocol to the Aries frameworks, access the protocol in your controllers and deploy your application.

However, in some cases where you want to use the capability in a closed ecosystem, you can add the capability to your deployments of ACA-Py by adding a plugin. A plugin is an external Python module that is loaded at run time by ACA-Py and available for use by your instance. The plugin might be a new protocol, including new messages, message handlers, a state object and administrative endpoints. A plugin might even add its own startup parameters to ACA-Py.

If you need to add something to your version of ACA-Py, here are a couple of examples of plugins that you can look at to get started:

* The Aries Toolbox that we looked at in Chapter 6 implements a couple of ACA-Py plugins that are defined in the [Aries Toolbox Plugins](https://github.com/hyperledger/aries-acapy-plugin-toolbox) repository.
* The BC Gov OrgBook includes a special protocol for organizations to register their use of ACA-Py that is implemented as a plugin. The code for that plugin is [here](https://github.com/bcgov/aries-vcr/tree/master/server/message_families/issuer_registration).

And with that, we’ll end our brief journey into Aries framework advanced capabilities. We’ll wrap up this chapter and move onto the last chapter in the course, which is all about what you can do with your newfound Aries developer chops.

## Knowledge check 8

1. All Indy ledgers have a TAA. True or <mark>False</mark>?
2. What is the "Community Coordinated Update" mechanism used for? Select all answers that apply.
  * <mark>To make a breaking change to the protocols agents are using</mark>
  * To integrate a range of agents
  * To change versions of different protocols
  * <mark>For agent interoperability</mark>
3. The encryption keys associated with an agent’s storage database should be stored with the data. True or <mark>False</mark>?
4. When using horizontal scaling, there are many instances of the Aries framework and controllers, but only one secure storage database. <mark>True</mark> or False?



## Summary chapter 8

In this chapter you learned about the challenges of production, covering a little about mobile and a large amount about enterprise agents. This chapter covered a lot of territory!

You learned about:

* The issue of keeping ledgers and agent storage in sync—and the danger of losing your agent secure storage—not good.
* The "best practice" pattern for managing ledger objects in production.
* The current process for writing transactions to a ledger that is fully permissioned.
* Deploying enterprise agents on scalable platforms such as Kubernetes.
* How you can create an agent platform, an agency, running hundreds or thousands of agents within a single Aries deployment.
* Some of the advanced capabilities in Aries Cloud Agent Python that you might (or might not) need as you begin to develop your own applications.

Don’t worry, you won’t need all of these features on day one of your work as an Aries developer. But we did want you to know about some of the more powerful features under the hood, as you get closer to releasing your first product.

# Chapter 9

We’ve covered the core of the materials. Congratulations—you’ve made it! We hope you are well on your way to becoming a Hyperledger Aries developer.

With the heavy content and labs complete, this chapter provides a look forward at what might be next on your journey. We have looked at agents and controllers, agents and protocols and agents and frameworks. We’ve looked at testing, message routing, mobile agents and moving things into production. Now it’s time to consider what you want to do with Aries.

In this chapter, you will learn about:

* Where your development efforts might fit best and how they apply to the technical layers of the Trust over IP (ToIP) stack.
* What Aries projects are active and where you can contribute.
* Aries Working Group Calls and other ways to get involved.

## Going forward

Going forward, consider what you want to work on next:

* Are you looking to build an application on top of Aries?
* Do you want to add a new capability to Aries?
* Do you want to contribute to the existing Aries projects?
* Do you want to contribute to the projects that are under Aries, such as Indy and Ursa?

In the following, we go through the technical layers of the Trust over IP (ToIP) stack from top to bottom and relate that to what you could work on next. As you will recall, the Trust over IP stack was introduced in the prerequisite course, "Introduction to Hyperledger Sovereign Identity Blockchain Solutions: Indy, Aries and Ursa" ([LFS172x](https://www.edx.org/course/identity-in-hyperledger-aries-indy-and-ursa)) and is represented in this image from the [Trust over IP Foundation](https://trustoverip.org/).

Our expectation is that the majority of developers will work on ToIP applications—building Aries controllers at Layer 4, that run on top of the Aries frameworks that occupy Layers 2 and 3 of the ToIP technical stack. More developers working at Layer 4 implies there will be fewer contributing developers in the technologies at or below Aries frameworks. This is not to dissuade anyone from contributing at the lower levels, but rather to say, if you are not going to contribute at the lower levels, you don't need to know everything about those layers. It's much like web development—you don't need to know TCP/IP to build web apps.

## Building decentralized identity / Trust over IP applications

<mark>If you just want to build enterprise applications on top of the decentralized identity-related Hyperledger projects</mark>, you will be doing exactly what we’ve discussed in this course, building cloud-based controller apps using any language you want, based on an agent framework such as aries-cloudagent-python ([ACA-Py](https://github.com/hyperledger/aries-cloudagent-python)). You can start by using the examples we have provided in the labs in this course, from scratch, or look in the community for starter kits that are beginning to emerge (such as this [Verifiable Credential Identity Starter Kit](https://github.com/bcgov/issuer-kit) from the BC Government). As we have seen throughout the course, developing enterprise issuer/verifier Aries agents is much like building any web service, receiving events, processing them and responding.

## Enterprise Agent Open Source Projects

While most folks are building ToIP applications for their own needs, here is an enterprise agent open source project that some might find intriguing.

Incubating within the [Hyperledger Labs](https://labs.hyperledger.org/) organization is the interesting [Business Partner Agent (BPA) open-source project](https://github.com/hyperledger-labs/business-partner-agent) that was started by Bosch. A BPA is a Hyperledger Aries agent for an organization that allows the organization to:

* Receive verifiable credentials issued to it.
* Share self-asserted data and data from the verifiable credentials it holds by responding to presentation requests.
* Request presentations from partners and other organizations.
* Issue verifiable credentials.

It’s kind of like the organizational equivalent of a person’s mobile wallet, but intended for business-to-business use cases, ideal for things such as controlled sharing of supply chain information to prove the provenance of products, and the organizations that produced those products.

In addition to its Aries DIDComm interface to interact with other agents, a BPA exposes a web-based interface for authorized users within the organization to see and respond to requests coming into their BPA, and to initiate requests to other BPAs (or other Hyperledger Aries) agents. Incoming requests are handled by configurable workflows that might be as simple as an automatic response, to as complex as a multi-person, multi-level approval business process. And of course, BPA users present data from verifiable credentials they hold for BPA authentication and authorization.

Just as almost all companies have a web presence today, we can see a future where all companies have their own public BPA for exchanging verifiable information to enable trusted business transactions. It’s a big project with a ton of potential!

## Mobile Hyperledger Aries Wallets

As we covered in Chapter 7, if you want to build and deploy a mobile agent, there are open source options available to start with, such as the [Aries MAX](https://github.com/hyperledger/aries-mobile-agent-xamarin) and [Aries Bifold](https://github.com/hyperledger/aries-mobile-agent-react-native) projects. If you are going to build off of the source bases, we strongly recommend you do most of the work within those projects, collaborating to everyone’s benefit. In the long run, we hope that there are a number of Aries mobile agents in the app stores that offer users fantastic mobile experiences. In the short run, we expect that some organizations will want to issue companion mobile agents that work closely with their enterprise web services. Organizations may even extend their existing mobile applications to include Aries agent/ToIP capabilities. Credit Union tech company [Bonifii](https://bonifii.com/) has taken this path with its [MemberPass](https://www.memberpass.com/members/) capability in its mobile app that makes verifying the user on support calls way more secure.

As a developer building applications that use/embed Aries frameworks, we recommend you join the regular [Aries Working Group calls](https://wiki.hyperledger.org/display/ARIES/Aries+Working+Group) (see the last section of this chapter) and Aries channels on the [Hyperledger RocketChat](https://chat.hyperledger.org/home). The maintainers of the Aries repositories often hold regular meetings about their project. For example, ACA-Py holds biweekly [ACA-Pug](https://wiki.hyperledger.org/pages/viewpage.action?pageId=24780322) (Users Group) meetings to bring together the ACA-Py developers and teams building applications on ACA-Py. Listed at the end of this chapter are the links for all of the Aries project chat channels and meeting pages.

Looking for Hyperledger Aries product ideas? The last chapter of the prerequisite course ([LFS172x](https://www.edx.org/course/identity-in-hyperledger-aries-indy-and-ursa)) provided a list of areas where the verifiable credentials model could be applied, ranging from identity to climate change. Jump back to that chapter to remind yourself of just some of the many possibilities with Aries technology. But realize that the possible applications are truly endless. We hear of new ideas everyday!

## Learning the protocols

As we’ve talked about a lot in the course, the [aries-rfcs](https://github.com/hyperledger/aries-rfcs) repo is an important resource. As you get started in building applications, we recommend you carefully review the Aries Interop Profiles (1.0 and 2.0) in [RFC 0302](https://github.com/hyperledger/aries-rfcs/tree/main/concepts/0302-aries-interop-profile), where you will find links to the set of RFCs/protocols that many of the existing agents and agent frameworks support. Sticking to these protocols will ensure that your applications will interact with other agent-based applications in the ecosystem. As well, you should look at the state of the agents and agent frameworks that are available, and choose the right one for your application.

**NOTE**: *If building apps is what you want to do, you don't need to do a deep dive into the Aries agent framework (beyond the API they expose), the indy-sdk/indy-vdr, or the indy-node ledger repositories. You need to know the concepts implemented by those projects, but it's not a requirement to know those code bases intimately.*

If we did our job right in building this course, you should now have all the tools you need to get started!

## Contributing to the Aries projects

As you build controllers on top of the Aries projects, you may find limitations in what is available today in Aries frameworks. When that happens the community would love it if you made a contribution. A start would be just to raise the topic on RocketChat or create an issue in GitHub that clearly outlines what you are trying to do, and the limitations you have hit. From there, the community will be more than willing to help you move forward. Perhaps you haven’t yet discovered the capability already exists, or perhaps it’s a deficiency that needs to be addressed. Either way, the community will help you find a way to make progress.

The following are some ideas for some ways you can contribute to Aries projects and the other open source projects on which they are built.

### Extending open source mobile apps

We’ve given two open source mobile "getting started" capabilities in this course. The developers working on those offerings would love to have other contributors with mobile expertise to expand those offerings. Ultimately, all published (in the app stores) mobile agents are by definition proprietary, but there is a lot of shared work needed to make it easier for organizations to create great mobile self-sovereign identity experiences.

### Mobile agent backup and restore

As we discussed in both this course and its prerequisite, mobile agent backup and restore is a crucial capability that must be both secure and easy for users. Consider designing and building a backup and restore capability for mobile agents that both automates the backup operation (that’s pretty easy) and provides a user-friendly, secure restore operation (that’s a bit more challenging!).

### Contributing to the Aries Agent Test Harness

Demonstrating interoperability is a huge part of building a sustainable ecosystem. The Hyperledger Aries community wants to empower creators of Aries agents to build their applications with confidence that they will work with all of the other agents in the community. As we talked about in Chapter 6, the Aries Agent Test Harness is a major piece of the interoperability puzzle, enabling continuous, automated testing of Aries agent frameworks and agents. Here are some of the ways that you can contribute to the [Aries Agent Test Harness](https://github.com/hyperledger/aries-agent-test-harness) open source project.

* Add tests that expand the coverage of Aries Interop Profiles, especially AIP 2.0 as its use expands.
* Develop backchannels to enable testing of specific, important Aries implementations (such as yours!).
  - In particular, there is not currently a way to do fully automated testing of mobile wallets. Do you have a background in mobile app testing? Jump in— please!!
* Monitor the test runs and take ownership of failing tests to at least determine the faulty component, and ideally, drive a fix to it. The faulty component could be in a number of places and it’s a challenge to figure out where the problem might be. Is it:
  - The RFC from which the test was defined?
  - The test itself?
  - One of the backchannels operating a component-under-test?
  - The functionality of a component-under-test?

### User experience

The focus of the Aries (and Indy and Ursa) communities to this point has definitely been on the underlying technology. The majority of the (mid 2021) community is technical and the focus has been more on getting things working in a secure manner. However, as products based on the technology begin to move into the mainstream, there is a need to focus on user experience. How can we provide the benefits of this new technology in ways that are easy for the majority of the population? We’ve seen enough examples in the community to think that this is possible, but ease-of-use needs to be at the core, and as such, we need more great designers to join the community to make that happen. Projects such as the mobile agents and Business Partner Agent on the enterprise side are great places to contribute.

### Driving RFCs from proposed to accepted

As you begin to build your applications and discover features you wish you had, ask in the community. You may find that an existing RFC covers your needs. Is your idea new and you need other agents to support it? Raise it in the community and if appropriate, contribute an RFC and drive it with an implementation. In reviewing and contributing RFCs, please make sure you look at the main [README](https://github.com/hyperledger/aries-rfcs/blob/main/README.md) for the repo, covering the RFC lifecycle, and the [contribution](https://github.com/hyperledger/aries-rfcs/blob/main/contributing.md) guidance.

### Contributing to the Aries "Shared Components"

While AIP 1.0 was largely focused on the use of Indy ledgers and the Indy AnonCreds verifiable credential format, AIP 2.0 (May 2021) provides a more agnostic approach to the location of DIDs and the format of the verifiable credentials and presentations. This has meant a transition from the use of the indy-sdk as core to Aries agents to some components that are general in focus. Those new components have collectively become known in the community as the "Aries shared components" and are implemented in Rust.

<mark>The most important of the shared components is Aries Askar, a secure storage implementation for an Aries agent. It replaces the indy-wallet code within the indy-sdk, expanding its capabilities beyond Indy specific handling to include additional uses, enabling easily adding new cryptographic signatures and primitives, including support for the keys used for the BBS+ verifiable credentials format.</mark> The other current shared components are new implementations of Indy-specific features, including interacting with Indy ledgers and interacting with Indy AnonCred verifiable credentials.

The ACA-Py code base that we’ve used in many of the labs in the course has recently (mid 2021) added the Aries shared components as a runtime option, and as confidence in the code base grows, will likely deprecate the indy-sdk in favor of the new components. As well, those components are good options for other Aries frameworks as they add support for the new capabilities in AIP 2.0. There are a number of opportunities for developers to contribute to either the shared components themselves, and to their use within Aries frameworks.

### Evolving Support for JSON-LD BBS+ Verifiable Credentials

The 2021 addition of support for W3C standard verifiable credentials has been important to Aries, opening up the possibility of exchanging verifiable credentials and presentations beyond the Aries ecosystem. However, the transition from Indy AnonCreds to BBS+ format verifiable credentials has meant a loss of capability, as we’ve discussed in this course. In the initial BBS+ signatures implementation, there is a lack of support for ZKP holder identifiers, and for a ZKP revocation scheme. Those are key features and work is ongoing in those areas. If you have the skills and experience to contribute in those areas, you are most welcome to help out.

### Improving indy-node

If you are interested in getting into the public ledger part of Indy, particularly if you are going to be a Sovrin Steward, you should take a deep look into [indy-node](https://github.com/hyperledger/indy-node). Like the indy-sdk, indy-node is robust, of high quality and is well thought out. As the network grows, use cases change and new cryptographic primitives move into the mainstream, and thus, indy-node capabilities will need to evolve. Indy-node is coded in Python.

The biggest issues with indy-node in mid 2021 is the need to be able to easily find and access data on multiple Indy instances, and adding full support for the W3C DID 1.0 specification.

Currently in Indy, the "did:sov" DID method does not define a way to say on which network a DID is to be found. That’s been annoying to this point as we’ve worked on promoting applications from, for example, Sovrin’s StagingNet to MainNet. This gets untenable as additional production ledgers come online, and verifiable credentials rooted in different ledgers are issued into a single Aries wallet. The community has defined how to add the network on which the DID resides into the DID Identifier, but the functionality is not yet part of Indy Node, indy-sdk or indy-vdr.

As indy-node pre-dates the 1.0 DID specification, it’s not surprising that it does not fully support the spec. Most notably, Aries agents writing DIDs to Indy cannot yet add what they might want into the DIDDoc of the DID. A design has been put forth for how to make that possible, but to date (mid 2021), that work has not been done.

Indy Node is a great place to contribute to the community!

### Working in Cryptography

Finally, at the deepest level, and core to all of the projects is the cryptography in [Hyperledger Ursa](https://github.com/hyperledger/ursa). If you are a cryptographer, that's where you want to be—and we want you there!

### Howto get involved

We’ve covered most of the ways to get involved in the content presented in this course, so the following is just a list of those resources with links:

* Hyperledger [Aries project](https://www.hyperledger.org/projects/aries) page
* The [Aries Working Group Wiki, and meetings schedule](https://wiki.hyperledger.org/display/ARIES/Aries+Working+Group)
* [Hyperledger RocketChat](https://chat.hyperledger.org/home) and the main [Aries channel](https://chat.hyperledger.org/channel/aries)
* [Aries Cloud Agent - Python User Group](https://wiki.hyperledger.org/pages/viewpage.action?pageId=24780322), [meetings schedule](https://wiki.hyperledger.org/display/ARIES/ACA-Pug+Meetings), and [RocketChat channel](https://chat.hyperledger.org/channel/aries-cloudagent-python)
* Aries Framework Go [Wiki](https://wiki.hyperledger.org/display/ARIES/aries-framework-go), [meetings schedule](https://wiki.hyperledger.org/display/ARIES/Framework+Go+Meetings) and [RocketChat channel](https://chat.hyperledger.org/channel/aries-go)
* Aries Framework JavaScript, [Wiki](https://wiki.hyperledger.org/display/ARIES/Aries+Framework+JavaScript), [meetings schedule](https://wiki.hyperledger.org/display/ARIES/Framework+JS+Meetings) and [RocketChat channel](https://chat.hyperledger.org/channel/aries-javascript)
* Aries Framework Bifold, [Wiki](https://wiki.hyperledger.org/display/ARIES/Aries+Bifold+User+Group), [meetings schedule](https://wiki.hyperledger.org/display/ARIES/Aries+Bifold+User+Group+Meetings) and [RocketChat channel](https://chat.hyperledger.org/channel/aries-bifold)

Starting from those links, you can learn anything more you need about becoming a Hyperledger Aries developer!

## Knowledge check chapter 9

1. If you just want to build enterprise applications on top of the decentralized identity-related Hyperledger projects, a good place to begin is the Verifiable Credential Identity Starter Kit from the BC Government. <mark>True</mark> or False?
2. When contributing to an RFC, you should:
  * Carefully review the README for the applicable RFC repo
  * Understand the lifecycle of the RFC
  * Fork the repo
  * <mark>All of the above</mark>
3. You are a cryptographer, so you should work on the indy-sdk. True or <mark>False</mark>?
4. As an Aries agent developer, what important resources are available to you?
  * The aries-rfcs repo
  * Aries Working Group weekly calls
  * Hyperledger Chat
  * The Aries Interop Profile
  * ACA-Pug Meetings
  * <mark>All of the above</mark>

## Chapter summary

That’s a wrap! Thank you for taking the course. We hope that you have acquired a sound understanding of Aries agents and are ready to jump in, contributing to this new and exciting technology.

# Final exam

1. What does the `~` symbol mean in the context of an Aries message?
  * The item is a <mark>decorator</mark>
  * Nothing special
  * It is a protocol
  * It is a data element containing localization information
2. ________ includes a shared cryptographic wallet for blockchain clients as well as a communications protocol for allowing off-ledger interaction between those clients.
  * Indy
  * <mark>Aries</mark> (cf. https://wiki.hyperledger.org/display/LMDWG/ARIES)
  * Ursa
  * A verifiable credential
3. If you want to build enterprise applications on top of the decentralized identity-related Hyperledger projects, you should start by digging into cryptography and distributed ledgers. True or <mark>False</mark>? (you should build Aries controllers and deploy the code with an agent framework)
4. What are some of the challenges with building Aries mobile agents? Select all answers that apply.
  * Using notifications for receiving encrypted Aries messages
  * <mark>Dealing with app store release processes</mark>
  * <mark>Getting users to upgrade</mark>
  * <mark>Supporting deprecated features in older releases</mark>
5. Which Aries framework is not suitable for use for mobile development?
  * Aries Framework JavaScript
  * Aries Framework .NET
  * <mark>Aries Cloud Agent Python</mark>
  * Aries Framework Go
6. When it comes to managing upgrades to Aries agents, what are the most important mechanisms you should use? Select all answers that apply.
  * <mark>The Aries Interop Profile (AIP)</mark> (provides a way to target the same versions of protocols used)
  * <mark>The Community Coordinated Update (CCU)</mark> (provides a way to make breaking changes to protocols)
  * DIDComm
  * ACA-Py
7. It is extremely important to back up your agent’s storage for the following reasons. Select all answers that apply.
  * <mark>It contains the data necessary to interact with other agents</mark>
  * <mark>It contains private keys</mark>
  * It contains the genesis file for connecting to a ledger
  * <mark>If you lose it, you can no longer issue credentials</mark>
8. Which of these Aries RFCs occur only in AIP 2.0? Select all answers that apply.
  * RFC 0160 Connections
  * <mark>RFC 0023 DID Exchange</mark>
  * <mark>RFC 0434 Out-of-Band</mark>
  * RFC 0095 Basic Message
  * RFC 0005 Protocols
9. A multi-tenant Aries instance supports multiple separate agents, each with their own secure storage. <mark>True</mark> or False?
10. In DIDComm, the term ______ is used to indicate that the message is being routed through one or more additional agents without the knowledge of the sender.
  * mediator
  * <mark>relay</mark>
  * sender
  * router
11. Mediators and relays are Aries agents. <mark>True</mark> or False?
12. Mobile applications do not have a physical endpoint that remote applications can use. As a result, if you are thinking about creating a mobile agent, which of the following do you have to consider. Select all answers that apply.
  * How to backup your mobile agent storage
  * <mark>How and where to deploy a cloud-based mediator agent</mark>
  * <mark>How messages will be handled when the phone is offline</mark>
  * How to use Google and Apple to relay messages
13. The Aries Toolbox is:
  * <mark>A desktop tool that allows a user to control the behavior of running Aries agents</mark>
  * An HTTP interface used by a typical ACA-Py controller
  * A GUI that is the basis for Aries mobile apps
  * A generic OpenAPI interface used to accomplish certain agent-related functions
14. What is different about the "invitation" message in the "connection" protocol from other Aries protocol message?
  * Nothing, it is just like any Aries message
  * <mark>It does not use DIDComm encryption when sent</mark>
  * It starts the protocol
  * It is not formatted as JSON
15. The Aries Toolbox can be used for experimenting with creating and writing a new schema and credential definition to a test ledger, and then trying out issuing credentials using those objects. <mark>True</mark> or False?
16. The format of Aries protocol messages is JSON and the `@type` and `@id` items are in every Aries protocol message. <mark>True</mark> or False?
17. As messages go back and forth between agents to complete an instance of a protocol (e.g. issuing a credential), the ______ decorator data lets the agents know to which protocol instance the message belongs, and the ordering of the message in the overall flow.
  * <mark>`~thread`</mark>
  * `~instance`
  * `~tracing`
  * `~timing`
18. Which special message types are used for error handling. Select all answers that apply.
  * <mark>`ack`</mark>
  * `acknowledge_msg`
  * `response`
  * <mark>`problem_report`</mark>
19. As of the time of writing this course, which Aries Framework does not use Hyperledger Indy or Ursa:
  * aries-cloudagent-python
  * aries-framework-dotnet
  * <mark>aries-framework-go</mark> (uses [tink](https://github.com/google/tink) and has no ledger)
  * aries-acapy-controllers
20. At startup, an Aries issuer agent needs to know what? Select all answers that apply.
  * <mark>The location of the genesis file for the Indy ledger it will use (if any)</mark>
  * <mark>How to create DIDs, schema and the credential definition on the ledger</mark>
  * <mark>The transport endpoints it uses for receiving messages from other agents</mark>
  * The endpoints of other agents with which it will issue credentials
  * <mark>Storage options for keys and other data</mark>
21. What is the VON Network? Select all answers that apply.
  * <mark>A pre-packaged Indy network build</mark>
  * <mark>A good place to start to learn about running a local Indy network</mark>
  * A requirement for running an Aries agent
  * <mark>A minimal four-node Indy network using docker containers</mark>
22. The Aries Agent Test Harness is used to run a common set of tests on an Aries agent. True or <mark>False</mark>? (The Aries Agent Test Harness is used to run tests involving multiple test agent implementations to demonstrate interoperability based on Aries Interop Profiles.)
23. The ______ contains information about the physical endpoints (IP addresses and ports) for some of the nodes in Indy ledger pool, and the cryptographic material necessary to communicate with those nodes.
  * ledger
  * <mark>genesis file</mark>
  * identity owner
  * agent
24. Who operates the nodes of an Indy network?
  * Trustees
  * Sovrin
  * <mark>Stewards</mark>
  * Endorsers
  * Hyperledger
25. All Aries agent deployments have two logical components. What are they?
  * <mark>A framework</mark>
  * An envelope
  * A container
  * <mark>A controller</mark>
26. In the labs, we used several mechanisms to enable developers to interactively trigger running agents to execute protocols. Select all answers that apply.
  * HTTP and WebSockets
  * <mark>Aries Toolbox</mark>
  * <mark>OpenAPI/Swagger</mark>
  * VON Network’s Web Server
27. AIP 2.0 introduced the ability to use verifiable credentials that use the BBS+ Signature suite. What features do such verifiable credentials support? Select all answers that apply. (cf https://www.evernym.com/blog/bbs-verifiable-credentials/)
  * <mark>Selective disclosure</mark>
  * ZKP Predicates
  * <mark>The W3C Verifiable Credential Standard</mark>
  * <mark>JSON-LD</mark>
  * Ledger-based Credential Definitions
28. What is the GitHub repository that provides a reference about Aries protocols?
  * `aries-cloudagent-python`
  * `indy-sdk`
  * `aries-framework-dotnet`
  * `aries-protocol-test-suite`
  * <mark>`aries-rfcs`</mark>
29. What information is not included in the specification of an Aries Protocol?
  * Message types
  * <mark>Business rules</mark>
  * Roles
  * States
  * Version
30. From which movie comes the (in)famous Faber College?
  * Dumb and Dumber
  * Bernie’s Weekend II
  * <mark>Animal House</mark>
  * Porky’s
  * Revenge of the Nerds
