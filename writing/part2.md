# Part 2: Autonomous Navigation in Simulation

## Student Information

- **Name:**
- **GitHub Username:**
- **Date:**

## Setup Verification

- [ ] Gazebo launched with `turtlebot3_world`
- [ ] Nav2 launched with your saved map
- [ ] Initial pose set in RViz (particle cloud converged)

## Screenshots

### Screenshot 1: Initial Pose Estimate

> Insert a screenshot showing RViz after you set the initial pose. The green particle cloud should be visible and concentrated around the robot.

![Initial Pose](screenshots/part2_initial_pose.png)

### Screenshot 2: Navigation Goal 1

> Insert a screenshot showing the robot navigating to your first goal position. The planned path should be visible in RViz.

![Goal 1](screenshots/part2_goal1.png)

### Screenshot 3: Navigation Goal 2

> Insert a screenshot showing navigation to a second goal position.

![Goal 2](screenshots/part2_goal2.png)

### Screenshot 4: Navigation Goal 3

> Insert a screenshot showing navigation to a third goal position. Choose a goal that requires navigating around an obstacle.

![Goal 3](screenshots/part2_goal3.png)

### Screenshot 5: Costmap Visualization

> Insert a screenshot showing the costmap layers visible in RViz. Enable both the global and local costmaps.

![Costmaps](screenshots/part2_costmaps.png)

## Questions

### Q1: Initial Pose Estimation

Why is it necessary to set an initial pose estimate before sending navigation goals? What happens if you skip this step or set the pose incorrectly?

**Answer:**

### Q2: Costmap Observations

Describe what you see in the global and local costmaps. How do they differ? What do the different colors represent?

**Answer:**

### Q3: Path Planning

When the robot navigates around an obstacle, describe the path it takes. Does it follow the shortest possible path, or does it maintain some distance from obstacles? Why might this behavior be desirable?

**Answer:**

### Q4: Recovery Behaviors

Did the robot ever get stuck, fail to reach a goal, or need to replan? Describe any issues you observed and how the Nav2 stack handled them. If the robot never got stuck, try sending a goal to a position that is difficult to reach and describe what happens.

**Answer:**

### Q5: Localization

As the robot moves, observe the green particle cloud in RViz. How does the cloud change as the robot navigates? What role does the lidar play in keeping the robot localized?

**Answer:**

### Q6: Connection to Textbook

Chapter 14 of the textbook discusses path planning algorithms. Based on what you observed, do you think the Nav2 planner is finding optimal paths? What trade-offs exist between path optimality and computational speed in a real-time navigation system?

**Answer:**
