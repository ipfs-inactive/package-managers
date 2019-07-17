# File System Based Package Managers on IPFS

WIP

## Definition

## List of FSB package managers

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

## References

- [Experiment: Setting up an Ubuntu mirror on IPFS](https://github.com/ipfs/package-managers/issues/18)
- [Experiment: Setting up a Clojars mirror on IPFS](https://github.com/ipfs/package-managers/issues/19)
- [Q3 OKRs](https://github.com/ipfs/package-managers/issues/69)
- [Improve performance of adding package-manager sized directories to go-ipfs](https://github.com/ipfs/package-managers/issues/77)
- [add an experimental version of `mount` to go-ipfs](https://github.com/ipfs/package-managers/issues/74)
- [unixfs v2](https://github.com/ipfs/roadmap/issues/19)
- [docs/blockers.md](https://github.com/ipfs/package-managers/blob/master/docs/blockers.md)
