# Slamtec RPLIDAR Public SDK for C++ as a ROS package

## Introduction

This is the public SDK of RPLIDAR products in C++ as a ROS package.

Slamtec RPLIDAR(<https://www.slamtec.com>) series is a set of high-performance and low-cost LIDAR(<https://en.wikipedia.org/wiki/Lidar>) sensors, which is the perfect sensor of 2D SLAM, 3D reconstruction, multi-touch, and safety applications.

The original SDK is here: <https://github.com/Slamtec/rplidar_sdk>

The README from the original SDK is in the src/sdk directory of this package.

## License

The original SDK is licensed under the BSD 2-clause license.
Original SDK copyright belongs to Slamtec Co., Ltd.

This ROS package is also licensed under the BSD 2-clause license.

## Compiler warnings

Compiling the original SDK will result in a number of compiler warnings.

Some of these warnings (mainly '-Wunused-parameter') have been fixed in this package, others have not been fixed.
So compiling this package will also result in compiler warnings.

## Another Slamtec ROS package

Slamtec also provides a ROS package: <https://github.com/slamtec/rplidar_ros>.

That package includes the SDK sources and provides a ROS node; but it does not provide a library for the SDK.
It does provide some usefull udev-rules.

---
![ROS 2](https://img.shields.io/badge/ROS%202-Jazzy%20(Ubuntu%2024.04)-blue?style=flat&logo=ros&logoSize=auto)
![C++17](https://img.shields.io/badge/C++-17-green)
![License](https://img.shields.io/badge/License-BSD--2--Clause-orange)
[![ROS 2 Build](https://github.com/RbSCR/rplidar_sdk_ros2/actions/workflows/ros2-build.yml/badge.svg)](https://github.com/RbSCR/rplidar_sdk_ros2/actions/workflows/ros2-build.yml)

Tested with:

![RPLIDAR C1](https://img.shields.io/badge/RPLIDAR--C1-green)
![ROS2 Jazzy](https://img.shields.io/badge/ROS2-Jazzy-blue)

---
