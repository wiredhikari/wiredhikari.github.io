+++

title = "RISC-V Support for Gentoo Prefix"
date = 2022-06-15

[taxonomies]
tags = ["gsoc" , "gentoo" , "prefix" ]

+++

Process of bootstrapping gentoo prefix on RISC-V architecture.

<!-- more -->

## Overview

## Workflow

### Machines used for development:

#### riscv-chroot-env

#### plct-lab

#### qemu image

## Current Progress:

### Setting up a profile for RISC-V:

#todo - Issue -> link -> workaround
### Issues faced during Stage-1:

### Issues faced during Stage-2:

* ncurses issue on plct machine and workaround :  https://raw.githubusercontent.com/wiredhikari/prefix_on_riscv/main/logs/stage1_logs/stage2.log

* gcc error due to missing : https://raw.githubusercontent.com/wiredhikari/prefix_on_riscv/main/logs/stage2_logs/stage2.log

### Issues faced during Stage-3:
* pam error https://github.com/wiredhikari/prefix_on_riscv/tree/main/logs/pam_error

* portage eror https://github.com/wiredhikari/prefix_on_riscv/tree/main/logs/portage_error

* pkg_conf error https://raw.githubusercontent.com/wiredhikari/prefix_on_riscv/main/logs/stage3/stage3.log

## Future Work:
* We will have a working gentoo prefix working soon on RISC-V, then we will do rigerous testing of all three stages.
* Write documentation on porting prefix to new architecture
* Test and keyword packages for RISC-V.

## References

### Repositories I am working on:

* Documentation : https://github.com/wiredhikari/prefix_on_riscv
* Prefix Fork : https://github.com/wiredhikari/prefix
* Gentoo Fork : https://github.com/wiredhikari/portage

### Pull Requests:
*  profiles: initial commits for riscv profile for prefix https://github.com/gentoo/gentoo/pull/25667
*  app-misc/pax-utils: Add prefix support https://github.com/gentoo/gentoo/pull/25855
*  sys-libs/pam: Add prefix support https://github.com/gentoo/gentoo/pull/25850
*  bootstrap-prefix.sh: adding riscv profile #6  https://github.com/gentoo/prefix/pull/6
