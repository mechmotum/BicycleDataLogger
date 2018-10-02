# Description

This is the repository for the 2018 UC Davis mechanical engineering senior design project to build a Bicyle Data Logger utilizing ROS on a 
Raspberry Pi. The repository contains all necessary files and the instructions for their use, as well as instructions for the configuration
of the Raspberry Pi. Once the Pi is set up and the proper hardware connections are made, the Pi will be activated and shut
down by a button that also launches/terminates the ROS nodes that collect and process data from an IMU, GPS, potentiometer, and 
calibration button.

# Software
Once the Raspberry Pi is running Raspian Jessie, follow the instructions in the link below to install ROS Kinetic from source on the 
Raspberry Pi.

http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Kinetic%20on%20the%20Raspberry%20Pi

Add the following two lines to the .bashrc file:

```bash
source /opt/ros/kinetic/setup.bash
```
```bash
source /home/pi/ros_catkin_ws/devel/setup.bash
```


Once ROS is installed, the following ROS packages must be downloaded:

https://github.com/ros-drivers/nmea_navsat_driver/tree/c457319ecbb4ccd97c559a801d552fdc486b927c (GPS)

https://github.com/RTIMULib/RTIMULib2/tree/3d62821fef0f2252c39c14321a68d8cf3a63b9ae (IMU)

https://github.com/romainreignier/rtimulib_ros/tree/325a3893fa65abd99bd4bbc6e604a18470854ad2 (IMU)


To download and initialize a ROS package, navigate into the /home/pi/ros_catkin_ws/src/ folder and clone the github repository:

```bash
cd ~/ros_catkin_ws/src
git clone https://github.com/RTIMULib/RTIMULib2/tree/3d62821fef0f2252c39c14321a68d8cf3a63b9ae
git clone https://github.com/RTIMULib/RTIMULib2/tree/3d62821fef0f2252c39c14321a68d8cf3a63b9ae
git clone https://github.com/romainreignier/rtimulib_ros/tree/325a3893fa65abd99bd4bbc6e604a18470854ad2 
```
The packages for the steering angle and calibration button must be made manually. To do this, run the following commands to create a package called "potread":
```bash
cd ~/ros_catkin_ws/src
catkin_create_pkg potread std_msgs rospy
```
and
```bash
catkin_create_pkg pushbutton std_msgs rospy
```
to make the calibration button package. Once all the packages are downloaded or manually created, execute the following:
```bash
cd ~/ros_catkin_ws
catkin_make
```
Run the following commands to install software for use with the Adafruit ADS1015 analog-to-digital converter, which reads
the potentiometer:
```bash
sudo apt-get install git build-essential python-dev
cd ~
git clone https://github.com/adafruit/Adafruit_Python_ADS1x15.git
cd Adafruit_Python_ADS1x15
sudo python setup.py install
```

# Raspberry Pi Configuration
Enable i2c on the Pi by going to Preferences -> Raspberry Pi Configuration -> Interfaces

In the /etc/modules file, make sure the lines i2c-dev and i2c-bcm2708 are uncommented

To configure the GPIO serial port used by the GPS, add the line "enable_uart=1" to the bottom of the /boot/config.txt file, then run the
following:
```bash
sudo systemctl stop serial-getty@ttyAMA0.service
sudo systemctl disable serial-getty@ttyAMA0.service
```
Next, remove the line console=serial0,115200 from the /boot/cmdline.txt file.
