# RMUC_Nav Project - Complete Project Description

## Project Overview

The **RMUC_Nav Project** (RoboMaster University Championship Navigation) is an advanced autonomous navigation system developed by HKU RoboMaster & Astar for the RoboMaster University Championship Sentinel Robot. This project provides a complete solution for 3D LiDAR-based SLAM, localization, and autonomous navigation in dynamic competition environments.

The project is specifically designed for the Sentinel Robot role in the RoboMaster competition, which requires autonomous patrol, obstacle avoidance, and strategic positioning on the competition field.

## Repository Structure

The repository consists of two main components:

```
HKU-RoboMaster-Sentinel-Autonomous-Navigation/
├── RMUC_Nav/                    # Navigation and SLAM workspace
│   ├── Point-LIO/               # High-frequency LiDAR-Inertial Odometry
│   ├── hdl_graph_slam/          # 3D Graph SLAM
│   ├── hdl_localization/        # Real-time 3D localization
│   ├── hdl_global_localization/ # Global localization module
│   ├── fast_gicp/               # Fast GICP registration
│   ├── ndt_omp/                 # Multi-threaded NDT
│   ├── pcd2pgm_package/         # Point cloud to 2D grid map conversion
│   └── bot_sim/                 # Robot simulation and control
│
└── sentry_planning/             # Planning and control workspace
    ├── src/
    │   ├── ocs2/                # Optimal Control for Switched Systems
    │   ├── ocs2_robotic_assets/ # Robotic assets for OCS2
    │   ├── rm2023_sentry_msgs/  # ROS message definitions
    │   ├── sentry_gazebo_2024/  # Gazebo simulation environment
    │   └── sentry_planning/     # Motion planning modules
    └── 哈尔滨工业大学哨兵开源技术文档.pdf
```

## System Architecture

### RMUC_Nav Package

The RMUC_Nav package provides the core navigation capabilities through an integrated SLAM and localization pipeline:

#### 1. Point-LIO (LiDAR-Inertial Odometry)
**Purpose:** High-frequency, robust LiDAR-inertial odometry for fast and aggressive motions

**Key Features:**
- High odometry output frequency (4-8 kHz)
- No motion distortion
- Robust to IMU saturation and severe vibrations (up to 75 rad/s)
- Suitable for aggressive motions common in competitive robotics
- Computationally efficient and versatile across different LiDAR types

**Supported Sensors:**
- Livox Avia, Mid-70, Mid-40
- Velodyne (HDL-32e, VLP-16, VLP-32, VLP-64)
- Ouster LiDARs
- Other time-stamped point cloud LiDARs

**Technology:**
- Tightly-coupled LiDAR-IMU fusion
- Iterated Extended Kalman Filter (IEKF)
- ikd-Tree for efficient point cloud management
- Real-time point-wise processing

#### 2. HDL Graph SLAM
**Purpose:** 3D Graph SLAM with loop closure detection for large-scale mapping

**Key Features:**
- Real-time 6DOF SLAM using 3D LiDAR
- NDT/GICP scan matching-based odometry
- Automatic loop detection and closure
- Multiple constraint types for improved accuracy
- Pose graph optimization using g2o

**Supported Constraints:**
- Odometry edges
- Loop closure edges
- GPS constraints (UTM coordinates)
- IMU acceleration (gravity alignment)
- IMU orientation (magnetic sensor)
- Floor plane constraints (for indoor environments)

**Technology:**
- Graph-based SLAM framework
- Multi-threaded processing
- Robust kernel for outlier rejection
- Support for various scan matching methods (NDT, GICP, FAST_GICP, FAST_VGICP)

#### 3. HDL Localization
**Purpose:** Real-time 3D localization against pre-built maps

**Key Features:**
- Unscented Kalman Filter (UKF) based pose estimation
- Multi-threaded NDT scan matching
- Optional IMU-based pose prediction
- Global relocalization capability
- High-frequency pose updates

**Workflow:**
1. Load pre-built global map (PCD format)
2. Initial pose estimation or global localization
3. Continuous pose tracking with scan matching
4. Kalman filter fusion of odometry and scan matching

#### 4. Fast GICP
**Purpose:** Fast and accurate point cloud registration

**Key Features:**
- GPU-accelerated version available
- Faster than standard GICP
- Voxelized GICP (VGICP) for improved performance
- Compatible with CUDA for hardware acceleration

