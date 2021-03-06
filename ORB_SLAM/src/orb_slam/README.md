

## catkin_build

 **修改成catkin_make方式的ros软件包**

- 修改CmakeLists.txt
- 添加package.xml
- 修改 src/ORBextractor.cc 文件
- 修改Thirdparty/g2o/g2o/solvers/linea_solver_eigen.h文件
- catkin_make

###  摄像头单次运行测试
```
run ORB_SLAM node
cd /home/hualong/ssd/coding/ros-SLAM-ws/src/ORB_SLAM
rosrun  ORB_SLAM ORB_SLAM   Data/ORBvoc.txt  Data/Settings.yaml

需要一个发布 /camera/image_raw的单目RGB话题
rosrun uvc_camera uvc_camera_node /image_raw:=/camera/image_raw

rviz 可视化
rosrun rviz rviz -d Data/rviz.rviz

```

###  数据集运行测试
run ORB_SLAM node
cd /home/hualong/ssd/coding/ros-SLAM-ws/src/ORB_SLAM
rosrun  ORB_SLAM ORB_SLAM   Data/ORBvoc.txt  Data/Settings.yaml

rviz 可视化
rosrun rviz rviz -d Data/rviz.rviz

rosbag play --pause Example.bag


## 源码分析

### 前端特征提取与特征追踪

### 单目VO初始化
在 Tracking::Initlize()中调用Initializer::Initialize
mpInitializer->Initialize(mCurrentFrame, mvIniMatches, Rcw, tcw, mvIniP3D, vbTriangulated)







