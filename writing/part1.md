# Part 1: SLAM Mapping in Simulation

## Student Information

- **Name:**
- **GitHub Username:**
- **Date:**

## Environment Setup

Confirm that your Docker environment is running and you can access noVNC at http://localhost:6080.

- [ ] Docker container is running
- [ ] noVNC desktop is accessible
- [ ] Gazebo launches successfully

## Screenshots

### Screenshot 1: Gazebo World

> Insert a screenshot of the `turtlebot3_world` loaded in Gazebo.

![Gazebo World](screenshots/part1_gazebo.png)

### Screenshot 2: Cartographer RViz — Early Map

> Insert a screenshot from RViz showing the map **partway through** your mapping process (the map should be incomplete).

![Early Map](screenshots/part1_early_map.png)

### Screenshot 3: Cartographer RViz — Complete Map

> Insert a screenshot from RViz showing the **completed** map after driving through the entire environment.

![Complete Map](screenshots/part1_complete_map.png)

### Screenshot 4: Saved Map File

> Insert a screenshot showing the saved `map.pgm` image file (you can open it with an image viewer).

![Saved Map](screenshots/part1_saved_map.png)

## Questions

### Q1: Topics and Nodes

List the ROS2 topics that Cartographer subscribes to. You can find this using `ros2 node info` on the Cartographer node. What data does each topic provide?

**Answer:**

### Q2: Occupancy Grid Values

Open your saved `map.pgm` file. Occupancy grids use three value ranges: occupied, free, and unknown. What pixel values (colors) correspond to each? Why is tracking "unknown" space important for a mapping robot?

**Answer:**

### Q3: Mapping Strategy

Describe the strategy you used to drive the robot while mapping. Did you encounter any areas that were difficult to map? Why?

**Answer:**

### Q4: Loop Closure

Did you notice any change in map quality when you revisited an area you had already mapped? Describe what happened (or what you expected to happen) when the robot returned to a previously visited location.

**Answer:**

### Q5: Connection to Textbook

How does the occupancy grid map produced by Cartographer relate to the probabilistic grid mapping approach described in Chapter 13 of the textbook? In your own words, explain how the probability of occupancy is updated as new sensor readings arrive.

**Answer:**
