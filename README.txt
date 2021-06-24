

ROS Second Project 2021 Student: Davide Mornatta 10657647

FOLDERS STRUCTURE: (name of the package: "second_project")

    launch: contains the requested launch files for gmapping and localization, plus the launch files for amcl, imu_tools and localization configuration
    maps: contains the generated map and the relative .yaml
    rviz: a rviz configuration that is started with the launch files
    CMakeLists.txt, package.xml: usual needed files to specify dependencies, build instructions, executables and so on

TF TREE: it's the default bag tree, but in gmapping I added a static transform between "camera link" and "laser", while in localization I used a static transfrom between "base_link" and "laser". This is done to adjust the rotation between the base line and the laser of 90 degrees.

BAGS: I used the old version of the bags, prior to the adjustments. In order to create the map, I used bag1, then I tested localization using bag2 and bag3, obtaining satisfying results.

DESCRIPTION: As requested, I created a launch file to start gmapping. It can be launched using "roslaunch second_project gmapping.launch.xml". My configuration uses the odometry on the camera, to make use of the Intel camera that has been mounted on the SCOUT robot.
Furthermore, I provided the requested launch file to start localization. This can be started with "roslaunch second_project scout_localization.launch".
This launch file specifies the map location, starts imu_tools to refine imu data, starts robot localization, starts amcl and eventually it start rviz with my custom configuration.
For imu_tools I decided to use a complementary filter.
Amcl is executed based on the filtered odometry that is the result of robot localization, published on the topic "/odometry/filtered".

ADDITIONAL REPORT: reguarding robot localization, to obtain the filtered odometry I used an ekf_localization_node.
The chosen sources are the provided manifacturer odometry and the refined imu data obtained from mavros/imu/data.
The first has been chosen to vary between the two provided odometries, as in gmapping I already used the camera one. Reguarding the config matrix, I have set to true the x and y position, the z rotation, the x and y linear velocities and the z angular velocity. That is all we need from odometry.
The second source has been used to make advantage of the velocity and acceleration data provided from the imu. For this reason, I set to true z rotation, z angular velocity, x linear acceleration and y linear acceleration.

