<img width="800" alt="Package Managers Task Force" src="https://user-images.githubusercontent.com/4827522/61231640-60005e00-a6e1-11e9-872e-950de6310bc3.png">

[![](https://img.shields.io/badge/made%20by-Protocol%20Labs-blue.svg?style=flat-square)](https://protocol.ai/)
[![](https://img.shields.io/badge/project-IPFS-blue.svg?style=flat-square)](http://ipfs.io/)
[![](https://img.shields.io/badge/freenode-%23ipfs--package--managers-blue.svg?style=flat-square)](http://webchat.freenode.net/?channels=%23ipfs-package-managers)

## Join our weekly sync (all welcome!)
Tuesdays at 3:15pm UTC

📞 Zoom: https://protocol.zoom.us/j/602589940

:orange_book: Notes and recordings from previous syncs [are in Issue #1043](https://github.com/ipfs/team-mgmt/issues/1043)
 
:heavy_check_mark: To see what's in progress, either visit our Zenhub [task board](https://app.zenhub.com/workspaces/package-managers-5d23270c7302a14dfba9d8c4/board?repos=20312497,39592840,59225160,69641599,170355990,41456203,55763542,79474042) or filter issues in this repo by the [`Status: In progress` label](https://github.com/ipfs/package-managers/issues?q=is%3Aopen+is%3Aissue+label%3A%22Status%3A+In+progress%22)

## Why are package managers important to the future of IPFS?

- IPFS is *very close* to becoming a great tool to solve real problems for package manager users, package publishers, and package manager maintainers! Plus, closing those gaps will make IPFS better for everyone while giving the IPFS community a yardstick for how to measure our success in the future.
- Package manager maintainers and consumers are an important demographic to IPFS' future: developers who may not be IPFS pros yet, but who can get excited and engaged about IPFS in its current state, enabling everyone to help IPFS improve both on existing fronts and in problem areas we haven't yet envisioned
- Introducing IPFS tooling and support can create direct value and impact in the overall package manager ecosystem by saving developers time and empowering resilient development experiences for developers using IPFS or IPFS-powered tools and registries.

## Goals for IPFS and package managers
1. Get the package manager maintainer community excited about the benefits of using IPFS in their ecosystems
2. Demonstrate to package consumers that IPFS is _holistically better_ than current centralized package managers (this may mean parity on some use cases, but significantly better on others)
3. Increase awareness and engagement with IPFS (either to build new tools, or contribute directly) among package manager users/package publishers

## Non-goals
- Become the sole centralized maintainers of large new package manager infrastructure
- Hack together a demo to “subvert” package manager maintainers or “steal control” from package creators
- Implement YAPM (yet another package manager)

## Starter reading

There's a wealth of research, analysis and other tasty info to be found in the [docs directory](docs), but if you're just getting started with package managers and IPFS, you may want to start with these:

- [Package management glossary](docs/glossary.md)
- [Package manager categories](docs/categories.md)
- [Problems with current-state package managers that IPFS can help solve](docs/problems.md)
- [How IPFS concepts map to package manager concepts](docs/concepts.md)
- [IPFS Package Manager Upgrade Paths slide deck](docs/ipfs-package-managers-upgrade-paths.pdf)
- [Finding the best abstractions for talking about package managers](docs/abstractions.md)
- [Tactical tree for implementing IPFS in a package manager](docs/tree.md)
- [Package manager (pre-IPFS) pain points, sorted by user type](https://app.mural.co/t/protocollabs6957/m/protocollabs6957/1557168696127/577c9453a3c51199c8163cf0fe5701294e55f99b)
- [Package manager goals in the 2019 IPFS roadmap](https://github.com/ipfs/roadmap#-package-managers-d1-e5-i3)
- [Directory of existing package managers](package-managers) with notes on language, categorization, clients and more

## Current IPFS integrations

These package managers already have some form of IPFS integration underway. If you're a package manager maintainer and want to be in this list, please reach out. :smile:

- [dpkg](package-managers/dpkg.md#existing-ipfs-support)
- [f-droid](package-managers/f-droid.md#existing-ipfs-support)
- [go](package-managers/go.md#existing-ipfs-support)
- [guix](package-managers/guix.md#existing-ipfs-support)
- [homebrew](package-managers/homebrew.md#existing-ipfs-support)
- [nix](package-managers/nix.md#existing-ipfs-support)
- [npm](package-managers/npm.md#existing-ipfs-support)
- [pacman](package-managers/pacman.md#existing-ipfs-support)
- [portage](package-managers/portage.md#existing-ipfs-support)
- [pypi](package-managers/pypi.md#existing-ipfs-support)
- [rpm](package-managers/rpm.md#existing-ipfs-support)
- [rubygems](package-managers/rubygems.md#existing-ipfs-support)
- [wapm](package-managers/wapm.md#existing-ipfs-support)

## Blockers

There are still some [current issues with IPFS](docs/blockers.md) that are limiting package manager adoption today.

## Task force members
- [@achingbrain](https://github.com/achingbrain)
- [@aschmahmann](https://github.com/aschmahmann)
- [@dirkmc](https://github.com/dirkmc)
- [@djdv](https://github.com/djdv)
- [@stebalien](https://github.com/Stebalien)

## License

All documents are licensed under the [CC-BY-SA 3.0](https://ipfs.io/ipfs/QmVreNvKsQmQZ83T86cWSjPu2vR3yZHGPm5jnxFuunEB9u) license © 2019 Protocol Labs Inc. Any code is under an [MIT license](LICENSE).
