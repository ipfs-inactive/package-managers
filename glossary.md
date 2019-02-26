# Package Management Glossary

A glossary of terms relating to package management.

## Application
  The end-user software project that utilises other software projects as dependencies but is not something that can be directly depended upon or published as a package.

## Binary Distribution
  A specific kind of Built Distribution that contains compiled extensions.

## Built Distribution
  A Distribution format containing files and metadata that only need to be moved to the correct location on the target system, to be installed.

## Client
  The locally installed software for installing and managing packages, usually provided as a command line interface. Often communicates with one or more registries to find package and release metadata and to download releases.

## Dependency
  The declaration of the dependency of one package on another.

## Dependency Tree
  A graph of all the resolved dependencies of a single package or manifest.

## Dependency Resolution Strategy
  The strategy used to decide which releases of each package should be picked to produce the complete dependency graph for a package or manifest.

## Dependency Specifier
  A format used to install packages from a Registry Index. For example, “foo>=1.3” is a dependency specifier, where “foo” is the project name, and the “>=1.3” portion is the Version Specifier

## Distribution Package
  A versioned archive file that contains packages, modules, and other resource files that are used to distribute a Release. The archive file is what an end-user will download from the internet and install.

  A distribution package is more commonly referred to with the single words “package” or “distribution”, but this guide may use the expanded term when more clarity is needed to prevent confusion with an Import Package (which is also commonly called a “package”) or another kind of distribution (e.g. a Linux distribution or a language distribution), which are often referred to with the single term “distribution”.

## Extension Module
  A module written in a low-level language of the implementation: C/C++, Java, Fortran. Typically contained in a single dynamically loadable pre-compiled file, e.g. a shared object, a DLL on Windows, or a Java class file.

## Import Package
  A module which can contain other modules or recursively, other packages.

  An import package is more commonly referred to with the single word “package”, but this guide will use the expanded term when more clarity is needed to prevent confusion with a Distribution Package which is also commonly called a “package”.

## Lockfile
  The file where a successfully resolved dependency tree is recorded so that it can be recreated at a later date.

## Manifest
  The file where a package's required dependencies are declared, often with version specifiers.

## Project
  A library, framework, script, plugin, application, or collection of data or other resources, or some combination thereof that is intended to be packaged into a Distribution.

  Projects have unique names, which are registered on a registry. Each project will then contain one or more Releases, and each release may comprise one or more distributions.

 There is a strong convention to name a project after the name of the package that is imported to run that project. However, this doesn’t have to hold true. It’s possible to install a distribution from the project ‘foo’ and have it provide a package importable only as ‘bar’.

## Registry Index
  A database of projects and their releases, provides information to a dependency resolution strategy. Often paired with a web interface for searching and viewing information on various packages.

## Release
  A snapshot of a Project at a particular point in time, denoted by a version identifier.

  Making a release may entail the publishing of multiple Distributions. For example, if version 1.0 of a project was released, it could be available in both a source distribution format and a Windows installer distribution format.

## Source Archive
  An archive containing the raw source code for a Release, prior to creation of an Source Distribution or Built Distribution.

## Source Distribution
  A distribution format that provides metadata and the essential source files needed for installing or for generating a Built Distribution.

## Source Repository
  The version control repository where development of a software project is co-ordinated, often hosted on GitHub, GitLab or Bitbucket.

## System Package
  A package provided in a format native to the operating system, e.g. an rpm or dpkg file.

## Transitive Dependency
  A transitive dependency is one that wasn't directly depended on by a package but instead the dependencies of one of a package's declared dependencies.

## Version Specifier
  The version component of a Dependency Specifier. It may be in the form of a single specific version or an acceptable range of versions (For example, the “>=1.3” portion of “foo>=1.3”)

## Versioning scheme
  The format and meaning applied to each release version number, often in the format X.Y.Z. Two popular schemes are https://semver.org and http://calver.org.

## Virtual Environment
  An isolated environment that allows packages to be installed for use by a particular application, rather than being installed system wide.
