# How IPFS Concepts map to package manager concepts

## The address of a package

HTTP is the most popular method for downloading the actual contents of a package, given a url, the package manager client makes a http request and the response body is the package contents, which gets saved to disk.

The package url usually contains the registry domain, package name and version number:

    http://package-manager-registry.com/package-name/1.0.0.tar.gz

When you want to download a package using IPFS, rather than a url that contains the name and version, instead you provide a cryptographic hash of the contents that you’d like to recieve, for example:

    /ipfs/QmTeHfjrEfVDUDRootgUF45eZoeVxKCy3mjNLA8q5fnc1B

You may notice, that unlike with the URL, there is no domain name, because that hash uniquely describes the contents, IPFS doesn’t need to load it from a particular server, it can request that package from anyone else who is sharing it on IPFS without worrying if it has been tampered with.

## Indexing packages

To work out the address of a package that you’d like to download, first you need to find out a few details about it, this is done using a package index, which is one of the primary roles of a registry.

Let’s say you already know the name of the package that you’d like to grab, “libfoobar”, to construct a http url your now only missing one part, the version number. Registries usually provide a way of finding out what the available version numbers for a package are. One way is with a JSON API over http:

    http://package-manager-registry.com/libfoobar/versions   

That returns a list of available version numbers:

```
[
  {
    "number": "0.0.1",
    ...
  },
  {
    "number": "0.2.0",
    ...
  },
  {
    "number": "1.0.0",
    ...
  },
  {
    "number": "1.0.1",
    ...
  }
]
```


To enable clients to download packages over IPFS you can provide the cryptographic hash (CID) of the contents of each version of the package along with it’s number:

```
[
  {
    "number": "0.0.1",
    "cid": "/ipfs/QmTeHfjrEfVDUDRootgUF45eZoeVxKCy3mjNLA8q5fnc1B"
    ...
  },
  {
    "number": "0.2.0",
    "cid": "/ipfs/QmTeHfj83a09e6b0da3a6e1163ce53bd03eebfc1c507ds"
    ...
  },
  {
    "number": "1.0.0",
    "cid": "/ipfs/QmTeHfjf778979bb559fbd3c384d9692d9260d5123a7b3"
    ...
  },
  {
    "number": "1.0.1",
    "cid": "/ipfs/QmTeHfjrEfVDUDRootgUFc9cbd8e968edfa8f22a33cff7"
    ...
  }
]
```

IPFS doesn’t just have to store the package contents, it can also store the JSON list of available versions.

```
$ ipfs add versions.json
=> QmctG9GhPmwyjazcpseajzvSMsj7hh2RTJAviQpsdDBaxz
```

Doing this introduces some challenges, for one thing the CID of a list of versions for a package isn’t human readable, so needs a second way of finding that CID.

One way to solve that would be to create another index of all package names with the CID of the json file of list of versions for that package:

```
[
  {
    "name": "libfoo",
    "versions": "/ipfs/QmTeHfjrEfVDUDRootgUF45eZoeVxKCy3mjNLA8q5fnc1B"
    ...
  },
  {
    "name": "libbar",
    "versions": "/ipfs/QmTeHfj83a09e6b0da3a6e1163ce53bd03eebfc1c507ds"
    ...
  },
  {
    "name": "libbaz",
    "versions": "/ipfs/QmTeHfjf778979bb559fbd3c384d9692d9260d5123a7b3"
    ...
  }
]
```

You could create these linked json files manually but IPFS already has a technology called IPLD.

Another challenge is that every time there’s a new version of “libfoobar” released, the contents of versions.json changes, which produces a different hash when added to IPFS, the index of all the package names can be updated after each release as well, producing a merkle tree of package data

But of course then the indexes of all package names has the same problem, it gets updated after any package has a new version released.

There are a couple of IPFS technologies to help with this: IPNS and DNSLink.

IPNS allows you to create a mutable link to content on IPFS, you can think of a IPNS name as a pointer to an IPFS hash, which may change in the future to point to a different IPFS hash.

For example we could use IPNS to point to the hash of a json file of released versions of libfoo, taking the CID of versions.json and publishing it to IPNS:

