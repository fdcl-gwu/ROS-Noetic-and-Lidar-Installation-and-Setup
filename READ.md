# Ros Noetic Setup for Ubuntu 20.04 

## First, ensure you have the required driver packages installed along with ROS Noetic. The urg_node package is commonly used for Hokuyo LiDARs in our case the Hokuyo UST-10LX.

Enter the following code into a blank terminal for ROS Noetic Installation

`sudo apt-get install ros-noetic-urg-node`

Add ROS Key

`sudo apt install curl`
`curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`

Update Package Index 

`sudo apt update`

Install ROS Noetic Desktop Full, (Includes every feature of noetic including GUI Tools)

`sudo apt install ros-noetic-desktop-full`

Initialize rosdep

`sudo rosdep init`
`rosdep update`

Environment Setup (There could be a chance that it doesn't work but it won't hinder things you can instead just continue to source manually)

`echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc`
`source ~/.bashrc`

Install and Build ROS Dependencies

`sudo apt install python3-rosinstall python3-rosinstall-generator python3-wstool build-essential`



## Installing Required ROS Packages

The urg_node package is the ROS driver for Hokuyo Lidar sensors so it is required

`sudo apt install ros-noetic-urg-node`

Rviz installation 

`sudo apt install ros-noetic-rviz`



## Set up Catkin Workspace 

Create Workspace Directory

`mkdir -p ~/catkin_ws/src`

Build the Workspace (If using githubs make sure that all files can be built by catkin if not it won't build and you'll get an error)

`cd ~/catkin_ws`
`catkin_make`

Source the Workspace (This may not work so you can source the workspace manually as you'll see later on)

`echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc`
`source ~/.bashrc`



## Configure Network Settings (Very important) Make sure Lidar is now connected via Ethernet port and has power

Identify your ethernet Interface (Look for interfaces like eth0 and enp3s0 in our case it is eth0)

`ip a`

Assign a Static IP Address to Your computer (Not the default address of the Lidar but very similar)

`sudo ip addr add 192.168.0.1/24 dev eth0`
`sudo ip link set eth0 up`

Disable Network Manager if needs

`sudo nmcli dev set enp3s0 managed no`

Check the Lidar connectivity default address is 192.168.0.10

`ping 192.168.0.10`



## Install the Necessary ROS Packages

Ensure that the required driver packages are installed

`sudo apt-get update`
`sudo apt-get install ros-noetic-urg-node`

Starting the driver via ethernet port

`roslaunch urg_node urg_node.launch ip_address:=192.168.0.10 ip_port:=10940`



**If you get an error that means that you more than likely have multiple ip address for the same port so to fix this run the following command in a new terminal**

Removes all Ip address for that specific interface

`sudo ip addr flush dev eth0`

Adding new ip address

`sudo ip addr add 192.168.0.1/24 dev eth0`

Activates Network interface

`sudo ip link set eth0 up`



## Confirming Lidars IP Address

`sudo apt install nmap`
`sudo nmap -sn 192.168.0.0/24`



## Once done you can retry and run urg_node now that correct configuration is there

`rosrun urg_node urg_node _ip_address:=192.168.0.10`

## Once packages are downloaded you should be able to run it like listed below

# Terminal 1

`sudo ip addr flush dev eth0`
`sudo ip addr add 192.168.0.1/24 dev eth0`
`sudo ip link set eth0 up`
`sudo nmcli dev set eth0 managed no`
`ping 192.168.0.10`

# Terminal 2

`source /opt/ros/noetic/setup.bash`
`roscore`

# Terminal 3

`source /opt/ros/noetic/setup.bash`
`source -/catkin_ws/devel/setup.bash`
`rosrun urg_node urg_node _ip_address:=192.168.0.10`

