# Bench-Network

A Guide To How BENCH'S Network Works vs. EOS and other MultiChains.

## Developing On BENCH
When it comes to the logic behind Bench’s MultiChain, the processing of native and remote transactions are deterministic. For example, lets say the Bench Multichain didn’t have deterministic native and remote transaction processing, a true consensus would never be reached by nodes and satellites on the network. 

If you want an example of something in the decentralized universe that is deterministic, look no further than Solidity as a deterministic programming language. Our idea was that we could utilize DappChains to utilize deterministic decentralized applications (dApps) using any programming language and by doing so we were able to help developers create deterministic decentralized application easily by giving them the ability to create random number generation methods that wouldn’t require seeding that was deterministic itself. It would also give developers the ability to create race conditions by doing so on the threads themselves or without threads at all. We also wanted to give developers a way to use their system time (clock), uninitialized memory that you would find in unsafe languages like C, true floating point arithmetic that you find in high end modern blockchains and map-iteration based language features that remove any non-determinism from the underlying foundation of the application itself. We are also working on special fuzz-testing and lint testing tools for most languages to help developers shorten the process even further.

## BENCH Network Participants
Bench Network participants that take part in the Bench Network’s protocol are called Rocketeers and each of them round robin proposing blocks of native and remote transactions on the network and then voting on those NTXs and RTXs (Native transactions and Remote transactions). The basis of how blocks are committed to the RootChain, a SideChain or a DappChain is simple. There is one block, per height. Like other blockchains, our multichain may have a block that fails to commit and in that particular situation, the Bench Network protocol would move to the next “MultiRound” and a new Rocketeer would get to propose the next block for the height that failed in the past MultiRound. To successfully commit a block to the MultiChain, there are two stages of voting (Pre-Vote and Pre-Commit) A block is committed when more than 2/3 of the Rocketeers “Pre-Commit” for the same block in the same MultiRound. 

We call a 2/3 pre-vote on the same block within a MultiRound a “Big Bang”. Every pre-commit MUST be authenticated by a Big Bang within the same MultiRound. There are many reasons that a Rocketeer can fail to commit a block whether it’s a network user that is proposing a block going offline, the network could be going through a latency period and many other reasons could make this possible. BenchCore allows the network to skip those Rocketeers. In any case, Rocketeers wait through a small period, before they receive a proposed block that is considered “Complete” via a proposer on the Bench Network, before he/she votes to move to the next MultiRound.  Considering Bench’s network relies on this small timeout, BenchCore’s `SyncProtocol` is weak at best, rather than it being considered as an `AsyncProtocol`.
On the other hand, the rest of the BenchCore protocol is an `AsyncProtocol` and Rocketeers will only progress through the logic of the underlying protocol if the `RocketeerSet` is counted at 2/3. The same way BenchCore commits a new block during a MultiRound, is the same way it skips to the next MultiRound. 


## BenchCore & The BFT Protocol
As quoted by Wikipedia - “Byzantine fault tolerance (BFT) is the dependability of a fault-tolerant computer system, particularly distributed computing systems, where components may fail and there is imperfect information on whether a component is failed.” On BenchChain, lets assume that less than 1/3rd of the Rocketeers are what would her called “Byzantine”, BenchCore at its very core, assures users, delegates, rocketeers and other networks that safety will never be violated. In other words, Rocketeers would be unable to commit blocks that are conflicting at the same height. 
In order to accomplish this, we have added several new functions to BenchCore, along with rulesets, surrounding the concept of Lock() and Unlock(). A moment a block is pre-committed by a Rocketeer, it is immediately “Locked” on that block and moments later a Pre-Vote will take place on that block that it’s locked on. To “Unlock” and then “Pre-Commit” this block, there would need to be a “BigBang” for that block in the next MultiRound. 

## Pledging On BenchCore Chains

Unlike blockchains, our first iteration of the Multichain Rocketeers will not have the same “Weight” or “Fuel” within the Bench Network’s protocol.  We were never interested in 1/3 or 2/3 of the Rocketeers but in the proportions of “Voting Fuel”, that we designed the BenchChain/BenchCore products to efficiently and uniformly deploy to each Rocketeer.  Since BenchCore can replicate state machines aka blockchains, sidechains or dappchains, it is possible to define a cryptocurrency / digital asset (give it a name in the Genesis file of your own chain), and designate voting fuel to the cryptocurrency/digital asset you created. When voting fuel is designated  to a cryptocurrency that is truly native to your blockchain - as BEN is native to our Multichain, your blockchain or any blockchain like it, would truly be considered DPOS or at the very least, proof of stake. Rocketeers can be forced through the logical algorithms within BenchCore, to attach or basically bond their cryptocurrency holdings in what we would call a security deposit, that can be destroyed if that particular Rocketeer is found to be misbehaving against the terms of BenchGov’s community rules for Rocketeers or if they’re going against the network protocols and network rules themselves. I believe this adds an extra element for Rocketeers to be aware of. Ted East and I have spoke about this at length. 

