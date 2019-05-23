# Problems with Package Managers

I thought it would be an interesting thought experiment to outline problems that package publishers, package consumers and package manager maintainers currently experience today, rather than outlining the benefits of IPFS and looking for places where those benefits can be applied to package management.

Doing this may highlight areas where introducing IPFS can provide significant improvements that wouldn't be a clear headline feature of IPFS.

For example, IPFS can offer large savings in bandwidth costs, but almost all the large community  package managers are offered free CDN and hosting services by Fastly, Cloudflare, Bintray etc, so that package managers don't see that as being a problem right now.

Not all problems will be present in all package managers and IPFS definitely won't be able to fix all problems either but this list may spark some interesting ideas that would otherwise have been missed, as well as helping us to focus our efforts on the key problems that IPFS can help with.

Feel free to add in other problems or group similar problems together, this list is off the top of my head and will likely be missing loads things.

## Package consumers

- Package releases being removed from registries
- Package maintainers transferring ownership to unknown entities
- Registry downtime stops building/developing of software that is dependent on it
- Not being able to reproduce a known working set of dependencies at a later date
- Not being able to opt-out of using new releases which have breaking changes
- Not being able to update/fix/swap old packages deep within a dependency tree when they cause problems
- Not being able to resolve conflicting dependency requirements when building a dependency tree
- Not knowing the status of a package (deprecated, unmaintained, broken etc)
- Not being able to confirm that the contents of a download package is the same as was originally published by the author
- Not being able to install more than one version of a dependency at once
- Not being able to install or build packages whilst offline
- Not being able to efficiently review the downloaded code within each dependency of an application
- Not being able to filter packages by compatible licenses
- Not being able to validate the license/copyright/patent details of a package
- Hosting internal or private mirrors of registries is time consuming and requires ongoing maintenance and infrastructure costs
- Language level packages that depend system level packages do not communicate or automate those dependency links effectively
- Not being able to find new packages that are relevant to consumers interests/needs
- Difficult to know when new releases of packages are published
- Difficult to known if a new release of a package is stable

## Package Producers

- Communicating with consumers of packages
- Maintaining compatibility with multiple platforms, architectures and run times
- Vetting contributions for security concerns
- Coordinating key signing infrastructure between maintainers is time consuming and error prone
- Difficult to test package against a range of different versions of upstream and downstream dependencies
- Difficult to know what percentage of consumers are using newer releases vs stuck on old releases due to incompatibilities
- Difficult to discourage users from using broken/deprecated releases

## Package Manager Maintainers

- Difficulties funding maintenance, development and infrastructure costs
- Large amount of trust placed on very small amount of maintainers
- Heavy support burden from both consumers and producers
- Having to police illegal/pirate/malicious packages from the registry
- Recovery of data after loss due to server failure or security breach
- Deploying significant changes can result in community backlash
- Difficulties communicating with package consumers and producers
- Difficult to hand over control/trust when maintainers step down
- Distributing infrastructure globally can be costly/complex
- Greatly increased infrastructure costs when storing binaries as well as source code
- Package managers often can't use themselves for managing dependencies, due to bootstrapping issues, resulting in duplicate efforts and reduced productivity
