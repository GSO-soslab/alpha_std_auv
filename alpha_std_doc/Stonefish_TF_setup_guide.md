# ALPHA Standard AUV

## Introduction
This document describes how each sensor/actuator on the ALPHA is configured on the actual vehicle and in the stonefish simulator. Herein, we use the standard ALPHA vehicle (`alpha_std_auv`) as an example. The standard vehicle has an IMU, DVL, and four thrusters (two vertical thrusters, one horizont thruster, and one main thruster in the back).
Without following the instruction, you may encounter weird performanace when simulating the ALPHA, especailly when using the MVP control, MVP mission and robot localization with GPS integrated.


## Common Frames.
### ***World related frames***
- **NED**: North-East-Down is widely used for marine vehicles. `Stonefish` simulator uses the **NED** frame. As a result, the odometry sensor in the Stonefish are generated based on the **NED** frame. 
- **world_ned**: we can define this as a fixed world frame based on the NED convention. The origin of world_ned can have a offset from **NED** in the x-y plane
- **ENU**: East-North-Up frame is the convention in ROS. For example, robot localization package accepts data in ENU and outputs odometry data in ENU frame. **Caution**: When an AHRS produces data in ENU frame, 1) the heading will be relative to the east and 2) it will increase when rotating it counter-clockwise when looking from above. We need to make sure both conditions are meet before importing the data into any localization packages in ROS.
- **world**: we can define this as a fixed world frame based on the ENU convention.

### ***Robot related frames***. We suggest to have namespaces for each robot related frames.
- **odom**: is the frame the robot's odometry based on. It is normally defined when a robot is spawn. In ROS odom should follow the ENU convention. Thus, it can be aligned with the `world_enu` or have a translation from the `world_enu`.
- **base_link**: is a link attacehd to the vehicle. The odometry message indicates the translation and rotation between the `baselink` and the `odom` frame.
-  **cg_link**: is a link we defined located at the center of gravity of the vehicle. This link follows the convention SNAME convention. Therefore, x is pointed forward, y is pointed starboard, and z is pointed downward. The `cg_link` and `base_link` are 180 deg apart in roll.
- **imu**: is defined based on the imu orientation and position on the vehicle. It is axis should be aligned with the x-y-z arrows displayed on the sensor. **Caution** if the IMU you are using is providing orientation estimate (known as AHRS), you need to convert the orientation measurements in to ENU since ROS, by default, thinks the orientation data in an IMU message is in the ENU frame. See ***Sensor setup in the Stonefish*** section to find out how we change the orientations.
- **dvl**: is defined based on the location of the dvl. In an actual DVL, the sensor will indicate the x-y-z axis. Normally, the z-axis is pointed towards the seabed, x-axis is pointed forward, and y-axis is pointed to the starboard. **Caution**: in the `Stonefish`, we found the DVL has to point upward in order to get a valid measurement on the seafloor. ***Sensor setup in the Stonefish*** section to find out how we deal with that. 
- **gps**: is defined based on the gps location and oriention on the vehicle. 
- **pressure_sensor**: it has the same orientation as the `cg_link` but offseted based on the location of the pressure sensor. When the vehicle is going down, we expect a positive pressure/depth reading.

### ***Sensor setup in the Stonefish*** ###

