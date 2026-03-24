# Project 4 — Reflection

## Student Information

- **Name:**
- **GitHub Username:**
- **Date:**

---

## Part 1: SLAM Mapping in Simulation

### Screenshots

#### Gazebo World

> Insert a screenshot of the `turtlebot3_world` loaded in Gazebo.

![Gazebo World](screenshots/part1_gazebo.png)

#### Cartographer RViz — Early Map

> Insert a screenshot from RViz showing the map **partway through** your mapping process.

![Early Map](screenshots/part1_early_map.png)

#### Cartographer RViz — Complete Map

> Insert a screenshot from RViz showing the **completed** map.

![Complete Map](screenshots/part1_complete_map.png)

#### Saved Map File

> Insert a screenshot showing the saved `map.pgm` image file.

![Saved Map](screenshots/part1_saved_map.png)

### Questions

**Q1:** List the ROS2 topics that Cartographer subscribes to (use `ros2 node info`). What data does each topic provide?

**Answer:**

**Q2:** Describe the strategy you used to drive the robot while mapping. Did you encounter any difficult areas? Did you notice any map quality changes when revisiting areas (loop closure)?

**Answer:**

---

## Part 2: Autonomous Navigation in Simulation

### Screenshots

#### Initial Pose Estimate

> Insert a screenshot showing RViz after setting the initial pose (green particle cloud visible).

![Initial Pose](screenshots/part2_initial_pose.png)

#### Navigation Goals (at least 3)

> Insert screenshots showing the robot navigating to three different goal positions with planned paths visible.

![Goal 1](screenshots/part2_goal1.png)

![Goal 2](screenshots/part2_goal2.png)

![Goal 3](screenshots/part2_goal3.png)

#### Costmap Visualization

> Insert a screenshot showing both global and local costmaps in RViz.

![Costmaps](screenshots/part2_costmaps.png)

### Questions

**Q3:** Why is setting an initial pose estimate necessary before sending navigation goals? What happens if you skip it or set it incorrectly?

**Answer:**

**Q4:** Describe the costmaps you observe and the path the robot takes when navigating around obstacles. Does it follow the shortest path or maintain distance from walls? Why?

**Answer:**

**Q5:** Did the robot ever get stuck or trigger recovery behaviors? Describe what happened and how the Nav2 stack responded.

**Answer:**

---

## Part 3: ROS2 Waypoint Navigation Node

### Screenshots

#### Node Running — First Waypoint

> Insert a screenshot showing your node running in a terminal with the robot navigating toward the first waypoint.

![First Waypoint](screenshots/part3_waypoint1.png)

#### All Waypoints Completed

> Insert a screenshot showing the terminal output confirming all waypoints were reached, or the robot at its final position.

![Completed](screenshots/part3_completed.png)

### Waypoint Coordinates

| Waypoint | x | y | yaw | Location description |
|----------|---|---|-----|---------------------|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |

### Questions

**Q6:** Explain how your `navigate_waypoints.py` node works. Describe the action client, how goals are sent, and how the node chains waypoints together.

**Answer:**

**Q7:** How did you determine valid (x, y) coordinates for your waypoints? What happens if you set a waypoint inside an obstacle or outside the mapped area?

**Answer:**

---

## Part 4: Physical TurtleBot 4 Mapping

- **TurtleBot ID used:**
- **Date/time of mapping session:**

### Screenshots

#### Physical Robot

> Take a photo of the physical TurtleBot 4 you used.

![Physical Robot](screenshots/part4_robot.jpg)

#### Map — Physical Environment

> Insert a screenshot from RViz showing the completed map of the physical environment.

![Completed Map](screenshots/part4_complete_map.png)

#### Side-by-Side Comparison

> Insert images comparing your simulation map (Part 1) and your physical map (Part 4).

![Simulation Map](screenshots/part4_sim_comparison.png)
![Physical Map](screenshots/part4_real_comparison.png)

### Questions

**Q8:** How did driving and mapping with the physical robot differ from simulation? Compare map quality, sensor noise, and any setup challenges you faced.

**Answer:**

**Q9:** What practical factors did you have to consider with the physical robot that don't exist in simulation (battery, safety, surfaces, people, etc.)?

**Answer:**

---

## Overall Reflection

**Q10:** Reflecting on all four parts, discuss the value and limitations of simulation in robotics. When is simulation sufficient? When is physical testing essential?

**Answer:**

**Q11:** How did writing your own ROS2 node (Part 3) deepen your understanding of the ROS2 communication model (topics, actions, nodes)? Compare this to the Arduino code in Project 1.

**Answer:**

## Time Log

| Part | Estimated Hours |
|------|----------------|
| Part 1: SLAM in Simulation | |
| Part 2: Navigation in Simulation | |
| Part 3: Waypoint Navigation Node | |
| Part 4: Physical TurtleBot Mapping | |
| Writing and Reflection | |
| **Total** | |
