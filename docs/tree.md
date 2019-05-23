# Cladistic tree of depths of integration

Preface: Understanding package managers -- let alone comprehensively enough to begin to describe the design spaces for growing them towards decentralizating and imagining the design space future ones -- is eternally tricky.  I've been hoping to get some more stuff rolling in terms of either short checklists, or terminology that we might define to help shortcut design discussions to clarity faster, and so on.  This is another attempt, and it certainly won't be an end-all, all-inclusive, etc; but it's a shot.

This is a quick, fairly high-level outline of tactical implementation choices that one could imagine making when designing a package manager with _some_ level of IPFS integration.

---

We could present this as a cladistic tree of tactical choices, one bool choice at a time:

- using IPFS: NO
	- (effect: womp womp)
- using IPFS: YES
	- aware of IPFS for content: NO
		- (effect: this is implemented by doing crass bulk snapshotting of some static http filesystem.  works, but bulky.)
	- aware of IPFS for content: YES
		- (effect: means we've got CIDs in the index metadata.)
		- uses IPFS for index: NO
			- (effect: well, okay, at least we provided a good content bucket!)
			- (effect: presumably this means some centralized index service is in the mix.  we don't have snapshotting over it.  we're no closer to scoring nice properties like 'reproducible resolve'.)
		- uses IPFS for index: YES
			- (effect: pkgman clients can do full offline operation!)
			- (effect: n.b., it's not clear at this stage whether we're fully decentralized: keep reading.)
			- index is just bulk files: YES
				- (effect: easiest to build this way.  leaves a lot of work on the client.  not necessarily very optimal -- need to get the full index as files even if you're only interested in a subset of it.)
				- (effect: this does not get us any closer to 'pollination'-readiness or subset-of-index features -- effectively forces centralization for updating the index file...!!!)
			- index is just bulk files: NO
				- (effect: means we've got them to implement some index features in IPLD.)
				- (effect: dedup for storage should be skyrocketing, since IPLD-native implementations naturally get granular copy-on-write goodness.)
				- (effect: depends on how well it's done...!  but this might get us towards subsets, pollination, and real decentralization.)
				- TODO: EXPAND THIS.  What choices are important to get us towards subsets, pollination, and real decentralization?

The "TODO" on the end is intentional :) and should be read as an open invitation for future thoughts.  I suspect there's a lot of diverse choices possible here.

---

Another angle for looking at this is which broad categories of operation are part of the duties typically expected of the hosting side of a package manager:

- distributing (read-only) content
- distributing (read-only) "metadata"/"index" data -- mapping package names to content IDs, etc
- updating the "metadata"/"index" -- accepting new content, reconciling metadata changes, etc

This is a much more compact view of things, but interestingly, the cladistic tree above maps surprising closely to this: as the tree above gets deeper, we're basically moving from "content" to "distributing the index" to (at the end, in TODOspace of the cladistic tree) "distributed publishing".

---

These are just a couple of angles to look at things from.  Are there better ways to lay this out?  Can we resolve deeper into that tree to see what yet-more decentralized operations would look like?
