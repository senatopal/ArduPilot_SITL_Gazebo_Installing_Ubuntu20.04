# ArduPilot__SITL_Installing_Ubuntu20.04
Ubuntu 20.04 (Focal Fossa) üzerine ArduPilot, ROS Noetic, openCV, Dronekit Kurulumu
Ubuntu 20.04 (Focal Fossa) üzerine ArduPilot, ROS Noetic, openCV, Dronekit Kurulumu
## Verilen her satır terminalde tek tek çalıştırılmalı

### ROS
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt install curl

curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add - 

sudo apt update 

sudo apt install ros-noetic-desktop-full 

source /opt/ros/noetic/setup.bash 

echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc 

source ~/.bashrc 

sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential 

sudo apt install python3-rosdep 

sudo rosdep init 

rosdep update

### ArduPilot ve openCV
sudo apt-get update

sudo apt-get upgrade

sudo apt-get install git

sudo apt-get install gitk git-gui

git clone https://github.com/ArduPilot/ardupilot.git

cd ardupilot

git submodule update --init --recursive

Tools/environment_install/install-prereqs-ubuntu.sh -y

cd

sudo apt install python-wxtools

sudo apt install python-lxml

sudo apt install python-pexpect

. ~/.profile

cd ardupilot/ArduCopter

sim_vehicle.py -w

### Restart Ubuntu System
cd

. ~/.profile

cd ardupilot/ArduCopter

sim_vehicle.py -w

cd ardupilot/ArduCopter

sim_vehicle.py --console --map

### Gazebo Kurulumu
#### Gerekli İzinlerin Ayarlanması

sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

wget https://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -


#### Gazebo Kurulumu

sudo apt update

sudo apt-get install gazebo9

sudo apt-get install libgazebo9-dev
#### Gazebo Ardupilot Eklentisi Kurulumu

git clone https://github.com/khancyr/ardupilot_gazebo

cd ardupilot_gazebo

mkdir build

cd build

cmake ..

make -j4

sudo make install

echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc

echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc

. ~/.bashrc

#### Gazebo Simülasyonunu ve Ardupilot SITL'i Başlatma

gazebo --verbose worlds/iris_arducopter_runway.world               *( BU KISMI ÇALIŞTIRDIKTAN SONRA TERMİNALDE + YA BASIP YENİ TERMİNAL YANDA BELİRİCEK ORAYADA DEVAMI YAZILICAK)

### Dronekit
pip install dronekit | pip3 install dronekit            * ikisinden biri çalıştırılmalı
