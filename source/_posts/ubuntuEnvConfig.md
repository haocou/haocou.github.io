---
title: Ubuntu Configuration
date: 2020-12-18 14:59:25
categories: 
    - ubuntu
tags:
    - system
comments: false
mathjax: true
---

This tutorial is for Ubuntu Environment config.

<!-- more -->

# ENV configuration

Description: Ubuntu16.04, Java 11.0.2 (not test in Ubuntu18, ubuntu18 can apt install openjdk-11 and ubuntu 16 default version is openJdk8)

- Download the source JDK 11.0.2 and install it using the following command

  ```
  tar -zxvf jdk-*
  sudo mv jdk* /usr
  ```

  ```
  ##Using update-alternatives to install Java
  ##This is Oracle Java JDK 11
  sudo update-alternatives --install /usr/bin/java java /usr/jdk-11.*/bin/java 2
  
  ##Or Oracle Java JDK 8
  sudo update-alternatives --install /usr/bin/java java /usr/jdk1.8.*/bin/java 2
  ```
  
  ```
  ##Config default Java if the system exists 2 version
  sudo update-alternatives --config java
  ```
  
  ```
  ## Setting the ENV for Java
  sudo gedit /etc/profile.d/javajdk.sh
  
  ## Input the variables
  export PATH=$PATH:/usr/jdk-11.0.2/bin
  export JAVA_HOME=/usr/jdk-11.0.2
  export J2SDKDIR=/usr/jdk-11.0.2
  
  ##Then source
  source /etc/profile.d/javajdk.sh
  ```
  
- Install and Crack for Intell Idea IDE

   Download Intell IDE(https://www.jetbrains.com/idea/download/#section=linux).

  

```
tar -zxvf idea*
sudo mv idea* /usr
cd /usr/idea*/bin
./idea.sh
```

Using the Link to crack the license:

百度网盘链接: 
链接: https://pan.baidu.com/s/1kkPp9BQts-zPYAsgTDoQYA 密码: e7qw

——

备用链接:
链接: https://pan.baidu.com/s/12Du0aGGbJ5n-CRCdZKEl3Q 密码: hvcp

see reference

https://www.exception.site/article/29





# Config the ubuntu system

1. change the mirror: aliyun recommend.

   ```bash
   sudo  cp   /etc/apt/sources.list   /etc/apt/sources.list.bak
   sudo  gedit   /etc/apt/sources.list
   http://mirrors.aliyun.com/ubuntu/
   sudo apt update
   ```
   
2. Install ROS（http://wiki.ros.org/melodic/Installation/Ubuntu）

   ```
   sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
   sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
   sudo apt update
   sudo apt install ros-melodic-desktop
   echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
   sudo apt install python-rosdep
   sudo rosdep init
   rosdep update
   ```

   MayBe some error in rosdep init： add below line to /etc/hosts
   
   ```
   199.232.96.133 raw.githubusercontent.com
   ```
   
   
   
3. Gazebo install

   ```
   sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
   wget https://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
   sudo apt-get update
   sudo apt-get install gazebo9
   # For developers that work on top of Gazebo, one extra package
   sudo apt-get install libgazebo9-dev
   ```

   

4. Some software should be installed.

   git: sudo apt install git

5. Ide

   c++: Clion

   ```bash
   tar -zxvf Clion*
   sudo mv clion* /usr
   cd /usr/clion-2020.3/bin
   ./clion.sh
   ```
   
   Java: Intell idea
   
   see before