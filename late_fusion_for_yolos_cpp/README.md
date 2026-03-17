# Late_Fusion_for_yolos_cpp

This package fuses:
- vision_msgs/msg/Detection2DArray
- sensor_msgs/msg/Image (debug images)

from up to 6 YOLO detector nodes into:

- /fused/detections
- /fused/debug_image (2x3 stitched grid)


------------------------------------------------------------
FEATURES
------------------------------------------------------------

- Supports 1–6 detector nodes
- Fresh-only detection fusion (no reuse across cycles)
- 2x3 image stitching layout
- Fully parameterized via YAML
- Compatible with ros2_yolos_cpp
- Built using ament_cmake
- Designed for ROS 2 Jazzy


------------------------------------------------------------
DEPENDENCIES
------------------------------------------------------------

- ROS 2 Jazzy
- rclcpp
- sensor_msgs
- vision_msgs
- cv_bridge
- image_transport
- OpenCV

Install cv_bridge if needed:

  sudo apt install ros-jazzy-cv-bridge

------------------------------------------------------------
PARAMETERS
------------------------------------------------------------

Defined in config/late_fusion_params.yaml

- Input Detection and Image Topics
- Grid size: Rows and Columns
- Tile Width and Height

------------------------------------------------------------
IMAGE STITCHING LAYOUT
------------------------------------------------------------

Fixed 2x3 grid:
```
  0   1   2
  3   4   5
```
The order of img_inputs determines placement:

- img_inputs[0] → top-left
- img_inputs[1] → top-middle
- img_inputs[2] → top-right
- img_inputs[3] → bottom-left
- img_inputs[4] → bottom-middle
- img_inputs[5] → bottom-right

To change the layout, reorder entries in late_fusion_params.yaml.
No rebuild required (unless not using symlink install).


------------------------------------------------------------
DETECTION FUSION LOGIC
------------------------------------------------------------

- Only detections received since the last publish cycle are used.
- Fresh detections are appended into a single Detection2DArray.
- The fused message uses:
    header.frame_id = "fused"
    header.stamp = now()

This is append-only aggregation, not geometric or semantic fusion.

## Demo on Rviz
<img src="assets/Fused_Rviz_view.gif">
