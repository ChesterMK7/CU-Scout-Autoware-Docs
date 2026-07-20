---
title: Scout UGV
parent: Hardware 
nav_order: 01
---

# AgileX Scout 2.0 & Mini

AgileX Scout 2.0 user manual:
[https://agilexrobotics.gitbook.io/scout2.0]

AgileX Scout Mini user manual:
[https://agilexrobotics.gitbook.io/scout-mini]

Scout ROS 2 packages:
[https://github.com/agilexrobotics/scout_ros2]

UGV SDK:
[https://github.com/westonrobot/ugv_sdk]

## ROS 2 Packages

Check the scout_ros2 readme for installation instructions. Git clone both the scout_ros2 and ugv_sdk repos into your colcon workspace. If you are using ROS 2 humble, be sure to git checkout the humble branch.

## Scout Base

This package allows for the UGV to be controlled using software such as Nav2 or teleop_twist. After a fresh install, the `ugv_sdk/scripts/setup_can2usb.bash` script must be executed once to enable the `gs_usb` kernel module. If you are working with a fresh Jetson Linux install, this module may be missing and it needs to be compiled.

After every system reboot, the `ugv_sdk/scripts/bringup_can2usb_500k.bash` script must be run to enable communication via the CAN port. `candump can0` can be run in a terminal to verify that the CAN port is running properly. If nothing is being print to the terminal after running this command, then debugging is required. It is not necessary to leave the candump command running to control the UGV.

Use the `ros2 launch scout_base scout_base.launch.py` launch script for the Scout 2.0, and use the `ros2 launch scout_base scout_mini_base.launch.py` launch script for the Scout mini. From here Nav2 or teleop_twist can be used to control the UGV. teleop_twist_keyboard may need to be installed using the package manager if it is not already present. **Reduce the speed before moving the UGV with teleop_twist_keyboard**

## Scout Description

The scout_description package is responsible for publishing information about the layout of the UGV. The location of all sensors must be specified here relative to the center of the UGV (referred to as the 'base_link'). This information is declared in a xacro file and is compiled during the colcon build process. To update the scout_description, modify the xacro files in the "src" folder and do another colcon build to have those changes be applied.

To quote to the most recent commit message for the scout_description xacro files, "initial commit, code compiles but not working yet". If you are working with a fresh install the xacro files will need to be updated.

Working xacro files for Scout 2 and Mini:
[https://github.com/arcater/CU_scout_description]

----

[https://agilexrobotics.gitbook.io/scout2.0]: https://agilexrobotics.gitbook.io/scout2.0
[https://agilexrobotics.gitbook.io/scout-mini]: https://agilexrobotics.gitbook.io/scout-mini
[https://github.com/agilexrobotics/scout_ros2]: https://github.com/agilexrobotics/scout_ros2
[https://github.com/westonrobot/ugv_sdk]: https://github.com/westonrobot/ugv_sdk
[https://github.com/arcater/CU_scout_description]: https://github.com/arcater/CU_scout_description