## Motions On BenchCore

One extra element I’d like to put out there is Motions. If people think that a Rocketeer is misbehaving, Motions can be filed for other Rocketeers to look at and for insured BEN holders (any BEN holders who have pledged any amount of BEN to a Rocketeer or Rocketeer candidate) to vote on. In the end this adds a economic element to truly secure Bench’s protocol, allowing Rocketeers and others on the network to truly quantify what it would cost them for violating the assumption that less than one-third of the network’s voting fuel is truly BFT.

## BENCH'S Network & The Lite Client Protocol
In the Bench Network, we use what is called a “Lite Client Protocol”, used to get a commit for a recent block hash. That particular commit always includes a majority of signatures from the last known RocketeerSet. From that point on - the entire dApp or Chain’s app state can be completely authenticated with Merkle Proofs through our SPRUCE framework that utilizes IAVLs. These attacks on Bitcoin as mentioned above, completely change the assumptions and ideas behind these Lite Client Proofs and being that we wanted to bring together all the blockchains into one MultiChain - we would need the right idea in order to scale this idea properly and do it correctly.

## BENCH and MIR Applications
Then comes MIR, basically a glorified upstream decentralized router, that is able to communicate with all the networks around it and translate what is receives from other networks, to BenchCore directly.  All data that MIR sends out, is numbered packets, just like the internet and telecom protocols we use every single day - only MIR does this with decentralized networks, can confirm that information has reached a network through auditing that blockchain’s transactions and other features. MIR is also used as a wrapper around other blockchains like BitShares, where BitShares Core software is converted to a MIR application, so that it can speak to any BenchCore-based network. Any blockchain can be wrapped by MIR and brought into the MultiChain and would then become usable by the underlying Bench Network. BenchChain is a MultiChain but is also built around the MIR framework / protocols. 

BenchChain is one of the first working MultiChains in the world, truly interoperable across the chains that are native to it (SideChain and DappChains) and chains that also utilize MIR or will soon utilize MIR. This enables BenchChain to enable the trestles exchange of digital assets across native and remote decentralized networks, through the RootChain itself. Creating a better decentralized web goes far beyond just the MultiChain capabilities and decentralized application development features of Bench. Soon Bench will bring to life a decentralized operating system - the world’s first decentralized operating system that has a GUI and many other features that are sure to rock the world the day it releases. 

## Comparing BENCH & EOS 


### Quick BENCH vs. EOS Comparison

**Number of Rocketeers/ Validators :** <br>
`BENCH:` 4 < x < no upper limit <br>
`EOS:`  21 <br>
<br>
**Block Time (Mean):** <br>
`BENCH:` 0.3 seconds<br>
`EOS:` 0.5 seconds<br>
<br>
**BFT %:** <br>
`BENCH:` 1/3 <br>
`EOS:` 1/3 <br>
<br>
**Ability To Scale:**<br>
`BENCH:` Horizontally Scales Through Both Sidechains and DappChains<br>
`EOS:` Small number of delegators enable high throughput, parallelism<br>
 <br>
**Developability:**<br>
`BENCH:` Usability for any developer with any programming language. One-click blockchains. Blockchain SDKs, dApp SDKs, Decentralized OS + GUI, dApp Store, Future hardware offerings and more.
<br>
`EOS:` Usability for varied users that include contract writers, developers and entrepreneurs. Currently a complicated process when launching blockchains, no GUI to their decentralized OS or way for users to interact with EOS other than through a server terminal <br>
<br>

### Comparison Overview 
Normally, with distributed systems that are controlled and ran by a single organization, a user within the network would gain trust via firewalls and roots of trusts via their hardware. Basically to protect themselves from the bad guys who long for the opportunity to undermine the consistent nature of the distributed database that their distributed system utilizes. Blockchain systems or in the case of BenchCore, a blockchain engine, requires a different architectural design, where the distribution of trust amongst MANY organizations, chains, networks and participants are all simultaneously protected from those very same bad guys that affect these distributed systems of old. The threat never changed - but the ideas have. One way to think of a system like Bench is a combination of Politics, Computer Science, Institutional Reputation and a large array of protocols and security models. 

