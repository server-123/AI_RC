# AI_RC
## 하드웨어

## 소프트웨어
### Jetpack & ROS install
'''
cd Downloads
git clone https://github.com/jetsonworld/jetson-fan-ctl.git
cd jetson-fan-ctl

sudo sh install.sh
'''

cd Downloads: 현재 디렉토리를 사용자의 'Downloads' 폴더로 변경합니다. 여기서 'Downloads'는 사용자의 다운로드 폴더를 의미합니다.  
git clone https://github.com/jetsonworld/jetson-fan-ctl.git: git라는 버전 관리 도구를 사용하여 https://github.com/jetsonworld/jetson-fan-ctl.git 주소에 있는 저장소를 복제합니다. 즉, GitHub에서 'jetson-fan-ctl' 라는 프로젝트를 현재 디렉토리(Downloads)에 다운로드합니다.  
cd jetson-fan-ctl: 현재 디렉토리를 방금 복제한 'jetson-fan-ctl' 폴더로 변경합니다.  
sudo sh install.sh: sudo는 "superuser do"의 줄임말로, 관리자 권한으로 명령어를 실행하라는 의미입니다.  
sh install.sh는 'install.sh'라는 쉘 스크립트 파일을 sh(본 쉘)을 사용해 실행하라는 명령어입니다.  
이 명령어를 통해 'jetson-fan-ctl' 프로젝트에 포함된 'install.sh' 스크립트를 관리자 권한으로 실행합니다. 이 스크립트는 프로젝트를 설치하거나 설정하는 데 필요한 여러 명령어들을 포함하고 있을 수 있습니다.
