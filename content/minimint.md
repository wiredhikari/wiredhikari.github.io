
+++
title = "Building a Nix Module for MiniMint"
date = 2022-06-15

[taxonomies]
tags = ["sob" , "nixos" , "minimint" ]
+++
Overview of the process of writing nix modules for minimint and nix-bitcoin.
<!-- more -->

## Overview:
&emsp; MiniMint is an implementation of federated e-cash written in rust. 





To make deployment easier having a Nix module allowing to easily set up a federation. Nix allows to write entire system configurations as a single configuration file and to reproducibly build that system. As such it is well-suited for security-critical applications.
-  Give masala
-  Why nix
-  Start kahani
    -  We started with nix-shell,
    -  Then why we needed nix build
    -  Optimizing build times
    -  Using docker for non nix users
    -  isme likh kyu use kra aur pathway ki kaha se kaha gae and niche vale me bas us topic ke upar baat kr

---------------------------------------------

## Nix expression which builds Minimint's binary outputs

### Nix-Shell :
&emsp; With nix-shell, we can create virtual environments per-project. When you enter nix-shell, the environment and all the dependencies are ready for use.
- has all necessary resources for development 
- nix-shell benefits
- used for integration testing
#### Porting integration tests in nix-shell
- benefits
- why do that
- 

### Nix-Build :
&emsp; 
- to build minimint via nix
- tools we currently use in nix build
- benefits of nix build
#### 

### Niv
&emsp; 
- why dependancy management
- 
### Setting up cachix :
&emsp; 
- benefits of cachix
- how did we set it up
- current condition
- 

### Nix-Flakes :
&emsp;Flakes allow you to specify your code's dependencies in a declarative way, simply by listing them inside a flake.nix. Each dependency gets then pinned, that is: its commit hash gets automatically stored into a file - named flake.lock - making it easy to, say, upgrade it
&emsp;
- benefits of flakes
- how did we set it up
- current condition
- how to use it
- comparing with other nix tools
- added support for non linux things like mac
### crane
- multi arches support
- 
### Github Actions : 
&emsp; Currently I have setup a working CI that tests flakes, nix-build, nix-shell and does integrations tests. We wanted a stable rust builder which allows granular caching and also supports sys crates.

&emsp; There aren't any stable rust builders in nix yet and so after testing all popular ones out there a.k.a cargo2nix, ceate2nix, naersk, crane, dream2nix; we narrowed them down to cargo2nix and crane. Cargo2nix doesn't support building -sys crates yet and crane isn't as good at caching as cargo2nix. So looking at the trade-off, we are using crane with flakes. 
#### Github actions powered by nix and cachix

### Docker Containers :
 - why did we set it up?
 - how is it working?
 - illustrate 
 - 

## Nix module in nix-bitcoin for Minimint :
 - 

### Working with nix-bitcoin :

* how to setup
    * To run  use ./deploy-qemu-vm.sh -i
    * c
* prs and plans
* current condition
    * we dont have a minimint package in nixpkgs as we are in experimentation so we added it as a seperate package in nix-bitcoin itself
* references
* 

### Documentation
&emsp; 
#### Installation
&emsp; 
#### Process
&emsp; 
####



-----------------------------------------
## Future Work :
- glipmse of current work
    - optimizing ci is almost done just some addons
    - nix-bitcoin is the major task remaining

### Optimizing CI :
- current condition and additions
- testing for different arches
- see if we want to stick to github actions
- 

### nix-bitcoin :
- see how to make this work 

### Rigorous Testing and Documentation :
- testing on different machines like elsirion said
- thanks to nix we dont need to worry much about one thing breaking on different systems except we need to see for arches
- documenting

-----------------------------------------

## References

### Repositories I am working on:
* Minimint - https://github.com/fedimint/minimint
* Fork of nix-bitcoin on fedimint - https://github.com/fedimint/nix-bitcoin
* Dockerizing minimint -  https://github.com/fedimint/minimint-docker
* secp256k1_zkp_flake - https://github.com/wiredhikari/secp256k1_zkp_flake
* Blogs and Documentation - https://github.com/wiredhikari/nix_minimint

