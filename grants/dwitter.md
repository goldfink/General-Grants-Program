# Dwitter 30k Grant Proposal

* **Project:** Dwitter Censorship resistant Web3 Microblogging and Social Networking service

## Project Overview 
The projects serves a web3 replacement of Twitter, which has become the Internet standard for 330 active users engaging in the creation of content as well as social interaction, political and journalistic review, news coverage and diplomatic representation of countries.

The platform has made it easy to pursue these means of communication using centralized instrastructure and services owned and operated by Twitter Inc. -- Parity's mission of launching the web3 in a decentralized manner aligns with the goal of including a version of the platform that relies on technology such as Polkadot human readable names and IPFS for self-hosted user content, providing uncensorable social content entirely in the users control.

### Overview

  * *Substrate / Polkadot / Kusama Integration* Dwitter provides a Polkadot Parachain for human readable names in a decentralized platform, as well as social currency exchange between users.
  * *Team* The team is experienced in cryptographic technology, distributed systems and shares the security community's and Parity's mission on decentralization.
    It serves experience in international entrepreneurship and is engaged in the security community. Open source value for a web3 and a more open and decentralized     Internet has been and is being created.

### Project Details 
The final state of the project renders a Website on Public DNS or browser supported crypto domains, as well as multiple clients for mobile and desktop usage. The user interfaces serves an intuitive frontend proxying social media funcitonality through censorship resistant and purposefully web3 proven Tor hidden services, IPFS/IPNS for user-controlled content delivery and a Polkadot parachain, rendering currency and human readable names.