##Check out our new [ORB-SLAM2](https://github.com/raulmur/ORB_SLAM2) (Monocular, Stereo and RGB-D)
---
# ORB-SLAM Monocular
**Authors:** [Raul Mur-Artal](http://webdiis.unizar.es/~raulmur/), [Juan D. Tardos](http://webdiis.unizar.es/~jdtardos/), [J. M. M. Montiel](http://webdiis.unizar.es/~josemari/) and [Dorian Galvez-Lopez](http://doriangalvez.com/) ([DBoW2](https://github.com/dorian3d/DBoW2))

**Current version:** 1.0.1 (see [Changelog.md](https://github.com/raulmur/ORB_SLAM/blob/master/Changelog.md))

ORB-SLAM is a versatile and accurate Monocular SLAM solution able to compute in real-time the camera trajectory and a sparse 3D reconstruction of the scene in a wide variety of environments, ranging from small hand-held sequences to a car driven around several city blocks. It is able to close large loops and perform global relocalisation in real-time and from wide baselines.

See our project webpage: http://webdiis.unizar.es/~raulmur/orbslam/

###Related Publications:

[1] Raúl Mur-Artal, J. M. M. Montiel and Juan D. Tardós. **ORB-SLAM: A Versatile and Accurate Monocular SLAM System**. *IEEE Transactions on Robotics,* vol. 31, no. 5, pp. 1147-1163, 2015. (2015 IEEE Transactions on Robotics **Best Paper Award**). **[PDF](http://webdiis.unizar.es/~raulmur/MurMontielTardosTRO15.pdf)**.

[2] Dorian Gálvez-López and Juan D. Tardós. **Bags of Binary Words for Fast Place Recognition in Image Sequences**. *IEEE Transactions on Robotics,* vol. 28, no. 5, pp.  1188-1197, 2012. **[PDF](http://doriangalvez.com/php/dl.php?dlp=GalvezTRO12.pdf)**.


#1. License

ORB-SLAM is released under a [GPLv3 license](https://github.com/raulmur/ORB_SLAM/blob/master/License-gpl.txt). For a list of all code/library dependencies (and associated licenses), please see [Dependencies.md](https://github.com/raulmur/ORB_SLAM/blob/master/Dependencies.md).

For a closed-source version of ORB-SLAM for commercial purposes, please contact the authors: orbslam (at) unizar (dot) es. 

If you use ORB-SLAM in an academic work, please cite:

    @article{murAcceptedTRO2015,
      title={{ORB-SLAM}: a Versatile and Accurate Monocular {SLAM} System},
      author={Mur-Artal, Ra\'ul, Montiel, J. M. M. and Tard\'os, Juan D.},
      journal={IEEE Transactions on Robotics},
      volume={31},
      number={5},
      pages={1147--1163},
      doi = {10.1109/TRO.2015.2463671},
      year={2015}
     }


#2. Prerequisites (dependencies)

##2.1 Boost

We use the Boost library to launch the different threads of our SLAM system.

	sudo apt-get install libboost-all-dev 
	- 多线程 Boost library : launch the different threads of our SLAM system.

##2.2 ROS
We use ROS to receive images from the camera or from a recorded sequence (rosbag), and for visualization (rviz, image_view). 
**We have tested ORB-SLAM in Ubuntu 12.04 with ROS Fuerte, Groovy and Hydro; and in Ubuntu 14.04 with ROS Indigo**. 
If you do not have already installed ROS in your computer, we recommend you to install the Full-Desktop version of ROS Fuerte (http://wiki.ros.org/fuerte/Installation/Ubuntu).

**If you use ROS Indigo, remove the depency of opencv2 in the manifest.xml.**

##2.3 OpenCV
We use OpenCV to manipulate images and features. If you use a ROS version older than ROS Indigo, OpenCV is already included in the ROS distribution. In newer version of ROS, OpenCV is not included and you will need to install it. **We tested OpenCV 2.4**. Dowload and install instructions can be found at: http://opencv.org/
- 图像处理 OpenCV library : manipulate images and features

##2.4 g2o (included in Thirdparty)
We use a modified version of g2o (see original at https://github.com/RainerKuemmerle/g2o) to perform optimizations.
In order to compile g2o you will need to have installed Eigen3 (at least 3.1.0).
	
	sudo apt-get install libeigen3-dev

##2.5 DBoW2 (included in Thirdparty)
We make use of some components of the DBoW2 and DLib library (see original at https://github.com/dorian3d/DBoW2) for place recognition and feature matching. There are no additional dependencies to compile DBoW2.


#3. Installation

1. Make sure you have installed ROS and all library dependencies (boost, eigen3, opencv, blas, lapack).

2. Clone the repository:

		git clone https://github.com/raulmur/ORB_SLAM.git ORB_SLAM
		
3. Add the path where you cloned ORB-SLAM to the `ROS_PACKAGE_PATH` environment variable. To do this, modify your .bashrc and add at the bottom the following line (replace PATH_TO_PARENT_OF_ORB_SLAM):

		export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:PATH_TO_PARENT_OF_ORB_SLAM

4. Build g2o. Go into `Thirdparty/g2o/` and execute:

		mkdir build
		cd build
		cmake .. -DCMAKE_BUILD_TYPE=Release
		make 
		- 图优化 g2o : use a modified version of g2o to perform optimizations(g2o依赖于Eigen库)

	*Tip: To achieve the best performance in your computer, set your favorite compilation flags in line 61 and 62 of* `Thirdparty/g2o/CMakeLists.txt` 
		  (by default -03 -march=native)

5. Build DBoW2. Go into Thirdparty/DBoW2/ and execute:

		mkdir build
		cd build
		cmake .. -DCMAKE_BUILD_TYPE=Release
		make  
		- 回环检测 DBoW2 : use of some components of the DBoW2 and DLib library  for place recognition and feature matching

	*Tip: Set your favorite compilation flags in line 4 and 5 of* `Thirdparty/DBoW2/CMakeLists.txt` (by default -03 -march=native)

6. Build ORB_SLAM. In the ORB_SLAM root execute:

	**If you use ROS Indigo, remove the depency of opencv2 in the manifest.xml.**

		mkdir build
		cd build
		cmake .. -DROS_BUILD_TYPE=Release
		make

	*Tip: Set your favorite compilation flags in line 12 and 13 of* `./CMakeLists.txt` (by default -03 -march=native)

#4. Usage

**See section 5 to run the Example Sequence**.

1. Launch ORB-SLAM from the terminal (`roscore` should have been already executed):

		rosrun ORB_SLAM ORB_SLAM PATH_TO_VOCABULARY PATH_TO_SETTINGS_FILE

  You have to provide the path to the ORB vocabulary and to the settings file. The paths must be absolute or relative   to the ORB_SLAM directory.  
  We already provide the vocabulary file we use in `ORB_SLAM/Data/ORBvoc.txt.tar.gz`. Uncompress the file, as it will be loaded much faster.

2. The last processed frame is published to the topic `/ORB_SLAM/Frame`. You can visualize it using `image_view`:

		rosrun image_view image_view image:=/ORB_SLAM/Frame _autosize:=true

3. The map is published to the topic `/ORB_SLAM/Map`, the current camera pose and global world coordinate origin are sent through `/tf` in frames `/ORB_SLAM/Camera` and `/ORB_SLAM/World` respectively.  Run `rviz` to visualize the map:
	
	*in ROS Fuerte*:

		rosrun rviz rviz -d Data/rviz.vcg

	*in ROS Groovy or a newer version*:

		rosrun rviz rviz -d Data/rviz.rviz

4. ORB_SLAM will receive the images from the topic `/camera/image_raw`. You can now play your rosbag or start your camera node. 
If you have a sequence with individual image files, you will need to generate a bag from them. We provide a tool to do that: https://github.com/raulmur/BagFromImages.


**Tip: Use a roslaunch to launch `ORB_SLAM`, `image_view` and `rviz` from just one instruction. We provide an example**:

*in ROS Fuerte*:

	roslaunch ExampleFuerte.launch

*in ROS Groovy or a newer version*:

	roslaunch ExampleGroovyOrNewer.launch


#5. Example Sequence
We provide the settings and the rosbag of an example sequence in our lab. In this sequence you will see a loop closure and two relocalisation from a big viewpoint change.

1. Download the rosbag file:  
	http://webdiis.unizar.es/~raulmur/orbslam/downloads/Example.bag.tar.gz. 

	Alternative link: https://drive.google.com/file/d/0B8Qa2__-sGYgRmozQ21oRHhUZWM/view?usp=sharing

	Uncompress the file.

2. Launch ORB_SLAM with the settings for the example sequence. You should have already uncompressed the vocabulary file (`/Data/ORBvoc.txt.tar.gz`)

  *in ROS Fuerte*:

	  roslaunch ExampleFuerte.launch

	*in ROS Groovy or newer versions*:

	  roslaunch ExampleGroovyHydro.launch

3. Once the ORB vocabulary has been loaded, play the rosbag (press space to start):

		rosbag play --pause Example.bag


#6. The Settings File

ORB_SLAM reads the camera calibration and setting parameters from a YAML file. We provide an example in `Data/Settings.yaml`, where you will find all parameters and their description. We use the camera calibration model of OpenCV.

Please make sure you write and call your own settings file for your camera (copy the example file and modify the calibration)

#7. Failure Modes

You should expect to achieve good results in sequences similar to those in which we show results in our paper [1], in terms of camera movement and texture in the environment. In general our Monocular SLAM solution is expected to have a bad time in the following situations:
- No translation at system initialization (or too much rotation).
- Pure rotations in exploration.
- Low texture environments.
- Many (or big) moving objects, especially if they move slowly.

The system is able to initialize from planar and non-planar scenes. In the case of planar scenes, depending on the camera movement relative to the plane, it is possible that the system refuses to initialize, see the paper [1] for details. 



##List of Known Dependencies
###ORB-SLAM Monocular 1.0.1

In this document we list all the pieces of code included  by ORB-SLAM Monocular and linked libraries which are not property of the authors of ORB-SLAM Monocular.


#####Code in **src** and **include** folders

* *ORBextractor.cc*.
This is a modified version of orb.cpp of OpenCV library. The original code is BSD licensed.

* *PnPsolver.h, PnPsolver.cc*.
This is a modified version of the epnp.h and epnp.cc of Vincent Lepetit. 
This code can be found in popular BSD licensed computer vision libraries as [OpenCV](https://github.com/Itseez/opencv/blob/master/modules/calib3d/src/epnp.cpp) and [OpenGV](https://github.com/laurentkneip/opengv/blob/master/src/absolute_pose/modules/Epnp.cpp). The original code is FreeBSD.

* Function *ORBmatcher::DescriptorDistance* in *ORBmatcher.cc*.
The code is from: http://graphics.stanford.edu/~seander/bithacks.html#CountBitsSetParallel.
The code is in the public domain.

#####Code in Thirdparty folder

* All code in **DBoW2** folder.
This is a modified version of [DBoW2](https://github.com/dorian3d/DBoW2) and [DLib](https://github.com/dorian3d/DLib) library. All files included are BSD licensed.

* All code in **g2o** folder.
This is a modified version of [g2o](https://github.com/RainerKuemmerle/g2o). All files included are BSD licensed.

#####Library dependencies 

* **Boost**. [Boost Software License](http://www.boost.org/users/license.html).

* **OpenCV**.
BSD licensed.

* **Eigen3**.
For versions greater than 3.1.1 is MPL2, earlier versions are LGPLv3.

* **ROS**.
BSD licensed. In the manifest.xml the only declared package dependencies are roscpp, tf, sensor_msgs, opencv2, image_transport, cv_bridge, which are all BSD licensed.



#Changelog of ORB-SLAM Monocular

## 1.0.1 (18-01-2016)
- Save/load vocabulary from text file instead of cv::FileStorage. 
- Unused files from Thirdparty/DBoW2 and Thirdparty/g2o have been removed. g2o files have been reorganized and we compile only
one library libg2o.so instead of four libraries (libg2o_core.so, libg2o_types.so, etc)
- We use now Eigen solver instead of Cholmod solver in g2o. Removed dependency on Suitesparse and Cholmod.
- Several bugs have been fixed.

## 1.0.0 (03-03-2015)
First ORB-SLAM Monocular version

