# File System Based Package Managers on IPFS

## OKR

TODO: add details about current focus for [Q3 2019](https://github.com/ipfs/package-managers/issues/69) on FSBs

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

TODO: Summarise importing details from [#18](https://github.com/ipfs/package-managers/issues/18) and [#19](https://github.com/ipfs/package-managers/issues/19)

### Setup

rsync from a nearby mirror to a local directory (`/data/apt` for example)

For apt, this directory was approximately 1.2TB in total size after rsync completed which took approximately 3.5 hours.

Total directory count: 58,765
Total files count: 912,830

### Experiment 1

<details><summary>Expand for details</summary>
<p>

Tested using go-ipfs 0.4.19

1. set up the ipfs repo, set config (based on [ipfs/notes/issues/212](https://github.com/ipfs/notes/issues/212)) and start the daemon running:

```shell
$ export IPFS_PATH=/data/.ipfs
$ export IPFS_FD_MAX=4096

$ ipfs init

$ ipfs config Reprovider.Interval "0"
$ ipfs config --json Datastore.NoSync true
$ ipfs config --json Experimental.ShardingEnabled true
$ ipfs config --json Experimental.FilestoreEnabled true

$ ipfs daemon
```

2. add the directory to IPFS

```shell
$ time ipfs add -r --progress --offline --fscache --quieter --raw-leaves --nocopy /data/apt
```

This took approximately 12 hours to complete successfully

3. Inspect the ipfs repo stats:

```shell
$ ipfs stats repo --human
```

```
NumObjects:       5824574
RepoSize (MiB):   1086
StorageMax (MiB): 9536
RepoPath:         /data/.ipfs
Version:          fs-repo@7
```

</p>
</details>

### Experiment 2

<details><summary>Expand for details</summary>
<p>

Tested using go-ipfs 0.4.19

1. set up the ipfs repo, set config (based on [ipfs/notes/issues/212](https://github.com/ipfs/notes/issues/212)) and start the daemon running:

```shell
$ export IPFS_PATH=/data/.ipfs
$ export IPFS_FD_MAX=4096

$ ipfs init --profile=badgerds

$ ipfs config Reprovider.Interval "0"
$ ipfs config --json Datastore.NoSync true
$ ipfs config --json Experimental.ShardingEnabled true
$ ipfs config --json Experimental.FilestoreEnabled true

$ ipfs daemon
```

2. add the directory to IPFS

```shell
$ time ipfs add -r --progress --offline --fscache --quieter --raw-leaves --nocopy /data/apt
```

This took approximately 18 hours to complete successfully

3. Inspect the ipfs repo stats:

```shell
$ ipfs stats repo --human
```

```
NumObjects:       5824574
RepoSize (MiB):   1825
StorageMax (MiB): 9536
RepoPath:         /data/.ipfs
Version:          fs-repo@7
```

</p>
</details>

## Experiment 3

<details><summary>Expand for details</summary>
<p>

Tested using go-ipfs 0.4.19

1. set up the ipfs repo, set config (based on [ipfs/notes/issues/212](https://github.com/ipfs/notes/issues/212)) and start the daemon running:

```shell
$ export IPFS_PATH=/data/.ipfs
$ export IPFS_FD_MAX=4096

$ ipfs init

$ ipfs config Reprovider.Interval "0"
$ ipfs config --json Datastore.NoSync true
$ ipfs config --json Experimental.ShardingEnabled true

$ ipfs daemon
```

2. add the directory to IPFS:

```shell
$ ipfs add -r --progress --offline --quieter --raw-leaves /data/apt
```
This took approximately 36 hours to complete successfully

</p>
</details>



## Updating

When the upstream repository is updated, mirrors need to download the files that have been updated and add them to IPFS.

## Experiment 1

When attempting to add the updated files with `--nocopy` and `--fscache` (Filestore), you always encounter an error. This is due to the [current behaviour](https://github.com/ipfs/go-ipfs/issues/5734#issuecomment-492931782) of the Filestore, which expects that once a file has been added, it will not change.

This means that for our test case of rsyncing updates from upstream and adding to IPFS can't currently use `--nocopy`

Related issue: https://github.com/ipfs/go-ipfs/issues/4603

<details><summary>Expand for details</summary>
<p>

```
$ ipfs add -r --progress --offline --fscache --quieter --raw-leaves --nocopy /data/apt
badger 2019/03/10 13:41:56 INFO: All 8 tables opened in 1.692s                                                                                  badger 2019/03/10 13:41:56 INFO: Replaying file id: 7 at offset: 43557796                                                                       badger 2019/03/10 13:41:56 INFO: Replay took: 12.022µs                                                                                           1.07 TiB / 1.18 TiB [============================= 1.18 TiB / 1.18 TiB [==================================================================================================================================] 100.00%^[[B^[[B^[[B^[[B^[[BQmQsQ9mtDXu5NTeXpinXuPUjy3nMbCi5rLfrycbf9rDdvh
Error: failed to get block for zb2rhjn4bxfqtxZrzfNYyQgm1EvKHcRket2TbdR6Y2L46zax3: data in file did not match. apt/dists/disco/universe/debian-installer/binary-i386/Packages.gz offset 0
badger 2019/03/10 17:02:14 INFO: Storing value log head: {Fid:7 Len:48 Offset:56345495}
badger 2019/03/10 17:02:17 INFO: Force compaction on level 0 done

real    200m23.176s
user    101m42.635s
sys     12m36.793s
```

</p>
</details>

## Experiment 2

## End user installation

TODO: Summarise installation details from [#18](https://github.com/ipfs/package-managers/issues/18) and [#19](https://github.com/ipfs/package-managers/issues/19)

## Flags and config

TODO: Explain that when different flags are used whilst importing files into IPFS that the hashes produced may be different, we want the default behaviour for people setting up mirrors to use the best flags and to all use the same flags, likely via a package manager specific profile.

### `ipfs add` flags

`-r`: recursive - required to add a directory and it's contents to IPFS (perhaps should be a pacakge-manager profile default).

`--fscache`: Check the filestore for pre-existing blocks. Improves performance if using the filestore (not an option for FSBPMs at the moment), doesn't do anything if not using the filestore (requires `$ ipfs config --json Datastore.NoSync true` before use).

`--cid-version`: CID version. Defaults to 0 unless an option that depends on CIDv1 is passed (perhaps `--cid-version=1` should be a pacakge-manager profile default).

`--nocopy`: Add the file using filestore. (requires `$ ipfs config --json Datastore.NoSync true` before use).

`--raw-leaves`: Use raw blocks for leaf nodes, enabled by default if using `--nocopy` or `--cid-version=1` (perhaps should be a pacakge-manager profile default).

`--hidden`: Include files that are hidden. Only takes effect on recursive add (`-r`).

`--trickle`: Use trickle-dag format for dag generation.

`--wrap-with-directory`: Wrap files with a directory object. (not required when adding a directory)

`--chunker`:  Chunking algorithm, size-[bytes] or rabin-[min]-[avg]-[max].

`--hash`: Hash function to use. Implies CIDv1 if not sha2-256.

`--inline`: Inline small blocks into CIDs (perhaps should be a pacakge-manager profile default).

`--inline-limit`:  Maximum block size to inline.

### `ipfs files write` flags

`--create`: Create the file if it does not exist (perhaps should be a pacakge-manager profile default).

`--parents`: Make parent directories as needed (perhaps should be a pacakge-manager profile default).

`--raw-leaves`: Use raw blocks for leaf nodes, enabled by default if using `--cid-version=1` (perhaps should be a pacakge-manager profile default).

`--cid-version`: CID version. Defaults to 0 unless an option that depends on CIDv1 is passed (perhaps `--cid-version=1` should be a pacakge-manager profile default).

`--hash`: Hash function to use. Implies CIDv1 if not sha2-256.

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

- [Improve performance of adding package-manager sized directories to go-ipfs](https://github.com/ipfs/package-managers/issues/77)
- [add an experimental version of `mount` to go-ipfs](https://github.com/ipfs/package-managers/issues/74)
- [unixfs v2](https://github.com/ipfs/roadmap/issues/19)
