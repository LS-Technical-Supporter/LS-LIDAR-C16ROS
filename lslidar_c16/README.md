# lslidar_c16
#version v3.0.6_201202

## version track
Author: zx
### ver3.0 zx

## Description
The `lslidar_c16` package is a linux ROS driver for lslidar c16.
The package is tested on Ubuntu 16.04/18.04 with ROS kinetic/melodic.

## Compling
This is a Catkin package. Make sure the package is on `ROS_PACKAGE_PATH` after cloning the package to your workspace. And the normal procedure for compling a catkin package will work.

```
cd your_work_space
catkin_make 
```

## Example Usage

### lslidar_c16_decoder

**Parameters**

`lidar_ip` (`string`, `default: 192.168.1.200`)

By default, the IP address of the device is 192.168.1.200.

`frame_id` (`string`, `default: laser_link`)

The frame ID entry for the sent messages.

**Published Topics**

`lslidar_point_cloud`

Each message corresponds to a lslidar packet sent by the device through the Ethernet.

### lslidar_c16_decoder

**Parameters**

`min_range` (`double`, `0.3`)

`max_range` (`double`, `200.0`)

Points outside this range will be removed.

`frequency` (`frequency`, `10.0`)

Note that the driver does not change the frequency of the sensor. 

`publish_point_cloud` (`bool`, `true`)

If set to true, the decoder will additionally send out a local point cloud consisting of the points in each revolution.

**Published Topics**

`lslidar_sweep` (`lslidar_c16_msgs/LslidarChSweep`)

The message arranges the points within each sweep based on its scan index and azimuth.

`lslidar_point_cloud` (`sensor_msgs/PointCloud2`)

This is only published when the `publish_point_cloud` is set to `true` in the launch file.

**Node**

```
roslaunch lslidar_c16_decoder lslidar_c16.launch --screen
```
Note that this launch file launches both the driver and the decoder, which is the only launch file needed to be used.


## FAQ


## Bug Report


##Version changes
/***********2020-01-03****************/
Original version : lslidar_c16_v2.02_190919
Revised version  : lslidar_c16_v2.03_200103
Modify  		 : Add a new calibration decode for the new lslidar c16
Author			 : zx
Date			 : 2020-01-03


/***********2020-01-16****************/
Original version : lslidar_c16_v2.03_200103
Revised version  : lslidar_c16_v2.6.0_200116
Modify  		 : Adds the vertical Angle correction file lslidar_c16_db.yaml for the RoS program code
				   Change the range resolution to 0.25cm according to the v2.6 protocol
Author			 : zx
Date			 : 2020-01-16

/***********2020-04-02****************/
Original version : lslidar_c16_v2.6.0_200116
Revised version  : lslidar_c16_v2.6.1_200402
Modify  		 : 1. The function of reading device package and analyzing vertical angle value is added to replace the original fixed vertical angle value.
		           2. Modified lslidar_ C16. Launch file, which is used for compatible selection parameters and functions
Author			 : zx
Date			 : 2020-04-02

Luanch File Description:  

<node pkg="lslidar_c16_decoder" type="lslidar_c16_decoder_node" name="lslidar_c16_decoder_node" output="screen">
    <param name="min_range" value="0.15"/>
    <param name="max_range" value="150.0"/>
    <param name="cbMethod" value="true"/>		//cbMethod = true:It means to increase the offset calculation compensation of X and Y coordinates. If false, it will not be added
    <param name="print_vert" value="true"/>		//print_vert = true:Indicates the angle information of the printing device package, and false means to turn off the printing information

​    <param name="distance_unit" value="0.25"/>		//distance_unit = 0.25:Represents distance in 0.25cm, = 1 indicates distance in 1cm
​    <param name="time_synchronization" value="$(arg time_synchronization)"/>  // time_synchronization = true:Indicates that GPG is used for time service
  </node>

/***********2020-09-10************/
Original version : LSLIDAR_C16_V3.0.3_200826_ROSK
Revised version  : LSLIDAR_C16_V3.0.4_200910_ROSK
Modify  	:       

1. The new compatible vertical angle resolution is 1.33 ° radar.

2. Added Laserscan message type publishing.

3. Increase scan angle clipping.

4. Launch File Description:

    ​	            <param name="degree_mode" value="1"/>   <!--1 represents the vertical angle resolution of 1.33 ° and 2 represents the vertical angle resolution of 2 ° -->
     		    <param name="scan_start_angle" value="0.0"/>     <!-- 

    Scan crop start angle-->
                     <param name="scan_end_angle" value="36000.0"/>   <!-- Scan clipping end angle, unit: 0.01 degree-->
                    <param name="scan_num" value="8"/>      <!--

    Channels selected by Laserscan-->
                    <param name="publish_scan" value="false"/>   <!--Whether to publish Laserscan message type-->

Author			 : lqm
Date			 : 2020-09-10



/***********2020-12-02************/
Original version : LSLIDAR_C16_V3.0.4_200910_ROSK
Revised version  : LSLIDAR_C16_V3.0.6_201202_ROSK
Modify  	:       



1. Compatible with the ROS melody of ubuntu18.04.



2. NEW nodelet.launch Documents.



3. Remove lines / ring information and use standard point cloud data type

Author			 : lqm
Date			 : 2020-12-02