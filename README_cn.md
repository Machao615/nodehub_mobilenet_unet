# mobilenet_unet

[English](README.md) | 中文

## 重要说明

⚠️ **本项目仅作为说明和演示用途**

- **源码仓库**：本项目所需的C++编译源码来自于 [https://github.com/D-Robotics/hobot_dnn](https://github.com/D-Robotics/hobot_dnn)，一切以该源地址为准
- **官方文档**：具体更新请以源地址和RDK官方手册为主：[https://developer.d-robotics.cc/rdk_doc/rdk_s/Robot_development/boxs/segmentation/mobilenet_unet/](https://developer.d-robotics.cc/rdk_doc/rdk_s/Robot_development/boxs/segmentation/mobilenet_unet/)
- **推荐使用方式**：大多数用户可以直接通过 `apt install` 安装预编译的功能包，无需编译
- **编译说明**：只有在需要自定义开发时才需要从源码编译

## 功能介绍

mobilenet_unet分割算法示例使用图片作为输入，利用BPU进行算法推理，发布包含分割结果msg。

mobilenet_unet是使用[Cityscapes](https://www.cityscapes-dataset.com/)数据集训练出来的Onnx模型，模型来源： https://github.com/HorizonRobotics-Platform/ModelZoo/tree/master/MobilenetUnet 。
支持对人、车辆、路面、路标等类别进行分割。

代码仓库： https://github.com/D-Robotics/hobot_dnn

应用场景：mobilenet_unet由MobileNet与UNet组成，能够从像素级别分割图像内容，可实现道路识别、遥感地图分析、医学影像诊断等功能，主要应用于自动驾驶、地质检测，医疗影像分析等领域。

背景虚化案例： https://github.com/rusito-23/mobile_unet_segmentation

## 支持平台

| 平台    | 运行方式      | 示例功能                       |
| ------- | ------------ | ------------------------------ |
| RDK X3, RDK X3 Module| Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) | · 启动MIPI/USB摄像头/本地回灌，渲染结果保存在本地 |
| RDK X5, RDK X5 Module| Ubuntu 22.04 (Humble) | · 启动MIPI/USB摄像头/本地回灌，渲染结果保存在本地 |
| RDK S100| Ubuntu 22.04 (Humble) | · 启动MIPI/USB摄像头/本地回灌，渲染结果保存在本地 |
| X86     | Ubuntu 20.04 (Foxy) | · 使用本地回灌，渲染结果保存在本地 |

## 安装方式

### 方式一：直接安装（推荐）

对于大多数用户，推荐直接使用预编译的功能包：

```bash
# 更新软件包列表
sudo apt update

# 安装mobilenet_unet功能包
sudo apt install tros-hobot-dnn-node
```

### 方式二：从源码编译（仅限自定义开发）

⚠️ **注意**：只有在需要自定义开发或修改源码时才需要编译。源码位于：[https://github.com/D-Robotics/hobot_dnn](https://github.com/D-Robotics/hobot_dnn)

#### 编译环境要求

- Ubuntu 20.04 (ROS2 Foxy) 或 Ubuntu 22.04 (ROS2 Humble)
- 已安装ROS2对应版本
- 已安装TogetheROS.Bot或tros.b

#### 编译步骤

```bash
# 1. 创建工作空间
mkdir -p ~/tros_ws/src
cd ~/tros_ws/src

# 2. 克隆源码
git clone https://github.com/D-Robotics/hobot_dnn.git

# 3. 安装依赖
cd ~/tros_ws
rosdep install --from-paths src --ignore-src -r -y

# 4. 编译
source /opt/tros/setup.bash  # 或 /opt/tros/humble/setup.bash
colcon build --packages-select hobot_dnn

# 5. 配置环境
source install/setup.bash
```

## 准备工作

### RDK平台

1. RDK已烧录好Ubuntu 20.04/Ubuntu 22.04系统镜像。

2. RDK已成功安装TogetheROS.Bot。

3. RDK已安装MIPI或者USB摄像头，无摄像头的情况下通过回灌本地JPEG/PNG格式图片的方式体验算法效果。

### X86平台

1. X86环境已配置好Ubuntu 20.04系统镜像。

2. X86环境系统已成功安装tros.b。

## 使用介绍

### RDK平台

#### 使用摄像头发布图片

##### 使用MIPI摄像头发布图片

mobilenet_unet分割示例订阅sensor package发布的图片，经过推理后发布算法msg，并在运行路径下自动保存渲染后的图片，命名方式为render_frameid_时间戳秒_时间戳纳秒.jpg。

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
# 配置tros.b环境
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
# 配置tros.b环境
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>

```shell
# 配置MIPI摄像头
export CAM_TYPE=mipi

# 启动launch文件
ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_dump_render_img:=1 dnn_example_config_file:=config/mobilenet_unet_workconfig.json dnn_example_image_width:=1920 dnn_example_image_height:=1080
```

##### 使用USB摄像头发布图片

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
# 配置tros.b环境
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
# 配置tros.b环境
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>

```shell
# 配置USB摄像头
export CAM_TYPE=usb

# 启动launch文件
ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_dump_render_img:=1 dnn_example_config_file:=config/mobilenet_unet_workconfig.json dnn_example_image_width:=1920 dnn_example_image_height:=1080
```

#### 使用本地图片回灌

mobilenet_unet分割示例使用本地JPEG/PNG格式图片回灌，经过推理后将算法结果渲染后的图片存储在本地的运行路径下。

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
# 配置tros.b环境
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
# 配置tros.b环境
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>


```shell
# 启动launch文件
ros2 launch dnn_node_example dnn_node_example_feedback.launch.py dnn_example_config_file:=config/mobilenet_unet_workconfig.json dnn_example_image:=config/raw_unet.jpg
```

### X86平台

#### 使用本地图片回灌

mobilenet_unet分割示例使用本地JPEG/PNG格式图片回灌，经过推理后将算法结果渲染后的图片存储在本地的运行路径下。

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
# 配置tros.b环境
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
# 配置tros.b环境
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>

```shell
# 启动launch文件
ros2 launch dnn_node_example dnn_node_example_feedback.launch.py dnn_example_config_file:=config/mobilenet_unet_workconfig.json dnn_example_image:=config/raw_unet.jpg
```

## 结果分析

### 使用摄像头发布图片

在运行终端输出如下信息：

```shell
[example-3] [WARN] [1655095719.035374293] [example]: Create ai msg publisher with topic_name: hobot_dnn_detection
[example-3] [WARN] [1655095719.035493746] [example]: Create img hbmem_subscription with topic_name: /hbmem_img
[example-3] [WARN] [1655095720.693716453] [img_sub]: Sub img fps 6.85
[example-3] [WARN] [1655095721.072909861] [example]: Smart fps 5.85
[example-3] [WARN] [1655095721.702680885] [img_sub]: Sub img fps 3.97
[example-3] [WARN] [1655095722.486407545] [example]: Smart fps 3.54
[example-3] [WARN] [1655095722.733431396] [img_sub]: Sub img fps 4.85
[example-3] [WARN] [1655095723.888407681] [example]: Smart fps 4.28
[example-3] [WARN] [1655095724.069835983] [img_sub]: Sub img fps 3.74
[example-3] [WARN] [1655095724.900725522] [example]: Smart fps 3.95
[example-3] [WARN] [1655095725.093525634] [img_sub]: Sub img fps 3.91
```

输出log显示，发布算法推理结果的topic为`hobot_dnn_detection`，订阅图片的topic为`/hbmem_img`，其中图片发布的帧率根据会根据算法推理输出帧率自适应。此外，RDK上会渲染语义分割结果并存储图片在运行路径下，会使帧率下降。

原始图片：
![raw](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/03_boxs/segmentation/image/mobilenet_unet/mobilenet_unet_raw.jpeg)

渲染后的图片：
![render_web](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/03_boxs/segmentation/image/mobilenet_unet/mobilenet_unet_render_web.jpeg)

### 使用本地图片回灌

在运行终端输出如下信息：

```shell
[example-1] [INFO] [1654769881.171005839] [dnn]: The model input 0 width is 2048 and height is 1024
[example-1] [INFO] [1654769881.171129709] [example]: Set output parser.
[example-1] [INFO] [1654769881.171206707] [UnetPostProcess]: Set out parser
[example-1] [INFO] [1654769881.171272663] [dnn]: Task init.
[example-1] [INFO] [1654769881.173427170] [dnn]: Set task_num [2]
[example-1] [INFO] [1654769881.173587414] [example]: The model input width is 2048 and height is 1024
[example-1] [INFO] [1654769881.173646870] [example]: Dnn node feed with local image: config/raw_unet.jpeg
[example-1] [INFO] [1654769881.750748126] [example]: task_num: 2
[example-1] [INFO] [1654769881.933418736] [example]: Output from image_name: config/raw_unet.jpeg, frame_id: feedback, stamp: 0.0
[example-1] [INFO] [1654769881.933542440] [UnetPostProcess]: outputs size: 1
[example-1] [INFO] [1654769881.995920396] [UnetPostProcess]: Draw result to file: render_unet_feedback_0_0.jpeg
```

输出log显示，算法使用输入的图片config/raw_unet.jpeg推理，存储的渲染图片文件名为render_unet_feedback_0_0.jpeg，渲染图片效果：

![render_feedback](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/03_boxs/segmentation/image/mobilenet_unet/mobilenet_unet_render_feedback.jpeg)