
+++
title = "Building a Nix Module for MiniMint"
date = 2022-06-15

[taxonomies]
tags = ["sob" , "nixos" , "minimint" ]
+++
Overview of the process of writing nix modules for minimint and nix-bitcoin.
<!-- more -->
## Overview:

I am new with blogs so please bear with me and ping me on how I can improve upon :) 

---------------------------------------------

## Nix expression which builds Minimint's binary outputs

### Nix-Shell :
&emsp; 
- has all necessary resources for development 
- nix-shell benefits
- used for integration testing

### Nix-Build :
&emsp; 
- to build minimint via nix
- tools we currently use in nix build
- benefits of nix build
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


### Github Actions : 
&emsp; Currently I have setup a working CI that tests flakes, nix-build, nix-shell and does integrations tests. We wanted a stable rust builder which allows granular caching and also supports sys crates.

&emsp; There aren't any stable rust builders in nix yet and so after testing all popular ones out there a.k.a cargo2nix, ceate2nix, naersk, crane, dream2nix; we narrowed them down to cargo2nix and crane. Cargo2nix doesn't support building -sys crates yet and crane isn't as good at caching as cargo2nix. So looking at the trade-off, we are using crane with flakes. 

### Docker Containers :
 - why did we set it up?
 - how is it working?
 - 

## Nix module in nix-bitcoin for Minimint :

### Working with nix-bitcoin :

* To run  use ./deploy-qemu-vm.sh -i
* 
-----------------------------------------
## Future Work :

### Optimizing CI :

### nix-bitcoin :

### Rigorous Testing :

-----------------------------------------

## References

### Repositories I am working on:
* Minimint - https://github.com/fedimint/minimint
* Fork of nix-bitcoin on fedimint - https://github.com/fedimint/nix-bitcoin
* Dockerizing minimint -  https://github.com/fedimint/minimint-docker
* secp256k1_zkp_flake - https://github.com/wiredhikari/secp256k1_zkp_flake

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
* [WiP] docs: adding docs for nix: [#154](https://github.com/fedimint/minimint/pull/154)
* docs:Adding minimint in application services: [#1](https://github.com/fedimint/nix-bitcoin/pull/1)

### Link to Issues 

* Figure out how to make the Nix CI build faster: [#67](https://github.com/fedimint/minimint/issues/67)
* default.nix doesn't build locally on stable NixOS: [#78](https://github.com/fedimint/minimint/issues/78)
* Fix default.nix build: [#90](https://github.com/fedimint/minimint/issues/90)
*  CI: Caching for integration test is broken: [#103](https://github.com/fedimint/minimint/issues/103)
* CI: @action-rs/toochain is not needed in integration test: [#105](https://github.com/fedimint/minimint/issues/105)
* Integration Tests: lightning-cli enters infinite loop: [#138](https://github.com/fedimint/minimint/issues/138)
*  nearsk unable to find certain packages: [#244](https://github.com/nix-community/naersk/issues/244)
*   cargo2nix unable to find certain packages: [#277](https://github.com/cargo2nix/cargo2nix/issues/277)


### Resources :
* Rust builders:
    * crane
    * cargo2nix
    * naersk
    * crates2nix
    * dream2nix
* Nix toolchains
    * [nix-flakes](https://nixos.wiki/wiki/Flakes)
    * [cachix](https://docs.cachix.org/)
    * [nix-shell](https://nixos.org/manual/nix/stable/command-ref/nix-shell.html)
-------------------------------------------------
