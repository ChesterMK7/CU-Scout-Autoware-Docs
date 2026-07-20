---
title: Intel NUC
parent: Hardware 
nav_order: 5
---

# Intel NUC Computers

## Specifications

- Intel Core i7-1165G7
- Nvidia GeForce RTX 2060 Mobile
- 2x16GB (Scout 2.0) / 2x32GB (Scout Mini) DDR4 3200MT/s SODIMM RAM
- 1TB NVMe SSD

## Reinstalling the OS

Both NUCs currently run Ubuntu 22.04.5 LTS, and have ROS2 Humble installed from previous years work. Should the OS need to be updated/reinstalled, I would *strongly* reccomend backing up the home directory as to not lose any important files. Most of the currently used files are located in the ~/autoware directory, shared with the docker container that runs Autoware on Ubuntu 24.04.

Due to the Autoware Ansible script's reliance on apt, I would strongly reccomend using either Ubuntu or an Ubuntu derivative (such as Linux Mint) as a host OS. A traditional x86_64 install image can be used by the NUCs, since they utilize an Intel CPU. More information can be found on the Ubuntu page.

----

