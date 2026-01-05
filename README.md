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
For details, please refer to the uploaded file keyparaseting.txt.

## 5. License

The source code of this package is released under the [**GPLv2**](http://www.gnu.org/licenses/) license. For commercial use, please contact me at <zzh268@foxmail.com> and Prof. Linyang Li at <linyangli@whu.edu.cn> to discuss an alternative license.

## 6.Acknowledgements
Thanks FAST-LIVO2, FAST-LIO2, and LVI-SAM. The code is modified based on their excellent open-source work.

