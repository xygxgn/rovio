## ROS Noetic version of rovio.

### Prerequisites
- **System**
  - Ubuntu 20.04
  - ROS Noetic
- **Libraries**
  - OpenCV 4.2.0 (default version of ROS Noetic)
  - [Ceres Solver-1.14.0](http://ceres-solver.org/installation.html)

### Build
- **download the source package**
  - `mkdir ~/catkin_ws/src && cd ~/catkin_ws/src`
  - `git clone --recurse-submodules https://github.com/xygxgn/rovio.git`
  - `cd rovio`
- **build with the default OpenCV 4.2.0 of ROS Noetic**
  - `gedit CMakeLists.txt`
  - Modify `set(OpenCV_DIR "/usr/lib/x86_64-linux-gnu/cmake/opencv4")`
  - *do NOT forget source the ros workspace*
  - `source /opt/ros/noetic/setup.bash`
  - `cd ~/catkin_ws/`
  - `catkin_make`
- **build with OpenCV installed by yourself *(install in `/usr/local`)***
  - `gedit CMakeLists.txt`
  - Modify `set(OpenCV_DIR "/usr/local/lib/cmake/opencv4")`
  - *do NOT forget source your own cv_bridge workspace*
  - `source ~/cv_bridge/devel/setup.bash`
  - `cd ~/catkin_ws/`
  - `catkin_make`

- **Notes**
  - ***The version of the OpenCV must be consistent with the version of OpenCV used by cv-bridge***

### Run
- **launch the package**
  - `cd ~/catkin_ws`
  - `source devel/setup.bash`
  - `roslaunch rovio rovio_node.launch`
- **play rosbag**
  - `rosbag play MH_01_easy.bag`

If you find this work useful or interesting, please kindly give us a star :star:, thanks!



# README #

This repository contains the ROVIO (Robust Visual Inertial Odometry) framework. The code is open-source (BSD License). Please remember that it is strongly coupled to on-going research and thus some parts are not fully mature yet. Furthermore, the code will also be subject to changes in the future which could include greater re-factoring of some parts.

Video: https://youtu.be/ZMAISVy-6ao

Papers:
* http://dx.doi.org/10.3929/ethz-a-010566547 (IROS 2015)
* http://dx.doi.org/10.1177/0278364917728574 (IJRR 2017)

Please also have a look at the wiki: https://github.com/ethz-asl/rovio/wiki

### Install without opengl scene ###
Dependencies:
* ros
* kindr (https://github.com/ethz-asl/kindr)
* lightweight_filtering (as submodule, use "git submodule update --init --recursive")

```
#!command

catkin build rovio --cmake-args -DCMAKE_BUILD_TYPE=Release
```

### Install with opengl scene ###
Additional dependencies: opengl, glut, glew (sudo apt-get install freeglut3-dev, sudo apt-get install libglew-dev)
```
#!command

catkin build rovio --cmake-args -DCMAKE_BUILD_TYPE=Release -DMAKE_SCENE=ON
```

### Euroc Datasets ###
The rovio_node.launch file loads parameters such that ROVIO runs properly on the Euroc datasets. The datasets are available under:
http://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets

### Further notes ###
* Camera matrix and distortion parameters should be provided by a yaml file or loaded through rosparam
* The cfg/rovio.info provides most parameters for rovio. The camera extrinsics qCM (quaternion from IMU to camera frame, Hamilton-convention) and MrMC (Translation between IMU and Camera expressed in the IMU frame) should also be set there. They are being estimated during runtime so only a rough guess should be sufficient.
* Especially for application with little motion fixing the IMU-camera extrinsics can be beneficial. This can be done by setting the parameter doVECalibration to false. Please be carefull that the overall robustness and accuracy can be very sensitive to bad extrinsic calibrations.
