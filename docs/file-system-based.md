# File System Based Package Managers on IPFS

WIP

## Definition

Clients are designed to be used with multiple primary registries, users have choices of multiple registries to publish to and mirrors are frequently used. Examples: Maven, apt, rpm, ports

Many system package managers (APT, apk, RPM, ports), plus some of the older language package managers (Maven, CPAN, CRAN) are literally a network attached folder full of files and other folders, often exposed over http, ftp etc.

Metadata is also stored as files so everything is quite self-contained and easily mirrored using rsync.

Mirroring these registries into MFS and adding the root CID to dnslink/ipns then rsyncing updates on a regular basis along with transport plugins like https://github.com/JaquerEspeis/apt-transport-ipfs

- Performance of adding/update large registries to MFS takes many hours, causing mirrors to lag behind the source
- updating indexes files like Packages.gz in MFS isn't supported with the filestore

## List of File System Based Linux package managers

Below is a table of the most widespread linux distributions and their main package repositories, including size, update frequency, methods of mirroring, and lists of mirrors, with the aim of looking for repeating patterns that we can focus on supporting.

All of the popular linux package managers use regular rsync runs for mirroring, sometimes via a custom script that checks a last modified file time before attempting to download updates.

| Distributions                                                       | Type                                  | Size  | Frequency | Method                                                                            | Docs                                                                        |
|---------------------------------------------------------------------|---------------------------------------|-------|-----------|-----------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| [Debian](https://www.debian.org/mirror/list)                        | [dpkg](../package-managers/dpkg.md)   | 2.4TB | 6 Hours   | [ftpsync](https://salsa.debian.org/mirror-team/archvsync/blob/master/bin/ftpsync) | https://www.debian.org/mirror/ftpmirror                                     |
| [Ubuntu](https://launchpad.net/ubuntu/+archivemirrors)              | [dpkg](../package-managers/dpkg.md)   | 1.1TB | 6 Hours   | [rsync](https://wiki.ubuntu.com/Mirrors/Scripts)                                  | https://wiki.ubuntu.com/Mirrors                                             |
| [Fedora](https://admin.fedoraproject.org/mirrormanager/)            | [rpm](../package-managers/rpm.md)     | 1.5TB | 10 mins   | [quick-fedora-mirror](https://pagure.io/quick-fedora-mirror)                      | https://fedoraproject.org/wiki/Infrastructure/Mirroring                     |
| [CentOS](https://www.centos.org/download/mirrors/)                  | [rpm](../package-managers/rpm.md)     | 400GB | 6 Hours   | rsync                                                                             | https://wiki.centos.org/HowTos/CreatePublicMirrors                          |
| [FreeBSD](https://www.freebsd.org/doc/handbook/eresources-web.html) | [ports](../package-managers/ports.md) | 1.4TB | 24 Hours  | rsync                                                                             | https://www.freebsd.org/doc/en_US.ISO8859-1/articles/hubs/mirror-howto.html |
| [openSUSE](https://mirrors.opensuse.org/)                           | [rpm](../package-managers/rpm.md)     | 3TB   | 6 Hours   | rsync                                                                             | https://en.opensuse.org/openSUSE:Mirror_howto                               |
| [Arch](https://www.archlinux.org/mirrorlist/all/)                   | [ports](../package-managers/ports.md) | 130GB | 1 Hour    | rsync                                                                             | https://wiki.archlinux.org/index.php/Mirrors                                |
| [Gentoo](https://www.gentoo.org/support/rsync-mirrors/)             | [ports](../package-managers/ports.md) | 550GB | 4 Hours   | rsync                                                                             | https://wiki.gentoo.org/wiki/Project:Infrastructure/Mirrors/Source          |
| [Alpine](https://mirrors.alpinelinux.org/)                          | [apk](../package-managers/apk.md)     | 560GB | 1 Hour    | rsync                                                                             | https://wiki.alpinelinux.org/wiki/How_to_setup_a_Alpine_Linux_mirror        |

## Initial import

## Updating

## End user installation

## Flags

## Blockers

Issues with IPFS that are limiting FSB package manager adoption today

### Requires 2x disk space for mirroring

Filestore expects files to be immutable once added, so rsyncing updates to existing files [causes errors](https://github.com/protocol/package-managers/issues/18#issuecomment-471365124), one workaround is to not use the filestore but that requires copying the entire mirror directory into `.ipfs`.

### No easy way to directly add a directory to MFS with go-ipfs

Adding a directory of files to MFS means calling out to `ipfs files write` for every file, ideally there should be one command to write a directory of files to MFS.

Alternative approach may be to mount MFS as a fuse filesystem (ala https://github.com/tableflip/ipfs-fuse)

### Updating rolling changes requires rehashing all files

If there is a regular cron job downloading updates to a mirror with rsync, there's currently no easy way to only re-add the files that have been added/changed/removed without rehashing every file in the whole mirror directory.

Mounting MFS as a fuse filesystem (ala https://github.com/tableflip/ipfs-fuse) and rsyncing directly onto fuse may be one approach.

Alternatively there could be a ipfs rysnc command line tool that could talk directly with rsync server protocol.


## References

- [Experiment: Setting up an Ubuntu mirror on IPFS](https://github.com/ipfs/package-managers/issues/18)
- [Experiment: Setting up a Clojars mirror on IPFS](https://github.com/ipfs/package-managers/issues/19)
- [Q3 OKRs](https://github.com/ipfs/package-managers/issues/69)
- [Improve performance of adding package-manager sized directories to go-ipfs](https://github.com/ipfs/package-managers/issues/77)
- [add an experimental version of `mount` to go-ipfs](https://github.com/ipfs/package-managers/issues/74)
- [unixfs v2](https://github.com/ipfs/roadmap/issues/19)
- [docs/blockers.md](https://github.com/ipfs/package-managers/blob/master/docs/blockers.md)
