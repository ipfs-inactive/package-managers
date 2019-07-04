# Package management document library

The following items are materials created or collected by the IPFS Package Managers Special Interest Group in order to facilitate research and development and, as such, act as a living inventory and analysis of the present package manager landscape. If you're interested in learning more about package management in general, or integrating IPFS into current-state package managers in particular, you'll find a lot to chew on here.

Is there something missing from this document library? Would you like to contribute your research or analysis? PRs are welcome!

## Introduction to package management

- [Package management glossary](glossary.md)<br/>
A glossary of common terms relating to package management.

- [Finding the best abstractions for talking about package managers](abstractions.md)<br/>
In order to discuss package management, we need to first identify and understand the stages of package management.

- [Package management categories](categories.md)<br/>
A look at the types of categories to which a package manager can belong, based on design, implementation and feature set.

- [Package manager registry categorization](https://docs.google.com/document/d/1WwekeTJ4tAPjLVDnfIt-dXrgu7vGD29T07EQWN2_G-A/edit#heading=h.kgd4ngectp6q)<br/>
Registry-based categorization model for how to approach integrating IPFS in package managers.

## The current package manager landscape

- [Audience segmentation, pain points, and communications channels](https://app.mural.co/t/protocollabs6957/m/protocollabs6957/1557168696127/577c9453a3c51199c8163cf0fe5701294e55f99b)<br/>
A cluster board collating pain points and communications channels for package consumers, package publishers, and package manager maintainers. 

- [Problems with current-state package managers](problems.md)<br/>
Outline of problems that package publishers, package consumers and package manager maintainers currently experience when working with pre-IPFS package managers. (One of the source documents for the cluster board listed above.)

- [Research by package manager](../package-managers)<br/>
Directory of researched package managers by package format and grouped by usage category, including notes on language, clients and more.

## User research and analysis

- [IPFS Camp 2019 Deep Dive: ranking pain points/benefits](https://github.com/ipfs/camp/tree/master/DEEP_DIVES/package-managers)<br/>
Summary and artifacts from a deep dive held at the July 2019 IPFS Camp, focusing on ranking and analyzing the key package manager pain points and potential benefits determined during Q3/Q4 2019 roadmapping.

- [Package manager outcomes, requirements and ideals statements](https://app.mural.co/t/protocollabs6957/m/protocollabs6957/1557168696127/577c9453a3c51199c8163cf0fe5701294e55f99b)<br/>
Outcomes statements, requirements statements and ideals statements for IPFS in package managers, broken down by package consumers, package publishers, and package manager maintainers.

- [Package manager audience segment personas](https://app.mural.co/t/protocollabs6957/m/protocollabs6957/1557515371017/a3b880188663ebd3655bcbc2388d1e47e52cc4f1)<br/>
Technical- and outcomes-focused personas for the three key package manager audience segments: package consumers, package publishers, and package manager maintainers.

## Resources for integrating IPFS in your favorite package manager

- [How IPFS concepts map to package manager concepts](concepts.md)<br/>
Breakdown of various package manager functions and how those work when IPFS is added into the mix.

- [Package manager implementation decision tree](https://app.mural.co/t/protocollabs6957/m/protocollabs6957/1556717261380/7d93181e586fc3416ef88a42ba6d6df4b964c89b)<br/>
This decision tree provides a high-level outline of tactical implementation choices and their effects when integrating IPFS within a software package manager.

- [Cladistic tree for implementing IPFS in a package manager](tree.md)<br/>
A high-level outline of tactical implementation choices one might need to make when designing a package manager with some level of IPFS integration. (Source document for the decision tree listed above.)

- [Category-based approaches for implementing IPFS support](https://github.com/ipfs/package-managers/blob/master/docs/category-based-implementation.md)<br/>
Some possible approaches for implementing IPFS support based on implementation category.

## Specific implementations

- [Republishing a project's npm dependencies to IPFS as a micro-registry](https://drive.google.com/file/d/1ExdN_t7xV2mEldjri5WXNDPU5hL4wa4q/view)<br/>
Talk from @andrew at [IPFS Camp 2019](https://github.com/ipfs/camp/) on decentralized dependency resolution in NPM using the "micro-registries" approach in [ipfs-npm-republish](https://github.com/andrew/ipfs-npm-republish)

## Deeper dives

- [Package indexing and linking](linking.md)<br/>
Investigation into linking between packages and if a package could also become its own index.

- [Decentralized Publishing](decentralization.md)<br/>
Investigation into the challenges involved in implementing decentralized publishing and how to combat them. What might the parts of a fully decentralized package manager look like?

## Academic research

- [Academic papers related to package management](papers.md)<br/>
A list of published papers relevant to package manager research and software dependencies.

## IPFS organizational and roadmap resources

- [Package manager goals in the 2019 IPFS roadmap](https://github.com/ipfs/roadmap#-package-managers-d1-e5-i3)<br/>
How enabling package managers is manifested as a primariy goal of the 2019 IPFS roadmap. 

- [Draft package manager roadmap](https://docs.google.com/document/d/1-HtUiRpMzYq9to56ShCGyCr-NCZ6TR49Zl-b5HHJdm0/edit#heading=h.5zpzsg32y0bx)<br/>
Q1 2019 recommendations for future efforts (some items out of date from a roadmap perspective, but concepts are still valid)

- [Open items tagged `package managers` in ipfs/notes](https://github.com/ipfs/notes/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A%22package+managers%22)<br/>
Primarily for historical context — package manager-related issues in the [IPFS Collaborative Notebook for Research](https://github.com/ipfs/notes)

