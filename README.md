<div align="center">
<h1 align="center">zion_ros</h1>

<h3>Zion wearable system - ROS</h3>

<img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="https://opensource.org/licenses/Apache-2.0" />
<img src="https://img.shields.io/github/last-commit/badges/shields/master" alt="git-last-commit" />

</div>

---

## Overview
This package lets you use the software packages of Zion wearable system which includes Nvidia Jetson Orin platform and Stereolabs ZED mini camera. It provides access to the following:

  - All of ZED camera features and data as it described in - [zed-ros-wrapper](https://github.com/stereolabs/zed-ros-wrapper/tree/master)
  - Bag recording interface 

---

## Installation

### Prerequisites

- [Ubuntu 20.04 (Focal Fossa)](https://releases.ubuntu.com/focal/) / [JetPack](https://docs.nvidia.com/sdk-manager/install-with-sdkm-jetson/index.html)>=5.1
- [ZED SDK](https://www.stereolabs.com/developers/release/latest/) >= v4.8 
- [CUDA](https://developer.nvidia.com/cuda-downloads)
- [ROS Noetic](https://wiki.ros.org/noetic/Installation/Ubuntu)
- [zed-ros-wrapper (v4.0.6 release or newer)](https://github.com/stereolabs/zed-ros-wrapper) 

### Build the package

To install the **zion_ros2**, open a bash terminal, clone the package from Github, and build it:

```bash
$ cd ~/catkin_ws/src/ 
$ git clone --recursive https://github.com/nimiCurtis/zion_ros
$ cd ..
$ rosdep install --from-paths src --ignore-src -r -y
$ catkin_make
$ source ~/.bashrc
```

#### Update the local repository

To update the repository to the latest release you must use the following command to retrieve the latest commits of `zion_ros` and of all the submodules:

```bash
$ git checkout master # if you are not on the main branch  
$ git pull --recurse-submodules # update recursively all the submodules
```

### Python Dependencies

TBD .. 
<!-- For python dependencies installation, open a terminal and use the ```install.sh``` file.

**For PC platform:**
```bash
$ ./install.sh 
```

**For Jetson Orin platform:**
```bash
$ ./install.sh jet
``` -->

<!-- ## Known issues -->

---

## Get Started

### Nodes

To start the **zion_ros** nodes, open a terminal and use the command `roslaunch`:

#### ZED Mini Node:
```bash
$ roslaunch zion_zed_ros_interface zed_operate.launch.py
```
- Adjust the camera parameters at [zion_zed_ros_interface/params](zion_zed_ros_interface/params)
- The launch file is also launching the ```obj_detect_publisher_node``` when setting the arg ```publish_obj_det``` to **true**

#### Object detection converter publisher:
- Node name: 
```obj_detect_publisher_node```
- This node is responsible for pulling the specific object instance from the object list publishing by the ```zed_node``` and publish it seperatly in different topic , based on the parameters in ```zion_zed_ros_interface/params/obj_detect_converter.yaml```
- Adjust the node parameters at [zion_zed_ros_interface/params/obj_detect_converter.yaml](zion_zed_ros_interface/params/obj_detect_converter.yaml)
- You can modify the object instance to listen to on the fly, by using the service ```/obj_detect_publisher_node/set_target_object```

```bash
$ rosservice call /obj_detect_publisher_node/set_target_object "label: <string> instance_id: <int>"
```

#### Recording ZED Node:
```bash
$ roslaunch zion_zed_ros_interface zed_recording.launch
```

- This command will launch the ```zed_recording_node``` which start a recording session of the topics specified in the ```zion_zed_ros_interface/params/record.yaml```. Adjust this file if needed. You can use CLI to choose different destination folder prefix and select the list of topics you manage to record. 
- Select start/stop recording by using the service ```~record ``` as follow:
```bash
$ rosservice call /zion/zed_recording_node/record "data: false" # false -> stop recording
                                                                # true -> start recording
```

- Stopping record will terminate the rosbag record and save the bag but the recording node will keep spinning such that you can record again by using the service

---

### TODO: 

- [x] Reset the odometry when start recording
- [ ] List of topics from CLI
- [ ] update zed_examples to the prereqiuerments
- [ ] Images of the system
- [ ] Add dependencies, requierments and update install.sh