```
    $ ipfs name publish QmctG9GhPmwyjazcpseajzvSMsj7hh2RTJAviQpsdDBaxz
    => Published to QmSRzfkzkgofxg2cWKiqhTQRjscS4DC2c8bAD2TbECJCk6: /ipfs/QmctG9GhPmwyjazcpseajzvSMsj7hh2RTJAviQpsdDBaxz
```

Now we can use the IPNS address to load versions.json:

```
$ ipfs cat /ipns/QmSRzfkzkgofxg2cWKiqhTQRjscS4DC2c8bAD2TbECJCk6
=> [
    {
      "number": "0.0.1",
      "cid": "/ipfs/QmTeHfjrEfVDUDRootgUF45eZoeVxKCy3mjNLA8q5fnc1B"
      ...
    },
    ...
   ]
```

After a new version of libfoo is published, we can add the tarball to IPFS, edit versions.json to include it:

```
[
  {
    "number": "0.0.1",
    "cid": "/ipfs/QmTeHfjrEfVDUDRootgUF45eZoeVxKCy3mjNLA8q5fnc1B"
    ...
  },
  {
    "number": "0.2.0",
    "cid": "/ipfs/QmTeHfj83a09e6b0da3a6e1163ce53bd03eebfc1c507ds"
    ...
  },
  {
    "number": "1.0.0",
    "cid": "/ipfs/QmTeHfjf778979bb559fbd3c384d9692d9260d5123a7b3"
    ...
  },
  {
    "number": "1.0.1",
    "cid": "/ipfs/QmTeHfjrEfVDUDRootgUFc9cbd8e968edfa8f22a33cff7"
    ...
  },
  {
      "number": "2.0.0",
    "cid": "/ipfs/Qm384d9692d9260d5123a7b3UFc9cbd8e968edfa8f2233"
    ...
  }
]
```

and then add the updated versions.json to IPFS:

```
$ ipfs add versions.json
=> added QmcCmvc9K7fVeY9xQEj3xoa9HWnxs2M5zX97wTAvTQ61a9 versions.json
```

Then we can update that same IPNS address to point to the new copy of versions.json:

```
$ ipfs name publish QmcCmvc9K7fVeY9xQEj3xoa9HWnxs2M5zX97wTAvTQ61a9
=> Published to QmSRzfkzkgofxg2cWKiqhTQRjscS4DC2c8bAD2TbECJCk6: /ipfs/QmcCmvc9K7fVeY9xQEj3xoa9HWnxs2M5zX97wTAvTQ61a9
```

The same IPNS address now points to the new versions.json file:

```
$ ipfs cat /ipns/QmSRzfkzkgofxg2cWKiqhTQRjscS4DC2c8bAD2TbECJCk6
=> [
    {
      "number": "0.0.1",
      "cid": "/ipfs/QmTeHfjrEfVDUDRootgUF45eZoeVxKCy3mjNLA8q5fnc1B"
      ...
    },
    ...
    {
      "number": "2.0.0",
      "cid": "/ipfs/Qm384d9692d9260d5123a7b3UFc9cbd8e968edfa8f2233"
      ...
    }
  ]
```

DNSLink gives you similar functionality to IPFS but adds a dependency to DNS. It works by using a domain name instead of a hash, for example:

    /ipns/package-manager-registry.com

To configure that domain name to point to a specific IPFS hash, you add a TXT dns record with content in the following form:

    dnslink=/ipfs/<CID for your content here>

When requesting content from a DNSLink, IPFS will look up the TXT record on that domain name and then fetch the content for the CID stored within it.

DNSLink can be useful for adding human readable names as well as adding a layer of social trust with users already familiar with your domain name, and at the time of writing DNSLink is quite a bit faster that using IPNS.

One downside of DNSLink is that updating a DNS record every time you wish to make a change can be fiddly, not every DNS provider has an API that can be used to automate the action and DNS propagation can take hours for changes to be updated world wide in some cases.

IPNS names on the other hand have the added benefit that they work purely using IPFS technology and can be resolved without needing any traditional infrastructure, as well as working “offline”.
Checking for new versions of packages

## Publishing packages

TODO

## Verifying package contents

TODO
