# AI_RC
참조 : https://zeta7.notion.site/zeta7/JessiCar-1449b3fd5c984bab816920cb2b92002d
## Hardware
![image](https://github.com/server-123/AI_RC/assets/73692229/7e44071a-1e45-4650-add9-6c03deea2334)  
PCA9685의 A5를 납땜

![image](https://github.com/server-123/AI_RC/assets/73692229/b4586a73-3e92-4fa0-9580-5a68d06d85b8)

## 소프트웨어(Software)
### ROS란?
ROS(로봇 운영 체제, Robot Operating System)는 로봇 및 자율 주행 시스템을 개발하고 제어하기 위한 오픈 소스 로봇 소프트웨어 프레임워크임  
모듈화: ROS는 모듈화된 구조를 가지며, 로봇 시스템을 여러 개의 독립적인 노드로 분할함. 각 노드는 특정 작업을 담당하고 ROS 마스터를 통해 통신함  
메시지 기반 통신: ROS 노드들은 메시지를 통해 데이터를 교환하고 통신함  
풍부한 라이브러리: ROS는 다양한 로봇 애플리케이션에 필요한 라이브러리와 도구를 제공함. 이러한 라이브러리는 로봇 운동 제어, 컴퓨터 비전, 센서 처리, 시뮬레이션 및 많은 로봇 관련 작업에 유용함  
시뮬레이션 지원: ROS는 로봇 시뮬레이션을 지원하며, 로봇 시스템의 테스트와 디버깅을 단순화함  
다양한 운영체제 지원: ROS는 다양한 운영체제에서 실행할 수 있으며, 주로 Ubuntu Linux를 지원하지만 다른 리눅스 배포판과 macOS에서도 사용할 수 있음  
오픈 소스: ROS는 BSD 라이선스와 같이 오픈 소스 라이선스로 배포되어 개발자 및 로봇 공동체가 자유롭게 사용, 수정 및 공유할 수 있음  
커뮤니티와 지원: ROS는 활발한 커뮤니티와 다양한 지원 리소스를 보유하고 있습니다. 사용자, 개발자 및 연구원 간의 지식 공유와 협력이 촉진됨
#### Node
노드는 핵심 컴포넌트 중 하나로, 로봇 소프트웨어 시스템의 기본 구성 요소임  
ROS 노드들은 분산 시스템을 구축하고 로봇 소프트웨어의 다양한 부분을 실행하며 협력하도록 설계됨
#### Rostopic
토픽을 모니터하거나 메시지를 발행시킴
#### Subscribe
노드는 특정 토픽을 구독하면 해당 토픽에서 발생하는 메시지를 받아들일 수 있음
### Jetpack & ROS install
#### Jetpack install
https://drive.google.com/file/d/1HU5F1cwiw2wzuNBdLL9R3Wvpg5AXLzw5/view?usp=sharing  
이미지 다운로드  
sd format 후 이미지 플래시  

#### 와이파이 연결(Connect the Internet)
```
sudo nmcli device wifi list
sudo nmcli device wifi connect <ssid_name> password <password>
ifconfig
```

#### 쿨링팬(Cooling Fan)
```
cd Downloads
git clone https://github.com/jetsonworld/jetson-fan-ctl.git
cd jetson-fan-ctl

sudo sh install.sh
```

cd Downloads: 현재 디렉토리를 사용자의 'Downloads' 폴더로 변경함  
git clone https://github.com/jetsonworld/jetson-fan-ctl.git: 주소에 있는 저장소를 복제함. 즉, GitHub에서 'jetson-fan-ctl' 라는 프로젝트를 현재 디렉토리(Downloads)에 다운로드함  
cd jetson-fan-ctl: 현재 디렉토리를 'jetson-fan-ctl' 폴더로 변경함  
sudo sh install.sh: sudo는 "superuser do"의 줄임말로, 관리자 권한으로 명령어를 실행하라는 의미임  
sh install.sh는 'install.sh'라는 쉘 스크립트 파일을 sh(본 쉘)을 사용해 실행하라는 명령어임  

#### I2C 장치 연결 확인(Check the connection of the I2C)
~/Downloads/jetson-fan-ctl에서
```
sudo i2cdetect -r -y 1
```
i2cdetect: I²C 버스에서 연결된 장치들을 검색하고 탐지하는 도구임. I²C는 연속된 데이터 전송을 위한 직렬 통신 버스임  
-r: 읽기 모드로 I²C 버스를 스캔하라는 옵션  
-y: 사용자에게 프롬프트 없이 진행하라는 옵션, 추가적 스캔 X  
1: I²C 버스의 번호. 대부분의 리눅스 기반 시스템에서 여러 I²C 버스들이 있을 수 있으며, 이것은 첫 번째 I²C 버스를 의미함  
  
실행 결과
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: 40 -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: 60 -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: 70 -- -- -- -- -- -- --
```
결과는 8x16의 그리드 형태로 표시됨  
그리드의 각 셀은 I²C 주소를 나타냄.  
주소에 연결된 장치가 있을 경우 그 주소값이, 없을 경우 --로 표시  
  
40 주소와 60 주소, 그리고 70 주소에 장치가 연결되어 있음  
60은 A5를 납땜으로 연결함

#### ros melodic install
```
cd ~/Downloads/
sudo apt update

git clone https://github.com/zeta0707/installROS.git
cd installROS
./install-ros.sh
```

#### .bashrc 수정
jetson nano에서 실행
```
cd
gedit ~/.bashrc 
```
```
# 파일 제일 아래에 다음과 같은 내용 입력
alias cma='catkin_make -DCATKIN_WHITELIST_PACKAGES=""'
alias cop='catkin_make --only-pkg-with-deps'
alias sds='source devel/setup.bash'
alias coc='catkin clean'
alias cca='catkin clean -y'

# mirco USB 케이블 연결을 사용할 경우, jetson master
#export ROS_MASTER_URI=http://192.168.55.1:11311
#export ROS_IP=192.168.55.1

source /opt/ros/melodic/setup.bash
source ~/catkin_ws/devel/setup.bash
```
저장 후
```
source ~/.bashrc
```
변경사항 적용
PC, Jetson에 작업을 편리하게 할 환경을 .bashrc에서 추가함. alias 명령어 추가, PC ↔ Jetson remote 환경을 설정  
alias는 컴퓨터 프로그램과 명령어를 사용자 정의 단축키나 별칭으로 만들 수 있게 해주는 기능임

#### Check ROS
```
# 터미널 1
cd catkin_ws
roscore
# 터미널 2
cd catkin_ws
rosrun turtlesim turtlesim_node
# 터미널 3
cd catkin_ws
rosrun turtlesim turtle_teleop_key
```
![Screenshot from 2023-11-01 18-56-08](https://github.com/server-123/AI_RC/assets/73692229/afec4eb0-3285-4007-93bb-dfee60dff6fd)


### Install JessiCar
```
cd ~/catkin_ws/src
git clone https://github.com/zeta0707/jessicar.git
cd ~/catkin_ws

cd ~/Downloads/opencvDownTo34
sudo patch -p1 /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake -p1 < cv_brige.patch
cd ~/catkin_ws

cma
```  
자동차 설정
```
cd ~/catkin_ws/src/jessicar/script
./jetRccarParam.sh pca9685Steer
```  
카메라는 기본 설정이 CSI이므로 설정 안 해줘도 괜찮음

### 다른 ROS 패키지(Other ROS Package)
```
sudo apt update
sudo apt install ros-melodic-joy* \
ros-melodic-teleop-twist-joy ros-melodic-teleop-twist-keyboard \
python-smbus ros-melodic-ackermann-msgs ros-melodic-web-video-server \
ros-melodic-image-pipeline python-pip

pip2 install Adafruit_PCA9685
```

### Joystick 연결(Connect the joystick)
https://drive.google.com/file/d/18O1liBbSaMBcq8TjGDt5VhOOyb7V58aT/view?usp=drive_link
```
sudo apt-get install joystick

sudo jstest /dev/input/js0 # 동작 확인
```
```
# 터미널 1
roslaunch jessicar_control joy_control.launch

# 터미널 2
roslaunch jessicar_joy jessicar_teleop_joy.launch
```
조이콘의 L을 누르고 조이스틱 움직이기

### Keyboard 연결(Connect the keyboard)
```
# 터미널 1
roslaunch jessicar_control keyboard_control.launch

# 터미널 2
roslaunch jessicar_teleop jessicar_teleop_key.launch
```

-|-|-|-
---|:---:|:---:|:---:
-||W(앞)|
-|A(바퀴 왼쪽 회전)|S(멈춤)|D(바퀴 오른쪽 회전)
-||X(뒤)|


### Check the CSI camera
```
gst-launch-1.0 nvarguscamerasrc sensor_id=0 ! 'video/x-raw(memory:NVMM),width=3280, height=2464, framerate=21/1, format=NV12' ! nvvidconv flip-method=0 ! 'video/x-raw,width=960, height=720' ! nvvidconv ! nvegltransform ! nveglglessink -e
```
![Screenshot from 2023-11-06 18-28-55](https://github.com/server-123/AI_RC/assets/73692229/6c9fb70f-6a52-4a0f-8aa8-346118ba56ab)

```
sudo apt install ros-melodic-image-view
```

```
# 터미널 1
$ roslaunch jessicar_camera csicam.launch

# 터미널 2
rqt_image_view
```
![Screenshot from 2023-11-06 18-40-04](https://github.com/server-123/AI_RC/assets/73692229/62064e2b-a00a-428a-a4db-31f662b574b5)

### Blob Tracking
https://drive.google.com/file/d/18LiPZYCcCwclCFF2_7QIPBJEvicKyw4S/view?usp=drive_link
```
sudo apt install libcanberra-gtk-module libcanberra-gtk3-module
```

```
# 터미널 1
roslaunch jessicar_control blob_all.launch

# image_view 실행, 이미지 확인
rosrun image_view image_view image:=/blob/image_blob
```
기본 설정은 초록 물체를 따라감  
우클릭 시 사진 저장
```
cd ~/catkin_ws/src/jessicar/jessicar_cv/include/
python range_detector.py --image frame0000.jpg --filter HSV --preview
```
사진 이용해 필터값 찾기
