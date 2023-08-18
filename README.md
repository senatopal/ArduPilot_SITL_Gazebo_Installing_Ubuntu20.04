# Installing Ardupilot and MAVProxy Ubuntu 20.04

## Video Tutorial at https://youtu.be/1FpJvUVPxL0

## Clone ArduPilot

In home directory:
```
cd ~
sudo apt install git
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
```

## Install dependencies:
```
cd ardupilot
Tools/environment_install/install-prereqs-ubuntu.sh -y
```

reload profile
```
. ~/.profile
```
## If the next step "git submodule update" fails
```
git config --global url.https://.insteadOf git://
```

## Checkout Latest Copter Build
```
git checkout Copter-4.0.4
git submodule update --init --recursive
```

Run SITL (Software In The Loop) once to set params:
```
cd ~/ardupilot/ArduCopter
sim_vehicle.py -w
```
# Installing Gazebo and ArduPilot Plugin

### Video Tutorial at https://youtu.be/m7hPyJJmWmU

## Overview 

Robot simulation is an essential tool in every roboticist's toolbox. A well-designed simulator makes it possible to rapidly test algorithms, design robots, perform regression testing, and train AI system using realistic scenarios. Gazebo offers the ability to accurately and efficiently simulate populations of robots in complex indoor and outdoor environments. At your fingertips is a robust physics engine, high-quality graphics, and convenient programmatic and graphical interfaces. Best of all, Gazebo is free with a vibrant community.

for more infromation on gazebo checkout http://gazebosim.org/

## Install Gazebo [***18.04-20.04***]

Setup your computer to accept software from http://packages.osrfoundation.org:
```
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
```

Setup keys:
```
wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
```

Reload software list:
```
sudo apt update
```

Install Gazebo:
### Ubuntu [***18.04***]
```
sudo apt install gazebo9 libgazebo9-dev
```
### Ubuntu [***20.04***]
```
sudo apt-get install gazebo11 libgazebo11-dev
```

for more detailed instructions for installing gazebo checkout http://gazebosim.org/tutorials?tut=install_ubuntu


## Install Gazebo plugin for APM (ArduPilot Master) :
```
cd ~
git clone https://github.com/khancyr/ardupilot_gazebo.git
cd ardupilot_gazebo
```
***Ubuntu 18.04 only*** checkout dev
```
git checkout dev
```
build and install plugin
```
mkdir build
cd build
cmake ..
make -j4
sudo make install
```
```
echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc
```
Set paths for models:
```
echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
. ~/.bashrc
```

## Run Simulator

**NOTE the iris_arducopter_runway is not currently working in gazebo11. The iq_sim worlds DO work**

In one Terminal (Terminal 1), run Gazebo:
```
gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world
```

In another Terminal (Terminal 2), run SITL:
```
cd ~/ardupilot/ArduCopter/
sim_vehicle.py -v ArduCopter -f gazebo-iris --console
```
# Install ROS and Setup Catkin  

## Video Tutorial at https://youtu.be/1FpJvUVPxL0

In this tutorial we are using **Ubuntu 20.04** and **ROS Noetic**

Code blocks are meant to be typed in Terminal windows. "Control+Alt+T" opens a new Terminal window.

## 1. Install ROS

   - Do _Desktop-full Install_
   - Follow until _Step 1.7_ at the end of the page

   First, install **ROS Noetic** using the following instructions: http://wiki.ros.org/noetic/Installation/Ubuntu


## 2. Set Up Catkin workspace

We use `catkin build` instead of `catkin_make`. Please install the following:
```
sudo apt-get install python3-wstool python3-rosinstall-generator python3-catkin-lint python3-pip python3-catkin-tools
pip3 install osrf-pycommon
```

Then, initialize the catkin workspace:
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin init
```

## 3. Dependencies installation

Install `mavros` and `mavlink` from source:
```
cd ~/catkin_ws
wstool init ~/catkin_ws/src

rosinstall_generator --upstream mavros | tee /tmp/mavros.rosinstall
rosinstall_generator mavlink | tee -a /tmp/mavros.rosinstall
wstool merge -t src /tmp/mavros.rosinstall
wstool update -t src
rosdep install --from-paths src --ignore-src --rosdistro `echo $ROS_DISTRO` -y

catkin build
```
Add a line to end of `~/.bashrc` by running the following command:
```
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
```

update global variables
```
source ~/.bashrc
```

install geographiclib dependancy 
```
sudo ~/catkin_ws/src/mavros/mavros/scripts/install_geographiclib_datasets.sh
```


## 4. Clone IQ Simulation ROS package 

```
cd ~/catkin_ws/src
git clone https://github.com/Intelligent-Quads/iq_sim.git
```
Our repository should now be copied to `~/catkin_ws/src/iq_sim/` (don't run this line. This is just saying that if you browse in the file manager, you will see those folders).

run the following to tell gazebo where to look for the iq models 
```
echo "GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:$HOME/catkin_ws/src/iq_sim/models" >> ~/.bashrc
```

## 5. Build instructions
Inside `catkin_ws`, run `catkin build`:

```
cd ~/catkin_ws
catkin build
```
update global variables
```
source ~/.bashrc
```
