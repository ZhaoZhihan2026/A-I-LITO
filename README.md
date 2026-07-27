# A-I-LITO: Adaptively enhanced thermal-LiDAR-IMU tightly-coupled positioning method based on field-of-view perception

## 1. Introduction
To address the significant decline in positioning accuracy and stability of existing multi-sensor fusion simultaneous localization and mapping in challenging scenarios such as low illumination and field-of-view change, a multi-modal tightly-coupled positioning framework integrating thermal camera/light detection and ranging/inertial measurement unit is established, and a method with adaptive enhancement based on field-of-view perception is proposed.

### 1.1 Related video

Our accompanying video is now available on [**Baidu Netdisk**](https://pan.baidu.com/s/1yraP4kM9DqLU3qpNP8CQ4g, Extraction code: qrm9).

## 2. Prerequisited

### 2.1 Ubuntu and ROS

Ubuntu 18.04~20.04.  [ROS Installation](http://wiki.ros.org/ROS/Installation).

### 2.2 PCL && Eigen && OpenCV && Sophus

PCL>=1.8, Follow [PCL Installation](https://pointclouds.org/). 

Eigen>=3.3.4, Follow [Eigen Installation](https://eigen.tuxfamily.org/index.php?title=Main_Page).

OpenCV>=4.2, Follow [Opencv Installation](http://opencv.org/).

Sophus Installation for the non-templated/double-only version.

## 3. Build

Clone the repository and catkin_make:

```
cd ~/catkin_ws/src
git clone https://github.com/ZhaoZhihan2026/A-I-LITO
cd ../
catkin_make
source ~/catkin_ws/devel/setup.bash
```

## 4. Key Parameter Setting Details
Supplementary files including `KeyParaSetting.txt` and `Sensitivity analysis of parameters.docx` are attached, which can be used to check the detailed values of all thresholds and the comparative experimental results under parameter perturbation.
### 4.1 Basic Threshold Parameters
The core threshold parameters used in the experiment are determined through multiple experiments based on typical urban scene datasets (roads, blocks, semi-structured parks), and can be fine-tuned for other scenarios. Specific parameter statistics are shown in Table 1:

| Parameter Category | Physical Significance | Values |
|--------------------|-----------------------|--------|
| Spatial Structure Parameters | Inner sphere radius $R_{inner}$, outer sphere radius $R_{outer}$ | 5 m, 20 m |
| Adaptive Factor Parameters | Normalized threshold $t$ | 0.6 |
| Adaptive Factor Parameters | Sigmoid Center Offset $s$ | 0.5 - 0.6 |
| Adaptive Factor Parameters | Sigmoid Steepness Coefficient $k$ | 7.0 - 8.5 |

*Table 1 Threshold Parameter Statistics*

The experiment adopts uniform parameters of $t = 0.6$, $s = 0.55$, and $k = 8.0$.

### 4.2 Dynamically Optimized Core Parameters
This paper determines the core parameter value range for dynamic optimization based on the adaptive factor $\alpha$, the tuning logic for field-of-view changes, default configurations of mainstream open-source frameworks, optimization suggestions from the open-source community, typical values from existing research, and extensive experimental verification. The specific details are as follows:

#### 4.2.1 Point Cloud Downsampling Density
The value range of point cloud downsampling density is from a minimum of 1/5 to a maximum of 1/3. This parameter represents the extraction ratio of point cloud downsampling. A value of 1/5 means that one point is retained for every five points, and 1/3 means that one point is retained for every three points. The larger the value, the higher the downsampling density and the more points are retained in the point cloud.

**Tuning Logic**:
- Narrow fields of view: Point cloud is highly dense with redundant data, select a lower downsampling density (close to 1/5) to compress data volume while retaining key geometric features.
- Wide fields of view: Point cloud is sparse, adopt a higher downsampling density (close to 1/3) to retain effective feature points and ensure constraint sufficiency for subsequent registration.

**Determination Basis**:
1. Default settings of mainstream open-source frameworks: Refer to commonly used point cloud downsampling intervals in FAST-LIO2, LVI-SAM, etc.
2. Open-source community suggestions: FAST-LIVO2 author Zheng Chunran suggests taking one point every five or three points in the LIVMapper community.
3. Extensive experimental verification: Comparative experiments in five typical scenarios (Street04, Street06, Street08, City01, City02) verify the rationality of this range.

#### 4.2.2 Root Voxel Map Resolution
The root voxel map resolution is adaptively adjusted by changing the size of the basic voxel unit during voxel map construction. The edge length of a single voxel ranges from 0.15m to 0.4m (corresponding to a resolution of 6.67 to 2.5 voxels/m, as spatial resolution is the reciprocal of voxel size).

**Tuning Logic**:
- Narrow fields of view: Point cloud is dense and susceptible to noise, use higher resolution (close to 6.67 voxels/m) to smooth noise and capture local structural details via octree subdivision.
- Wide fields of view: Point cloud is sparse and evenly distributed, use lower resolution (close to 2.5 voxels/m) to ensure plane fitting integrity and reduce storage/computational costs.

**Determination Basis**:
1. Typical values from existing research: Refer to voxel edge length settings in Davide De Pazzi et al.'s 2022 Sensors paper "3D Radiometric Mapping by Means of LiDAR SLAM and Thermal Camera Data Fusion".
2. Default configurations of mainstream open-source systems: Draw on recommended values from better_fastlio2, R3LIVE++, etc.
3. Open-source community experience: Refer to voxel resolution setting experience in GitHub issues (https://github.com/hku-mars/FAST_LIO/issues/169).
4. Extensive experimental verification: Verified in FOV sudden change scenarios to achieve optimal positioning accuracy while ensuring real-time performance.

#### 4.2.3 Maximum Number of Iterations for Multi-layer Update in VIO
The range of the maximum number of iterations for multi-layer update in VIO is from 4 to 5 times per layer.

**Tuning Logic**:
- Narrow FOV: Heat sources are dense, occlusion is severe, and optimization problem is highly nonlinear; set maximum iterations close to 5 to ensure full convergence.
- Open FOV: Thermal radiation distribution is uniform, optimization problem structure is simple; adjust iterations to close to 4 to balance accuracy and computational overhead.

**Determination Basis**:
1. Default configuration of mainstream open-source frameworks: Refer to FAST-LIVO2's default recommended value for VIO image pyramid iteration number per layer.
2. Extensive experimental verification: Comparative experiments under different FOV change scenarios verify that 4-5 iterations meet the optimization requirements of the infrared image gray error model.

## 5. License

The source code of this package is released under the [**GPLv2**](http://www.gnu.org/licenses/) license. For commercial use, please contact me at <zzh268@foxmail.com> and Prof. Linyang Li at <linyangli@whu.edu.cn> to discuss an alternative license.

## 6. Acknowledgements
Thanks FAST-LIVO2, FAST-LIO2, and LVI-SAM. The code is modified based on their excellent open-source work.
