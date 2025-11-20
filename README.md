# HKU RoboMaster Sentinel Autonomous Navigation

<div align="center">

**Advanced Autonomous Navigation System for RoboMaster University Championship**

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)
[![ROS Version](https://img.shields.io/badge/ROS-Melodic%20|%20Noetic-blue)](http://wiki.ros.org)

</div>

## 🎯 Project Overview

This repository contains a complete autonomous navigation solution for the **Sentinel Robot** in the RoboMaster University Championship (RMUC). The system integrates state-of-the-art SLAM, localization, path planning, and optimal control to enable fully autonomous operation in dynamic competition environments.

Developed by **HKU RoboMaster & Astar**, this project provides:
- ✅ High-frequency LiDAR-Inertial odometry (4-8 kHz)
- ✅ Robust 3D SLAM with loop closure
- ✅ Real-time localization and mapping
- ✅ Dynamic path planning and replanning
- ✅ Optimal trajectory generation and tracking
- ✅ Complete simulation environment

## 📁 Repository Structure

```
├── RMUC_Nav/              # Navigation and SLAM workspace
│   ├── Point-LIO/         # High-frequency LiDAR-IMU odometry
│   ├── hdl_graph_slam/    # 3D Graph SLAM with loop closure
│   ├── hdl_localization/  # Real-time 3D localization
│   ├── fast_gicp/         # Fast point cloud registration
│   ├── ndt_omp/           # Multi-threaded NDT scan matching
│   ├── pcd2pgm_package/   # Map format conversion
│   └── bot_sim/           # Robot control and decision-making
│
├── sentry_planning/       # Motion planning and control
│   ├── ocs2/              # Optimal control framework
│   ├── sentry_planning/   # Path and trajectory planning
│   └── sentry_gazebo_2024/# Gazebo simulation environment
│
├── PROJECT_DESCRIPTION.md # Complete project documentation
└── LICENSE                # GNU GPL v3.0
```

## 🚀 Key Features

### SLAM & Localization
- **Point-LIO**: Tightly-coupled LiDAR-IMU odometry for aggressive motions (75 rad/s tested)
- **HDL Graph SLAM**: 6DOF graph SLAM with NDT/GICP scan matching and loop closure
- **HDL Localization**: UKF-based real-time localization with global relocalization
- **Multi-sensor Fusion**: LiDAR, IMU, GPS integration for robust state estimation

### Motion Planning & Control
- **Topological Path Search**: Efficient global path planning
- **Dynamic Replanning**: Real-time obstacle avoidance with D* Lite
- **NMPC**: Nonlinear Model Predictive Control for optimal trajectory tracking
- **OCS2 Framework**: Advanced optimal control for switched systems

### Competition-Ready
- ⚡ Real-time performance optimized for competition
- 🎮 Complete Gazebo simulation environment
- 🗺️ RoboMaster field map support
- 🔄 Dynamic replanning for moving obstacles
- 📊 Comprehensive visualization in RViz

## 🛠️ Quick Start

### Prerequisites

- Ubuntu 20.04 (recommended) or 18.04
- ROS Noetic (or Melodic)
- LiDAR: Velodyne or Livox series
- IMU: 6-axis or 9-axis

### Installation

```bash
# Create workspace
mkdir -p ~/rmuc_ws/src && cd ~/rmuc_ws/src

# Clone repository
git clone https://github.com/Black-Jack-3844/HKU-RoboMaster-Sentinel-Autonomous-Navigation.git

# Install dependencies
cd ~/rmuc_ws
rosdep install --from-paths src --ignore-src -r -y

# Build
catkin_make -DCMAKE_BUILD_TYPE=Release
source devel/setup.bash
```

### Running SLAM

```bash
# Point-LIO for mapping
roslaunch point_lio mapping_avia.launch

# HDL Graph SLAM
roslaunch hdl_graph_slam hdl_graph_slam.launch
```

### Running Navigation

```bash
# Launch simulation environment
roslaunch mbot_gazebo mbot_rmuc_lidar_gazebo.launch

# Launch navigation stack
roslaunch global_searcher global_searcher.launch
roslaunch trajectory_planning trajectory_planning.launch

# Set goals in RViz (press 'G' for 3D Goal tool)
```

## 📖 Documentation

For complete documentation, please see:
- **[PROJECT_DESCRIPTION.md](PROJECT_DESCRIPTION.md)** - Comprehensive project documentation
- **[RMUC_Nav/README.md](RMUC_Nav/README.md)** - SLAM and localization details
- **[sentry_planning/src/sentry_planning/README.md](sentry_planning/src/sentry_planning/README.md)** - Planning module details

## 🎥 System Demonstration

The system includes:
- Real-time SLAM and mapping
- Autonomous navigation with dynamic obstacle avoidance
- Strategic patrol patterns for competition
- High-frequency odometry for fast motions
- Robust operation under sensor noise and disturbances

## 🔧 System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Sensor Layer                         │
│  [Velodyne/Livox LiDAR] [IMU] [GPS (optional)]         │
└────────────────┬────────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────────┐
│              SLAM & Localization                        │
│  Point-LIO → HDL Graph SLAM → HDL Localization         │
└────────────────┬────────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────────┐
│              Path Planning                              │
│  Global Search → Trajectory Generation → Optimization   │
└────────────────┬────────────────────────────────────────┘
                 │
┌────────────────▼────────────────────────────────────────┐
│           Trajectory Tracking                           │
│        NMPC Controller → Robot Control                  │
└─────────────────────────────────────────────────────────┘
```

## 📊 Performance

- **Odometry Frequency**: 4-8 kHz (Point-LIO)
- **Planning Frequency**: 10-100 Hz (configurable)
- **Control Frequency**: 100-500 Hz
- **Max Angular Velocity**: 75 rad/s (tested)
- **Replanning Latency**: < 100ms

## 🏆 Competition Features

Specifically designed for RoboMaster competition:
- Autonomous patrol on competition field
- Strategic positioning and navigation
- Dynamic obstacle avoidance for moving robots
- Robust to occlusions and disturbances
- Real-time performance guarantee
- Comprehensive monitoring and diagnostics

## 🤝 Contributing

This is an open-source project developed for the robotics community. Contributions, issues, and feature requests are welcome!

## 📄 License

This project is licensed under the **GNU General Public License v3.0** - see the [LICENSE](LICENSE) file for details.

Copyright (C) 2025 Yuhang ZHOU together with HKU RoboMaster & Astar

## 🙏 Acknowledgments

This project builds upon excellent open-source work:
- [Point-LIO](https://github.com/hku-mars/Point-LIO) by HKU MARS Lab
- [HDL Graph SLAM](https://github.com/koide3/hdl_graph_slam) by Kenji Koide
- [OCS2](https://github.com/leggedrobotics/ocs2) by Robotic Systems Lab, ETH Zurich

Special thanks to:
- HKU RoboMaster Team
- Astar Team
- RoboMaster community
- All open-source contributors

## 📞 Contact

For questions or collaboration:
- Open an issue on GitHub
- Contact the HKU RoboMaster team

---

<div align="center">

**Built for RoboMaster | Powered by ROS | Open Source**

⭐ Star this repo if you find it helpful! ⭐

</div>
