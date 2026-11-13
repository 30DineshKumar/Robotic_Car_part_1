#  DriveBot-ROS2-Part1

A ROS 2 Python package that drives a simulated two-wheeled robot in **RViz2**. It listens to velocity commands on `/cmd_vel`, updates the robot's position and heading, spins the wheels, and publishes joint states and TF transforms.

[RViz Demo]
<br><br>
<img width="1207" height="884" alt="Screenshot From 2026-11-13 16-53-02" src="https://github.com/user-attachments/assets/00f237d2-bc6d-4236-88f5-06dd8ab9565d" />


##  Features

-  **Velocity control**: subscribes to `/cmd_vel` (`geometry_msgs/Twist`)
-  **Pose tracking**: integrates linear and angular speed to compute x, y and yaw
-  **Wheel animation**: publishes `left_wheel_joint` and `right_wheel_joint` angles
-  **TF broadcasting**: publishes the robot's transform using `tf2_ros`
-  **10 Hz update loop**
-  **RViz2 visualization** using the `/robot_description` topic

##  Tech Stack

- Python 3
- ROS 2 (`rclpy`)
- `sensor_msgs`, `geometry_msgs`, `tf2_ros`
- RViz2

## Project Structure

```
my_robot_driver/
├── my_robot_driver/
│   ├── __init__.py
│   └── drive_robot.py        # Main driver node
├── resource/
│   └── my_robot_driver
├── test/
│   ├── test_copyright.py
│   ├── test_flake8.py
│   ├── test_mypy.py
│   ├── test_pep257.py
│   └── test_xmllint.py
├── package.xml
├── setup.py
└── setup.cfg
```

## Prerequisites

- ROS 2 (Humble / Jazzy / your distro)
- `colcon` build tools
- RViz2
- A robot description (URDF) published on `/robot_description`

## Installation

```bash
# 1. Build the package
cd ~/ros2_ws
colcon build --packages-select my_robot_description

# 2. Source the workspace
source install/setup.bash
```

## ▶️ Usage

**Terminal 1**: start the driver node:

```bash
ros2 launch my_robot_description display.launch.py
```


**Terminal 2**: open RViz2 and set the **Fixed Frame** to `base_link`:

```bash
rviz2
```

##  Topics

| Topic          | Type                      | Direction  | Description                   |
|----------------|---------------------------|------------|-------------------------------|
| `/cmd_vel`     | `geometry_msgs/Twist`     | Subscribe  | Linear and angular velocity   |
| `/joint_states`| `sensor_msgs/JointState`  | Publish    | Left and right wheel angles   |
| `/tf`          | `tf2_msgs/TFMessage`      | Publish    | Robot pose transform          |

##  How It Works

1. `/cmd_vel` sets the linear and angular speed.
2. Every 0.1 s the node updates the robot's pose:
   - `yaw += angular_speed × dt`
   - `x += linear_speed × cos(yaw) × dt`
   - `y += linear_speed × sin(yaw) × dt`
3. Wheel angles are increased in proportion to forward speed.
4. Joint states and the TF transform are published for RViz.

##  Contributing

Contributions, issues and feature requests are welcome! Feel free to open an issue or submit a pull request.

##  License

Apache-2.0

##  Author

**Dinesh Kumar**
GitHub: [K N DINESH KUMAR](https://github.com/30DineshKumar)
