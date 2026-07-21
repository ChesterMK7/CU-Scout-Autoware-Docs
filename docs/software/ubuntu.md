---
title: Ubuntu
parent: Software
nav_order: 10
---

# Ubuntu

[Ubuntu](https://ubuntu.com) is a Linux distribution developed by British comapny Cannonical, used as the basis for many other distros. Our use of it stems from its support for the [ROS2 platform](https://www.ros.org/) that Autoware requires.

Currently, both NUCs use Ubuntu 22.04.5 as their host OS, with the Autoware Docker container utilizing Ubuntu 24.04. Both are currently still supported with updates and security patches, although support for 22.04 will end on May 31st 2027. This *shouldn't* effect anything given we're using containers for our core setup, but if security of the system becomes a concern I'd recommend updating the host to the latest Long Term Support (LTS) release, Ubuntu 26.04, which will recieve support through May of 2031.

You can find more in-depth information about working with Ubuntu in the subpages of this page.

----
