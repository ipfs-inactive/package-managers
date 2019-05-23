# Package indexing and linking

The crux of package management is really the act of maintaining one or more **indexes** of package releases, with some rules around how to group, connect and order releases.

In an index releases usually have a version identifier (e.g. `1.0.0`) and are grouped together by a project identifier (e.g. `react`), but those identifiers don't have to match up with the what the source code contents (often a tarball) of a release believes it is (although it usually does).

Indexes tend to store project/release identifiers, which usually translate into an address that points to where the actual contents of a release is.

This has some benefits, for one, it decouples the index from the storage and transportation of the release content. It also allows for the index to be updated, mutated and mirrored without changing any data inside the packages.

Many package managers take further advantage of these indexes when declaring dependencies within an package, using only the bare minimum project identifier with an optional version range selector, relying on the index to provide the available version identifiers within that project identifier group and then the actual address of the desired release of that dependency once selected.

That separation also requires a lot of trust in both the index and the translation of the addresses the index provides into actual package contents. There are a lot of assumptions that the relevant data that was in the index when the release was initially published will continue to be there.

*Side note: The community solution to this trust problem is solved by many package managers by defaulting everyone to a single, large centralized index that only allows certain users to add new data to it. These large indexes often become central points of failure, becoming too large for regular users to mirror and operationally taxing for the maintainers of the index.*

When the contents of a release has so little knowledge of itself and it's context, it always needs to be paired with a separate index.

So what if... **a package could also be it's own index?**

A release can't contain a hash of itself or be updated with information about later releases, but it can contain an IPFS CID of the release that came before it (except for the initial release), creating a linked list of previous releases:

```
               0.1.1 <----0.1.2
             /
0.0.1 <--- 0.1.0 <--- 1.0.0 <--- 1.1.0 <--- 1.1.1
                        \
                        1.0.1 <--- 1.0.2
```

#### Why is this useful?

One of the most frequent lookups a package manager will do in an index is to get a list of available releases for a given project identifier, the resulting list of releases is then passed into dependency resolution.

- Reproducibility: If you already have a particular package, the list of available versions that can be pulled from this linked list of previous packages will always be the same.

- Trust: once published, the list of previous versions for that package is immutable, this reduces the reliance on external indexes which have the possibility to change the items in a list of available versions.

- Size: Rather than having one large index that contains all available releases across all packages, this approach breaks it up into many small indexes, one per package and community indexes then only need index the leaf nodes of each package tree.

#### What's missing?

- Discovery: Users expect to be able to run `xpm update` and automatically discover newer releases than the ones they already have, which still requires a separate index, although that index only needs to point at the latest version rather than keep track of all possible versions

#### A note about dependencies:

You could use this same method for referencing other package linked lists (via IPFS CID) for dependencies, pointing to the tip of a branch from your release, which then points to an immutable, reproducible transitive dependency tree, again without an index:

```
0.0.1 <--- 0.0.2 <--- 0.1.0 <--- 1.0.0 <--- 1.1.0 <--- 1.1.1
             /                               /
          dep@1.0.1 <--- dep@2.0.2 <--- dep@2.1.0 <--- dep@3.0.0
                            /                            /   
                       trans-dep@0.0.1 <--- trans-dep@0.0.1
```

Whilst being very reliable for reproducibility, fast moving communities will often want to pick up the latest versions of transitive dependencies without requiring changes to intermediate dependencies.

This is one of the pain points for [gx](https://github.com/whyrusleeping/gx), as often the developer does not have the ability to publish new releases of an intermediate dependency to the central index.

One solution to this may be to enable users to easily create and manage their own small indexes, possibly even at a per-application level which would  allow easy forking and patching of dependencies without needing to change project identifiers. I'll be writing more on this idea soon.