When Satoshi created Bitcoin’s P2P blockchain software, he created a logical consensus that remove the idea of having to tolerate these bad guys. In conventional distributed systems run by a single organization trust is provided by firewalls, infosec teams and hardware roots of trust to ensure that malicious actors can’t undermine the consistency of distributed databases. Blockchain systems demand a different architecture where trust is distributed among many organizations but we have to tolerate adversarial actors in the system. The design of blockchain systems is a tradeoff between security models, game theory, computer science and institutional reputation. Bitcoin’s Nakamoto consensus abandons the distributed systems of old and their guarantees of finality that you see in traditional Byzantine fault-tolerant (BFT) architectures, and instead, chose what we call an “Open Admission Security Model”. 

There are many issues with this. Many of which Dan Larimer has pointed out of the years. One is a popular one - the likes that a bad guy can control over 50% of what Bitcoin’s network knows as “HashPower”. At the very same time, Bitcoin doesn’t offer any sort of justice for its users or guarantees in any way, shape or form. At 25% control of the “HashPower”, you could theoretically game the entire network or at least begin the process of doing so. In that process you will begin to see a destabilization, that is truly due to the selfish users who take part in “Greedy mining schemes”. If a malicious actor can control 50.1% of the hashpower, the system offers no guarantees at all. At 25%, the game theoretic mechanism starts to destabilize due to selfish mining and the probability of merging on the network becomes the least stable piece - which becomes a nightmare in the end. 

Bench and EOS are both ambitious projects - much different than any other decentralized-based projects you see today. I don’t care what they are - you won’t find two other projects that have the ambitions that these two projects have. Below we will explain the true differences between both networks.

### BENCH'S Protocol Punishment vs. EOS's Institutional Reptuation 
Bench’s network is built around two concepts of punishment for equivocation and a constantly increasing set of guarantees that will be used to extend what we call the DWeb (Decentralized Web).  EOS relies on mostly an institutional reputation and utilize what we see as a “Proforma” network logic, that basically sits between what Bitcoin’s consensus originally was and what Computer Science tells us is technically possible. 

Let’s keep in mind that Bench was originally created as a MultiChain, where DPOS wouldn’t end up as “NAS” (Nothing At Stake), a problem that was suffered by networks like BitShares 1.0 early on in the beginning years of BitShares. There are several important pieces to note about Bench’s decentralized network. The more prominent ones I will mention now are BenchCore, which is Bench Network’s blockchain engine, powered on a DPOS/BFT logical protocol and was built too withstand double-spending attacks and is completely tolerant against the 1/3rd attack that the Byzantine protocol was designed to prevent. Then comes BenchChain, which is basically a blockchain development kit (BDK), that works with any programming language with a massive suite of tools underneath it like dappJS, that allow a way higher level of decentralized application development, that has yet to be seen in our industry. The importance of this is, BenchChain allows developers to utilize BenchCore as a blockchain engine within their dappChains and SideChains, without the need for developing the difficult parts of a blockchain or running a blockchain. This ultimately allows developers to focus on their applications, instead of the hard stuff that they shouldn’t have to focus on. 

### BENCH's GUI-BASED DOS vs. EOS'S TERMINAL-BASED DOS
Coincidentally, EOS advertises itself as a decentralized operating system and it truly aims to bring to life dApps that are built for consumer-use. EOS utilizes smart contracts when it comes to decentralized application development, that enables the hosting of those contracts. EOS has said that they can run Ethereum in one single smart contract but to do so - they will have to trade away a lot of the features that make them truly decentralized. EOS will use ranked delegates, that run a set of master nodes who are in charge of Validating transactions on the network. While Ethereum is a distributed virtual machine, EOS is considered a distributed operating system. 

### COMPARING THE CONSENSUSES 

#### EOS'S CONSENSUS
EOS defines it’s delegators as elected block validators of the EOS blockchain and it seems as if they are elected democratically. There is a immutable number of delegates on EOS at 21. These delegates act as “Master Nodes” within the EOS network. 

