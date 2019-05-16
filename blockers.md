# Blockers

Issues with IPFS that are limiting package manager adoption today

## File system based package managers

### Requires 2x disk space for mirroring

Filestore expects files to be immutable once added, so rsyncing updates to existing files [causes errors](https://github.com/protocol/package-managers/issues/18#issuecomment-471365124), one workaround is to not use the filestore but that requires copying the entire mirror directory into `.ipfs`.

### No easy way to directly add a directory to MFS with go-ipfs

Adding a directory of files to MFS means calling out to `ipfs files write` for every file, ideally there should be one command to write a directory of files to MFS.

Alternative approach may be to mount MFS as a fuse filesystem (ala https://github.com/tableflip/ipfs-fuse) 

### Updating rolling changes requires rehashing all files

If there is a regular cron job downloading updates to a mirror with rsync, there's currently no easy way to only re-add the files that have been added/changed/removed without rehashing every file in the whole mirror directory. 

Mounting MFS as a fuse filesystem (ala https://github.com/tableflip/ipfs-fuse) and rsyncing directly onto fuse may be one approach. 

Alternatively there could be a ipfs rysnc command line tool that could talk directly with rsync server protocol.

## IPFS (go-ipfs) may kill routers

https://github.com/ipfs/go-ipfs/issues/3320
