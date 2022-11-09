# Tutorial based on the work by teams <Trang Hoang, Evelyn Griffith> and 

1. Open the box for the robot and take out the dock. Place the dock on the floor in an open space and plug it into the nearest outlet. Take the turtlebot out of it's box and place it on the dock so that the two metal pieces on the bottom of the robot are connected to the two metal pieces on the charger (see images below). The robot should turn on with a flashing white light around the center power button. Once you hear two chimes (the second one will sound a little while after the first) you're good to go!

![unnamed](https://user-images.githubusercontent.com/89279012/199825967-8fa2aaef-5a79-40e3-a021-cfe3fe004e53.jpg)
![unnamed](https://user-images.githubusercontent.com/89279012/199826070-d8a7a069-d3e6-4f61-96ac-958881e8b604.jpg)

2. Now get the router out and plug it in so that the light on the front flashes blue. Find a linux laptop for the turtlebot and connect the laptop to the router (name and password are on the router).

Turtlebot should automatically connect to the router. If you think it hasn't done so,  you can press the two buttons on either side of the turtlebot's power button and hold them at the same time. The ring around the power button will change to blue and the robot will beep, which means that it is ready to connect to a network. You will connect to the turtlebot which will have a network name of "CXXX-XXXX"
    
    - Once the turtlebot is connected to your laptop, you will go to this URL `http://192.168.10.1/`.
    
    - From here, you will connect the turtlebot to the network indicated on the router.
    
    - The turtlebot will beep once it is connected.

3. Since we don't have a joystick, in order to get your robot running, you're going to want to use a keyboard application on your PC! `teleop` package should already be installed on turtlebot laptops, if not, run the following commands:

```
sudo apt update
sudo apt install ros-galactic-teleop-twist-keyboard
```

Before doing the next part, make sure you're connected to the right robot! If not, you can use this command to change it: 

`export ROS_DOMAIN_ID={ROS_DOMAIN_ID}` 

Example:
`export ROS_DOMAIN_ID=1`

Now you should be able to set up your `teleop` keyboard and drive the turtlebot as follows:

    - `source /opt/ros/galactic/setup.bash` 
    - `ros2 run teleop_twist_keyboard teleop_twist_keyboard`
        
When you do this, you'll see the following messages:
```
This node takes keypresses from the keyboard and publishes them
as Twist messages. It works best with a US keyboard layout.
---------------------------
Moving around:
   u    i    o
   j    k    l
   m    ,    .

For Holonomic mode (strafing), hold down the shift key:
---------------------------
   U    I    O
   J    K    L
   M    <    >

t : up (+z)
b : down (-z)

anything else : stop

q/z : increase/decrease max speeds by 10%
w/x : increase/decrease only linear speed by 10%
e/c : increase/decrease only angular speed by 10%

CTRL-C to quit

currently:	speed 0.5	turn 1.0 
```

As you can see above you use the following commands to move around:
```
U = Forward Left
I = Straight Forward
O = Forward Right
J = Rotate Left
K = Center # This doesn't really do anything
L = Rotate Right
M = Left Backwards
, = Straight Backwards
. = Right Backwards
```

4. Now that you have your teleop keyboard working, you should be able to run the slam, rviz, and teleop keyboard commands in separate terminals. **SLAM** (Simultaneous localization and mapping) is a method for creating a map of the area around the robot and keeping track of the robot position in that map. The TurtleBot 4 uses `slam_toolbox` by combing the data from Create3 and laser scans from `RPLIDAR`. We use the asynchronous SLAM when generating a map for the robot because it uses less processing power, but the map generated cannot be accurate as the synchronous SLAM method. **Rviz2** is a port of Rviz2 to ROS2. Rviz2 gives us a graphical interface to view the robot, the map and the sensor data while it is running to generate the map.  The needed packages should be installed on the turtlebot laptops. If not,  you can install them from the [turtlebot4 packages](https://turtlebot.github.io/turtlebot4-user-manual/software/turtlebot4_packages.html). You do not need the robot package, however, you do need the Desktop and Simulator packages which will allow you to run the rviz and slam packages.

    - To start, run this command in a new terminal: `source /opt/ros/galactic/setup.bash` then run in the same terminal `ros2 launch turtlebot4_navigation slam_sync.launch.py`
    
    - Next run this command in a NEW TERMINAL: `source /opt/ros/galactic/setup.bash` then run, in the same terminal, this command: `ros2 launch turtlebot4_viz view_robot.launch.py`
    
    - Finally, run a fresh `teleop` keyboard in a NEW TERMINAL. Note: re-run the teleop in a new terminal even if you already have one running, this is because it will sometimes not connect with the new programs you have running if you use something that was already running.

You should now be able to drive your turtlebot and have it generate a map on your computer.

In order to save the map, run this command: `ros2 service call /slam_toolbox/save_map slam_toolbox/srv/SaveMap "name: data: 'map_name'"`