Each master node or EOS delegate, is to sign and validate transactions and in the process the EOS chain will extend itself to the next block. EOS delegates are truly voted into office, by EOS holders. Daniel Larimer through his many years of experience chose 21 delegates due to his many years of experience over the years. One quote comes to mind - 

“You require a ⅔ majority to have an honest system. Originally BitShares started with 100. There’s not enough oversight of who those 100 people are because there’s not enough bandwidth of voters’ attention to decide. Bringing it down to 21 reduces the cost of the system. The network has to pay each person that runs a full node.” — **Daniel Larimer**

He truly chose 21 because if there were any more than 21, it would become a nightmare to stakeholder attention and voters would end up making poor decisions. Funny enough, for some reason, Vitalik Buterin describes EOS as a consortium blockchain, where in his mind,  “Merkle proofs and any other protections that would allow regular users to audit any part of the system’s execution unless they want to personally run a full node themselves.”. 

What Vitalik is saying isn’t something I would consider to be a practical solution -  because relying on users to run full nodes in order to be capable of auditing and authenticating Byzantine or negligent delegators without some heavy, built-in client-side mechanisms like Merkle proofs really makes coordination problems very hard to fix. EOS would need to rely on extra built-in mechanisms and would be heavily dependent on many protocols, however many that would be necessary in this situation and it would also become a network consensus issue, more importantly. 

EOS DPOS is dependent on the network’s stakeholders. Without said built-in mechanisms, heavy reliance on extra-protocol means is necessary, and it even becomes a consensus problem. EOS dPoS depends on its stakeholders externally to justly and accurately evaluate the performance of delegators on the network, to hopefully make accurate and positive hiring decisions when it comes to delegates and good firing decisions as well.  On the other hand, with Bench Network, any major software and protocol changes happen through the filing of motions and the BenchGov governance. 

EOS uses coin voting to achieve decentralization and truly, the more EOS tokens a stakeholder owns, the greater their voting power. EOS tokens can additionally be used as “Staking Weapons”, when it comes to businesses and enterprises running applications for their end users, instead of transaction fees. This staking dApp development creates usability issues that I will cover soon.  According to Daniel Larimer,  the LIB “is a block which has been confirmed by ⅔ or more of the elected block producers. No node will automatically switch to a fork that isn’t built on top of the LIB.”

Lastly, with EOS, Daniel has been quoted as saying that the last irreversible block is a block that has been confirmed by 2/3rd of EOS’s block producers and no EOS node on the network can automatically switch to an EOS fork that isn’t built on too of that last irreversible block. From our take there is a chance that this approach with the last irreversible block could possibly bring the entire network to an instant halt. 

**EOS and EOS Collateral**
EOS doesn’t carry any sort of penalties or punishment when it comes to the EOS protocol. Instead, they use collateral where a ranking of delegates would lose “Reputation” if they are found guilty of any violations of the protocol or any wrongdoing as considered by the community themselves. In our opinion, this doesn’t offer much of an incentive financially, when the network is confronted by a Byzantine mafia. 

DPOS should assume the combo of the opportunity cost of getting rid of a ranking delegate job and the sunk cost of campaigning to becoming a delegate on EOS, should be greater than the money gained from executing the famous double-spend attacks. Therefore, we believe the EOS network may have several vulnerabilities, since it’s protocol lacks the ability to truly punish bad actors and mafias and in this way, the “NOS Consensus Nightmare” remains. 

**EOS & Transaction As Proof of Stake (TaPos)**
When it comes to forks, EOS doesn’t operate the same way Bench does. EOS utilizes TaPos where every transaction requires a corresponding hash taken from the most recent block header. 

***What do TaPos Transaction Hashes Do?***
1. Keeps a replay attack from taking place because if a fork took place, the transaction on the fork that’s lacking this hash would automatically assume the fork is a fake.
2. It signals the entire EOS network that a particular user and their “Staked” EOS tokens are located on a specific chain.

In this way, TaPos would only be able to stop what is called a “Long-Range Attack”, and the EOS network IS able to recover from these attacks. Although, block finality is lacking on a near-term basis, which would therefore leave the EOS network vulnerable to forking if all the transactions haven’t been seen. If a transaction is valid and hasn’t been witnessed by EOS delegates and lacks TaPos-based hashes, those transactions would end up “Orphaned” in a near-term situation. 

#### BENCH'S CONSENSUS
BenchChain’s does use DPOS, aka delegated proof-of-stake but the mechanism behind the BenchChain consensus differs from EOS’s way of doing things. 

