---
title: ROS 2 Humble
parent: Archive
nav_order: 7
---

# Robot Operating System (ROS) 2

ROS2 Humble documentation:
[https://docs.ros.org/en/humble/index.html]

ROS2 Humble install guide (Debian packages for Ubuntu 22.04)
[https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html]

## Workspaces

There is no correct way to organize your ROS workspaces. I recommend organizing related packages into separate workspaces. For example, Robosense packages are in one workspace while Livox packages are in another. This way you can easily source packages as needed.

Whenever sourcing a ROS 2 workspace, I strongly recommend using the local_setup.bash script instead of the setup.bash script. local_setup.bash will set up all packages in the workspace, while setup.bash will set up all packages in the workspace along with any other workspace that was sourced at the time of the workspaces compilation. This can lead to unintended results where a conflicting workspace is sourced when it is not needed. Running multiple setup.bash scripts in your .bashrc file from multiple workspaces can also cause a few second delay before a new terminal window is ready.

## Publisher/Subscriber Development Guides

Python Publisher/Subscriber tutorial:
[https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.html]

Minimal subscriber cookbook recipes:
[https://github.com/ros2/examples/tree/humble/rclpy/topics/minimal_subscriber]

## Humble Message Reference

[https://docs.ros.org/en/humble/p/sensor_msgs/]

[https://docs.ros.org/en/humble/p/std_msgs/]

[https://docs.ros.org/en/humble/p/nav_msgs/index.html]

[https://docs.ros.org/en/humble/p/geometry_msgs/]

----

[https://docs.ros.org/en/humble/index.html]: https://docs.ros.org/en/humble/index.html
[https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html]: https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html
[https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.html]: https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.html
[https://github.com/ros2/examples/tree/humble/rclpy/topics/minimal_subscriber]: https://github.com/ros2/examples/tree/humble/rclpy/topics/minimal_subscriber
[https://docs.ros.org/en/humble/p/sensor_msgs/]: https://docs.ros.org/en/humble/p/sensor_msgs/
[https://docs.ros.org/en/humble/p/std_msgs/]: https://docs.ros.org/en/humble/p/std_msgs/
[https://docs.ros.org/en/humble/p/nav_msgs/index.html]: https://docs.ros.org/en/humble/p/nav_msgs/index.html
[https://docs.ros.org/en/humble/p/geometry_msgs/]: https://docs.ros.org/en/humble/p/geometry_msgs/
