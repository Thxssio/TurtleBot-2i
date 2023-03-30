<h1 align="center"> 
     TurtleBot-2i
</h1>

>  installation turtlebot 2i


<p> 
    Make the workspace
</p>
 
    mkdir ~/catkin_ws
    cd catkin_ws
    mkdir -p src
    catkin_make
    cd src

<p>
    Clone the required repositories
</p>


     git clone https://github.com/turtlebot/turtlebot.git
     git clone https://github.com/turtlebot/turtlebot_msgs.git
     git clone https://github.com/turtlebot/turtlebot_apps.git
     git clone https://github.com/turtlebot/turtlebot_simulator



<p>
    Following
</p>

    https://github.com/yujinrobot/kobuki/issues/427

    git clone https://github.com/yujinrobot/yujin_ocs.git

<p>
    Remove all but 'yocs_cmd_vel_mux', 'yocs_controllers', and 'yocs_velocity_smoother'
</p>

    mv yujin_ocs/yocs_cmd_vel_mux yujin_ocs/yocs_controllers yujin_ocs/yocs_velocity_smoother .
    rm -rf yujin_ocs

<p>
    Add the battery monitor package
</p>

    git clone https://github.com/ros-drivers/linux_peripheral_interfaces.git
    mv linux_peripheral_interfaces/laptop_battery_monitor ./
    rm -rf linux_peripheral_interfaces

<p>
    You need to MANUALLY, for now, apply the changes proposed in

</p>
> https://github.com/ros-drivers/linux_peripheral_interfaces/pull/18
<p>
    Clone the MELODIC branch of the kobuki git
</p>

    git clone https://github.com/yujinrobot/kobuki.git
#
    sudo apt install liborocos-kdl-dev -y
    sudo apt install ros-noetic-joy 
    rosdep install --from-paths . --ignore-src -r -y

<p>
    Build the packages
</p>

    cd ..

    catkin_make
<p>
    If you have a LDS-01 laser mounted on the robot
</p>

    sudo apt install ros-noetic-hls-lfcd-lds-driver -y

<p>
    If you have a 3d sensor
</p>

    sudo apt install ros-melodic-openni2-launch -y
    
    sudo apt install ros-melodic-depthimage-to-laserscan -y
#
    echo source "$HOME/catkin_ws/devel/setup.bash" >> ~/.bashrc
    sudo adduser $USER dialout

<p>
    
Setting up the udev rule for creating a /dev/ link to the LIDAR if you have one
You need to define the file  /etc/udev/rules.d/60-hlds-laser.rules with the following content

   On precise, for some reason, USER and GROUP are getting ignored.
   So setting mode = 0666 for now.

</p>

    SUBSYSTEM=="tty", ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea60", MODE:="0666", GROUP:="dialout", SYMLINK+="LDS01"

<p>
    Followed by 
</p>

    sudo udevadm control --reload-rules
    sudo udevadm trigger

<p>
    You should then be able to access the device through   /dev/LDS01
</p>