### Pull Requests:

* nix: adding nix shell support: [#45](https://github.com/fedimint/minimint/pull/45)
* Nix expression to build minimint’s binary outputs: [#60](https://github.com/fedimint/minimint/pull/60)
* Optimizing Build times: [#73](https://github.com/fedimint/minimint/pull/73)
* Docker container that builds nix scripts and integration tests [#83](https://github.com/fedimint/minimint/pull/83)
* Cleaning up yaml file: [#91](https://github.com/fedimint/minimint/pull/91)
* Masking Fenix: [#92](https://github.com/fedimint/minimint/pull/92)
* Fixing the broken nix-build: [#98](https://github.com/fedimint/minimint/pull/98)
* Updating the links to rust-secp256k1-zkp repo: [#108](https://github.com/fedimint/minimint/pull/108)
* Dockerfile for building nix-builds: [#110](https://github.com/fedimint/minimint/pull/110)
* Adding tests for nix-build: [#111](https://github.com/fedimint/minimint/pull/111)
* pulling back naersk to older version for fixing nix-build: [#135]( https://github.com/fedimint/minimint/pull/135)
* Flakifying minimint: [#147](https://github.com/fedimint/minimint/pull/147)
* docs: adding docs for nix: [#154](https://github.com/fedimint/minimint/pull/154)
* cachix: switching to auth token [#164](https://github.com/fedimint/minimint/pull/164)
* actions: run cachix only on fedimint/minimint [#170](https://github.com/fedimint/minimint/pull/170)
*  niv: updating nixpkgs to pull latest packages [#202](https://github.com/fedimint/minimint/pull/202) 
* docs:Adding minimint in application services: [#1](https://github.com/fedimint/nix-bitcoin/pull/1)

### Link to Issues 

* Figure out how to make the Nix CI build faster: [#67](https://github.com/fedimint/minimint/issues/67)
* default.nix doesn't build locally on stable NixOS: [#78](https://github.com/fedimint/minimint/issues/78)
* Fix default.nix build: [#90](https://github.com/fedimint/minimint/issues/90)
* CI: Caching for integration test is broken: [#103](https://github.com/fedimint/minimint/issues/103)
* CI: @action-rs/toochain is not needed in integration test: [#105](https://github.com/fedimint/minimint/issues/105)
* Integration Tests: lightning-cli enters infinite loop: [#138](https://github.com/fedimint/minimint/issues/138)
* Run cachix action only run on fedimint/minimint repo and not on forks [#169](https://github.com/fedimint/minimint/issues/169) 
* nearsk unable to find certain packages: [#244](https://github.com/nix-community/naersk/issues/244)
* cargo2nix unable to find certain packages: [#277](https://github.com/cargo2nix/cargo2nix/issues/277)


### Resources :
* Some Rust builders:
    * [crane](https://github.com/ipetkov/crane)
    * [cargo2nix](https://github.com/cargo2nix/cargo2nix)
    * [naersk](https://github.com/nix-community/naersk)
    * [crate2nix](https://github.com/kolloch/crate2nix)
    * [dream2nix](https://github.com/nix-community/dream2nix)
    * [Docs on Packaging in Rust](https://nixos.org/manual/nixpkgs/stable/#rust)
* Nix toolchains we used:
    * [nix-flakes](https://nixos.wiki/wiki/Flakes)
    * [cachix](https://docs.cachix.org/)
    * [nix-shell](https://nixos.org/manual/nix/stable/command-ref/nix-shell.html)
    * [nix-build](https://nixos.org/manual/nix/stable/command-ref/nix-build.html)
    * [niv](https://github.com/nmattia/niv)
    * [fenix](https://github.com/nix-community/fenix)
-------------------------------------------------
