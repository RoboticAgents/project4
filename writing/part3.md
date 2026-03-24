# Part 3: ROS2 Waypoint Navigation Node

## Student Information

- **Name:**
- **GitHub Username:**
- **Date:**

## Implementation

- [ ] ROS2 Python package created (`waypoint_nav`)
- [ ] `navigate_waypoints.py` written and functional
- [ ] Node registered in `setup.py`
- [ ] Package builds successfully with `colcon build`
- [ ] Node runs and sends goals to Nav2

## Screenshots

### Screenshot 1: Node Running — First Waypoint

> Insert a screenshot showing your node running in a terminal, with the robot navigating toward the first waypoint visible in RViz.

![First Waypoint](screenshots/part3_waypoint1.png)

### Screenshot 2: Navigation in Progress

> Insert a screenshot showing the robot partway through its waypoint route, with the planned path visible.

![Navigation](screenshots/part3_navigation.png)

### Screenshot 3: All Waypoints Completed

> Insert a screenshot showing the terminal output confirming all waypoints were reached, or the robot at its final position.

![Completed](screenshots/part3_completed.png)

## Waypoint Coordinates

List the waypoints you used and briefly describe each location on the map:

| Waypoint | x | y | yaw | Location description |
|----------|---|---|-----|---------------------|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |

## Questions

### Q1: Code Walkthrough

Explain how your `navigate_waypoints.py` node works. Describe the role of the action client, how goals are sent, and how the node knows when one waypoint is reached so it can send the next.

**Answer:**

### Q2: Coordinate System

How did you determine the (x, y) coordinates for your waypoints? What coordinate frame are they in, and how does that frame relate to the map?

**Answer:**

### Q3: Orientation

The waypoints include a `yaw` value that controls the robot's final orientation at each goal. Why might the final orientation matter for a real-world robot performing tasks at each waypoint?

**Answer:**

### Q4: Error Handling

What happens if one of your waypoints is inside an obstacle or outside the mapped area? Did you test this? Describe what the Nav2 stack does when it cannot reach a goal.

**Answer:**

### Q5: Extensions

If you were to improve this node for a real-world application (e.g., a delivery robot or patrol robot), what features would you add? Name at least two and explain why.

**Answer:**
