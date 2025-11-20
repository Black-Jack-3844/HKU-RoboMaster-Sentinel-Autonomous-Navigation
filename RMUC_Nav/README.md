# RMUC_Nav

## Overview

RMUC_Nav is a comprehensive navigation package for the RoboMaster University Championship Sentinel Robot, providing state-of-the-art 3D LiDAR SLAM, localization, and autonomous navigation capabilities.

## Key Components

This workspace integrates multiple advanced navigation modules:

- **Point-LIO**: High-frequency LiDAR-Inertial Odometry (4-8 kHz) for robust state estimation
- **hdl_graph_slam**: 3D Graph SLAM with loop closure detection for large-scale mapping
- **hdl_localization**: Real-time 3D localization using UKF and multi-threaded NDT
- **hdl_global_localization**: Global relocalization without initial pose
- **fast_gicp**: Fast and accurate point cloud registration (GPU-accelerated)
- **ndt_omp**: Multi-threaded Normal Distributions Transform for scan matching
- **pcd2pgm_package**: Point cloud to 2D occupancy grid conversion
- **bot_sim**: Robot simulation, control, and decision-making for competition

## Quick Start

See the [complete project description](../PROJECT_DESCRIPTION.md) for detailed installation and usage instructions.

### Basic Build

```bash
cd ~/catkin_ws
catkin_make -DCMAKE_BUILD_TYPE=Release
source devel/setup.bash
```

### Run SLAM

```bash
# For Point-LIO with Livox
roslaunch point_lio mapping_avia.launch

# For HDL Graph SLAM with Velodyne
roslaunch hdl_graph_slam hdl_graph_slam.launch
```

## Documentation

For complete documentation, including:
- Detailed component descriptions
- System architecture
- Installation instructions
- Usage examples
- Configuration guides
- Troubleshooting

Please refer to [PROJECT_DESCRIPTION.md](../PROJECT_DESCRIPTION.md) in the root directory.

## License

GNU General Public License v3.0 - See LICENSE file for details
