# Facilitating the Correct Abstractions

(Acknowledging, of course, the hubris of the title -- we can only hope and try!)

To contribute meaningfully to advancing the state of package management, we must first understand package management.

First in understanding package management, we should identify and understand the stages of package management.  These are stages I would identify:

- *[human authorship phase is ready to produce a package]*
- pack content
- write release metadata (version name, etc)
- upload content and release metadata
- *[-- switch between producer to consumer --]*
- fetch release metadata
- transitive dependency resolution
- lockfile creation
- *[-- possible switch to even further downstream consumer --]*
- lockfile read and content fetch
- content unpack
- *[a new human authorship phase takes over!]*

![cycle](https://user-images.githubusercontent.com/627638/53887868-6f7ce580-4023-11e9-9109-803fbd5a06ef.jpg)

(Image is an earlier visualization of roughly the same concepts, but pictured with authorship cycle closed.
Also note this image contains an "install" phase which is elided in the list above
or perhaps equates to "content unpack" depending on your POV; and several other steps
were combined rather than enumerated clearly.)

Understanding these phases of package management, we can begin to identify what
might be the key concepts of APIs that haul data between each of the steps.
And understanding what key concepts and data need to be hauled between each
step gives us a roadmap to how IPFS/IPLD can help haul that data!

---

Now.  There's many interesting things in the above:

- Never forget that rather than a list, there is actually a cycle when creation
  gets involved.  I won't talk about this more in this issue, but in the longest
  runs, it's incredibly important to mind how we can close this loop.

- Some of these phases are particularly clear in how they can relate to IPFS!
  For example, uploading of packages and fetching of packages: clearly, these
  operations can benefit from IPFS by treating it as a simple content bucket
  that happens to be be particularly well decentralized.  Since this is already
  clear, I also won't talk any more about this in this issue.

- You might have noticed I injected some Opinions into a few of the steps.
  In particular, that ordering of transitive resolution vs lockfile creation
  vs metadata fetch is not entirely universally adopted!  Some systems skip
  the lockfile concept entirely and re-do dependency resolve every time they're used!
  Some systems vary in what the lockfile contains (version numbers that are still
  technically somewhat vague and need centralized/online translation into content,
  versus content-identifiers/hashes, etc).  Of course, systems vary *wildly*
  in terms of what information they actually act on and what exact logic they
  use for transitive dependency resolution.  And alarmingly, most systems don't
  clearly separate metadata fetch from resolution processes at all.

---

That last set of things I really want to focus in on.

I think my biggest takeaway by far from the last couple years of thinking about this whole domain is that segmenting resolve from all other operations is absolutely Of The Essence.
It's the point that never ceases to be contended, and for fundamental rather than incidental reasons: it is *correct* for different situations and packagers and user stories to use different resolution strategies.

It's also (what a coincidence) the key API concept that lets IPFS help other systems while keeping clear boundaries that let them get on with whatever locally contendable (e.g language specific) logic they need to.

---

But here we've got a bummer.  Essentially no modern package managers I can think
of actually intentionally designed their resolve stages to be separate and pluggable.

The more we encourage separation of resolve from the steps that follow it,
the more clear it becomes for every system to have lockfiles;
and the more things have lockfiles, the happier we are,
because the jump from lockfile to content-addressable distribution system gets
more incremental and becomes more obviously a right choice.
But this is already widely clear and quite popular!

More interesting is what happens when we encourage separation of resolve from
the steps that *precede* it -- namely, from "metadata fetch".

If we can encourage a world of package managers which have clearly delineated
boundaries between metadata fetch and the evaluation of transitive dependency resolution
upon that metadata, we both get clearer points for integration IPFS/IPLD in the
metadata distribution, AND we provide a huge boost to enabling "reproducible resolve" --
an issue I've written more about [here](https://repeatr.io/vision/strategy/reproducible-resolve/) in the Timeless Stack docs --
which sets up the whole world nicely for bigger and better ecosystems of reproducible builds.

---

Thank you for coming to my Github issue / thinkpiece.

Where can we go from here?  No idea: I just want to put all these thoughts out there to cook.  We'll probably want to consider these in roadmapping anything beyond the most shortterm basic content-bucket integrations; and perhaps start circulating concepts like separating resolve from metadata transport sooner rather than later to prepare the ground for future work in that direction.
