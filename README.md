# lidar_camera_calibration
An algorithm used to calib extrinsic parameter between fisheye camera and VLP-16
This repo is modified from https://github.com/icameling/lidar_camera_calibration.

# USAGE:
## 1、DATA COLLECTION
Record the bag including the image topic and pointcloud topic.Please keep still when recording data, and make sure the number of topic messeages is less than 5.
Rename the bag as fisheye-1.bag、fisheye-2.bag...Using other name please change the parameter in the launch file.
## 2、IMAGE UNDISTORTION
Organize the calibration results into the following yaml format. Here also set 3 parameters of the calibration board, the number of corner points of the two axes of the calibration board, and the size of a grid of the calibration board (in meters).
```
%YAML:1.0

K: !!opencv-matrix
   rows: 3
   cols: 3
   dt: d
   data: [323.82823054353539, 0, 565.5876786211957,0, 322.99613532520558, 557.2287783914765,0, 0, 1]
d: !!opencv-matrix
   rows: 4
   cols: 1
   dt: d
   data: [-0.014256972346800642, -0.00019721990901892033, -0.001333996556456368, 0.000035260300340928418]

Camera.width: 1120
Camera.height: 1120

grid_length: 0.045
corner_in_x: 11
corner_in_y: 8
```

One thing worth mention is that our distort model is **KB4**,which have four distiort coefficients:k1、k2、k3、k4.

```
roslaunch ilcc2 image_corners.launch
```
The rectify image will be saved under process data
## 3、FIND THE IMAGE CORNER
Copy the rectify image to a folder in windows system and change the path according yours.Using demo_all_pic.m to find the image corner.
You will get a txt file that records the coordinates of the corner points.Copy the txt file to process data(in Ubuntu)
![微信截图_20221207143448](https://user-images.githubusercontent.com/77578976/206106174-0bf6efc7-7105-45de-bffc-d41cc9a0eb6b.png)
## 4、FIND THE LIDAR CORNER
```
roslaunch ilcc2 lidar_corners.launch
```
For how to use this part, please refer to [icameling/lidar_camera_calibration/激光提取角点ILCC2](https://github.com/icameling/lidar_camera_calibration#5%E6%BF%80%E5%85%89%E6%8F%90%E5%8F%96%E6%A0%87%E5%AE%9A%E6%9D%BF%E8%A7%92%E7%82%B9ilcc2)

## 5、CALIB THE EXTRINSIC PARAMETER

```
roslaunch ilcc2 calib_lidar_cam.launch
```
## 6、VISUALIZATION
First run:
```
roslaunch ilcc2 pcd2image.launch
```
and then run:
```
rosbag play -l bagname.bag
```
using -l can loop playback.