#### 5. NDT OMP
**Purpose:** Multi-threaded Normal Distributions Transform

**Key Features:**
- OpenMP parallelization
- Multiple neighbor search methods
- Configurable for speed vs. accuracy trade-offs
- Compatible with PCL

#### 6. PCD2PGM Package
**Purpose:** Convert 3D point cloud maps to 2D occupancy grid maps

**Key Features:**
- PCD to PGM conversion (点云pcd文件转二维栅格地图)
- Configurable height filtering
- Map resolution control
- Output compatible with ROS navigation stack

#### 7. Bot Sim Package
**Purpose:** Robot simulation, control, and decision-making

**Key Components:**
- **Path Planning:**
  - D* Lite algorithm for dynamic replanning
  - DBSCAN-based 3D obstacle clustering
  - Real-time path replanning
  
- **Sensor Processing:**
  - Multi-LiDAR point cloud merging
  - 3D LiDAR filtering and preprocessing
  - IMU data filtering
  
- **Robot Control:**
  - Serial communication interface (ser2msg)
  - Transform broadcasting for sensor frames
  - Decision-making logic for autonomous operation
  - Map server for occupancy grid distribution
  
- **Monitoring:**
  - Node health monitoring (node_watcher)
  - LiDAR status monitoring
  - System diagnostics
  
- **Navigation Behaviors:**
  - Autonomous patrol patterns
  - Dynamic obstacle avoidance
  - Strategic positioning

### Sentry Planning Package

The sentry_planning package provides advanced motion planning and control for the Sentinel robot:

#### 1. OCS2 (Optimal Control for Switched Systems)
**Purpose:** Advanced optimal control framework

**Components:**
- **ocs2_core:** Core optimal control algorithms
- **ocs2_ipm:** Interior Point Method solver
- **ocs2_frank_wolfe:** Frank-Wolfe algorithm implementation
- **ocs2_perceptive:** Perceptive motion planning

**Features:**
- Model Predictive Control (MPC)
- Switched system dynamics
- Real-time trajectory optimization
- Constraint handling

#### 2. Trajectory Generation
**Purpose:** Generate feasible and optimal trajectories

**Features:**
- Topological path search (拓扑搜索)
- Trajectory optimization (轨迹优化)
- Dynamic feasibility checking
- Multiple trajectory alternatives

#### 3. Trajectory Tracking
**Purpose:** Execute planned trajectories with high precision

**Features:**
- NMPC (Nonlinear Model Predictive Control) flow (NMPC流程)
- Real-time trajectory following
- Disturbance rejection
- Path deviation correction

#### 4. Waypoint Generator
**Purpose:** Convert high-level goals to waypoint sequences

**Features:**
- 3D goal selection in RViz
- Collision-free waypoint generation
- Strategic positioning for competition

#### 5. RViz Plugins
**Purpose:** Visualization and interaction tools

**Features:**
- 3D Goal tool (press G for quick access)
- Trajectory visualization
- Robot state display
- Path planning visualization

#### 6. Sentry Gazebo 2024
**Purpose:** Realistic simulation environment

**Components:**
- **mbot_description:** URDF/Xacro robot models
- **mbot_gazebo:** Gazebo world and launch files
- **mbot_teleop:** Teleoperation control
- **mbot_control:** Robot controllers

**Features:**
- RoboMaster competition field simulation
- Realistic sensor simulation (Velodyne LiDAR)
- Physics-based robot dynamics
- Testing environment for algorithms

#### 7. RM2023 Sentry Messages
**Purpose:** Custom ROS message types for the Sentinel robot

**Features:**
- Competition-specific message definitions
- Sensor data structures
- Command interfaces

## Key Technologies

### SLAM and Localization
- **Point-LIO:** Tightly-coupled LiDAR-IMU odometry with high output frequency
- **Graph SLAM:** Loop closure detection and pose graph optimization
- **NDT/GICP:** Robust scan matching algorithms
- **Multi-sensor Fusion:** LiDAR, IMU, GPS integration

### Motion Planning
- **Topological Search:** Efficient path finding in complex environments
- **Trajectory Optimization:** Smooth and dynamically feasible trajectories
- **MPC/NMPC:** Real-time optimal control
- **Replanning:** Dynamic obstacle avoidance (重规划流程)

### Robot Control
- **OCS2 Framework:** Advanced optimal control
- **Switched Systems:** Handle different robot behaviors
- **Real-time Control:** High-frequency control loops
- **Sensor Fusion:** Multi-sensor state estimation

