# gstreamer

jetson보드에는 gsteramer가 설치되어 있음

$ gst-launch-1.0 --help -> 도움말 출력되면 설치확인

# How to install gstreamer on wsl2-Ubuntu20.04

$ sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio

$ gst-launch-1.0 --help -> 도움말 출력되면 설치완료

# how to send camera image on Jetson nano

$ gst-launch-1.0 nvarguscamerasrc sensor-id=0 ! 'video/x-raw(memory:NVMM),format=NV12,width=640,height=360' ! nvvidconv flip-method=0 ! nvv4l2h264enc insert-sps-pps=true ! h264parse ! rtph264pay pt=96 ! udpsink host=192.168.0.19 port=8001 sync=false -q

# wsl2-Ubuntu20.04에서 영상수신하는 명령어

$ gst-launch-1.0 -v udpsrc port=8001 ! application/x-rtp,encoding-name=H264,payload=96 ! rtph2
64depay ! queue ! avdec_h264 ! videoconvert! autovideosink

# Windows에서 gstreamer 설치하는 방법

https://gstreamer.freedesktop.org/download<img width="921" height="88" alt="image" src="https://github.com/user-attachments/assets/ad2221fe-86d7-48d8-a1a6-5591edb3ce22" />

# Windows에서 영상수신하는 명령어

> gst-launch-1.0 -v udpsrc port=8001 ! ‘application/x-rtp,encoding-name=(string)H264,payload=(int)96’ ! rtph264depay ! queue ! avdec_h264 ! videoconvert! autovideosink -> 종료는 ctrl+c
