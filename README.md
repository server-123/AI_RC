# AI_RC
참조 : https://zeta7.notion.site/zeta7/JessiCar-1449b3fd5c984bab816920cb2b92002d
## 하드웨어

## 소프트웨어
### Jetpack & ROS install
#### Jetpack install
https://drive.google.com/file/d/1HU5F1cwiw2wzuNBdLL9R3Wvpg5AXLzw5/view?usp=sharing  
이미지 다운로드  
sd format 후 이미지 플래시  

#### 와이파이 연결
```
sudo nmcli device wifi list
sudo nmcli device wifi connect <ssid_name> password <password>
ifconfig
```

#### 쿨링팬
```
cd Downloads
git clone https://github.com/jetsonworld/jetson-fan-ctl.git
cd jetson-fan-ctl

sudo sh install.sh
```

cd Downloads: 현재 디렉토리를 사용자의 'Downloads' 폴더로 변경함  
git clone https://github.com/jetsonworld/jetson-fan-ctl.git: 주소에 있는 저장소를 복제함. 즉, GitHub에서 'jetson-fan-ctl' 라는 프로젝트를 현재 디렉토리(Downloads)에 다운로드함.  
cd jetson-fan-ctl: 현재 디렉토리를 'jetson-fan-ctl' 폴더로 변경함.  
sudo sh install.sh: sudo는 "superuser do"의 줄임말로, 관리자 권한으로 명령어를 실행하라는 의미임.  
sh install.sh는 'install.sh'라는 쉘 스크립트 파일을 sh(본 쉘)을 사용해 실행하라는 명령어임.  

#### I2C 장치 연결 확인
~/Downloads/jetson-fan-ctl에서
```
sudo i2cdetect -r -y 1
```
i2cdetect: I²C 버스에서 연결된 장치들을 검색하고 탐지하는 도구임. I²C는 연속된 데이터 전송을 위한 직렬 통신 버스임.  
-r: 읽기 모드로 I²C 버스를 스캔하라는 옵션  
-y: 사용자에게 프롬프트 없이 진행하라는 옵션, 추가적 스캔 X  
1: I²C 버스의 번호. 대부분의 리눅스 기반 시스템에서 여러 I²C 버스들이 있을 수 있으며, 이것은 첫 번째 I²C 버스를 의미함.  
  
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
결과는 8x16의 그리드 형태로 표시됨.  
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
PC, Jetson에 작업을 편리하게 할 환경을 .bashrc에서 추가해줍니다. alias 명령어추가, PC ↔ Jetson remote 환경을 설정

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
![Screenshot from 2023-11-01 18-56-08](https://github.com/server-123/AI_RC/assets/73692229/e55b54d5-74cc-46f5-8ffd-7ef9c990484b)

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
카메라는 기본 설정이 CSI이므로 설정 안 해줘도 괜찮음.

### 다른 ROS 패키지
```
sudo apt update
sudo apt install ros-melodic-joy* \
ros-melodic-teleop-twist-joy ros-melodic-teleop-twist-keyboard \
python-smbus ros-melodic-ackermann-msgs ros-melodic-web-video-server \
ros-melodic-image-pipeline python-pip

pip2 install Adafruit_PCA9685
```

### Joystick 연결
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

### Keyboard 연결
```
# 터미널 1
roslaunch jessicar_control keyboard_control.launch

# 터미널 2
roslaunch jessicar_teleop jessicar_teleop_key.launch
```

-|-|-
---|---|---
---|W|---
A|S|D
---|X|---

