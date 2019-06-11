# Category-based approaches for implementing IPFS support

Some possible approaches for implementing IPFS support based on implementation category.

## File system-based

### Approach 1

Mirroring these registries into MFS and adding the root CID to dnslink/ipns then rsyncing updates on a regular basis along with transport plugins like https://github.com/JaquerEspeis/apt-transport-ipfs

#### Problems

- Performance of adding/update large registries to MFS takes many hours, causing mirrors to lag behind the source
- updating indexes files like Packages.gz in MFS isn't supported with the filestore

## Database-based

### Approach 1

IPFS support directly in mainline registries:

- On package upload, resulting artifacts are added to IPFS and CID is stored and served alongside other metadata
- Clients receive CIDs along with other metadata from registry APIs and use IPFS to download artifacts
- Clients store resolved CIDs in lockfiles to allow for  installation

#### Problems

Requires direct buy-in from package manager maintainers

### Approach 2

IPFS Wrappers for package manager clients:

- Separate client command line tool that calls out to underlying package manager but downloads/adds artifacts to IPFS and places them where original client would expect
- some package manager clients have plugin architectures that allow for more integration with client without requiring upstream maintainers to take on the responsibility

#### Problems

- Much smaller uptake in users than direct integration
- wrappers tend to only run ipfs daemon for short periods, doesn't help increase number of artifact seeders

### Approach 3

HTTP proxy for package manager clients:

- proxy http requests from clients to registries and cache to IPFS
- cache is lazy, only storing what's requested rather than whole mirrors, requiring fewer resources
- proxied can be ran on individual's computers or hosted within datacenters or offices for groups of developers in an organsiaton

#### Problems

- metadata requests can't be aggressively cached due to frequent upstream changes
- proxy addresses often stored in lockfiles, making personal proxies unattractive to developers working in groups or open source

## Git-based

TODO
