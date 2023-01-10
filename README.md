# ALPHA Standard AUV.


This repository includes configuration for standard ALPHA AUV.
- The standard ALPHA AUV uses AHRS/IMU, DVL, GPS to localize itself.
- ALPHA uses our `mvp_control` and `mvp_mission` for low-level pose control and vehicle guidance.
- This repository is built based on ou previous vehicle repositores, `alpha_sci_auv` and `alpha_auv`. The major change is that we have gone through the TF setup for the vehicle to make urdf and stonefish are consistent.
- The localization configuration we have here runs a single `robot_localization` stack. We used `navsat_transform` node to anchor `odom` to a specific gps coordination. Then, the `robot_localzation` fuses the IMU, DVL, GPS odometry(output from the `navsat_transform` node) for localzation. When the AUV is resurfaced, sudden position jump is visible, similar to a real mission.
- Currently, we only have simulation configured. Later, the physical system configuration will be included and the standard vehicle could be launch using another launch file.


- ROS version Noetic,
- Ubuntu 20.04


## Directory structure
`alpha_rise_auv`
Meta package for ALPHA RISE AUV.