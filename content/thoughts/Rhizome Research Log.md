---
title: "Rhizome Research Log"
date: 2022-05-03
tags:
- evergreen
- rhizome
---

## May
### May 14th
- Finish testing harness - it looks so pretty!
- Finally updating research proposals after putting it off for 3 days. I suspect I'm using `miniraft` as an excuse to avoid the grant writing because making things legible is hard!! I'd much rather write code and look at pretty command line outputs instead but this is important work that needs to be done.

### May 12th + May 13th
- Reaffirming myself that a lot of this is necessary learning and this is a worthwhile project
	- Not sure if this is actually true
	- But more so convincing myself of it so that I have the energy/motivation to go through with it
- A lot of technical refactoring going on to accommodate unit testing
	- Removed a lot of unnecessary lifetimes while changing `RaftServer` functions to return a vector of sendable messages rather than directly having each server hold a mutable reference to the transport layer (Rust doesn't allow multiple mutable references without a `RefCell`!)

> Let's say you want to become good at [x].
> 
> It's almost impossible to do it because every day on Twitter you have friends who’ve raised 6 million to do crazy stuff. And so every single day, you open your books, and you take your notes and you start writing stuff, and you have to solve those equations.
> 
> And every single day you tell yourself, why am I doing this?
> 
> I could just go out and bullshit investors and build a company. And I think too many people actually do that. Myself included. I managed to resist for a while and I spent a lot of time learning different, difficult things, but it's very hard not to have ADD in this world. It's very hard to stay focused on important things that take a while to be learned.
> 
> - [Justin Glibert on doing hard things](https://masterplan.substack.com/p/master-plan-justin-glibert-foundation?curius=1294&s=r)

### May 11th
- Finished the first pass of implementation of `miniraft`! In the midst of adding test infrastructure and verifying correctness of the implementation.
	- Probably spent tooo long making it look nice but hey, if I'm going to be spending hours looking at this it might as well be good to look at
- Also spent an hour trying to debug a test only before realizing `cargo test` runs in parallel so debug messages were out of whack
- Feeling quite demotivated regarding overall self-belief in the project even though I'm only 11 days in! Been trying to explain Rhizome to a few folks who have experience in the space and it is often so intimidating.
	- Like yeah, I know this probably isn't the best way to go about it. Maybe they'll tell me what I'm working on is a long solved problem and I'm wasting my time. Or "couldn't you just use x and y to achieve the same effect"? I can't help but sometimes feel like I'm wasting my time -- there are so many smart people working on the same problem, what makes me feel like I can be the one to make a meaningful contribution to it?
	- I know that regardless of whether this project succeeds a lot, I'm already learning a lot in terms of technical skills and also about myself in the face of uncertainty and more independent work so I will take that as a win regardless.

### May 10th
- Discussing grant proposals with Verses folks, doing a lot more grant/proposal writing than I'd like these days
- Finished most of `miniraft` logic up until `commit_log_entries`. Still need to add tests though :')
- Tech bear market isn't promising for raising funding, esp for more experimental/greenfield work like this :((

### May 9th
- Literally just wrestling with Rust's borrow checker because `dyn` traits are funky :((
	- Ran into a really weird design problem where I wasn't sure how to order the lifetimes of the log or the state machine (should the app own the log which owns the state machine? should the log hold a reference to the app)? 
	- I opted to construct the application first then pass a pointer to the log so that when appending entries to the log, it can just call `self.entries.iter().for_each(|entry| self.app.transition_fn(entry))`
- Finally caved and watched an hour long video on closures, `Fn`, `FnOnce`, `FnMut`, boxed closures, and function pointers ([Jon Gjengset, I owe you my life](https://www.youtube.com/watch?v=dHkzSZnYXmk&t=3005s))
	- Feels really stupid but it was literally a change from `&'s mut dyn App<'s, T, S>` to `Box<dyn App<T, S>`
	- When lifetimes get as messy as they did, there's probably a cleaner way to do it with a heap allocated value :)) Use `Box` more often!

### May 8th
- Sketching out grant proposals to Emergent Ventures + Protocol Labs
- Had a chat with Sebastien about research institutes and what long-term support for work like this could look like in the context of Verses
- More implementation work for `miniraft`, about halfway done I think?
- More of a slower day to spend time with family for Mothers Day :)

### May 7th
- Does not seem promising that my research work will be support by Verses this summer...
- Looking for other places to apply for funding but ugh this is unfortunate
- Lots of coding today for `miniraft`! Finally feeling like I'm becoming more fluent in Rust. Figured out some nasty named lifetime stuff today by drawing a few diagrams and kinda feel like a wizard!!! Small wins

### May 6th
- Mostly writing up recent learnings and incorporating them into the [[thoughts/Rhizome Proposal|research proposal]]... lots of words today
	- Sometimes I feel like I'm doing research to be able to do more research...
- I think I am finally getting to a point where Rhizome is making more and more sense and obvious why it is necessary
	- I started this project/research very much like "oh wow, this is a cool set of technologies and here are some vague words and feelings about what I think is inadequate in the space" and it has sort of refined itself into a clear use case!
	- Came across the concept of a "cloud peer" today in Hypercore documentation and it was like "WOW I had this exact same idea and they already have a name for it" and it was so cool
	- Really excited about this future of 'personal cloud computing'
	- I think this summer will be mostly focused on the data replication / identity aspect of Rhizome, realizing that I think I was way too ambitious with my first proposal
- More implementation on `miniraft`. Rust feels so slow to get back into a 'de-rusted state' (hah) where code just 'flows'. It feels fun though! Type system reminds me a lot of Haskell.

![[thoughts/images/rhizome-may-6.jpeg]]

### May 5th
- Finishing up [[thoughts/distributed systems#Martin Kleppmann's Course|Martin Kleppmann's Course on Distributed Systems]]
	- Cleaning up notes into atomic concepts that I can reference
- Continuing implementation of `miniraft`
- What if... Rhizome had built in mechanisms for managing 'branches'
	- Default branches are single stream
	- To make a collaborative doc you can 'fuse' or 'join' branches together temporarily to sync them with each other
		- What if we made something on top of `git` like this that actually functions on a syntax level rather than a character level... one for the [[thoughts/idea list|idea list]]
	- Pace layers for collaboration
		- Real-time (keystroke-by-keystroke)
		- On-click (manually click refresh)
		- Suggest changes (like Google Docs, accept/reject)
- Agreeing on what operations a [[thoughts/CRDT|CRDT]] can perform still seems to be difficult ([see 1hr into this talk](https://youtu.be/Qytg0Ibet2E?t=3665))
	- Possible room for data lensing on public schemas to be useful here

### May 4th
- Skimming [[thoughts/distributed systems#Martin Kleppmann's Course|Martin Kleppmann's Course on Distributed Systems]]
	- Really good foundation to work off of
	- Learned about differences between physical and logical [[thoughts/clocks|clocks]] and realizing that `miniraft` should probably use some sort of [[thoughts/clocks|logical clock]] rate

### May 3rd
- Read about more [[thoughts/NAT#Efficacy|NAT traversal and holepunching efficacy]], turns out hole punching is just not as reliable as I thought it was
- Compared more traditional consensus algorithms like [[thoughts/Raft Consensus Algorithm|Raft]] to [[thoughts/Solana|Solana]].
- First formal architecture sketch?
	- Need to read more about DID and IPFS but this seems like a promising start?
	- Each user is essentially a DID that is associated with an IPFS document that references a bunch of other things
		- Each device in the devices array runs a Rhizome Node which is essentially a wrapped IPFS node that pins the user IPFS object and can edit it
		- Right now, this means that if all a user's devices are offline, those files are unreachable. For people who still want their stuff to be replicated online, perhaps can integrate FileCoin to incentivize other nodes to pin their document?
	- The devices array is also used by Raft to coordinate what devices should be included in the cluster
		- Modifications in the devices array leads to a Raft configuration update
		- All devices that are reachable sync via Raft to keep an `appState` object up to date for the user
		- When any `appState` log gets too long, it is snapshotted by the leader and persisted in IPFS.
	- All the questions that are unanswered right now are in red. Lots of unanswered questions :))
		- How does auth work for applications?
		- How will schemas be published? Is there an app store?
		- Who runs the web host? Is it self-hosted?
			- What about non-technical people?
		- How is a user created?

![[thoughts/images/rhizome-may-3.jpeg]]

### May 2nd
- Mostly reading about [[thoughts/Raft Consensus Algorithm|Raft]] consensus algorithm today and understanding how it works
	- Always wondered how these consensus algorithms deal with bad actors -- turn out they don't! That's where [[thoughts/fault tolerance#Byzantine Faults|BFT]] comes in
	- Seems to be promising for replicating between trusted peers (potentially applicable)
- Starting a very minimal stripped down implementation of [[thoughts/Raft Consensus Algorithm|Raft]] in Rust I am nicknaming `miniraft`. Code [here](https://github.com/jackyzha0/miniraft) (but will most likely be private until it is done).

### May 1st
- Settling back into home, general research reading + writing [[thoughts/Rhizome Proposal|the proposal]]
- Read various papers
	- [[thoughts/internet computing#Changing an entrenched internet|Changing an entrenched Internet]]
	- [[thoughts/mechanism design|Mechanism Design]]
	- [[thoughts/Raft Consensus Algorithm|Raft]]
- Learned about the basic premise of [[thoughts/Self-sovereign Identity (SSI)|SSI]]