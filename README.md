# ROS serial
Serial communication between an arduino and a raspberry using ROS from scratch.

**Arduino codes come from ROS wiki.**

## Config
I'm currently using a raspberry pi 3 B+, running on ubuntu mate 18.04 ([Ubuntu Mate 18.04 disk image](https://releases.ubuntu-mate.org/archived/18.04/arm64/)) and ROS distro is ros-melodic.

You should consider using ros-noetic as it is the new ros LTS (needs Ubuntu 20.04).

https://forum.arduino.cc/t/installing-arduino-sam-boards-on-arduino-ide-for-arm64/550398


## Installing ROS
### Installation

[Original tuto](https://roboticsbackend.com/install-ros-on-raspberry-pi-3/#Which_Operating_System_for_ROS_on_Raspberry_Pi)

First, we need to allow restricted, universe and multiverse repositories

```
$ sudo add-apt-repository universe
$ sudo add-apt-repository restricted
$ sudo add-apt-repository multiverse
```
then we need to execute the two below commands :
```
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
```
$ sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```
As an output of these commands, you should see in the terminal : 
```
Executing: /tmp/apt-key-gpghome.hyHvOq1ted/gpg.1.sh --keyserver hkp://keyserver.ubuntu.com:80 --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
gpg: key F42ED6FBAB17C654: public key "Open Robotics <info@osrfoundation.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```
Now simply execute ```$ sudo apt update```

As we want to install ROS on a raspberry, we won't use GUIs tools. That is why we only need ros-base. To do so, type this command :
```
$ sudo apt install ros-melodic-ros-base
```
The installation can take some time as many packages will be dowloaded.

By doing it this way, we only have ros core. If you want to install any package, just type 
``` 
$ sudo apt install ros-melodic_PACKAGE_NAME
```
Now we need to install ros dependencies.
```
$ sudo rosdep init
$ rosdep update
```

### Work with ROS

We are now going to setup our environnement. Type :
```
$ echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
```
This will allow you to setup your environnement just once instead of doing ```$ source /opt/ros/melodic/setup.bash``` every time you want to run ros.
You can now launch the command ```roscore```

If your environnemnt isn't sourced you can get this kind of error message :
```
$ roscore

Command 'roscore' not found, but can be installed with:

sudo apt install python-roslaunch
```

The last step is to create yout working directory. This is mandatory as ROS will only work in your catkin_ws.
```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws
$ catkin_make
```
```catkin_make``` allows you to root of your workspace and build any package found in ```~/catkin_ws/src```
Just finish by sourcing devel directory :
```
$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
```

## Setting up Arduino IDE

[Arduino IDE setup](http://wiki.ros.org/rosserial_arduino/Tutorials/Arduino%20IDE%20Setup)

Start by dowloading rosserial packages

```
$ sudo apt-get install ros-melodic-rosserial-arduino
$ sudo apt-get install ros-melodic-rosserial
```
On the original link above, they talk about another way to install those packages, which I strongly advise not to do. WHen I tried to do it, the raspberry was just crashing. It must come froma lack a memory on the raspberry 3 B+, may be it can work on a raspberry pi 4. However, the method we used work just fine.

We have to make sure ros_lib is not present to avoid any error.
```
cd <sketchbook>/libraries
rm -rf ros_lib
rosrun rosserial_arduino make_libraries.py .
```
```<sketchbook>``` is the path to your directory where your sketches are. In my case it's directly ```~/sketchbook/``` but it can also be : ```~/Arduino/libraries```

You can now restart your IDE.

## Arduino publisher node

http://wiki.ros.org/rosserial_arduino/Tutorials/Hello%20World

## Arduino subscriber

http://wiki.ros.org/rosserial/Overview/Publishers%20and%20Subscribers














