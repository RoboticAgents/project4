# Project 4: ROS2 Mapping, Navigation & Physical TurtleBots

[![build](../../actions/workflows/build.yml/badge.svg)](../../actions/workflows/build.yml)

## Table of Contents

- [Overview](#overview)
- [Learning Outcomes](#learning-outcomes)
- [Individual Assignment](#individual-assignment)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
  - [Part 1: SLAM Mapping in Simulation](#part-1-slam-mapping-in-simulation)
  - [Part 2: Autonomous Navigation in Simulation](#part-2-autonomous-navigation-in-simulation)
  - [Part 3: ROS2 Waypoint Navigation Node](#part-3-ros2-waypoint-navigation-node)
  - [Part 4: Physical TurtleBot3 Mapping](#part-4-physical-turtlebot3-mapping)
- [Assessment](#assessment)
- [Resources](#resources)
- [Submission Instructions](#submission-instructions)
- [Academic Integrity](#academic-integrity)
- [Getting Help](#getting-help)

---

## Timeline

- **Assigned:** March 24, 2026
- **Part 1 checkpoint:** March 27, 2026 (push `writing/part1.md` with screenshots)
- **Part 2 checkpoint:** April 3, 2026 (push `writing/part2.md` with screenshots)
- **Final Submission:** [April 14, 2026 at 4:00 PM](https://calendar.google.com/calendar/render?action=TEMPLATE&text=CS304+P4:+ROS+Simulation+%26+TurtleBots+Due&dates=20260414T160000/20260414T170000&ctz=America/New_York&details=Project+4:+ROS+Simulation+and+TurtleBots)

---

## Overview

In this project you will progress from basic SLAM mapping and autonomous navigation in the course Docker simulation environment to writing your own ROS2 node, and finally to building a map with a physical TurtleBot3 robot.

The project has four parts that build on each other conceptually:

| Part | Focus | Environment | Key Concepts |
|------|-------|-------------|-------------|
| **1** | Build a map using SLAM | Docker / Gazebo | Occupancy grids, lidar, Cartographer SLAM |
| **2** | Navigate autonomously using the map | Docker / Gazebo | Nav2, costmaps, path planning, localization |
| **3** | Write a ROS2 waypoint navigation node | Docker / Gazebo | Python publishers, Nav2 action client, waypoints |
| **4** | Map a real environment with a physical TurtleBot3 | Physical robot | Sim-to-real gap, real lidar, hardware constraints |

**Parts 1–3** use the same Docker simulation environment from Activity 5 — you can start immediately.
**Part 4** uses physical TurtleBot3 Burger robots (4 robots shared among the class — sign up for time slots on Discord).

---

## Learning Outcomes

By completing this project, you will be able to:

1. **Use SLAM** to build an occupancy grid map of a simulated environment using lidar data  
   *Fulfills Course Learning Outcome 3*
2. **Configure and launch** the Nav2 navigation stack to autonomously move a robot to goal positions  
   *Fulfills Course Learning Outcome 3, 4*
3. **Write a ROS2 Python node** that programmatically sends navigation goals  
   *Fulfills Course Learning Outcome 2, 3*
4. **Transfer** simulation skills to a physical robot, identifying and addressing sim-to-real differences  
   *Fulfills Course Learning Outcome 2, 4*
5. **Explain** core mapping and navigation concepts: occupancy grids, SLAM, costmaps, localization, and path planning  
   *Fulfills Course Learning Outcome 4*
6. **Document** experiments, observations, and technical reasoning clearly  
   *Fulfills Course Learning Outcome 2*

---

## Individual Assignment

This is an **individual project**. All code, documentation, screenshots, and reflections must be your own work. You may discuss concepts with classmates, but all submissions must be independently produced.

For **Part 4** (physical TurtleBot3), you will work with the robot during your scheduled time slot. You may be at the same table as another student using a different robot, but your mapping task, screenshots, and writeup must be your own.

---

## Prerequisites

- **Activity 5 completed**: Docker Desktop installed, environment working, comfortable with basic ROS2 commands (`ros2 node list`, `ros2 topic list`, `ros2 topic echo`)
- **Docker Desktop running** on your laptop

### Launching the Environment

All simulation work uses the same Docker environment from Activity 5:

```bash
cd project4
docker compose up
```

Connect via browser at **http://localhost:6080**. Right-click the desktop → **Terminal** to open terminals.

---

## Project Structure

### Part 1: SLAM Mapping in Simulation

**Goal:** Build a complete occupancy grid map of the `turtlebot3_world` environment using SLAM.

**Concepts:** SLAM (Simultaneous Localization and Mapping) is the process of building a map of the environment while simultaneously tracking the robot's position within it. The TurtleBot3's lidar sensor produces laser scans that measure distances to obstacles. The Cartographer SLAM algorithm uses these scans, combined with odometry, to incrementally build an **occupancy grid map** — a grid where each cell stores the probability that it is occupied, free, or unknown.

**Steps:**

1. **Launch Gazebo** with the TurtleBot3 world:

   ```bash
   ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
   ```

2. **Launch Cartographer SLAM** in a second terminal:

   ```bash
   ros2 launch turtlebot3_cartographer cartographer.launch.py
   ```

   An RViz window will open showing the robot, laser scans, and the map being built in real time.

3. **Launch teleop** in a third terminal:

   ```bash
   ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -p stamped:=true
   ```

4. **Drive the robot** slowly and methodically through the entire environment. Tips:
   - Drive slowly — fast motion causes scan distortion
   - Cover all corridors and rooms
   - Revisit areas to improve map quality (loop closure)
   - Watch the RViz map update as you drive

5. **Save the map** once the environment is fully covered:

   ```bash
   ros2 run nav2_map_server map_saver_cli -f ~/map
   ```

   This creates two files: `map.pgm` (the image) and `map.yaml` (metadata).

6. **Copy the map files** to your project repository. From your **host machine** (not inside Docker):

   ```bash
   docker cp ros2-turtlebot3:/home/ubuntu/map.pgm maps/sim_map.pgm
   docker cp ros2-turtlebot3:/home/ubuntu/map.yaml maps/sim_map.yaml
   ```

**Deliverable:** Complete `writing/part1.md` with screenshots and answers.

---

### Part 2: Autonomous Navigation in Simulation

**Goal:** Use the Nav2 navigation stack with your saved map to autonomously navigate the robot to specified goal positions.

**Concepts:** Once a map exists, the robot can **localize** itself within it (using AMCL — Adaptive Monte Carlo Localization) and **plan paths** to goal positions. The Nav2 stack uses **costmaps** — layered grids that combine the static map with real-time sensor data to mark obstacles, inflation zones, and free space. A **global planner** finds the shortest path on the costmap, and a **local planner** adjusts the path in real time to avoid dynamic obstacles.

**Steps:**

1. **Launch Gazebo** (same world you mapped):

   ```bash
   ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
   ```

2. **Launch Nav2** with your saved map:

   ```bash
   ros2 launch turtlebot3_navigation2 navigation2.launch.py map:=$HOME/map.yaml
   ```

   RViz will open with the map loaded.

3. **Set the initial pose** using the "2D Pose Estimate" button in RViz:
   - Click the button in the RViz toolbar
   - Click and drag on the map where the robot is, pointing the arrow in the direction the robot is facing
   - The particle cloud (green arrows) should converge around the robot's true position

4. **Send a navigation goal** using the "Nav2 Goal" button in RViz:
   - Click the "Nav2 Goal" button
   - Click on the map where you want the robot to go, dragging to set the desired orientation
   - Watch the robot plan a path and navigate autonomously

5. **Experiment:**
   - Send the robot to at least **three different goal positions**
   - Observe the global and local costmaps in RViz
   - Try sending a goal that requires navigating around obstacles
   - Note the path the robot takes and whether it replans if blocked

**Deliverable:** Complete `writing/part2.md` with screenshots and answers.

---

### Part 3: ROS2 Waypoint Navigation Node

**Goal:** Write a Python ROS2 node that programmatically sends a sequence of navigation goals (waypoints) to the Nav2 stack.

**Concepts:** Instead of manually clicking goals in RViz, autonomous robots need to receive goals programmatically. The Nav2 stack exposes a `NavigateToPose` action that accepts goal poses. Your node will send a sequence of waypoints and monitor progress.

**Steps:**

1. **Create your workspace** inside the Docker environment:

   ```bash
   mkdir -p ~/ros2_ws/src
   cd ~/ros2_ws/src
   ```

2. **Create a Python package:**

   ```bash
   ros2 pkg create --build-type ament_python waypoint_nav
   ```

3. **Write the waypoint navigation node.** Create the file `~/ros2_ws/src/waypoint_nav/waypoint_nav/navigate_waypoints.py` with the following structure:

   ```python
   #!/usr/bin/env python3
   """Send a sequence of waypoints to Nav2."""

   import rclpy
   from rclpy.node import Node
   from rclpy.action import ActionClient
   from nav2_msgs.action import NavigateToPose
   from geometry_msgs.msg import PoseStamped
   import time


   class WaypointNavigator(Node):
       """Node that sends a list of waypoints to Nav2 sequentially."""

       def __init__(self):
           super().__init__('waypoint_navigator')
           self._action_client = ActionClient(
               self, NavigateToPose, 'navigate_to_pose'
           )

           # Define your waypoints here as (x, y, yaw) tuples.
           # Determine these coordinates from RViz by hovering over
           # positions on the map.
           # TODO: Replace these example coordinates with positions
           #       from YOUR map.
           self.waypoints = [
               (0.5, -1.0, 0.0),
               (2.0, -1.0, 1.57),
               (2.0, 0.5, 3.14),
           ]

           self.current_waypoint = 0

       def send_goal(self, x, y, yaw):
           """Send a single navigation goal."""
           goal_msg = NavigateToPose.Goal()
           goal_msg.pose = PoseStamped()
           goal_msg.pose.header.frame_id = 'map'
           goal_msg.pose.header.stamp = self.get_clock().now().to_msg()
           goal_msg.pose.pose.position.x = x
           goal_msg.pose.pose.position.y = y

           # Convert yaw to quaternion (rotation around z-axis only)
           import math
           goal_msg.pose.pose.orientation.z = math.sin(yaw / 2.0)
           goal_msg.pose.pose.orientation.w = math.cos(yaw / 2.0)

           self.get_logger().info(
               f'Sending goal: x={x:.2f}, y={y:.2f}, yaw={yaw:.2f}'
           )

           self._action_client.wait_for_server()
           future = self._action_client.send_goal_async(goal_msg)
           future.add_done_callback(self.goal_response_callback)

       def goal_response_callback(self, future):
           """Handle the goal acceptance/rejection."""
           goal_handle = future.result()
           if not goal_handle.accepted:
               self.get_logger().warn('Goal rejected')
               return

           self.get_logger().info('Goal accepted')
           result_future = goal_handle.get_result_async()
           result_future.add_done_callback(self.result_callback)

       def result_callback(self, future):
           """Handle result: send next waypoint or finish."""
           self.current_waypoint += 1
           if self.current_waypoint < len(self.waypoints):
               wp = self.waypoints[self.current_waypoint]
               self.send_goal(wp[0], wp[1], wp[2])
           else:
               self.get_logger().info('All waypoints reached!')
               rclpy.shutdown()

       def start_navigation(self):
           """Begin navigating through waypoints."""
           if self.waypoints:
               wp = self.waypoints[0]
               self.send_goal(wp[0], wp[1], wp[2])


   def main(args=None):
       rclpy.init(args=args)
       navigator = WaypointNavigator()
       navigator.start_navigation()
       rclpy.spin(navigator)


   if __name__ == '__main__':
       main()
   ```

4. **Register the node** in `setup.py` by adding to the `console_scripts` list:

   ```python
   'navigate_waypoints = waypoint_nav.navigate_waypoints:main',
   ```

5. **Build and run:**

   ```bash
   cd ~/ros2_ws
   colcon build --packages-select waypoint_nav
   source install/setup.bash
   ```

   First, launch Gazebo and Nav2 (as in Part 2), set the initial pose, then:

   ```bash
   ros2 run waypoint_nav navigate_waypoints
   ```

6. **Customize your waypoints:**
   - Open RViz and hover over positions on your map to find coordinates
   - Update `self.waypoints` with at least **four** waypoints that form a meaningful route (e.g., patrol a perimeter, visit multiple rooms)
   - The route should require navigating around at least one obstacle

7. **Copy your final node** to the project repository:

   ```bash
   # From your host machine:
   docker cp ros2-turtlebot3:/home/ubuntu/ros2_ws/src/waypoint_nav/waypoint_nav/navigate_waypoints.py src/navigate_waypoints.py
   ```

**Deliverable:** Push `src/navigate_waypoints.py` and complete `writing/part3.md`.

---

### Part 4: Physical TurtleBot3 Mapping

**Goal:** Use a physical TurtleBot3 Burger robot to build a map of a real environment using SLAM, then compare your real-world map to the simulation experience.

**Concepts:** Physical robots face challenges that don't exist in simulation: sensor noise, wheel slip, uneven surfaces, lighting variations, and limited battery life. This part asks you to experience the **sim-to-real gap** firsthand.

#### Hardware Overview

We have **4 TurtleBot3 Burger** robots available. Each robot includes:

- **Raspberry Pi 4** running ROS2 Jazzy (Ubuntu 24.04)
- **ROBOTIS OpenCR** motor controller board
- **LDS-02 (HLS-LFCD3)** 360° lidar sensor
- **Two Dynamixel XL430** wheel motors
- **Li-Po battery** (~2 hours runtime)

#### Scheduling

Since we have 4 robots for the class, you will **sign up for a time slot** on Discord. Each slot is approximately 1 hour. Plan to complete your mapping in one session.

Available times will be posted on Discord. First-come, first-served — sign up early.

#### Setup and Networking

The instructor will have the TurtleBots pre-configured and connected to a lab Wi-Fi network. You will connect your laptop to the same network.

1. **Connect your laptop** to the lab Wi-Fi network (name and password provided in lab).

2. **Verify connectivity** — open a terminal on your laptop and check that you can see the robot's topics:

   ```bash
   source /opt/ros/jazzy/setup.bash
   export ROS_DOMAIN_ID=<your_robot_id>
   ros2 topic list
   ```

   You should see topics including `/scan`, `/cmd_vel`, `/odom`, etc.

3. **Launch SLAM** on your laptop:

   ```bash
   ros2 launch turtlebot3_cartographer cartographer.launch.py
   ```

4. **Launch teleop** in another terminal on your laptop:

   ```bash
   ros2 run teleop_twist_keyboard teleop_twist_keyboard
   ```

   > **Note:** Physical TurtleBot3 Burger uses plain `Twist` messages (not `TwistStamped`), so do **not** add `--ros-args -p stamped:=true` here.

5. **Drive the robot** around the designated area. Tips for physical mapping:
   - Drive **very slowly** — the lidar needs time to scan
   - Avoid sudden turns
   - Map a defined area (e.g., a hallway, a section of a room)
   - Revisit your starting point to allow loop closure
   - Watch the map in RViz as you go

6. **Save the map:**

   ```bash
   ros2 run nav2_map_server map_saver_cli -f ~/real_map
   ```

7. **Copy the map** to your project repository:

   ```bash
   cp ~/real_map.pgm maps/real_map.pgm
   cp ~/real_map.yaml maps/real_map.yaml
   ```

**Deliverable:** Push map files to `maps/` and complete `writing/part4.md`.

---

## Assessment

**Total: 10 points** (part of 40 points for all projects)

### Part 1: SLAM Mapping in Simulation (2.0 points)
- [ ] Map saved to `maps/sim_map.pgm` and `maps/sim_map.yaml` (0.5 pts)
- [ ] Map is reasonably complete (walls, obstacles, corridors visible) (0.5 pts)
- [ ] `writing/part1.md` completed with screenshots and all questions answered (1.0 pts)

### Part 2: Autonomous Navigation in Simulation (2.5 points)
- [ ] Evidence of successful navigation to at least three goals (screenshots) (1.0 pts)
- [ ] `writing/part2.md` completed with costmap observations and questions answered (1.5 pts)

### Part 3: ROS2 Waypoint Navigation Node (3.0 points)
- [ ] `src/navigate_waypoints.py` exists and is functional (1.0 pts)
- [ ] Node includes at least four meaningful waypoints (0.5 pts)
- [ ] Route navigates around at least one obstacle (0.5 pts)
- [ ] `writing/part3.md` completed with code explanation, screenshots, and questions answered (1.0 pts)

### Part 4: Physical TurtleBot3 Mapping (2.5 points)
- [ ] Real map saved to `maps/real_map.pgm` and `maps/real_map.yaml` (0.5 pts)
- [ ] Map shows recognizable features of the mapped area (0.5 pts)
- [ ] `writing/part4.md` completed with comparison, observations, and all questions answered (1.5 pts)

### Checkpoint Deductions

Each checkpoint not met by its deadline may result in a 0.5 point deduction:
- **Part 1 checkpoint (March 27):** `writing/part1.md` pushed with at least one screenshot
- **Part 2 checkpoint (April 3):** `writing/part2.md` pushed with at least one screenshot

---

## Resources

### Textbook
- *Introduction to Autonomous Robots* (Correll et al.) — Chapter 13 (Mapping), Chapter 14 (Path Planning) for navigation

### ROS2 and Nav2
- [Nav2 Documentation](https://docs.nav2.org/)
- [Nav2 Getting Started](https://docs.nav2.org/getting_started/index.html)
- [TurtleBot3 SLAM Guide (ROS2 Jazzy)](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/)
- [TurtleBot3 Navigation Guide (ROS2 Jazzy)](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/)
- [ROS2 Jazzy Tutorials](https://docs.ros.org/en/jazzy/Tutorials.html)

### Physical TurtleBot3
- [TurtleBot3 e-Manual](https://emanual.robotis.com/docs/en/platform/turtlebot3/overview/)
- [TurtleBot3 Quick Start (Jazzy)](https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/)

### Course Materials
- [Activity 5: ROS2 Setup & First Simulation](../activity05/README.md) — Docker environment setup
- [ROS2 Intro Slides](/slides/ros2intro.html)
- Lecture slides on SLAM, mapping, and navigation (posted during Weeks 11–12)

---

## Submission Instructions

Submit via your GitHub repository:

| Deadline | What to push |
|----------|-------------|
| **March 27 (checkpoint)** | `writing/part1.md` with screenshots |
| **April 3 (checkpoint)** | `writing/part2.md` with screenshots |
| **April 14 at 4:00 PM** | Everything: all `writing/` files, `src/navigate_waypoints.py`, all `maps/` files |

### Repository Checklist

Before submitting, verify:

- [ ] `maps/sim_map.pgm` and `maps/sim_map.yaml` exist
- [ ] `maps/real_map.pgm` and `maps/real_map.yaml` exist
- [ ] `src/navigate_waypoints.py` exists and runs
- [ ] All four `writing/part*.md` files are complete
- [ ] `writing/reflection.md` is complete
- [ ] Regular Git commits with clear messages throughout the project

---

## Academic Integrity

This is an individual assignment. You may:
- Discuss ROS2 concepts and debugging strategies with classmates
- Reference official ROS2 and Nav2 documentation
- Use the starter code provided in Part 3 as a foundation

You may NOT:
- Copy another student's code, maps, screenshots, or written responses
- Use AI tools to generate your written reflections (your reflections should describe YOUR experience)
- Submit maps or screenshots that are not from your own work

---

## Getting Help

- **Office hours:** See [instructor's schedule](https://janyljumadinova.com/schedule)
- **Technical Leaders:** See [TL office hours](https://www.cs.allegheny.edu/teaching/technicalleaders/)
- **Discord:** Post questions in the course channel
- **In class:** March 30 and 31 are P4 work days — bring your laptop and questions
