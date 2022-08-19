
+++
title = "Building a Nix Module for Fedimint"
date = 2022-08-18

[taxonomies]
tags = ["sob" , "nixos" , "minimint" ]
+++
Overview of the process of writing nix modules for Fedimint in Nix-Bitcoin.
<!-- more -->



# Nix Module of Fedimint in Nix-Bitcoin
---------------

## Overview:

&emsp; Fedimint(earlier known as Minimint) is an implementation of federated e-cash system written in Rust. It is a federated Chaumian e-cash mint backed by bitcoin with deposits and withdrawals that can occur on-chain or via Lightning. 

&emsp; To make deployment easier having a Nix module allowing to easily set up a federation. Nix allows to write entir e system configurations as a single configuration file and to reproducibly build that system. As such it is well-suited for security-critical applications.

&emsp; Nix-Bitcoin is a collection of Nix packages and NixOS modules for easily installing full-featured Bitcoin nodes with an emphasis on security. Nodes can be deployed on dedicated hardware, virtual machines or containers. The Nix packages and NixOS modules can be used independently and combined freely. With Fedimint Module in Nix-Bitcoin we can easily build. 

---------------

## Fedimint Module:
The [Fedimint Module](https://github.com/fedimint/nix-bitcoin/blob/master/modules/minimint.nix)(currently named Minimint) is in working state and we can also pegin some coins. There were a lot of trial and errors that happened during the process but we finally have a working module. `regtest` has been enabled for testing. Recently we had a lot of patches like `rocksdb` support on Fedimint, so I have added support for them and required tools used for using/developing Fedimint. Nix-Bitcoin has nice support for handling secrets, they are enabled as well.

The data directory is located at `/var/lib/minimint` where config and other relevent files are located. All necessary binaries are available once we get into the environment. We also have a `systemd` service enabled for Fedimint. The `preStart` and `execStart` script executes all necessary steps required to setup Fedimint and generate config file. Eventually we will also be adding tor support. 

Along with Fedimint module we also have fedimint-gateway module which acts as a bridge and helps in connecting Fedimint and lightning network. We are currently using same data directory as Fedimint. It is being used as a plugin for `clightning`. As it sounds, most of the time went into testing and observing other modules in nix-bitcoin.

Moving ahead we will be refactoring some parts of the code  and also add signet support upstream [#532](https://github.com/fort-nix/nix-bitcoin/issues/532). This will allow us to test the integration of lightning gateway as it is difficult to test on regtest. Adding support upstream will be the next step and then eventually we will upstream Fedimint module as well.

### Deploying Fedimint with Nix-Bitcoin:


  We can use nix flakes for deploying Fedimint. Flakes are a new feature in Nix ecosystem. They allow you to specify your code’s dependencies in a declarative way, simply by listing them inside a flake.nix. Each dependency gets then pinned, that is: its commit hash gets automatically stored into a file - named flake.lock - making it easy to, say, upgrade it. It supports several architectures as well.
  
  Without worrying much we can install all necessary tools, thanks to nix. We have enabled `minimint` (soon will be called fedimint), `fedimint-gateway` and `bitcoind`; `regtest` being enabled in `minimint` module. Also `fallbackfee` is enabled and can be modified as needed. We can also pull a specific branch by adding `github:fedimint/nix-bitcoin/branch-name`.
  
```
{
  description = "A basic nix-bitcoin node";

  inputs.nix-bitcoin.url = "github:fedimint/nix-bitcoin";

  outputs = { self, nix-bitcoin }: {
        {
          # Automatically generate all secrets required by services.
          # The secrets are stored in /etc/nix-bitcoin-secrets
          nix-bitcoin.generateSecrets = true;

          services.minimint.enable = true;
          services.fedimint-gateway.enable = true;
          services.bitcoind.extraConfig = ''
                fallbackfee=0.0004
          '';
          # When using nix-bitcoin as part of a larger NixOS configuration, set the following to enable
          # interactive access to nix-bitcoin features (like bitcoin-cli) for your system's main user
          nix-bitcoin.operator = {
                      enable = true;
            name = "main"; # Set this to your system's main user
          };

          # The system's main unprivileged user. This setting is usually part of your
          # existing NixOS configuration.
          users.users.main = {
            isNormalUser = true;
            password = "a";
          };
        }
      ];
    };
  };
}
```

  To use this minimal flake in your system copy the above`flake.nix` in `/etc/nixos` and use `nix flake update`. Also make sure flakes are enabled in your system as it is an experimental feature and one needs to allow it manually in order to use it. To update your system run `nixos-rebuild switch --impure`. 




---------------

## Future Plans:

I like exploring new tools and technologies, and what's a better way to learn than working on something cool. Contributing to Fedimint has been an amazing experience (Credits to my mentor and community). There's a lot of areas one can work and contribute to Fedimint, so I will learn and contribute to the best of my ability. Here are some of my current plans:

### Maintaning CI

&emsp; Fedimint is growing and developing at an amazing speed so we keep getting new errors and the need of new patches to keep CI upto date. I will work on maintaining CI and improve Nix support of different systems/arch.

### Maintaning Nix Builds

&emsp; Fedimint has Nix support and uses Nix for managing builds and to test dependencies. We also have support for nix flakes. CI is nix driven as well. As codebase expands and more features are added we will need better support and I will try to contribute in doing so. 

### Nix-Bitcoin
&emsp; Currently we have a working Fedimint module and we can run and test Fedimint on Nix-Bitcoin. As patches keep getting added there is still quite some work we can do.Also we will add some tests for checking Nix-Bitcoin builds for Fedimint. Below are some of the issues I am looking into. 

#### Issues:
- Get rid of any unrelated changes [#7](https://github.com/fedimint/nix-bitcoin/issues/7)
- Don't run fedimint as clightning user [#8](https://github.com/fedimint/nix-bitcoin/issues/8)
- Get rid of "minimint" [#9](https://github.com/fedimint/nix-bitcoin/issues/9)
- Get rid of dbDir [#10](https://github.com/fedimint/nix-bitcoin/issues/10)
- Signet support [#532](https://github.com/fort-nix/nix-bitcoin/issues/532)
-  Client should connect to federation via Tor [#391](https://github.com/fedimint/fedimint/issues/391)
 
### Fixing Other Issues:
I am also getting my hands on Rust and will soon start solving Fedimint's code base related [issues](https://github.com/fedimint/fedimint/issues). 

------------------------
### Relevent Repositories:
* FediMint - https://github.com/fedimint/fedimint
* Fork of Nix-Bitcoin on FediMint - https://github.com/fedimint/nix-bitcoin
* Nix-Bitcoin - https://github.com/fort-nix/nix-bitcoin

------------------------

## Thanks

For me Open-Source and Bitcoin was kind off a wild forest which pretty much confused me, but contributing to Fedimint was one of the best experiences I have had and it gave me quite some clarity and confidence on contributing to large codebases and fixing issues. Thanks to Summer of Bitcoin I could learn more about Bitcoin(those sessions were amazing) and contribute to one of the amazing organisations. It has been a fun experience. I am grateful that I got an opportunity to get mentored by Elsirion, they helped and guided me whenever I was stuck and also thanks for bearing the silly mistakes I did and helping me throughtout the journey >_< .