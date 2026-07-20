---
title: Pointcloud to Laserscan
parent: Archive
nav_order: 5
---

# Pointcloud to Laserscan

[https://index.ros.org/p/pointcloud_to_laserscan/]

[https://github.com/ros-perception/pointcloud_to_laserscan]

Humble installation using apt-get:
```bash
sudo apt-get install ros-humble-pointcloud-to-laserscan
```
## Usage

This package can be used to convert 3D pointcloud data from your LiDAR to 2D laserscan data usable by Nav2 and slam_toolbox. The sample launch files can act as a template for your custom launch file. Remap cloud_in to your LiDAR's pointcloud topic, and leave the scan output as default. Both Nav2 and slam_toolbox are expecting the laserscan data to be available in the /scan topic by default.

To visualize the scan topic in RVIZ, the reliability policy for the laserscan topic must be set to "Best Effort"

Note that Autoware utilizes 3D Pointcloud data, so this package is not compatable with it.

----

[https://index.ros.org/p/pointcloud_to_laserscan/]: https://index.ros.org/p/pointcloud_to_laserscan/
[https://github.com/ros-perception/pointcloud_to_laserscan]: https://github.com/ros-perception/pointcloud_to_laserscan
