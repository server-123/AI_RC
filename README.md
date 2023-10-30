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

#### I2C 장치 연결 확인
~/Downloads/jetson-fan-ctl에서
```
sudo i2cdetect -r -y 1
```
i2cdetect: I²C 버스에서 연결된 장치들을 검색하고 탐지하는 도구임. I²C는 연속된 데이터 전송을 위한 직렬 통신 버스임.  
-r: 읽기 모드로 I²C 버스를 스캔하라는 옵션  
-y: 사용자에게 프롬프트 없이 진행하라는 옵션, 추가적 스캔 X  
1: I²C 버스의 번호. 대부분의 리눅스 기반 시스템에서 여러 I²C 버스들이 있을 수 있으며, 이것은 첫 번째 I²C 버스를 의미합니다.  
  
실행 결과
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- 3c -- -- -- 
40: 40 -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: 70 -- -- -- -- -- -- --
```
결과는 8x16의 그리드 형태로 표시됨.  
그리드의 각 셀은 I²C 주소를 나타냄.  
주소에 연결된 장치가 있을 경우 그 주소값이, 없을 경우 --로 표시  
  
3c 주소와 40 주소, 그리고 70 주소에 장치가 연결되어 있음

#### ros melodic install
```
cd ~/Downloads/
sudo apt update

git clone https://github.com/zeta0707/installROS.git
cd installROS
./install-ros.sh
```
