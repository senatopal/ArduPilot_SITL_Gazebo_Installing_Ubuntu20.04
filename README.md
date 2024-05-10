# Installing Ardupilot and MAVProxy Ubuntu 20.04



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
 
