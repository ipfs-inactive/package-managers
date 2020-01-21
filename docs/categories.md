# Package Manager categories

## Centralized Registry
  Client, tooling and community are focused primarily on a single registry and namespace for all packages, public mirrors and alternative registries are infrequent. Examples: npm, bower, rubygems, pypi

## Portable Registry
  Usually similar to Centralized Registry but with one standout feature, users keep a local copy of the whole registry, often as a git repository, which is regularly synced by the users. Examples: Homebrew, Cocoapods, Cargo, Spack

## Multi-Registry
  Clients are designed to be used with multiple primary registries, users have choices of multiple registries to publish to and mirrors are frequently used. Examples: Maven, apt, rpm, ports

## Registry-less
  There is no concept of a registry or namespace where packages are published to, clients directly link to tarballs or git repositories on any urls, resolved with DNS. Examples: Go, carthage, swiftpm

## Implementation categories

### File-system based

This maps closely to the Multi-Registry category.

Many system package managers (APT, apk, RPM, ports), plus some of the older language package managers (Maven, CPAN, CRAN) are literally a network attached folder full of files and other folders, often exposed over http, ftp etc.

Metadata is also stored as files so everything is quite self-contained and easily mirrored using rsync.

### Database based

This maps closely to the Centralized Registry category.

Many newer language package managers (rubygems, npm, Packagist, PyPI, NuGet, Cargo, bower) run a database-backed web application that handles authentication, uploading, provides APIs for the clients to list packages and versions and other metadata on demand.

Actual package contents is often hosted on s3 and requests to download packages may be proxied through the application to track download statistics, or redirected to a CDN link.

Mirroring these package managers usually requires either trawling package list APIs and recursively downloading packages and their metadata, or, if the registry provides it, downloading a dump of the registries database and running a copy of the web application or a slimmed down alternative that provides a similar API.

Unlike the file-system based package managers, almost every database based registry implements it's own, unique API for querying metadata and publishing packages.

This may explain why there are generally less public mirrors available for Centralized/database-backed package managers as there's a lot more overhead and complexity in keeping a mirror up to date than in the file-system based registries.

The one shared attribute they all share is that clients communicate with registries remotely via https urls.

### Git based

#### Portable Registries

A number of Portable registries (Homebrew, CocoaPods, Spack) use Git (usually GitHub) as a database rather than a traditional centralized database, often as a way to avoid becoming a full time, on-call DBA for their community.

One notable exception is Cargo's main registry, https://crates.io, which has parts of both a portable registry in https://github.com/rust-lang/crates.io-index and central database in https://github.com/rust-lang/crates.io

With Homebrew, PRs can be opened directly on their GitHub repository database to add and update Formula files, which contain the metadata for a package, including links to the source and compiled binaries with integrity hashes.

With CocoaPods, you used to be able to send PRs to their GitHub repository database but after they merged a PR publishing a new version by someone who shouldn't have been able to, they implemented a separate web service called Trunk to handle adding new version to the git database.

Similarly with Crates.io, their GitHub repository database is updated automatically whenever someone publishes a new version via the crates.io website.

In all three of these cases, the end user only ever uses data from the latest commit that they have locally, tags and branches are not utilized and in the case of Cocoapods and Cargo, the history of the repository is not used for previous versions, in fact end users often do a shallow clone (git clone --depth 1 so don't even have history data to go back on.

Homebrew only keeps the latest version of a Formula in it's database, but does clone the full repository (243.22 MiB as of February 2019) so users can check out previous revisions of the repository to install old versions of a Formula, although it's not encouraged (Formula cannot declare a particular version of a dependency) because the speed of operations on the large repo are slow.

#### Registry-less

Some package managers (Go, Carthage, Swiftpm) do away with hosted registries all together, instead preferring to declare dependencies as fully qualified http urls or shortcuts for GitHub urls (owner/repo-name for example).

These package managers often have support for checking to see if the url is a git repository and then querying for git tags as the list of published, named versions, and git commits as a full list of all possible versions (named by git commit sha and/or branch).

The end result is instead of a single registry, these package managers have thousands of single project registries, and rely on APIs in Git and/or GitHub for metadata on releases.
