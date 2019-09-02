# AR Tag

## Tracking Ar tags in ROS
Version : ubuntu 16.04 / kinetic

### reference
[Traking AR tags in ros](https://varunagrawal.github.io/2017/10/23/ar-track/)
[Ubuntu install of ROS Kinetic](http://wiki.ros.org/Installation/Ubuntu)
[AR_Marker](https://cafe.naver.com/openrt/18964)
[AR_tag reference](https://web.archive.org/web/20120814082107/http://www.artag.net/)

### install & running guide
install the packages
```bash
sudo apt-get install ros-kinetic-ar-track-alvar
```
(추가)web cam driver, kinect driver 설치
```bash
# web cam driver
sudo apt-get install ros-kinetic-usb-cam
# Kinect driver
sudo apt-get install ros-kinetic-freenect-launch
```
usb 카메라 실행
```bash
rosrun usb_cam usb_cam_node
```

tracker를 위한 roslaunch 파일 생성
`usb_cam_ar_track.launch`
```xml
<launch>

    <arg name="marker_size"          default="5.0" />
    <arg name="max_new_marker_error" default="0.05" />
    <arg name="max_track_error"      default="0.05" />

    <arg name="cam_image_topic"      default="/usb_cam/image_raw" />
    <arg name="cam_info_topic"       default="/usb_cam/camera_info" />
    <arg name="output_frame"         default="/map" />

    <node name="ar_track_alvar" pkg="ar_track_alvar" type="individualMarkersNoKinect" respawn="false" output="screen">
		<param name="marker_size"           type="double" value="$(arg marker_size)" />
		<param name="max_new_marker_error"  type="double" value="$(arg max_new_marker_error)" />
		<param name="max_track_error"       type="double" value="$(arg max_track_error)" />
		<param name="output_frame"          type="string" value="$(arg output_frame)" />

		<remap from="camera_image"  to="$(arg cam_image_topic)" />
		<remap from="camera_info"   to="$(arg cam_info_topic)" />
	</node>
</launch>
```
출력을 위한 output frame 설정 / default값을 /map /header_cam 으로 설정
```
rosrun tf2_ros static_transform_publisher 0 0 0 0 0 0 1 /map /header_cam
```

`AR tracker 실행`
roslaunch usb_cam_ar_track.launch