**Delegator Facts:**
- Delegates are used differently on the Bench Network
- A Delegator is any person on the Bench Network who wants to delegate or “pledge” (EOS calls this staking), cryptocurrency (such as BenchChain’s native cryptocurrency BEN), to use towards contributing “Voting Fuel” to a Rocketeer (A validator on the EOS network), of their choice and would earn a portion of the block reward in the process.
- Delegators put their BEN (BenchCoin) at stake with a Rocketeer of their choice. 
- Delegators can lose their BEN pledged to another Rocketeer, if the Rocketeer breaks the rules, doesn’t do his/her job or creates issues on the network. 

**Rocketeer Facts:**
- Validators with Bench are actually called Rocketeers and are in charge of validating transactions and committing new blocks to the MultiChain.
- Rocketeers on Bench’s network participate in the network’s consensus protocol by broadcasting crypto signatures, that at their very core are votes to simply extend the BenchChain MultiChain. 
- In order to become a Rocketeer and to have a positive amount of “Voting Fuel”, the Rocketeer candidate has to “Lock Up” a predetermined amount of BEN (If being done on the RootChain)
- Rocketeers can self-fund their candidacy / campaign to become Rocketeers or they can acquire Voting Fuel from other Pledging BEN holders by convincing them to pledge BEN to them. 
- During a BRI (Block Rocketing Interval), known as a MultiRound, a “RocketeerSet” is defined as the set of Rocketeers who are responsible for signing native/remote transactions that agree to commit the next block.
- The RocketeerSet dynamically changes as Rocketeers join or leave the Bench Network’s DPOS consensus process. 
- Bench’s RootChain, SideChains or DappChains only require 4 Rocketeers but there is no upper limit to the number of Rocketeers that a BenchCore-based chain can have. 
- BenchChain will have 100 Rocketeers but over time this will increase to 300 Rocketeers and this process will take place automatically based on a programmatically determined expansion schedule. 
- Although this is programmatically determined, this programming can be changed via Software-based Motions within the Governance. 

**Every Block Is Final **
Depending on the number of Rocketeers within BenchChain or it’s SideChains or DappChains, the finality of a block can be achieved in a single second. On average through our testing and “QControl” testing, BenchChain’s Rootchain finds block finality within around 3 seconds on average. 

**How BENCH Solves The NOS Consensus Nightmare**
Nothing at stake is a major problem with blockchain networks where Byzantine mafias strategize ways to overtake networks at literally no cost and to make matters even worse, there are no penalties or punishment for these actions. These mafias end up taking over these blockchain networks and destroying the true decentralized nature and purpose the original creators of these networks had in mind. 

**BENCH'S Insured Native Transactions**
BenchCore does a really good job of getting rid of the NOS Consensus Nightmare by creating “Insured Deposits”, where if a network participant was to unlock the Insured Deposit, the user must first unlock the deposit and then it goes through a period of time, where the deposit is unusable until it “Lives” again. On average, these times will be two to three months. This period is defined as the “UnInsuring Period” on the Bench Network of chains. 

**BENCH AND LITE CLIENTS**
Lite Clients like our future benOS-powered hardware whether it’s a desktop, laptop, mobile phone etc, and their users that are not syncing constantly with the blockchain through a BenchCore Full Node a responsive alert on how the “RocketeerSet” is about to mutate. If Bench didn’t have this UnInsuring Period, users within the Bench Network would be open to attacks where the MultiChain seems as if it has done something via a previous RocketeerSet but in reality, that RocketeerSet has blasted to another universe and their BEN and/or other coins are permanently gone. 

**What Is Fork Liability?**
In typical Proof of Stake protocols a fork is truly possible in one way. When at least 1/3rd of the Rocketeers or Validators within the RocketeerSet or ValidatorSet within one state of the blockchain or multichain itself, are colluding in some way. To limit the likelihood of a illegal fork, there MUST be protections built into the Multichain or Blockchain’s protocol itself. 

