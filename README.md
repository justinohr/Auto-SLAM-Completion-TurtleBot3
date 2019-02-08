How to setup your TurtleBot3
============================
1. Preface
----------
* TurtleBot3 system has two different chips: SBC and OpenCR.
* This document is written for TurtleBot burger model with Raspberry Pi 3.
* If you are using other SBC, DO NOT follow the instructions given in number 2.
* You NEED TO follow the instructions in given order, otherwise it will lead to unintended consequences.

2. SBC Setup
------------
> 2.1. Installing Linux
---------------------
* You have two options: installing Ubuntu Mate 16.04 or Linux based on Raspbian.
* Installing Ubuntu Mate 16.04 is strongly recommended since ROS uses lots of Ubuntu based packages.
* You may download Ubuntu Mate 16.04 at the following link: https://ubuntu-mate.org/raspberry-pi/ubuntu-mate-16.04.2-desktop-armhf-raspberry-pi.img.xz
* Unzip the image file with the following instructions.
* $ sudo apt-get install gddrescue xz-utils
* $ unxz ubuntu-mate-16.04.2-desktop-armhf-raspberry-pi.img.xz
* $ sudo ddrescue -D --force ubuntu-mate-16.04.2-desktop-armhf-raspberry-pi.img /dev/sdx
* Write this image file to your own microSD.
* Notice that you MUST write the image file to microSD NOT a USB since Raspberry Pi 3 does not have on-chip storage.

> 2.2. Installing ROS on TurtleBot PC
-------------------------------------
* Following scripts will automatically install ROS Kinetic.
* $ sudo apt-get update
* $ sudo apt-get upgrade
* $ wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic_rp3.sh && chmod 755 ./install_ros_kinetic_rp3.sh && bash ./install_ros_kinetic_rp3.sh

> 2.3. Installing dependent packages
------------------------------------
* The following commands will install dependent ROS packages on your TurtleBot.
* $ sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation
* $ cd ~/catkin_ws/src
* $ git clone http://github.com/ROBOTIS-GIT/turtlebot3.git
* $ git clone http://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
* $ git clone http://github.com/ROBOTIS-GIT/hls_lfcd_lds_driver.git
* cd ~/catkine_ws && catkin_make

> 2.4. Configuring network
--------------------------
* VERY IMPORTANT: Raspberry Pi 3 CANNOT be connected to the wireless LAN with 5GHz frequency band. It can ONLY be connected to the WLAN with up to 2.4GHz frequency band.
* First of all, you need to figure both remote PC's and TurtleBot's IP address.
* You may check your IP address using ifconfig command on your terminal.
* Usually, your LAN IP is printed out next to "inet addr".
* You can change your network configuration by editing ".bashrc" file on your home directory using any editor. (The example here will use vim editor)
* $ vim ~/.bashrc
* Set ROS_HOSTNAME to the TurtleBot's IP address.
* e.g.) export ROS_HOSTNAME=192.168.1.13
* Set ROS_MASTER_URI to http://${Remote PC's IP address}:11311
* e.g.) export ROS_MASTER_URI=http://192.168.1.15:11311

3. OpenCR Setup
---------------
* This document is using shell script method to do OpenCR firmware upload.
* Connect either to remote PC or your TurtleBot PC then and execute following commands.
* export OPENCR_PORT=/dev/ttyACM0
* export OPENCR_MODEL=burger
* rm -rf ./opencr_update.tar.bz2
* $ wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS1/latest/opencr_update.tar.bz2 && tar -xvf opencr_update.tar.bz2 && cd ./opencr_update && ./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr && cd ..
* "jump_to_fw" string will be printed on terminal if firmware upload is successfully completed.