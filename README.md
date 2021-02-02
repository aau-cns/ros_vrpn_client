ros_vrpn_client
===============

Ros interface for http://www.cs.unc.edu/Research/vrpn/

Dependencies
-------------------
### ROS Packages

Git clone the packages  `vrpn_catkin` `glog_catkin` and `catkin_simple`
```bash
git clone https://github.com/ethz-asl/vrpn_catkin
git clone https://github.com/ethz-asl/glog_catkin.git
git clone https://github.com/catkin/catkin_simple.git
```

You do not have to install glog by itself, if you encounter issues in which glog is not found for the build of `ros_vrpn_client` refere to the known issues section.

### Other Libraries
On Ubuntu 20.04 / ROS Noetic it might be necessary to install `libtoolize` and `eigen-conversions`. For this run:

```bash
apt install libtool ros-$(rosversion -d)-eigen-conversions
```

Usage
-----------------
You have to start a ROS node per tracked object and the ROS node name has to be the name of the trackable object.

     rosrun ros_vrpn_client ros_vrpn_client _object_name:=object_name _vrpn_server_ip:=192.168.1.1

Or in a launch file:
```XML
<arg name="object_name" default="some_object_name" />
<node name="some_object_name_vrpn_client" type="ros_vrpn_client" pkg="ros_vrpn_client" output="screen">
 <param name="vrpn_server_ip" value="192.168.1.100" />
 <param name="object_name" value="$(arg object_name)" />
 <!-- or directly:
 <param name="object_name" value="some_object_name" /> -->
</node>
```
Installation HowTo
===============
Installation Ubuntu
-------------------
A catkinized version of VRPN can be found here: https://github.com/ethz-asl/vrpn_catkin

For further information about VRPN, please consult their website:
https://github.com/vrpn/vrpn

Installation OS X
-----------------
Use the catkinized package above.

TF coord frames
----------------

1. /optitrak
        - world frame that we will use.
        - X axis is along the x axis of the calibration pattern.
        - Z axis is vertically up.

2. Every tracked object has a coord frame whose TF name is the name of
   the ros node (given from the launch file or command line).

   Hitting "Reset To Current Orientation" in the TrackingTools
   software (Trackable properties) aligns the object coord frame with
   the /optitrak frame.

Coord frames vodoo
------------------
The TrackingTools software outputs the position and orientation in a
funky coord frame which has the Y axis pointing vertically up, the X
axis along the x axis of the calibration square and Z axis along the
-ve z axis of the calibration square.

We perform some rotations to get rid of this funky frame and use the
/optitrak frame described above as our fixed world coord frame. The
code is in the "`VRPN_CALLBACK track_target`" function in
`ros_vrpn_client.cpp`


## Known issues
Building `ros_vrpn_client` with `catkin_make` on Ubuntu 20.04 shows dependency issues. Using catkin build workds correctly. To solve this issue, ensure `glog_catkin` is built before `ros_vrpn_client` or switch to `catkin build`. In both cases `ros_vrpn_client` compiles on Ubuntu 20.04.