#### Architecture Mockup
![alt text](https://github.com/goldfink/General-Grants-Program/blob/Web3Foundation-patch-1/grants/Architecture_Mockup.png?raw=true)

#### UI Prototype samples
![alt text](https://github.com/goldfink/General-Grants-Program/blob/Web3Foundation-patch-1/grants/dwitter_electron.png?raw=true)
![alt text](https://github.com/goldfink/General-Grants-Program/blob/Web3Foundation-patch-1/grants/dwitter_timeline.png?raw=true)
![alt text](https://github.com/goldfink/General-Grants-Program/blob/Web3Foundation-patch-1/grants/dwitter_profile.png?raw=true)


The UI mimics the Twitter.com user experience with additional features. 

#### Preliminary Architecture and API Outline

The concept of follow, the code base at the start of the implementation holds at its core the concept of an identity. The identity object is where the data about the users posts, the people they "follow", and whatever else they might want to include is stored. An identity object is the fundamental unit that we'll use to create a distributed social graph.

First, we connect to IPFS and retrieve our ID. On first boot, we instantiate a new "Identity" object, which is ultimately saved to disk as a json file.

Most of the identity logic is contained in the Identity class:
`follow/modules/identity.js`

Identity object structure:

root level keys:

```
{
    "av": "", // base64 encoded image data for "avatar"
    "aux": {}, // an object for arbitrary, user-defined data. Ex.
    "dn": "", // user-defined display name
    "id": "", // users IPFS ID
    "meta": [""], // list of CIDs that represent meta objects
    "posts": [""], // a list of CIDs that represent post objects
    "following": [""], // a list of ID's the user follows
    "ts": 10000, // UTC adjusted UNIX timestamp of the identities last edit
}
```

aux object:

```
{
    "btc": "",
    "website": ""
}
```

post object:

```
{
    "body": "", // the text body of the post
    "publisher": "", // original publisher, will be used for "re-post" functionality
    "magnet": "", // a webtorrent magnet link for redundancy
    "cid": "", // IPFS CID of the root directory of the post
    "files": [], // a list of file paths, relative to the user data directory
    "ts": 0 // UTC adjusted UNIX timestamp of the post
}
```

We cache every post (user and following) object and "post body"(text) to disk for faster load times, and we automatically pin post CIDs to strengthen the network. ID caches follow a strict directory structure.

Once an identity object has been generated and saved to disk, it is "uploaded" and "pinned" via IPFS.
Then, in an attempt to abide by unix philosophy, we publish the identity CID to IPNS wrapped in a directory, so other things can be stored there as well.

TODO: recursively merge new publication with the "root directory" of previously published record.

The users identity object CID can now be retrieved by querying IPNS with their ID and the object itself can be downloaded.

Once a user "follows" another user, the process of fetching their posts is done automatically. The posts are presented chronologically in a "feed" with the all posts from all the other ID's the user follows.

Identity objects _will_ be automatically re-fetched periodically, followed by their new posts.

#### Technology Stack
The used open source technologies for censorship resistance, decentralization and tokenization, human readable domain names, render as [[Tor|https://github.com/TheTorProject]], [[IPFS|https://github.com/ipfs/ipfs]], [[IPNS|https://docs.ipfs.io/concepts/ipns/]], and [[OpenBazzar tor onion ipfs transport||https://github.com/OpenBazaar/go-onion-transport]]. 

#### Docuemntation
[[MISSING: *  Documentation of core components, protocols, architecture etc. to be deployed]]

#### Proof of Concept
* https://github.com/iohzrd/follow

### Ecosystem Fit 
The Ecosystem fit is proven by purpose, integration of the Polkadot Stack and architectural censorship resistant design of the project. The AutoDapp project seems to align https://raymondcheng.net/projects/decentralization/autodapp-proposal.html with the web3 mission as well.

## Team

### Team members
* Goldfink
* iohzrd
* weev	

### Team Website	
* https://github.com/goldfink/dtwitter

### Legal Structure 
The company is registered in London, named Wyvern Intrinsics Ltd. as is owned by Dan Marsollek <goldfink@wyvern.ch>

### Team's experience
The team is experienced in decentralized technology and security community involvement, contributions to major open source project and creation of tools. 

### Team References
* https://github.com/iohzrd
* https://github.com/iohzrd/follow
* https://github.com/dan_cipher
* https://github.com/weev
* https://www.eff.org/press/releases/eff-joins-andrew-auernheimer-case-appeal
* https://www.youtube.com/watch?v=7cgWrnPeIKs
* https://www.businessinsider.com/peter-thiel-fellows-2013-2013-5?r=DE&IR=T

### Team LinkedIn Profiles
* https://www.linkedin.com/<person_1>
* https://www.linkedin.com/<person_2>

## Development Roadmap :nut_and_bolt: 

Below we provide the roadmap of the Dwitter project. The milestones will be executed in parallel and due to research in performance and scalability, censorship resistance and potential improvements and feature merge requests to multiple open source projects involved, as well deciding upon further production features such as prominency social reward, incentivizing the use of the platform.

### Overview
* **Total Estimated Duration:** 1 year, 5 months 30k PoC, with additional 200k in requested funding for facing the public Internet and creating a startup company
* **Full-time equivalent (FTE):**  Full time work on the project will be pursued.
* **Total Costs:** The request of this proposal for the creation of a Proof-Of-Concept is $30k.

### Milestone 1 — Implement a Dapp with IPFS/IPNS Pinning, and Tor hidden service content delivery in a UI client 
* **Estimated Duration:** 3 months
* **FTE:**  1.0
* **Costs:** $15,000

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | ISC |
| 0b. | Documentation | We will provide both inline documentation of the code and a basic tutorial that explains how a user can (for example) spin up one of our Substrate nodes. Once the node is up, it will be possible to send test transactions that will show how the new functionality works. |
| 0c. | Testing Guide | The code will have proper unit-test coverage (e.g. 90%) to ensure functionality and robustness. In the guide we will describe how to run these tests | 
| 1. | Substrate module: X | We will create a Substrate module that will... (Please list the functionality that will be coded for the first milestone) |  
| 2. | Substrate module: Y | We will create a Substrate module that will... |  
| 3. | Substrate module: Z | We will create a Substrate module that will... |  
| 4. | Substrate chain | Modules X, Y & Z of our custom chain will interact in such a way... (Please describe the deliverable here as detailed as possible) |  
| 5. | Docker | We will provide a dockerfile to demonstrate the full functionality of our chain |

### Milestone 2 — Implement Substrate Modules, Launch a Polkadot parachain for human readable names and social currency 
* **Estimated Duration:** 3 months
* **FTE:**  1.0
* **Costs:** $15,000

| Number | Deliverable | Specification |
| ------------- | ------------- | ------------- |
| 0a. | License | ISC |
| 0b. | Documentation | We will provide both inline documentation of the code and a basic tutorial that explains how a user can (for example) spin up one of our Substrate nodes. Once the node is up, it will be possible to send test transactions that will show how the new functionality works. |
| 0c. | Testing Guide | The code will have proper unit-test coverage (e.g. 90%) to ensure functionality and robustness. In the guide we will describe how to run these tests | 
| 1. | Substrate module: X | We will create a Substrate module that will... (Please list the functionality that will be coded for the first milestone) |  
| 2. | Substrate module: Y | We will create a Substrate module that will... |  
| 3. | Substrate module: Z | We will create a Substrate module that will... |  
| 4. | Substrate chain | Modules X, Y & Z of our custom chain will interact in such a way... (Please describe the deliverable here as detailed as possible) |  
| 5. | Docker | We will provide a dockerfile to demonstrate the full functionality of our chain |


### Community engagement

A thourough [Medium](https://medium.com/)) article will be written and published upon completion of the grant.

## Future Plans
The long-term goal of the project is to fullfill the goal of Bluesky to provide a web3 version of Twitter with a focus on performance and censorship resistance, in the format of a publicly used Internet service instead of a Blockchain and Security Community exclusive project.

## Additional Information :heavy_plus_sign: 
* Work done so far: Architectural setup, Follow code and UI for social media messaging and IPFS pinning
* No further grant applications have been made, and no further financial involvement has been present