## System Requirements

### Hardware Requirements
- **LiDAR:** Velodyne (HDL-32e, VLP-16) or Livox series
- **IMU:** 6-axis or 9-axis IMU (built-in or external)
- **GPS (Optional):** For outdoor global positioning
- **Computing Platform:** Ubuntu-compatible computer with sufficient CPU/GPU

### Software Requirements
- **OS:** Ubuntu 18.04/20.04 (Ubuntu 20.04 with ROS Noetic recommended)
- **ROS:** ROS Melodic or ROS Noetic
- **Dependencies:**
  - PCL (Point Cloud Library)
  - Eigen3
  - OpenMP
  - g2o (graph optimization)
  - GTSAM (optional)
  - Ceres Solver
  - CUDA (optional, for GPU acceleration)

### ROS Package Dependencies
```bash
# Core dependencies
sudo apt-get install ros-noetic-geodesy 
sudo apt-get install ros-noetic-pcl-ros 
sudo apt-get install ros-noetic-nmea-msgs 
sudo apt-get install ros-noetic-libg2o
sudo apt-get install ros-noetic-pcl-conversions
sudo apt-get install libeigen3-dev

# For Point-LIO
# Livox ROS driver required
```

## Installation

### 1. Setup ROS Workspace

```bash
# Create workspace
mkdir -p ~/rmuc_ws/src
cd ~/rmuc_ws/src

# Clone repository
git clone https://github.com/Black-Jack-3844/HKU-RoboMaster-Sentinel-Autonomous-Navigation.git

# Or clone submodules if needed
cd HKU-RoboMaster-Sentinel-Autonomous-Navigation/RMUC_Nav/Point-LIO
git submodule update --init
```

### 2. Install Dependencies

```bash
# Install ROS dependencies
cd ~/rmuc_ws
rosdep install --from-paths src --ignore-src -r -y

# Install Livox ROS driver (for Livox LiDARs)
# Follow: https://github.com/Livox-SDK/livox_ros_driver
```

### 3. Build the Workspace

```bash
cd ~/rmuc_ws
catkin_make -DCMAKE_BUILD_TYPE=Release

# Or build specific packages
catkin_make -DCATKIN_WHITELIST_PACKAGES="global_searcher;rviz_plugins;slaver;waypoint_generator"

# Source the workspace
source devel/setup.bash
```

## Usage

### SLAM and Mapping

#### Using Point-LIO for Mapping

**For Livox Avia:**
```bash
# Terminal 1: Launch Point-LIO
roslaunch point_lio mapping_avia.launch

# Terminal 2: Launch Livox driver
roslaunch livox_ros_driver livox_lidar_msg.launch

# Or play rosbag
rosbag play your_data.bag
```

**For Velodyne:**
```bash
# Launch Point-LIO with Velodyne configuration
roslaunch point_lio mapping_velody16.launch

# Play rosbag or run Velodyne driver
rosbag play velodyne_data.bag
```

**Save PCD Map:**
- Set `pcd_save_enable` to `1` in launch file
- Map saved to `Point-LIO/PCD/scans.pcd` on termination

#### Using HDL Graph SLAM

**Indoor Environment:**
```bash
# Set simulation time
rosparam set use_sim_time true

# Launch SLAM
roslaunch hdl_graph_slam hdl_graph_slam_501.launch

# Launch RViz
roscd hdl_graph_slam/rviz
rviz -d hdl_graph_slam.rviz

# Play bag file
rosbag play --clock hdl_501_filtered.bag
```

**Outdoor Environment:**
```bash
rosparam set use_sim_time true
roslaunch hdl_graph_slam hdl_graph_slam_400.launch
rosbag play --clock hdl_400.bag
```

**Save Map:**
```bash
rosservice call /hdl_graph_slam/save_map "resolution: 0.05
destination: '/full_path/map.pcd'"
```

### Localization

**Using HDL Localization:**
```bash
# Load pre-built map and localize
roslaunch hdl_localization hdl_localization.launch

# Perform global relocalization
rosservice call /relocalize
```

### Navigation and Planning

#### Simulation Environment

