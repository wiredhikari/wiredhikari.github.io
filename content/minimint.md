
+++
title = "Building a Nix Module for MiniMint"
date = 2022-06-15

[taxonomies]
tags = ["sob" , "nixos" , "minimint" ]
+++
Overview of the process of writing nix modules for minimint and nix-bitcoin.
<!-- more -->
## Overview:

I am new with writing blogs so please bear with me and ping me on how I can improve upon :) 

## Nix expression which builds Minimint's binary outputs

### Nix-Shell :

### Nix-Build :

### Setting up cachix :

### Nix-Flakes :


### Github Actions : 
Currently I have setup a working CI that tests flakes, nix-build, nix-shell and does integrations tests. We wanted a stable rust builder which allows granular caching and also supports sys crates.

There aren't any stable rust builders in nix yet and so after testing all popular ones out there a.k.a cargo2nix, ceate2nix, naersk, crane, dream2nix; we narrowed them down to cargo2nix and crane. Cargo2nix doesn't support building -sys crates yet and crane isn't as good at caching as cargo2nix. So looking at the trade-off, we are using crane with flakes. 

### Docker Containers :

## Nix module in nix-bitcoin for Minimint :

### Working with nix-bitcoin :

* To run  use ./deploy-qemu-vm.sh -i
* 

## Future Work :

### Optimizing CI:


## References

#### Repositories I am working on:
* https://github.com/fedimint/minimint
* https://github.com/fedimint/nix-bitcoin
* https://github.com/fedimint/minimint-docker
* https://github.com/wiredhikari/secp256k1_zkp_flake

#### Pull Requests:

* nix: adding nix shell support #45 https://github.com/fedimint/minimint/pull/45
* Nix expression to build minimint’s binary outputs https://github.com/fedimint/minimint/pull/60
* Optimizing Build times https://github.com/fedimint/minimint/pull/73
* Docker container that builds nix scripts and integration tests https://github.com/fedimint/minimint/pull/83
* Cleaning up yaml file  https://github.com/fedimint/minimint/pull/91
* Masking Fenix https://github.com/fedimint/minimint/pull/92
* Fixing the broken nix-build https://github.com/fedimint/minimint/pull/98
* Updating the links to rust-secp256k1-zkp repo https://github.com/fedimint/minimint/pull/108
* Dockerfile for building nix-builds https://github.com/fedimint/minimint/pull/110
* Adding tests for nix-build https://github.com/fedimint/minimint/pull/111
* Dockerfile for building nix-builds https://github.com/fedimint/minimint/pull/110
* docs:Adding minimint in application services https://github.com/fedimint/nix-bitcoin/pull/1
* pulling back naersk to older version for fixing nix-build https://github.com/fedimint/minimint/pull/135
* [WiP] Flakifying minimint https://github.com/fedimint/minimint/pull/147
* [WiP] Docs: adding nix documentation


#### Link to Issues 
* Integration Tests: lightning-cli enters infinite loop. https://github.com/fedimint/minimint/issues/138
*  CI: Caching for integration test is broken #103 https://github.com/fedimint/minimint/issues/103
* Fix default.nix build #90  https://github.com/fedimint/minimint/issues/90
* Figure out how to make the Nix CI build faster https://github.com/fedimint/minimint/issues/67
* default.nix doesn't build locally on stable NixOS https://github.com/fedimint/minimint/issues/78
* CI: @action-rs/toochain is not needed in integration test https://github.com/fedimint/minimint/issues/105
*  nearsk unable to find certain packages #244 https://github.com/nix-community/naersk/issues/244
*   cargo2nix unable to find certain packages #277 https://github.com/cargo2nix/cargo2nix/issues/277
* 



#### Resources Used:
* Rust builders
    * crane
    * cargo2nix
    * naersk
    * crates2nix
* 
* 
* 
* 