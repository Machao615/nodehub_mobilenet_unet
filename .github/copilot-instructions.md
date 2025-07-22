# GitHub Copilot Instructions for mobilenet_unet

## Project Overview

This is a **documentation-only repository** for the mobilenet_unet ROS2 segmentation algorithm example. The actual source code is maintained at [D-Robotics/hobot_dnn](https://github.com/D-Robotics/hobot_dnn).

## Key Architecture Points

- **Framework**: ROS2 (Foxy/Humble) with TogetheROS.Bot/tros.b runtime
- **Hardware**: Designed for RDK (X3/X5/S100) boards with BPU acceleration
- **Model**: MobileNet+UNet ONNX model trained on Cityscapes dataset
- **Input Sources**: MIPI/USB cameras or local JPEG/PNG images
- **Output**: Semantic segmentation masks with topic `hobot_dnn_detection`

## Critical Conventions

### Documentation Structure
- Maintain parallel Chinese (`README_cn.md`) and English (`README.md`) documentation
- Always include language switcher links: `[English](README.md) | 中文` / `English | [中文](README_cn.md)`
- Reference official RDK documentation: `https://developer.d-robotics.cc/rdk_doc/`

### Installation Guidance Pattern
```bash
# Method 1: Direct install (recommended for users)
sudo apt install tros-hobot-dnn-node

# Method 2: Source compilation (only for custom development)
# Always reference https://github.com/D-Robotics/hobot_dnn
```

### Environment Setup Pattern
```bash
# Version-specific sourcing
source /opt/tros/setup.bash        # For Foxy
source /opt/tros/humble/setup.bash # For Humble
```

### Launch File Patterns
- Camera modes: Set `CAM_TYPE=mipi` or `CAM_TYPE=usb`
- Config file: Always reference `config/mobilenet_unet_workconfig.json`
- Image dimensions: Standard `1920x1080` for camera input
- Feedback mode: Use `dnn_node_example_feedback.launch.py` for local images

## Development Workflows

### When Adding New Platforms
1. Update support table in both README files
2. Verify ROS2 distribution compatibility (Foxy=Ubuntu 20.04, Humble=Ubuntu 22.04)
3. Test both camera and feedback modes

### When Updating Instructions
1. Always update both language versions simultaneously
2. Preserve the "Important Notice" section emphasizing this is documentation-only
3. Keep references to upstream sources current

### Source Code Modifications
- **Never modify source code here** - this repository is documentation-only
- Direct users to fork/modify the upstream repository: `https://github.com/D-Robotics/hobot_dnn`
- For custom development, guide users through the colcon build process in their workspace

## Key Dependencies
- `tros-hobot-dnn-node` package (installable via apt)
- ROS2 environment (Foxy/Humble)
- BPU hardware acceleration (RDK boards) or CPU fallback (X86)

## Testing Patterns
- Use sample image: `config/raw_unet.jpg`
- Expected output naming: `render_unet_feedback_0_0.jpeg`
- Monitor topics: `/hbmem_img` (input), `hobot_dnn_detection` (output)

## Important Constraints
- This repository should remain lightweight (documentation + configs only)
- Always direct implementation questions to upstream repository
- Maintain clear separation between usage instructions and development guidance
