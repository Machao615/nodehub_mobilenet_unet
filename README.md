# mobilenet_unet

English | [中文](README_cn.md)

## Important Notice

⚠️ **This project is for demonstration and documentation purposes only**

- **Source Repository**: The C++ compilation source code required for this project comes from [https://github.com/D-Robotics/hobot_dnn](https://github.com/D-Robotics/hobot_dnn), and everything is subject to that source repository
- **Official Documentation**: For specific updates, please refer to the source repository and RDK official manual: [https://developer.d-robotics.cc/rdk_doc/rdk_s/Robot_development/boxs/segmentation/mobilenet_unet/](https://developer.d-robotics.cc/rdk_doc/rdk_s/Robot_development/boxs/segmentation/mobilenet_unet/)
- **Recommended Usage**: Most users can directly install pre-compiled packages via `apt install` without compilation
- **Compilation Note**: Compilation from source is only needed for custom development

## Feature Introduction

The mobilenet_unet segmentation algorithm example uses images as input, utilizes BPU for algorithm inference, and publishes messages containing segmentation results.

mobilenet_unet is an ONNX model trained using the [Cityscapes](https://www.cityscapes-dataset.com/) dataset. Model source: https://github.com/HorizonRobotics-Platform/ModelZoo/tree/master/MobilenetUnet
It supports segmentation of categories such as people, vehicles, roads, road signs, etc.

Code repository: https://github.com/D-Robotics/hobot_dnn

Application scenarios: mobilenet_unet consists of MobileNet and UNet, capable of pixel-level image content segmentation. It can be used for road recognition, remote sensing map analysis, medical image diagnosis, etc., mainly applied in autonomous driving, geological detection, medical image analysis, and other fields.

Background blur case: https://github.com/rusito-23/mobile_unet_segmentation

## Supported Platforms

| Platform | Operating Mode | Example Functions |
| ------- | ------------ | ------------------------------ |
| RDK X3, RDK X3 Module| Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) | · Start MIPI/USB camera/local playback, save rendered results locally |
| RDK X5, RDK X5 Module| Ubuntu 22.04 (Humble) | · Start MIPI/USB camera/local playback, save rendered results locally |
| RDK S100| Ubuntu 22.04 (Humble) | · Start MIPI/USB camera/local playback, save rendered results locally |
| X86     | Ubuntu 20.04 (Foxy) | · Use local playback, save rendered results locally |

## Installation Methods

### Method 1: Direct Installation (Recommended)

For most users, it's recommended to use pre-compiled packages directly:

```bash
# Update package list
sudo apt update

# Install mobilenet_unet package
sudo apt install tros-hobot-dnn-node
```

### Method 2: Compile from Source (Custom Development Only)

⚠️ **Note**: Compilation is only needed for custom development or source code modification. Source code is located at: [https://github.com/D-Robotics/hobot_dnn](https://github.com/D-Robotics/hobot_dnn)

#### Compilation Environment Requirements

- Ubuntu 20.04 (ROS2 Foxy) or Ubuntu 22.04 (ROS2 Humble)
- Corresponding ROS2 version installed
- TogetheROS.Bot or tros.b installed

#### Compilation Steps

```bash
# 1. Create workspace
mkdir -p ~/tros_ws/src
cd ~/tros_ws/src

# 2. Clone source code
git clone https://github.com/D-Robotics/hobot_dnn.git

# 3. Install dependencies
cd ~/tros_ws
rosdep install --from-paths src --ignore-src -r -y

# 4. Build
source /opt/tros/setup.bash  # or /opt/tros/humble/setup.bash
colcon build --packages-select hobot_dnn

# 5. Setup environment
source install/setup.bash
```

## Prerequisites

### RDK Platform

1. RDK has Ubuntu 20.04/Ubuntu 22.04 system image burned.

2. RDK has successfully installed TogetheROS.Bot.

3. RDK has MIPI or USB camera installed. Without camera, experience the algorithm effects through local JPEG/PNG format image playback.

### X86 Platform

1. X86 environment has Ubuntu 20.04 system image configured.

2. X86 environment system has successfully installed tros.b.

## Usage Instructions

### RDK Platform

#### Using Camera to Publish Images

##### Using MIPI Camera to Publish Images

The mobilenet_unet segmentation example subscribes to images published by the sensor package, performs inference, publishes algorithm messages, and automatically saves rendered images in the running path, named as render_frameid_timestamp_seconds_timestamp_nanoseconds.jpg.

```bash
# Configure tros.b environment (choose based on your ROS2 version)
source /opt/tros/setup.bash        # For Foxy
# or
source /opt/tros/humble/setup.bash # For Humble
```

```shell
# Configure MIPI camera
export CAM_TYPE=mipi

# Launch the launch file
ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_dump_render_img:=1 dnn_example_config_file:=config/mobilenet_unet_workconfig.json dnn_example_image_width:=1920 dnn_example_image_height:=1080
```

##### Using USB Camera to Publish Images

```bash
# Configure tros.b environment (choose based on your ROS2 version)
source /opt/tros/setup.bash        # For Foxy
# or
source /opt/tros/humble/setup.bash # For Humble
```

```shell
# Configure USB camera
export CAM_TYPE=usb

# Launch the launch file
ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_dump_render_img:=1 dnn_example_config_file:=config/mobilenet_unet_workconfig.json dnn_example_image_width:=1920 dnn_example_image_height:=1080
```

#### Using Local Image Playback

The mobilenet_unet segmentation example uses local JPEG/PNG format images for playback, and stores the rendered images with algorithm results in the local running path after inference.

```bash
# Configure tros.b environment (choose based on your ROS2 version)
source /opt/tros/setup.bash        # For Foxy
# or
source /opt/tros/humble/setup.bash # For Humble
```

```shell
# Launch the launch file
ros2 launch dnn_node_example dnn_node_example_feedback.launch.py dnn_example_config_file:=config/mobilenet_unet_workconfig.json dnn_example_image:=config/raw_unet.jpg
```

### X86 Platform

#### Using Local Image Playback

The mobilenet_unet segmentation example uses local JPEG/PNG format images for playback, and stores the rendered images with algorithm results in the local running path after inference.

```bash
# Configure tros.b environment (choose based on your ROS2 version)
source /opt/tros/setup.bash        # For Foxy
# or
source /opt/tros/humble/setup.bash # For Humble
```

```shell
# Launch the launch file
ros2 launch dnn_node_example dnn_node_example_feedback.launch.py dnn_example_config_file:=config/mobilenet_unet_workconfig.json dnn_example_image:=config/raw_unet.jpg
```

## Result Analysis

### Using Camera to Publish Images

The running terminal outputs the following information:

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

The output log shows that the topic for publishing algorithm inference results is `hobot_dnn_detection`, and the topic for subscribing to images is `/hbmem_img`. The image publishing frame rate adapts to the algorithm inference output frame rate. Additionally, RDK will render semantic segmentation results and store images in the running path, which will reduce the frame rate.

Original image:
![raw](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/03_boxs/segmentation/image/mobilenet_unet/mobilenet_unet_raw.jpeg)

Rendered image:
![render_web](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/03_boxs/segmentation/image/mobilenet_unet/mobilenet_unet_render_web.jpeg)

### Using Local Image Playback

The running terminal outputs the following information:

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

The output log shows that the algorithm uses the input image config/raw_unet.jpeg for inference, and the stored rendered image file name is render_unet_feedback_0_0.jpeg. Rendered image effect:

![render_feedback](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/03_boxs/segmentation/image/mobilenet_unet/mobilenet_unet_render_feedback.jpeg)
