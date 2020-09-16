# A-LOAM
## Advanced implementation of LOAM

A-LOAM is an Advanced implementation of LOAM (J. Zhang and S. Singh. LOAM: Lidar Odometry and Mapping in Real-time), which uses Eigen and Ceres Solver to simplify code structure. This code is modified from LOAM and [LOAM_NOTED](https://github.com/cuitaixiang/LOAM_NOTED). This code is clean and simple without complicated mathematical derivation and redundant operations. It is a good learning material for SLAM beginners.

<img src="https://github.com/HKUST-Aerial-Robotics/A-LOAM/blob/devel/picture/kitti.png" width = 55% height = 55%/>

**Modifier:** [Tong Qin](http://www.qintonguav.com), [Shaozu Cao](https://github.com/shaozu)


## 1. Prerequisites
### 1.1 **Ubuntu** and **ROS**
Ubuntu 64-bit 16.04 or 18.04.
ROS Kinetic or Melodic. [ROS Installation](http://wiki.ros.org/ROS/Installation)


### 1.2. **Ceres Solver**
Follow [Ceres Installation](http://ceres-solver.org/installation.html).

### 1.3. **PCL**
Follow [PCL Installation](http://www.pointclouds.org/downloads/linux.html).


## 2. Build A-LOAM
Clone the repository and catkin_make:

```
    cd ~/catkin_ws/src
    git clone https://github.com/HKUST-Aerial-Robotics/A-LOAM.git
    cd ../
    catkin_make
    source ~/catkin_ws/devel/setup.bash
```

## 3. Velodyne VLP-16 Example
Download [NSH indoor outdoor](https://drive.google.com/file/d/1s05tBQOLNEDDurlg48KiUWxCp-YqYyGH/view) to YOUR_DATASET_FOLDER. 

```
    roslaunch aloam_velodyne aloam_velodyne_VLP_16.launch
    rosbag play YOUR_DATASET_FOLDER/nsh_indoor_outdoor.bag
```


## 4. KITTI Example (Velodyne HDL-64)
Download [KITTI Odometry dataset](http://www.cvlibs.net/datasets/kitti/eval_odometry.php) to YOUR_DATASET_FOLDER and set the `dataset_folder` to /home/arghya/KITTI/data/odometry/ (use your name instead of arghya) and `sequence_number` parameter to 00 (or anything else like 01, 02, 03, ...) in `kitti_helper.launch` file.  

```
    roslaunch aloam_velodyne aloam_velodyne_HDL_64.launch
    roslaunch aloam_velodyne kitti_helper.launch
```
The directory structure for providing data looks like the following:
```
home
    |-- arghya (use your name instead of arghya)
        |-- KITTI
            |-- data
                |-- odometry
                    |-- results
                    |    |-- 00.txt (Ground Truth Poses file)
                    |-- sequences
                    |    |-- 00
                    |        |-- image_0
                    |        |   |-- 000000.png
                    |        |   |-- 000001.png
                    |        |   |-- .......png
                    |        |-- image_1
                    |        |   |-- 000000.png
                    |        |   |-- 000001.png
                    |        |   |-- .......png
                    |        |-- calib.txt
                    |        |-- times.txt
                    |-- velodyne
                        |-- sequences
                            |-- 00
                                |-- velodyne
                                    |-- 000000.bin
                                    |-- 000001.bin
                                    |-- .......bin
 ```
In order to convert the sequence as a bag file, set `to_bag` parameter to true and `output_bag_file` parameter to /home/arghya/KITTI/data/odometry/kitti.bag (use your name instead of arghya) inside `kitti_helper.launch`. 
You can confirm the data structure of the files inside KITTI directory. 
```
    sudo snap install tree
    cd KITTI
    tree -d
```
Once the sequences are converted to bag file and saved under the name kitti.bag, you can later on play from that saved bag file with the following command.
```
    roslaunch aloam_velodyne aloam_velodyne_HDL_64.launch
    rosbag play /home/arghya/KITTI/data/odometry/kitti.bag
```
              
<img src="https://github.com/HKUST-Aerial-Robotics/A-LOAM/blob/devel/picture/kitti_gif.gif" width = 720 height = 351 />

## 5. Docker Support
To further facilitate the building process, we add docker in our code. Docker environment is like a sandbox, thus makes our code environment-independent. To run with docker, first make sure [ros](http://wiki.ros.org/ROS/Installation) and [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) are installed on your machine. Then add your account to `docker` group by `sudo usermod -aG docker $YOUR_USER_NAME`. **Relaunch the terminal or logout and re-login if you get `Permission denied` error**, type:
```
cd ~/catkin_ws/src/A-LOAM/docker
make build
```
The build process may take a while depends on your machine. After that, run `./run.sh 16` or `./run.sh 64` to launch A-LOAM, then you should be able to see the result.


## 6.Acknowledgements
Thanks for LOAM(J. Zhang and S. Singh. LOAM: Lidar Odometry and Mapping in Real-time) and [LOAM_NOTED](https://github.com/cuitaixiang/LOAM_NOTED).

