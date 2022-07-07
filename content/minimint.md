
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

&emsp; We have setup `nix-shell` which included necessary tools for development and integration testing. Then we have `nix-build` to install minimint using a single command, currently we are using `naersk` with it, using `niv` to manage dependancies and automating the process. We also have setup `nix-flake` to install minimint without actually cloning the repository and pinned package versions which offer more reproducibility, we are going with `crane` as of now. To optimize build times we use `cachix` and it helps to cache any Nix build result and hence helps in cutting down build times. Also looking at various ways to optimize and cut down build times in github actions. Setting up a minimint module in nix-bitcoin is in progress and will be usable soon. For non-nix users we have made `Dockerfile` which will install nix and minimint , also it can be used for integration testing.

---------------------------------------------

## Nix expression which builds Minimint's binary outputs

### Nix-Shell :
&emsp; With nix-shell, we can create virtual environments per-project. When you enter nix-shell, the environment and all the dependencies are ready for use.We will be able to benefit from nix's reproducibility. Our current nix-shell has all the necessary tools to build minimint and it saves a lot of time for developers and users to reproduce the same environment again.
#### Porting integration tests in nix-shell
&emsp; Currently integration tests are carried out from a bash script and eventually we plan to port them to nix as well, so that everything moves to nix and we avoid redundancy. 

### Nix-Build :
&emsp; Nix-Builds -> Works on all machines, works in CI, works in production. Thats some superpowers of nix. We are using Nix-Build to build minimint with naersk.


### Niv
&emsp; We need dependancy management to handle and upgrade toolchains with ease.`niv` simplifies adding and updating dependencies in Nix projects. It uses a single file, `nix/sources.json`, in which it stores the data necessary for fetching and updating the packages. We are currently using it for `naersk` and `nixpkgs`.


### Nix-Flakes :
&emsp; Flakes are a new feature in Nix ecosystem. They allow you to specify your code's dependencies in a declarative way, simply by listing them inside a flake.nix. Each dependency gets then pinned, that is: its commit hash gets automatically stored into a file - named flake.lock - making it easy to, say, upgrade it. It supports several architectures and also has support for M1 chip. 

### Crane :
&emsp; We are using Crane as a rust builder with Nix-Flakes. It is composible and split builds and tests into granular steps. Gate CI without burdening downstream consumers building from source. It is incremental and build your workspace dependencies just once, then quickly lint, build, and test changes to your project without slowing down.

### Cargo2Nix :

&emsp; There aren't any stable rust builders in nix yet and so after testing all popular ones out there a.k.a cargo2nix, ceate2nix, naersk, crane, dream2nix; we narrowed them down to cargo2nix and crane. Cargo2nix doesn't support building `sys` crates yet and crane isn't as good at caching as cargo2nix. So looking at the trade-off, we are using crane with flakes. I am working on fixing it upstream.

### Github Actions : 
&emsp; Currently I have setup a working CI that tests flakes, nix-build, nix-shell and does integrations tests. We wanted a stable rust builder which allows granular caching and also supports sys crates.

### Setting up cachix :
&emsp; With Nix and Cachix we can cache any Nix build result. Nix can fetch a binary result instead of performing the build by looking up store hash in binary cache API.  CI can build and cache developer environments for every project on every branch using binary caches. 

### Docker Containers :
&emsp; At this point in time docker is more widely used than nix and hence its easier for users to set it up and pull images. So we have setup `Dockerfile` to install minimint. We also have setup `Dockerfile.integrationtest` for doing integrations tests without installing nix.

## Nix module in nix-bitcoin for Minimint :
&emsp;[Nix-Bitcoin](https://github.com/fort-nix/nix-bitcoin) is a set of nix modules for easily installing full-featured Bitcoin nodes with an emphasis on security. Having minimint in nix-bitcoin will help in easy integrations with `cln` and `bitcoind`. Users can rely on nix rather than setting everything up manually.
Overall the process of adding minimint in nix-bitcoin involves adding minimint as a module and then integrating with other tools. So far, we have added minimint as a [package](https://github.com/fedimint/nix-bitcoin/blob/minimint/pkgs/minimint/default.nix)   and soon we will have integrations with `cln` and `bitcoind` working. This will also be helpful in testing and 


-----------------------------------------
## Future Work :

### Optimizing CI :
&emsp; So far we have setup github actions for testing and post that we worked on bringing down the build times, for the upcoming coding period I will work on making it more efficient and fast.

### Nix-Bitcoin :
&emsp; Currently we have minimint [package](https://github.com/fedimint/nix-bitcoin/blob/minimint/pkgs/minimint/default.nix) within nix-bitcoin itself, in the upcoming coding period we will be working on integrations of `cln` and `bitcoind`. 

### Rigorous Testing and Documentation :
&emsp; So far we have setup github actions for testing and after that we started testing on different systems like M1, raspi and ARM. Thanks to nix we dont need to worry much about one configuration breaking on different machines. Documentation on CI/CD, nix-builds and of nix-bitcoin will be worked.

----------------------------------------

## References

### Repositories I am working on:
* Minimint - https://github.com/fedimint/minimint
* Fork of nix-bitcoin on Fedimint - https://github.com/fedimint/nix-bitcoin
* Dockerizing Minimint -  https://github.com/fedimint/minimint-docker
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
*   cachix: enabling cachix to run on forks [#214](https://github.com/fedimint/minimint/pull/214) 
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
    * [Docs for Packaging in Rust](https://nixos.org/manual/nixpkgs/stable/#rust)
* Nix toolchains we used:
    * [nix-flakes](https://nixos.wiki/wiki/Flakes)
    * [cachix](https://docs.cachix.org/)
    * [nix-shell](https://nixos.org/manual/nix/stable/command-ref/nix-shell.html)
    * [nix-build](https://nixos.org/manual/nix/stable/command-ref/nix-build.html)
    * [niv](https://github.com/nmattia/niv)
    * [fenix](https://github.com/nix-community/fenix)
-------------------------------------------------