**Launch Complete Navigation System:**
```bash
# Terminal 1: Start Gazebo simulation
roslaunch mbot_gazebo mbot_rmuc_lidar_gazebo.launch
# Adjust model positions: RMchangdi1230 (0,0,0), mrobot (5,7.5,0.3)

# Terminal 2: Launch robot controller
roslaunch mbot_control mbot_control.launch

# Terminal 3: Launch SLAM/mapping
roslaunch fast_lio mapping_velodyne.launch

# Terminal 4: Launch path planning
roslaunch global_searcher global_searcher.launch

# Terminal 5: Launch trajectory planning
roslaunch trajectory_planning trajectory_planning.launch

# In RViz: Use 3DGoal tool (press G) to set target position
```

#### Real Robot Deployment

**Launch Navigation Stack:**
```bash
# Launch localization
roslaunch hdl_localization hdl_localization.launch

# Launch path planning with D* Lite
roslaunch bot_sim dstarlite.launch

# Launch obstacle detection
roslaunch bot_sim dbscan_bfs_3D.launch

# Launch robot control
roslaunch bot_sim ser2msg_tf_decision.launch

# Launch map server
roslaunch bot_sim map_server.launch
```

**Monitoring:**
```bash
# Monitor node health
roslaunch bot_sim node_watcher.launch

# Monitor LiDAR status
rosrun bot_sim LidarMonitor.py
```

### Planning Module Only

**Run planning without simulation:**
```bash
# Compile planning modules
catkin_make -DCATKIN_WHITELIST_PACKAGES="global_searcher;rviz_plugins;slaver;waypoint_generator"

# Launch planning
roslaunch global_searcher global_searcher.launch

# Set goal in RViz using 3DGoal tool (press G)
```

## Configuration

### Point-LIO Configuration
**File:** `RMUC_Nav/Point-LIO/config/avia.yaml` or `velodyne.yaml`

Key parameters:
- `lid_topic`: LiDAR topic name
- `imu_topic`: IMU topic name
- `extrinsic_T`, `extrinsic_R`: LiDAR-IMU extrinsic calibration
- `satu_acc`, `satu_gyro`: IMU saturation values
- `acc_norm`: Acceleration norm based on IMU units
- `timestamp_unit`: Time unit for point cloud timestamps

### HDL Graph SLAM Configuration
**File:** `RMUC_Nav/hdl_graph_slam/launch/hdl_graph_slam.launch`

Key parameters:
- `registration_method`: NDT_OMP, FAST_GICP, FAST_VGICP
- `ndt_resolution`: Voxel size (0.5-2.0m indoor, 2.0-10.0m outdoor)
- Loop closure parameters
- GPS/IMU constraint weights
- Robust kernel settings

### Planning Configuration
**File:** `sentry_planning/src/sentry_planning/.../cfg/global_searcher.yaml`

Key parameters:
- PCD map file path
- Planning constraints
- Obstacle detection parameters
- Velocity and acceleration limits

## System Workflow

### 1. Mapping Phase
1. Collect sensor data (LiDAR + IMU)
2. Run Point-LIO or HDL Graph SLAM for mapping
3. Save point cloud map (PCD format)
4. Convert to 2D grid map if needed (pcd2pgm)
5. Verify map quality in simulation

### 2. Localization Phase
1. Load pre-built map
2. Initialize robot pose (manually or global localization)
3. Run HDL localization for continuous tracking
4. Monitor localization quality

### 3. Navigation Phase
1. Load map and start localization
2. Launch path planning (global and local)
3. Launch trajectory optimization
4. Launch trajectory tracking controller
5. Set navigation goals via RViz
6. Monitor execution and replan as needed

### 4. Competition Execution
1. Pre-match: Verify all systems
2. Match start: Autonomous localization
3. Strategic patrol and positioning
4. Dynamic obstacle avoidance
5. Continuous replanning (重规划流程)
6. Emergency stop handling

## Key Features for Competition

### Robust SLAM
- High-frequency odometry for fast motions
- Loop closure for long-term operation
- Multi-sensor fusion for reliability

### Dynamic Planning
- Real-time replanning with D* Lite
- 3D obstacle detection and avoidance
- Topological path search for efficiency

### Optimal Control
- NMPC for smooth and optimal trajectories
- Constraint handling for safety
- Fast computation for real-time control

### Competition-Specific Features
- RoboMaster field knowledge
- Strategic patrol patterns
- Collision avoidance for dynamic obstacles
- Robust to occlusions and disturbances

## Important Notes

