# AI_RC
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

#### ros melodic install
```
cd ~/Downloads/
sudo apt update

git clone https://github.com/zeta0707/installROS.git
cd installROS
./install-ros.sh
```
