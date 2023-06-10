# hayden_opencv_tools

# 01 orb_compare

### 功能

测试orb_slam2和opencv中的orb特征提取器的功能差异

### 编译

```
cd orb_compare && mkdir build
cd build
cmake ..
make
```

### 运行

```
../bin/orb_compare ../../Figs/test.png
```

### 结果

![img](Figs/result.png)

save_slam_orb

![img](Figs/save_slam_orb.jpg)

save_opencv_orb

![img](Figs/save_opencv_orb.jpg)

# 02  Aruco Marker 功能包

### 依赖

- [OpenCV](https://github.com/opencv/opencv)

### 编译

```
cd aruco
mkdir build
cd build
cmake ..
make
```

### 生成marker

```
../bin/generate_marker 6X6_250 0 50 100 150 200
```

"6X6_250"指的是字典名，表示生成一个拥有250个6X6 marker的字典

"0 50 100 150 200"marker的id号，需要生成几个marker就在后面写几个id

### 普通相机位姿估计

```
../bin/estimate_pose 6X6_250 ../config/param.yaml ../../Figs/0.jpg
```

"param.yaml"配置文件，包含相机内参和实际打印出来的marker的边长

"0.jpg"测试照片

结果:
![](Figs/aruco.jpg)
```
marker 0 : rvec->[1.87135,1.91877,-0.346457], tvec->[0.0138539,0.0666454,0.234551]
marker 1 : rvec->[1.78666,1.90724,-0.646364], tvec->[0.110677,0.0647166,0.21952]
marker 2 : rvec->[1.95008,1.95168,-0.199137], tvec->[-0.0863286,0.0133206,0.251713]
marker 3 : rvec->[2.02278,2.05034,-0.347798], tvec->[0.11477,-0.0359532,0.271804]
marker 4 : rvec->[1.98997,1.99299,-0.274266], tvec->[0.0113914,-0.0375248,0.269299]
```

### 鱼眼相机位姿估计

```
../bin/estimate_pose 6X6_250 ../config/fisheye_param.yaml ../data/test_fisheye.jpg
```

"fisheye_param.yaml"配置文件，包含鱼眼相机内参和实际打印出来的marker的边长

"test_fisheye.jpg"测试照片
未实现

# 03 tool


### 功能

主要是包含一些平常使用的opencv工具包

### 依赖

- [OpenCV](https://github.com/opencv/opencv)

### 工具

**record_usb_img**: 抓取usb相机图像到本地

# 04 calibrate
### 依赖

- [OpenCV](https://github.com/opencv/opencv)

### 编译

```
​```
git clone https://github.com/chennuo0125-HIT/opencv_demo.git
cd calibrate
mkdir build
cd build
cmake ..
make
```

### 标定

```
../bin/calibrator ../config/param.yaml
```

结果保存在 "out_camera_data.xml"文件中

### 配置文件解释

```
<?xml version="1.0"?>
<opencv_storage>
<Settings>
  <!-- 标定板尺寸 -->
  <BoardSize_Width> 9</BoardSize_Width>
  <BoardSize_Height>6</BoardSize_Height>
  
  <!-- 标定板栅格大小(单位mm) -->
  <Square_Size>70</Square_Size>
  
  <!-- 标定板类型(默认选择棋盘格) -->
  <Calibrate_Pattern>"CHESSBOARD"</Calibrate_Pattern>
  
  <!-- 输入数据源
  	如果选择相机在线标定，直接将下面的Input参数置为"相机id"
  	如果选择输入视频进行标定，Input参数置为视频路径"video.mp4"
  	如果选择抓拍的图片集进行标定，则Input参数设置为相片集路径配置文件(如下面默认配置所示)
  	-->
  <Input>"/home/cn/learn/opencv_demo/calibrate/config/img_path.xml"</Input>
  
  <!--  If true (non-zero) we flip the input images around the horizontal axis.-->
  <Input_FlipAroundHorizontalAxis>0</Input_FlipAroundHorizontalAxis>
  
  <!-- Time delay between frames in case of camera. -->
  <Input_Delay>100</Input_Delay>	
  
  <!-- How many frames to use, for calibration. -->
  <Calibrate_NrOfFrameToUse>11</Calibrate_NrOfFrameToUse>
  <!-- Consider only fy as a free parameter, the ratio fx/fy stays the same as in the input cameraMatrix. 
	   Use or not setting. 0 - False Non-Zero - True-->
  <Calibrate_FixAspectRatio> 1 </Calibrate_FixAspectRatio>
  <!-- If true (non-zero) tangential distortion coefficients  are set to zeros and stay zero.-->
  <Calibrate_AssumeZeroTangentialDistortion>1</Calibrate_AssumeZeroTangentialDistortion>
  <!-- If true (non-zero) the principal point is not changed during the global optimization.-->
  <Calibrate_FixPrincipalPointAtTheCenter> 1 </Calibrate_FixPrincipalPointAtTheCenter>
  
  <!-- The name of the output log file. -->
  <Write_outputFileName>"out_camera_data.xml"</Write_outputFileName>
  <!-- If true (non-zero) we write to the output file the feature points.-->
  <Write_DetectedFeaturePoints>1</Write_DetectedFeaturePoints>
  <!-- If true (non-zero) we write to the output file the extrinsic camera parameters.-->
  <Write_extrinsicParameters>1</Write_extrinsicParameters>
  <!-- If true (non-zero) we show after calibration the undistorted images.-->
  <Show_UndistortedImage>1</Show_UndistortedImage>
  
  <!-- 是否使用鱼眼模型进行标定，如果是鱼眼相机的标定，此处一定要设置为1，否则标定结果很烂 -->
  <Calibrate_UseFisheyeModel>0</Calibrate_UseFisheyeModel>
  <!-- If true (non-zero) distortion coefficient k1 will be equals to zero.-->
  <Fix_K1>0</Fix_K1>
  <!-- If true (non-zero) distortion coefficient k2 will be equals to zero.-->
  <Fix_K2>0</Fix_K2>
  <!-- If true (non-zero) distortion coefficient k3 will be equals to zero.-->
  <Fix_K3>0</Fix_K3>
  <!-- If true (non-zero) distortion coefficient k4 will be equals to zero.-->
  <Fix_K4>1</Fix_K4>
  <!-- If true (non-zero) distortion coefficient k5 will be equals to zero.-->
  <Fix_K5>1</Fix_K5>
</Settings>
</opencv_storage>

```

注: 以上用中文注释的参数需要了解并根据需求配置之外，其它参数可以使用默认的．

### 标定结果