### For Point-LIO
1. **Synchronization:** Ensure LiDAR and IMU are synchronized
2. **IMU Saturation:** Configure `satu_acc` and `satu_gyro` correctly
3. **Timestamps:** Each point must have timestamp (use `livox_lidar_msg.launch`)
4. **Extrinsics:** Set `extrinsic_est_en` to false if extrinsics are known
5. **Aggressive Motion:** Set `start_in_aggressive_motion: true` for high-speed starts

### For HDL Graph SLAM
1. **Registration Method:** Use FAST_GICP for most cases
2. **NDT Resolution:** Tune based on environment (indoor vs outdoor)
3. **Loop Closure:** Enable for large environments
4. **GPS:** Use 2D constraints if altitude is unreliable

### For Competition
1. **Map Verification:** Check stairs, ramps, bridges in simulation
2. **Height Differences:** Verify passable vs non-passable regions
3. **Bridge Behavior:** Test going under vs over bridges
4. **Field Boundaries:** Ensure proper boundary handling

## Troubleshooting

### Point-LIO Issues
- **"Failed to find match for field 'time'"**: Points lack timestamps
- **High CPU usage**: Reduce point cloud density or use downsampling
- **TF_REPEATED_DATA warning**: Expected at high output frequency
- **IMU saturation**: Check and update `satu_gyro` value

### HDL Graph SLAM Issues
- **Poor odometry**: Tune `ndt_resolution` or switch to FAST_GICP
- **No loop closure**: Check loop closure parameters
- **High CPU usage**: Change `ndt_neighbor_search_method` to "DIRECT1"

### Planning Issues
- **No path found**: Check map quality and goal feasibility
- **Collision in simulation**: Verify obstacle detection parameters
- **Slow planning**: Reduce map resolution or planning horizon

## Performance

### Point-LIO
- Odometry frequency: 4-8 kHz
- Max angular velocity: 75 rad/s (tested)
- Latency: < 1ms for odometry output

### HDL Graph SLAM
- Processing speed: Real-time (depends on settings)
- Map size: Tested on large indoor/outdoor environments
- Loop closure: Automatic detection and optimization

### Trajectory Planning
- Planning frequency: Configurable (10-100 Hz typical)
- Replanning latency: < 100ms for dynamic obstacles
- Control frequency: 100-500 Hz

## Visualization

The system provides comprehensive visualization in RViz:
- Point cloud maps (colored by height/intensity)
- Robot trajectory (odometry and optimized)
- Pose graph (nodes and edges)
- Planned paths and trajectories
- Obstacle detection results
- Sensor data (LiDAR, IMU)

## License

This project is licensed under the GNU General Public License v3.0.

Copyright (C) 2025 Yuhang ZHOU together with HKU RoboMaster & Astar

## References

### Academic Papers
- **Point-LIO**: DOI: 10.1002/aisy.202200459, Advanced Intelligent Systems
- **HDL Graph SLAM**: Kenji Koide et al., A Portable 3D LIDAR-based System (2019)

### Open Source Dependencies
- [Point-LIO](https://github.com/hku-mars/Point-LIO)
- [HDL Graph SLAM](https://github.com/koide3/hdl_graph_slam)
- [HDL Localization](https://github.com/koide3/hdl_localization)
- [Fast GICP](https://github.com/SMRT-AIST/fast_gicp)
- [NDT OMP](https://github.com/koide3/ndt_omp)
- [Livox ROS Driver](https://github.com/Livox-SDK/livox_ros_driver)

### Related Documentation
- RoboMaster University Championship Official Rules
- 哈尔滨工业大学哨兵开源技术文档.pdf (in sentry_planning directory)

## Contact and Support

**Development Team:**
- HKU RoboMaster Team
- Astar Team

**Maintainer:**
- Yuhang ZHOU

**Repository:**
- GitHub: [Black-Jack-3844/HKU-RoboMaster-Sentinel-Autonomous-Navigation](https://github.com/Black-Jack-3844/HKU-RoboMaster-Sentinel-Autonomous-Navigation)

For issues, questions, or contributions, please open an issue on GitHub or contact the development team.

## Acknowledgments

This project integrates and builds upon several excellent open-source projects:
- Point-LIO by HKU MARS Lab
- HDL packages by Kenji Koide
- OCS2 by Robotic Systems Lab, ETH Zurich
- ROS community packages

Special thanks to the RoboMaster community and all contributors to the open-source robotics ecosystem.

---

**Last Updated:** 2025
**Version:** 1.0
**Status:** Active Development
