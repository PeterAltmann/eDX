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

<img src="https://courses.edx.org/assets/courseware/v1/e6878fa7000fb538a7e8b9dec066d0b1/asset-v1:LinuxFoundationX+LFS173x+3T2021+type@asset+block/LFS172x_CourseGraphics_V1-04.png" alt="drawing" width="500"/>
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
