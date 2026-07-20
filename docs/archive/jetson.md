---
title: Jetson AGX Orin
parent: Archive 
nav_order: 7
---

# Jetson AGX Orin Developer kit

Jetson Orin Product Page:
[https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/]

User Guide:
[https://developer.nvidia.com/embedded/learn/jetson-agx-orin-devkit-user-guide/index.html]

Jetson Linux Archive:
[https://developer.nvidia.com/embedded/jetson-linux-archive]

## Note

As of January 2026, the Jetson is no longer in use on the Scout 2.0, and now resides in its box in the back of the lab. I have kept this page here in case it needs to be used again, as the documentation provided here is quite useful for working with it.

In refrence to the flashing instructions below, the Jetson also has a 1TB NVMe SSD installed, which is located on the bottom of the unit. Should it need an OS reinstall, you can install it to the drive rather than the NAND flash. Note that the Jetson utilizes the ARM64 ISA and thus the OS will not boot on a system utilizing more traditional x86_64 ISAs (AMD or Intel CPUs).

## Flashing the Jetson

The package manager for the Jetson can be fragile at times. Encountering 'broken packages' when attempting to install new packages can be a common occurrence. If the Jetson Linux install is broken beyond repair, it is necessary to re-flash the Jetson to return it to working order.

I would recommend attempting to flash the latest version of Jetson Linux that is compatible with the AGX Orin. Ensure that the version of Ubuntu is compatible with your target ROS distribution (or a docker container could be used as a workaround). Navigate to the Jetson Linux archive, select a version, and navigate to the developer guide for flashing instructions.

Consult this pdf file for instructions on how to flash Jetson Linux 36.3 with a virtual machine:
[https://github.com/RowanW09/AgileX_Autonomous_Driving_CU_Summer_2024/blob/Summer-2025/Readme.pdf]

----

[https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/]: https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/
[https://developer.nvidia.com/embedded/learn/jetson-agx-orin-devkit-user-guide/index.html]: https://developer.nvidia.com/embedded/learn/jetson-agx-orin-devkit-user-guide/index.html
[https://developer.nvidia.com/embedded/jetson-linux-archive]: https://developer.nvidia.com/embedded/jetson-linux-archive
[https://github.com/RowanW09/AgileX_Autonomous_Driving_CU_Summer_2024/blob/Summer-2025/Readme.pdf]: https://github.com/RowanW09/AgileX_Autonomous_Driving_CU_Summer_2024/blob/Summer-2025/Readme.pdf
