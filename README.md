# Xbox joystick teleoperation for mobile robots using ROS and Python

This repository provides a ROS (catkin) package named `xbox` for teleoperating mobile robots with a Microsoft Xbox controller.

It includes Python teleoperation nodes for TurtleBot 2 and TurtleBot 3 style command topics:
- `src/Joyteleop_turtlebot2.py`
- `src/Joyteleop_turtlebot3.py`

## Prerequisites / dependencies

- Ubuntu with ROS 1 (the package structure is catkin-based)
- ROS packages:
  - `rospy`
  - `roslib`
  - `sensor_msgs`
  - `geometry_msgs`
  - `joy` (for joystick input)
- A connected Xbox controller recognized by Linux (`/dev/input/js*` and `/dev/input/event*`)

Install joystick support if needed:

```bash
sudo apt update
sudo apt install ros-${ROS_DISTRO}-joy joystick
```

## Build / installation

1. Put this repository in your catkin workspace `src` folder.
2. Build the workspace.
3. Source the workspace setup file.

Example:

```bash
cd ~/catkin_ws/src
git clone <this-repository-url>
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

## How to run

### Option A: use launch file (TurtleBot 3 teleop node)

```bash
roslaunch xbox joyteleop.launch
```

This launch file starts:
- `joy_node` from package `joy`
- `Joyteleop_turtlebot3.py` from package `xbox`

### Option B: run nodes manually

Start joystick driver:

```bash
rosrun joy joy_node
```

Run TurtleBot 3 teleop node:

```bash
rosrun xbox Joyteleop_turtlebot3.py
```

When prompted by the script:
- enter `0` to publish on `cmd_vel`
- enter `1` to publish on `cmd_vel_human`

Run TurtleBot 2 teleop node:

```bash
rosrun xbox Joyteleop_turtlebot2.py
```

## Controller usage / button mapping

The mapping below is based on `src/Joyteleop_turtlebot3.py` and `src/Joyteleop_turtlebot2.py`.

### TurtleBot 3 node (`Joyteleop_turtlebot3.py`)

- Left stick vertical (`axes[1]`): forward/backward reference
- Right stick horizontal (`axes[3]`): angular reference
- `A` (`buttons[0]`): slow mode (`linear * 0.1`, `angular * 0.2`)
- `B` (`buttons[1]`) or right trigger pressed (`rt > 1.2`): immediate stop
- `Y` (`buttons[3]`): publish reset odometry command (`/mobile_base/commands/reset_odometry`)
- `LB` (`buttons[4]`): fixed left turn (`angular.z = 1`)
- `RB` (`buttons[5]`): fixed right turn (`angular.z = -1`)
- Left trigger (`axes[2]` converted to `lt`): throttle-like forward motion branch

### TurtleBot 2 node (`Joyteleop_turtlebot2.py`)

- Left stick vertical (`axes[1]`): linear command basis
- Left stick horizontal (`axes[0]`): angular command basis
- `A` (`buttons[0]`): normal mode (`linear * 0.4`, `angular * 1.2`)
- `X` (`buttons[2]`): quick mode (`linear * 0.7`, `angular * 2`)
- `Y` (`buttons[3]`): publish reset odometry command
- `B` (`buttons[1]`): stop

## Troubleshooting

- `Resource not found: xbox`  
  Ensure the workspace is built and sourced:
  ```bash
  cd ~/catkin_ws && catkin_make
  source ~/catkin_ws/devel/setup.bash
  ```

- No joystick messages on `/joy`  
  Check device visibility and permissions:
  ```bash
  ls /dev/input/js*
  jstest /dev/input/js0
  ```
  Confirm `joy_node` is running:
  ```bash
  rostopic echo /joy
  ```

- Robot does not move  
  Verify the command topic expected by your robot (`cmd_vel`, `cmd_vel_human`, or `/mobile_base/commands/velocity`) and choose the matching node/script.

- Reset odometry command has no effect  
  Your platform may not subscribe to `/mobile_base/commands/reset_odometry`; adapt the topic for your robot if necessary.
