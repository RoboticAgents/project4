# Project 4: ROS2 Mapping, Navigation & Physical TurtleBots

[![build](../../actions/workflows/build.yml/badge.svg)](../../actions/workflows/build.yml)

## Table of Contents

| | Section | Description |
|---|---------|-------------|
| | [Overview](#overview) | Project summary and structure |
| | [Timeline](#timeline) | Deadlines and checkpoints |
| | [Prerequisites](#prerequisites) | Docker setup, local install option |
| **Part 1** | [SLAM Mapping in Simulation](#part-1-slam-mapping-in-simulation) | Build a map using Cartographer SLAM |
| **Part 2** | [Autonomous Navigation in Simulation](#part-2-autonomous-navigation-in-simulation) | Navigate with Nav2 and costmaps |
| **Part 3** | [ROS2 Waypoint Navigation Node](#part-3-ros2-waypoint-navigation-node) | Write a Python node to send goals |
| **Part 4** | [Physical TurtleBot 4 Mapping](#part-4-physical-turtlebot-4-mapping) | Map a real environment with a physical robot |
| | [Assessment](#assessment) | Grading rubric (10 points) |
| | [Resources](#resources) | Documentation and reference links |
| | [Submission Instructions](#submission-instructions) | Deadlines and repository checklist |
| | [Academic Integrity](#academic-integrity) | Allowed and prohibited actions |

---

## Timeline

- **Assigned:** March 24, 2026
- **Part 1 checkpoint:** March 27, 2026 (push `writing/reflection.md` with Part 1 screenshots)
- **Part 2 checkpoint:** April 3, 2026 (push `writing/reflection.md` with Part 2 screenshots)
- **Final Submission:** [April 14, 2026 at 4:00 PM](https://calendar.google.com/calendar/render?action=TEMPLATE&text=CS304+P4:+ROS+Simulation+%26+TurtleBots+Due&dates=20260414T160000/20260414T170000&ctz=America/New_York&details=Project+4:+ROS+Simulation+and+TurtleBots)

---

## Overview

In this project you will progress from basic SLAM mapping and autonomous navigation in the course Docker simulation environment to writing your own ROS2 node, and to building a map with a physical TurtleBot 4 robot.

The project has four parts that build on each other conceptually:

| Part | Focus | Environment | Key Concepts |
|------|-------|-------------|-------------|
| **1** | Build a map using SLAM | Docker / Gazebo | Occupancy grids, lidar, Cartographer SLAM |
| **2** | Navigate autonomously using the map | Docker / Gazebo | Nav2, costmaps, path planning, localization |
| **3** | Write a ROS2 waypoint navigation node | Docker / Gazebo | Python publishers, Nav2 action client, waypoints |
| **4** | Map a real environment with a physical TurtleBot 4 | Physical robot | Sim-to-real gap, real lidar, hardware constraints |

**Parts 1–3** use the same Docker simulation environment from Activity 5 — you can start immediately.
**Part 4** uses physical TurtleBot 4 robots (2 TurtleBot 4 + 2 TurtleBot 4 Lite, shared among the class — sign up for time slots on Discord). **Part 4 does not need to be done last** — you can complete it whenever you have a robot time slot, even before finishing Parts 2 or 3.

---

## Learning Outcomes

By completing this project, you will be able to:

1. **Use SLAM** to build an occupancy grid map of a simulated environment using lidar data  
   *Fulfills Course Learning Outcome 3*
2. **Configure and launch** the Nav2 navigation stack to autonomously move a robot to goal positions  
   *Fulfills Course Learning Outcome 3, 4*
3. **Write a ROS2 Python node** that programmatically sends navigation goals  
   *Fulfills Course Learning Outcome 2, 3*
4. **Transfer** simulation skills to a physical TurtleBot 4, identifying and addressing sim-to-real differences  
   *Fulfills Course Learning Outcome 2, 4*
5. **Explain** core mapping and navigation concepts: occupancy grids, SLAM, costmaps, localization, and path planning  
   *Fulfills Course Learning Outcome 4*
6. **Document** experiments, observations, and technical reasoning clearly  
   *Fulfills Course Learning Outcome 2*

---

## Individual Assignment

This is an **individual project**. All code, documentation, screenshots, and reflections must be your own work. You may discuss concepts with classmates, but all submissions must be independently produced.

> **Exception — Part 4:** Part 4 (Physical TurtleBot 4 Mapping) may be completed in a group. Group members may submit the same map files. Each student must still complete the Part 4 section of their own `writing/reflection.md` individually and must list the names of everyone they worked with.

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

### Alternative: Local Installation (No Docker)

If you prefer to run ROS2 natively on Ubuntu 24.04 instead of using Docker, install the following:

1. **ROS2 Jazzy:**

   ```bash
   # Follow the official installation guide:
   # https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html
   sudo apt install ros-jazzy-desktop
   source /opt/ros/jazzy/setup.bash
   ```

2. **TurtleBot3 simulation packages:**

   ```bash
   sudo apt install ros-jazzy-turtlebot3-gazebo \
                    ros-jazzy-turtlebot3-cartographer \
                    ros-jazzy-turtlebot3-navigation2 \
                    ros-jazzy-turtlebot3-teleop
   ```

3. **Navigation and SLAM packages:**

   ```bash
   sudo apt install ros-jazzy-navigation2 \
                    ros-jazzy-nav2-bringup \
                    ros-jazzy-slam-toolbox \
                    ros-jazzy-cartographer-ros
   ```

4. **Teleop and DDS:**

   ```bash
   sudo apt install ros-jazzy-teleop-twist-keyboard \
                    ros-jazzy-rmw-cyclonedds-cpp
   ```

5. **Set environment variables** (add to your `~/.bashrc`):

   ```bash
   source /opt/ros/jazzy/setup.bash
   export TURTLEBOT3_MODEL=burger
   export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
   ```

**References:**
- [ROS2 Jazzy Installation (Ubuntu)](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html)
- [TurtleBot3 Quick Start (Jazzy)](https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/)
- [TurtleBot3 Simulation Setup](https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/)
- [Nav2 Installation](https://docs.nav2.org/getting_started/index.html)

> **Note:** Local installation requires Ubuntu 24.04. macOS and Windows users should use the Docker environment.

---

## Project Structure

### Part 1: SLAM Mapping in Simulation

**Goal:** Build a complete occupancy grid map of a simulated environment using SLAM.

**Concepts:** SLAM (Simultaneous Localization and Mapping) is the process of building a map of the environment while simultaneously tracking the robot's position within it. The TurtleBot3's lidar sensor produces laser scans that measure distances to obstacles. The Cartographer SLAM algorithm uses these scans, combined with odometry, to incrementally build an **occupancy grid map** — a grid where each cell stores the probability that it is occupied, free, or unknown.

**Steps:**

1. **Launch Gazebo** with a TurtleBot3 world. Choose one of the following:

   ```bash
   # House environment (recommended) — rooms, corridors, and furniture
   ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py

   # Alternatively: open world with cylindrical obstacles
   # ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
   ```

   > **Tip:** The house environment has more features to map and works more reliably in Docker.

   > **Expected warnings:** When Gazebo launches you will see errors about `Could not resolve file [Wood_Floor_Dark.jpg]` and `[Maple.jpg]`, QML binding loop warnings, and an anti-aliasing message. These are all cosmetic — missing textures render as a solid color and the other warnings are standard for the Docker environment. They do not affect the simulation, lidar, or SLAM.

2. **Launch Cartographer SLAM** in a second terminal:

   ```bash
   ros2 launch turtlebot3_cartographer cartographer.launch.py
   ```

   [Cartographer](https://google-cartographer-ros.readthedocs.io/) is Google's open-source SLAM library — it takes the lidar scans and odometry data and stitches them together into a consistent 2D occupancy grid map in real time, handling loop closures and scan matching automatically.

   An RViz window will open showing the robot, laser scans, and the map being built in real time.

   **Understanding the RViz display:**

   - **Left panel (Displays):** Lists everything RViz is visualizing. Each item has a status icon:
     - ✅ **Green checkmark** = working normally (you should see this on **Global Status**)
     - ⚠️ **Yellow !** = minor warning but still functional (you will likely see this on **TF** — this is normal and means some coordinate frames have slight timing delays; it does **not** affect SLAM or navigation)
     - ❌ **Red ✗** = error, that display is not receiving data
   - **Map (grey/white/black grid):** The occupancy grid being built by SLAM. **Black** cells = occupied (walls/obstacles), **white** cells = free space, **grey** cells = unknown/unexplored
   - **Red dots/lines (LaserScan):** Real-time lidar readings — these are the distance measurements the robot's laser sensor is currently seeing
   - **Robot model:** The small robot icon showing the robot's estimated position and orientation
   - **Green line (path):** When using Nav2, this shows the planned path to the goal
   - **Colored overlays (costmaps):** In Nav2 navigation, these show areas the robot should avoid (inflated obstacles) vs. safe areas to drive through
   - **Bottom bar:** Shows the coordinate position of your mouse cursor on the map — useful for finding waypoint coordinates in Part 3

3. **Launch teleop** in a third terminal:

   ```bash
   ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -p stamped:=true
   ```

   The keyboard controls use this layout:

   ```
   u : forward-left     i : forward       o : forward-right
   j : rotate left      k : STOP          l : rotate right
   m : backward-left    , : backward      . : backward-right
   ```

   Speed adjustments:
   - **q / z** — increase / decrease all speeds by 10%
   - **w / x** — increase / decrease linear speed only
   - **e / c** — increase / decrease angular speed only

   > **Important:** Click on the teleop terminal window to make sure it has focus before pressing keys.

4. **Inspect the running nodes and topics** in a fourth terminal. This is the node-topic graph from the ROS2 intro slides, running live:

   ```bash
   ros2 node list
   ros2 topic list
   ros2 topic info /scan
   ros2 topic info /cmd_vel
   ```

   > **Note:** You may see a message like `selected interface "lo" is not multicast-capable: disabling multicast`. This is normal inside Docker — all nodes are in the same container, so communication works fine without multicast.

   Notice which nodes are publishing and subscribing to each topic — for example, the Gazebo simulation publishes `/scan` (lidar data), and Cartographer subscribes to it to build the map. The teleop node publishes to `/cmd_vel`, which the simulation subscribes to for motor control.

5. **Drive the robot** slowly and methodically through the entire environment. Tips:
   - Drive slowly — fast motion causes scan distortion
   - Cover all corridors and rooms
   - Revisit areas to improve map quality (loop closure)
   - Watch the RViz map update as you drive

6. **Save the map** once the environment is fully covered:

   ```bash
   ros2 run nav2_map_server map_saver_cli -f ~/map
   ```

   This creates two files: `map.pgm` (the image) and `map.yaml` (metadata).

7. **Copy the map files** to your project repository. Inside the Docker terminal:

   ```bash
   cp ~/map.pgm ~/project_maps/sim_map.pgm
   cp ~/map.yaml ~/project_maps/sim_map.yaml
   ```

   The `project_maps` folder is mounted to the `maps/` directory in your project repository, so the files will appear there automatically.

**Deliverable:** Complete the Part 1 section of `writing/reflection.md` with screenshots and answers.

---

### Part 2: Autonomous Navigation in Simulation

**Goal:** Use the Nav2 navigation stack with your saved map to autonomously navigate the robot to specified goal positions.

**Concepts:** Once a map exists, the robot can **localize** itself within it (using AMCL — Adaptive Monte Carlo Localization) and **plan paths** to goal positions. The Nav2 stack uses **costmaps** — layered grids that combine the static map with real-time sensor data to mark obstacles, inflation zones, and free space. A **global planner** finds the shortest path on the costmap, and a **local planner** adjusts the path in real time to avoid dynamic obstacles.

**Steps:**

1. **Launch Gazebo** (same world you mapped in Part 1):

   ```bash
   ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
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

**Deliverable:** Complete the Part 2 section of `writing/reflection.md` with screenshots and answers.

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

3. **Write the waypoint navigation node.** Create the file `~/ros2_ws/src/waypoint_nav/waypoint_nav/navigate_waypoints.py`:

   > **Editing files in Docker:** Use `nano` to create and edit files inside the container (e.g., `nano ~/ros2_ws/src/waypoint_nav/waypoint_nav/navigate_waypoints.py`). Alternatively, you can write the file on your host machine in VS Code, save it to your project's `src/` folder, and then copy it into the ROS2 workspace: `cp ~/project_src/navigate_waypoints.py ~/ros2_ws/src/waypoint_nav/waypoint_nav/`

   Use the following structure:

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

7. **Copy your final node** to the project repository. Inside the Docker terminal:

   ```bash
   cp ~/ros2_ws/src/waypoint_nav/waypoint_nav/navigate_waypoints.py ~/project_src/navigate_waypoints.py
   ```

   The `project_src` folder is mounted to the `src/` directory in your project repository.

**Deliverable:** Push `src/navigate_waypoints.py` and complete the Part 3 section of `writing/reflection.md`.

---

### Part 4: Physical TurtleBot 4 Mapping

**Goal:** Use a physical TurtleBot 4 robot to build a map of a real environment using SLAM, then compare your real-world map to the simulation experience.

> **Note:** Part 4 can be completed on April 13 or April 14 in ALIC.

**Concepts:** Physical robots face challenges that don't exist in simulation: sensor noise, wheel slip, uneven surfaces, lighting variations, and limited battery life. This part asks you to experience the **sim-to-real gap** firsthand.

#### Hardware Overview

We have **4 robots** available: **2 TurtleBot 4** and **2 TurtleBot 4 Lite**. Both models use the same iRobot Create 3 base and run the same ROS2 software — the main difference is that the standard TurtleBot 4 includes an OAK-D Pro camera and a display, while the Lite does not. For this project (lidar-based SLAM), both models work identically.

Each robot includes:

- **iRobot Create 3** mobile base (differential drive, bump/cliff sensors, IMU)
- **Raspberry Pi 4** running ROS2 Jazzy (Ubuntu 24.04)
- **RPLIDAR A1** 360° 2D lidar sensor
- **Create 3 lithium-ion battery** (~2.5 hours runtime)
- **OAK-D Pro stereo camera** (TurtleBot 4 standard only — not used in this project)

#### Setup

Each TurtleBot 4 has a dedicated laptop with ROS2 Jazzy pre-installed. You will use this laptop (not your personal machine) to run SLAM and teleop.

1. **Log in** to the robot's laptop. The password is `alleghenycollege`.

2. **Verify connectivity** — open a terminal and check that you can see the robot's topics:

   ```bash
   ros2 topic list
   ```

   You should see topics including `/scan`, `/cmd_vel`, `/odom`, etc.

3. **Launch SLAM:**

   ```bash
   ros2 launch turtlebot4_navigation slam.launch.py
   ```
4. **Launch rviz** in another terminal:

   ```bash
   ros2 launch turtlebot4_viz view_navigation.launch.py
   ```

5. **Launch teleop** in another terminal:

   ```bash
   ros2 run teleop_twist_keyboard teleop_twist_keyboard
   ```

   or

   ```bash
   ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -p stamped:=true
   ```

6. **Drive the robot** around the designated area. Tips for physical mapping:
   - Drive **very slowly** — the lidar needs time to scan
   - Avoid sudden turns
   - Map a defined area (e.g., a hallway, a section of a room)
   - Revisit your starting point to allow loop closure
   - Watch the map in RViz as you go

7. **Save the map:**

   ```bash
   ros2 run nav2_map_server map_saver_cli -f ~/real_map
   ```

   or

   ```bash
   ros2 service call /slam_toolbox/save_map slam_toolbox/srv/SaveMap "name:data: 'map_name'"
  ```

8. **Copy the map files** to your project repository. Clone your repo on the robot's laptop (or use a USB drive), then:

   ```bash
   cp ~/real_map.pgm <your-repo>/maps/real_map.pgm
   cp ~/real_map.yaml <your-repo>/maps/real_map.yaml
   ```

   Commit and push from the robot's laptop, or transfer the files to your own machine.

**Group work:** Part 4 may be completed in a group. Group members may submit the same map files (`maps/real_map.pgm` and `maps/real_map.yaml`). Each student must complete the Part 4 section of their own `writing/reflection.md` individually.

**Deliverable:** Push map files to `maps/` and complete the Part 4 section of `writing/reflection.md`. **If you worked with others, list their names at the top of the Part 4 section of your reflection.**

---

## Assessment

**Total: 10 points** (part of 40 points for all projects)

### Part 1: SLAM Mapping in Simulation (2.0 points)
- [ ] Map saved to `maps/sim_map.pgm` and `maps/sim_map.yaml` (0.5 pts)
- [ ] Map is reasonably complete (walls, obstacles, corridors visible) (0.5 pts)
- [ ] Part 1 section of `writing/reflection.md` completed with screenshots and questions answered (1.0 pts)

### Part 2: Autonomous Navigation in Simulation (2.5 points)
- [ ] Evidence of successful navigation to at least three goals (screenshots) (1.0 pts)
- [ ] Part 2 section of `writing/reflection.md` completed with costmap observations and questions answered (1.5 pts)

### Part 3: ROS2 Waypoint Navigation Node (3.0 points)
- [ ] `src/navigate_waypoints.py` exists and is functional (1.0 pts)
- [ ] Node includes at least four meaningful waypoints (0.5 pts)
- [ ] Route navigates around at least one obstacle (0.5 pts)
- [ ] Part 3 section of `writing/reflection.md` completed with code explanation, screenshots, and questions answered (1.0 pts)

### Part 4: Physical TurtleBot 4 Mapping (2.5 points)
- [ ] Real map saved to `maps/real_map.pgm` and `maps/real_map.yaml` (0.5 pts)
- [ ] Map shows recognizable features of the mapped area (0.5 pts)
- [ ] Part 4 section of `writing/reflection.md` completed with comparison, observations, and questions answered (1.5 pts)
- [ ] If completed in a group, names of group members listed in the Part 4 section of `writing/reflection.md`

### Checkpoint Deductions

Each checkpoint not met by its deadline may result in a 0.5 point deduction:
- **Part 1 checkpoint (March 27):** Part 1 section of `writing/reflection.md` pushed with at least one screenshot
- **Part 2 checkpoint (April 3):** Part 2 section of `writing/reflection.md` pushed with at least one screenshot

---

## Resources

### Textbook
- *Introduction to Autonomous Robots* (Correll et al.) — Chapter 13 (Mapping), Chapter 14 (Path Planning) for navigation

### ROS2 and Nav2
- [Nav2 Documentation](https://docs.nav2.org/)
- [Nav2 Getting Started](https://docs.nav2.org/getting_started/index.html)
- [TurtleBot3 SLAM Guide (ROS2 Jazzy)](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/) — simulation reference
- [TurtleBot3 Navigation Guide (ROS2 Jazzy)](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/) — simulation reference
- [ROS2 Jazzy Tutorials](https://docs.ros.org/en/jazzy/Tutorials.html)

### Physical TurtleBot 4
- [TurtleBot 4 User Manual](https://turtlebot.github.io/turtlebot4-user-manual/)
- [TurtleBot 4 Setup](https://turtlebot.github.io/turtlebot4-user-manual/setup/)
- [TurtleBot 4 Navigation](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/navigation.html)

### Course Materials
- [Activity 5: ROS2 Setup & First Simulation](../activity05/README.md) — Docker environment setup
- [ROS2 Intro Slides](/slides/ros2intro.html)
- Lecture slides on SLAM, mapping, and navigation (posted during Weeks 11–12)

---

## Submission Instructions

Submit via your GitHub repository:

| Deadline | What to push |
|----------|-------------|
| **March 27 (checkpoint)** | `writing/reflection.md` with Part 1 screenshots |
| **April 3 (checkpoint)** | `writing/reflection.md` with Part 2 screenshots |
| **April 14 at 4:00 PM** | Everything: `writing/reflection.md`, `src/navigate_waypoints.py`, all `maps/` files |

### Repository Checklist

Before submitting, verify:

- [ ] `maps/sim_map.pgm` and `maps/sim_map.yaml` exist
- [ ] `maps/real_map.pgm` and `maps/real_map.yaml` exist
- [ ] `src/navigate_waypoints.py` exists and runs
- [ ] `writing/reflection.md` is complete (all four parts + overall reflection)
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

> **Part 4 exception:** Group members may share the same map files for Part 4. Written reflections must still be your own. You must identify your group members in your reflection.