**How BenchCore Deals With Fork Liability**
BenchCore holds it’s Rocketeers accountable by identifying any Rocketeer who has taken part in a illegal fork within the RootChain, SideChains or DappChains. If a Rocketeer is round to have taken part in a malicious fork on any Native Chain or RootChain, they are immediately slapped with a heavy “Fine”, by destroying their Insured Deposit that was used to become a Rocketeer. In the end, this punishment is meant to be significant, where a third of all the pledged BEN in the RootChain network or the native pledged coins within SideChains and DappChains during that given state, or block time are immediately forfeited back to the network. If there is a hard fork because of the actions of a Rocketeer, the Rocketeer that causes the fork itself is removed from all networks immediately. It’s important that Bench can recover from a hard fork, if one happens through the malicious actions of a Rocketeer. If a hard fork was to happen from a 1/3rd set of malicious Rocketeers and other network actors - protocol-features are built-in to BenchChain to handle this major issue. BEN holders are able to work together, off chain to create “Reorganization Motions”, allowing them the power to fork the RootChain, SideChain or DappChain effected, as long as a significant number of Rocketeers on the network agree that a minority on the network who are bad actors, maliciously co-opted the Chain at a specific height.  

**Brewer’s Theorem and BENCH**
According to Wikipedia - In theoretical computer science, the CAP theorem, also named Brewer's theorem after computer scientist Eric Brewer, states that it is impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees. If BenchCore is facing a DDOS attack, it will stop in its tracks. EOS on the other hand will continue running through a DDOS, creating inconsistent replicative states, which are easily taken advantage of by attackers. BenchCore gives consistency a priority or the network’s availability - EOS takes the opposite approach. 

## Conclusion:
BFT is needed by BenchCore, BenchChain and the overarching Bench Network in order for it to be open, transparent and truly decentralized. We don’t want our RootChain, Sidechains or DappChains to be easily manipulated through its data, even if there is a temporary DDOS attack. Since state-based agents or blockchain mafias have a goal of keeping people from accessing these open and fair systems based around freedom and future Bench products must center around customer privacy and security, while bringing to life the best possible decentralized UX and UI, we wanted to build something great, rather than wasting time on hand waving, raising money, creating and ICO and looking to brag about what we are doing. We wanted our work to speak for itself. We truly wanted to change the world. 

While many claim that no one has hacked a live blockchain network doesn’t mean that it’s hack proof. Older networks are approaching a time where hacking them will become easier and easier by the day. The importance of applying the best possible “Mathematical proofs” that are needed to verify that a network is truly secure, instead of a network claiming that it is. Being that the decentralized space is mostly based around money and with the amount of money flowing into the top networks, it’s only a matter of time before attackers figure out a way in - this is why security, protocols and the privacy of users is a must. DPOS does have a 0.0001% edge case, which means that it can be attacked. Although with the combination of BenchCore, users will find through our constant public BenchMark/QControl testing, public audits and other future analytics, our vision for a secure, private and entertaining decentralized community is alive and well and it will only get better over time. Thanks for joining on for the ride and thanks for appreciating our hard work - it keeps us moving. See you on the chain. 

## Why The Comparison Doesn't Matter
BENCH was never built to destroy blockchains. BENCH was built to build blockchains and connect to other blockchains in order to achieve our vision of building the largest and safest network in the world. Our mission to bring consumers to the blockchain isn't just a tagline or a slogan - it's all we care about and we are going to achieve it. We want to stay out of the clutches of mediocrity by inventing and reinventing what we do, on a daily basis. Therefore we never want to be categorized, we want to categorize. This writeup is purely our opinion from our years of experience working with blockchain software and decentralized applications. Dan Larimer is a personal hero of many developers within BenchLabs, his father Stan Larimer sits at the top of the BENCH organization and has become one of the top contributors to not only the project but also the vision itself. Stating our opinion as thought leaders in this space, so that we can create better technologies for the people who truly need them, is one of our main goals and trying to build upon what the leaders in this space, like EOS have built, is how quickly growing technological advancements like decentralization can move mountains.

We say all of this to say, we would have never built BENCH and would have never visioned this idea without Dan Larimer's genius and his vision for a decentralized operating system. The decentralized space would have never made it as far as it has without Dan's dedication, wisdom and vision over the past 5 years either. His creation of BitShares, Steem and now EOS have set the standard in an industry where developers are typically forking, rather than innovating bigger and better solutions. So in the end, we tip our hat to you Dan, EOS, your team of amazing developers and engineers. Soon we will welcome your entire network to the Bench MultiChain, where we will utilize our MIR technology to connect BENCH with EOS to bring the best of both worlds into one interface for our customers. So Dan, it is your will to succeed and your wisdom to change the world that drives us to create better products and therefore, without you, none of this would be possible. In the end, you give us someone to chase, not someone to compete with. Until the next block...

