# gstreamer

# how to send camera image on Raspberry Pi5

- Raspberry Pi5보드에는 gsteramer가 설치되어 있음
```bash
$ gst-launch-1.0 --help 
```
-도움말 출력되면 설치확인
```bash
$ gst-launch-1.0 -q libcamerasrc ! video/x-raw,format=YUY2,width=640,height=480 ! queue ! videoconvert ! videoflip method=rotate-180 ! x264enc tune=zerolatency bitrate=2000 speed-preset=superfast ! rtph264pay ! udpsink host=203.234.58.xxx port=8001
```
-host의 ip주소는 영상을 수신할 컴퓨터의 ip주소임
-port는 1024번 이상 아무거나 사용가능하고 client의 port번호와 일치해야함
-종료는 Ctrl+c를 누르면 됨

# Windows에서 영상수신하는 명령어
```bash
PS> gst-launch-1.0 -v udpsrc port=8001 ! ‘application/x-rtp,encoding-name=(string)H264,payload=(int)96’ ! rtph264depay ! queue ! avdec_h264 ! videoconvert! autovideosink
```
 -종료는 ctrl+c
 -port는 server의 port와 동일하게 설정해야함
 
# How to install gstreamer on wsl2-Ubuntu20.04

jetson보드에는 gsteramer가 설치되어 있음

$ gst-launch-1.0 --help -> 도움말 출력되면 설치확인

$ sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio

$ gst-launch-1.0 --help -> 도움말 출력되면 설치완료

# how to send camera image on Jetson nano

$ gst-launch-1.0 nvarguscamerasrc sensor-id=0 ! 'video/x-raw(memory:NVMM),format=NV12,width=640,height=360' ! nvvidconv flip-method=0 ! nvv4l2h264enc insert-sps-pps=true ! h264parse ! rtph264pay pt=96 ! udpsink host=192.168.0.19 port=8001 sync=false -q

# wsl2-Ubuntu20.04에서 영상수신하는 명령어

$ gst-launch-1.0 -v udpsrc port=8001 ! application/x-rtp,encoding-name=H264,payload=96 ! rtph2
64depay ! queue ! avdec_h264 ! videoconvert! autovideosink

# Windows에서 gstreamer 설치하는 방법

https://gstreamer.freedesktop.org/download

MSVC 64비트 runtime installer만 설치하면 됨, 시스템 환경변수 path에 실행파일 경로 C:\gstreamer\1.0\msvc_x86_64\bin 추가

PS> gst-launch-1.0 --help -> 도움말 출력되면 설치확인

# Windows에서 영상수신하는 명령어

PS> gst-launch-1.0 -v udpsrc port=8001 ! ‘application/x-rtp,encoding-name=(string)H264,payload=(int)96’ ! rtph264depay ! queue ! avdec_h264 ! videoconvert! autovideosink -> 종료는 ctrl+c
