A custom autonomous navigation stack for a differential-drive mobile robot using ROS 2 Humble.
The system includes custom C++ implementations of A* global planning, Potential Field local control, 3D LiDAR mapping, and a sensor-fusion safety layer.

**🔍 Overview**

The system works in two phases:
-->Mapping Phase – Manually drive the robot to create a 3D map
-->Navigation Phase – Autonomous navigation using the saved map

🧠 System
High-Level Architecture
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/5ee8945f-df2f-4663-943f-9b04096cbe37" />

**Main Components**

● A* Global Planner – Computes shortest path on a 2D costmap
● Potential Field Local Planner – Reactive obstacle avoidance
● 3D Map Accumulator – Builds and saves a voxel map from LiDAR
● Safety Monitor – Stops the robot if LiDAR or camera detects danger

**📦 Packages**
Package	                                              Purpose
my_robot_controller	                                  Mapping, planners, navigation logic
my_robot_description	                                URDF, worlds, maps
my_robot_teleop	                                      Manual control during mapping
my_robot_vision                                     	Camera-based safety

**⚙️ Installation**
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
git clone https://github.com/karadasrikarreddy/CPP-Autonomous-Navigation.git
cd ~/ros2_ws
colcon build
source install/setup.bash

🗺️ Phase 1: 3D Mapping

Launch mapping mode:
ros2 launch my_robot_controller mapping_3d.launch.py

Drive the robot manually:
ros2 run teleop_twist_keyboard teleop_twist_keyboard

Save the map:
ros2 service call /save_map_3d std_srvs/srv/Trigger {}

📁 Map saved as:
my_robot_controller/maps/my_3d_map.pcd

👉 You may also reuse the prebuilt map provided in the repository.

📸 

<img width="1366" height="768" alt="Screenshot from 2026-02-01 14-25-10" src="https://github.com/user-attachments/assets/71f64e4f-750a-4494-9365-6ed6bdff7738" />

🤖 Phase 2: Autonomous Navigation

Launch navigation mode:
ros2 launch my_robot_controller indoor.launch.py

In RViz:
You can use my_robot_controller/Rviz/my_robot_vs.rviz
Set 2D Pose Estimate
Send 2D Nav Goal

The robot will autonomously navigate while avoiding obstacles.

<img width="1366" height="768" alt="Screenshot from 2026-02-01 14-31-02" src="https://github.com/user-attachments/assets/32de448a-649c-4141-8a38-e28168cd785e" />

<img width="1366" height="768" alt="Screenshot from 2026-02-01 14-29-22" src="https://github.com/user-attachments/assets/24355f8d-8a6d-404f-95c2-3d3aa583ff81" />

🧠 Execution Flow
Mapping:
LiDAR → 3D Map Accumulator → Save .pcd

Navigation:
Load Map → 2D Costmap → A* Planner
→ Local Planner → Safety Monitor → Robot
