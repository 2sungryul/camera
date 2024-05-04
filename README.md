# camera
CSI camera ros2 test package on Jetson nano

dependency : ros2 foxy, gstreamer, opencv 4.8, cmake 3.16

Publisher node captures an image from csi camera via gstreamer and publishes a compressed image topic with jpg format using a ros2 interface sensor_msgs/msg/CompressedImage.

Subscriber node subscribes the compressed image topic and sends it to PC via gstreamer.

Open linux terminal on Jetson nano

$ cd ~/ros2_ws/src

$ git clone https://github.com/2sungryul/camera.git

Make sure you need to change the ip address in sub.cpp to that of your own PC.

$ cd ~/ros2_ws

$ colcon build --symlink-install --packages-select camera

$ source install/local_setup.bash

$ ros2 run camera pub

Open new linux terminal on Jetson nano

$ ros2 run camera sub

Open windows powershell on PC

PS> gst-launch-1.0 -v udpsrc port=8001 ! ‘application/x-rtp,encoding-name=(string)H264,payload=(int)96’ ! rtph264depay ! queue ! avdec_h264 ! videoconvert! autovideosink

![image](https://github.com/2sungryul/camera/assets/67367753/61171e79-f093-441a-ad77-ae4f7b8adc19)

Gstreamer command on Linux for video streaming from csi camera

$ gst-launch-1.0 nvarguscamerasrc sensor-id=0 ! 'video/x-raw(memory:NVMM),format=NV12,width=640,height=360' ! nvvidconv flip-method=0 ! nvv4l2h264enc insert-sps-pps=true ! h264parse ! rtph264pay pt=96 ! udpsink host=203.234.58.121 port=8001 sync=false -q
