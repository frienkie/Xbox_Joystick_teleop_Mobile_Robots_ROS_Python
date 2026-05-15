# Xbox joystick teleoperation for mobile robots using ROS and Python

Simple ROS teleoperation package for mobile robots using a Microsoft Xbox controller.

## Features
- Uses `joy` to read Xbox controller input
- Publishes velocity commands for mobile robot teleoperation
- Supports TurtleBot 2 / TurtleBot 3 style ROS control topics
- Includes button mappings for speed modes, stop, and odometry reset
- Python-based ROS node with catkin package configuration

## Notes
- The package name in the ROS manifest is `ps4`, but the code and README are for Xbox joystick teleoperation.
- The repository contains ROS Python nodes and a catkin build setup.
