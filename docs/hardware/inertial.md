---
title: Inertial Sense GPS
parent: Hardware 
nav_order: 40
---

# Inertial Sense uINS

Data sheet:
[https://docs.inertialsense.com/datasheets/uIMU_uAHRS_uINS-3_Datasheet.pdf]

Inertial Sense SDK Tag 1.11.2:
[https://github.com/inertialsense/inertial-sense-sdk/tree/1.11.2]

Inertial Sense 1.11.2 Docs:
[https://github.com/inertialsense/docs.inertialsense.com/tree/1.11.0]

Release 1.11.2 (EvalTool download):
[https://github.com/inertialsense/inertial-sense-sdk/releases/tag/1.11.2]

## Notice

**AVOID UPDATING THE FIRMWARE AT ALL COSTS**. The most up to date firmware that is compatible with the uINS is already installed. A failed firmware update attempt will brick the sensor and you will need to contact Inertial Sense for assistance in saving the sensor.

The uINS is an old sensor and is no longer supported with the latest SDK. As such SDK tag 1.11.2 is the most up to date version compatible with the uINS. When consulting any Inertial Sense resources/documentation, make sure you are looking at documentation for 1.11.2.

## EvalTool

`EvalTool` is a GUI program for viewing IMU and GPS data from the sensor. There are additional features as well, but the SDK is preferred for doing anything other than visualizing sensor data. The EvalTool can also be used to install sensor firmware if needed. The EvalTool is not compatible with the Jetson due to the ARM CPU.

## SDK

Steps to compile the Inertial Sense SDK:
1. git clone the Inertial Sense SDK
1. `git checkout tags/1.11.2` for the SDK
1. In a seperate folder git clone [libusb]
1. `git checkout tags/v1.0.26` for libusb
1. Copy the `libusb` folder within the libusb repo to the inertial sense `src/libusb` folder
**THERE MUST BE TWO NESTED LIBUSB FOLDERS** eg: `src/libusb/libusb/libusb.h`
1. From the libusb repo, run `autogen.sh`
1. Copy `config.h` to the inertial sense `src/libusb/libusb` folder

From here you can now use CMake to compile the SDK. The CLTool can serve as a test to confirm that everything compiles and runs smoothly. Consult the readmes in the CLTool or `ExampleProjects` folders for instructions on compilation.

## ROS 2

A custom NMEA message publisher and parser was written to enable ROS 2 support for the uINS. One piece of software uses the Inertial Sense SDK to publish NMEA messages from the sensor to a network port. Another program in the ROS environment parses these NMEA messages and publishes the appropriate sensor message topics. Consult the readmes of each repo for more more information.

NMEA publisher (No corrections data):
[https://github.com/arcater/ISNMEA]

NMEA publisher (With corrections data):
[https://github.com/arcater/ISRoverNetworkNMEA]

NMEA parser (ROS 2 Humble/Jazzy):
[https://github.com/ChesterMK7/is_gps_publisher_ros2]

----

[https://docs.inertialsense.com/datasheets/uIMU_uAHRS_uINS-3_Datasheet.pdf]: https://docs.inertialsense.com/datasheets/uIMU_uAHRS_uINS-3_Datasheet.pdf
[https://github.com/inertialsense/inertial-sense-sdk/tree/1.11.2]: https://github.com/inertialsense/inertial-sense-sdk/tree/1.11.2
[https://github.com/inertialsense/docs.inertialsense.com/tree/1.11.0]: https://github.com/inertialsense/docs.inertialsense.com/tree/1.11.0
[https://github.com/inertialsense/inertial-sense-sdk/releases/tag/1.11.2]: https://github.com/inertialsense/inertial-sense-sdk/releases/tag/1.11.2
[libusb]: https://github.com/libusb/libusb/tree/v1.0.26
[https://github.com/arcater/ISNMEA]: https://github.com/arcater/ISNMEA
[https://github.com/arcater/ISRoverNetworkNMEA]: https://github.com/arcater/ISRoverNetworkNMEA
[https://github.com/ChesterMK7/is_gps_publisher_ros2]: https://github.com/ChesterMK7/is_gps_publisher_ros2
