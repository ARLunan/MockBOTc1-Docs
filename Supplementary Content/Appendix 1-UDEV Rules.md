### Appendix 1: UDEV Rules to manage USB Connected Devices

Udev Rules may be necessary when 2 or more serial USB devices are plugged into the Raspberry Pi. A Udev Rule is a short script in a plain text file named, for example, **50-devicename.rules**, that is saved into **/etc/udev/rules.d/** .
 
From Clearpath ™ documentation
[](https://www.clearpathrobotics.com/assets/guides/kinetic/ros/Udev%20Rules.html#)
 
Udev is a Ubuntu device manager for Linux that dynamically creates and removes nodes for hardware devices. In short, it helps your computer find your robot easily. By default, hardware devices attached to your Linux (Ubuntu) PC will belong to the root user. This means that any programs (e.g. ROS nodes) running as an unprivileged (i.e. not root) user will not be able to access them. On top of that, devices will receive names such as ttyACMx and ttyUSBx arbitrarily based on the order in which they were plugged in. Luckily, you can solve this, and more, with udev rules that create a “symlink” associating a specific device name, such as “rplidar” to whatever actual ttyUSB name the system assigns.

#### UDEV Rules Guide
[UDEV Guide](https://www.clearpathrobotics.com/assets/guides/kinetic/ros/Udev%20Rules.html)

Specifically for the **MockBOTc1**, a **UDEV** rules, e.g. **50-create.rules /etc/udev/rules.d** and or **60-ros-humble-rplidar-ros.rules**,  will be necessary when using both a Lidar and Create Base connected to Raspberry Pi USB ports to associate specific device names with unpredctably assigned by Linux to ttyUSB ports.
 
#### RPLidar Sensor and Roomba or Create 1 Base Device
Here is the udev rules that is installed by the system for a Binary (Debian) install of the Slamtech RPLidar package that puts a udev rules in the **/etc/lib/udev/rules.d** directory that configures a symlink in the /dev folder as described above.  

#### set the udev rule , make the device_port be fixed by rplidar

KERNAL=="ttyUSB*", ATTRS{idVendor}=="0403" ATTRS{idProduct}=="6001", MODE:-"0666', SYMLINK+="create_1"  

KERNEL=="ttyUSB*", ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea60", MODE:="0666", SYMLINK+="rplidar"  

From pwd  
**\$ sudo cp 50\-rplidar.rules  /etc/udev/rules.d**
**\$ sudo cp 60\-rplidar.rules  /etc/udev/rules.d**

\$ sudo udevadm control --reload**

Here is the create1 base launch command line script.  
**\$ ros2 launch create\_bringup create1.launch decs:=false dev:=/dev/create1**

This script sets the USB device name to “create\_1’
Here is a list display of the MockBOTc1 serial ttyUSBx ports:  

Here is the launch command line for a rplidar\_a1 device:  
**ros2 launch rplidar\_ros rplidar\_a1\_launch.py serial/_port:='/dev/rplidar’**  

In the launch file text for either a Binary or Source Install, ensure the there is a parameter value  **serial\_port:=”/dev/rplidar”**  



ubuntu@rp5-ub24h-mb:~$ ls -l /dev/ |grep USB  
lrwxrwxrwx  1 root   root           7 Mar  5 14:02 create\_1 -> ttyUSB0  
lrwxrwxrwx  1 root   root           7 Mar  5 14:02 rplidar -> ttyUSB1  
crw-rw-rw-  1 root   dialout 188,   0 Mar  5 14:02 ttyUSB0  
crw-rw-rw-  1 root   dialout 188,   1 Mar  5 14:02 ttyUSB1






