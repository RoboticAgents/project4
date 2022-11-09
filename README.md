# Project 04: Robot Manipulation or Mapping

## Table of Contents

- [Timeline](#timeline)
- [Class Community Guidelines](#class-community-guidelines)
- [Summary](#summary)
- [Team Assignments](#team-assignments)
- [Instructions](#Instructions)

  - [Turtlebot 4 Lite](#turtlebot-4-lite)
  - [EvoArm](#evoarm)

- [Project Demonstrations](#project-demonstrations)

- [Assessment](#assessment)

- [Receiving Assistance](#receiving-assistance)

## Timeline

Activity                                                                                               | Deadline
------------------------------------------------------------------------------------------------------ | -------------------------------------------
Complete Step 1 and Step 1 part of the report (At least 1 commit from each team member):               | by the end of class on November 9th, 2022
Project Work Time - make significant progress toward Step 2 (At least 1 commit from each team member): | by the end of the lab on November 9th, 2022
Project Work Time - finish the project and the report (At least 1 commit from each team member):       | November 10-17, outside of class and lab
Demo and Final Submission:                                                                             | November 17th, 2022 at 9:30am

## Class Community Guidelines

Throughout the completion of this project you must adhere to the [community guidelines](https://github.com/CMPSC-311-Allegheny-College-Fall-2022/course-information/blob/main/community_guidelines.md) that we developed as a class. To report any violations of the code of conduct, please submit an [anonymous form](https://forms.gle/tePfnLY12hyN1Xbd6). Students who think that the class should revise some aspect of the guidelines must use the GitHub issue tracker for that repository to suggest, discuss, and implement any required changes.

By working on and completing this assignment you agree to use the hardware given to you in a responsible manner. Each team is responsible for the safety and security of the robot while it is in your possession. You are allowed to take the robots used in this project outside of ALIC but you have to return all parts at the completion of this project, or if requested, at the end of the semester.

## Summary

This project assignment invites you to work in a team to implement either an autonomous manipulation or autonomous mapping technique depending on a selected robot type. In this project, as was the case in the previous project, the class members will be split into two categories: 1) those working with Turtlebot 4 Lite, and 2) those working with EvoArm. In both cases the initial builds, configurations and set up has been already completed by the previous teams. In this project, each team will extend the functionality beyond the `teleop` operation of the robots. Specifically, if you are working with EvoArm, you will focus on developing a manipulation application of your choice using Python programming language. On the other hand, if you are working with Turtlebot 4 Lite, you will create autonomous mapping for a navigation task of your choice. Finally, teams will reflect on their experiences in writing in the file `writing/report.md` and demonstrate their developed applications in person. `writing/report.md` is a Markdown file that must adhere to the standards described in the [Markdown Syntax Guide](https://guides.github.com/features/mastering-markdown/).

## Team Assignments

View [team assignments](https://docs.google.com/spreadsheets/d/1167k-1ZGXR8TJqWdfp61kC7IDzhSVTUTHP7-Urx7ehc/edit?usp=sharing) to see who you will be working with.

## Instructions

### Turtlebot 4 Lite

You can learn about the hardware specifications of this robot in the [turtlebot4-hardware GitHub repository](https://github.com/turtlebot/turtlebot4-hardware). Please also review the features of this robot on [its website](https://turtlebot.github.io/turtlebot4-user-manual/overview/features.html).

#### Step 1:

You should first get connected and drive the robot using remote control, `teleop`. Please follow the [tutorial](turtlebot_tutorial.md) compiled by the previous teams. You can also use resources below. Please note that Turtlebot 4 does not include Bluetooth Controller.

#### Step 2:

Once the robot is up and running, you need to have it navigate in the environment. you need to come up with a navigation task and have your robot build a map of a relatively complex environment and have it navigate **autonomously** for a specific purpose or application. You can use the map of the previous team or follow [Generating a map tutorial](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/generate_map.html) to create your own. Then follow [Navigation tutorial](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/navigation.html) or watch [TurtleBot 4 | Mapping & Navigation with ROS 2 Navigation Stack Video](https://www.youtube.com/watch?v=T3if0aPj0Eo) to learn how to have the Turtlebot build a map and navigate in the environment given the map.

#### Other Resources:

- [TurtleBot 4 | Unboxing & Getting Started Video](https://www.youtube.com/watch?v=QN01AXjoLdQ&t=0s).
- [Driving your Turtlebot 4 Tutorial](https://turtlebot.github.io/turtlebot4-user-manual/tutorials/driving.html).
- [Turtlebot 4 Software information](https://turtlebot.github.io/turtlebot4-user-manual/software/) to learn about Turtlebot 4 packages, sensors, using `rviz2`, using SLAM technique to create maps, using navigation stack in ROS 2, and setting up Turtlebot 4 simulator.
- Questions and issue logging support can be found at [Turtlebot 4 GitHub Issues](https://github.com/turtlebot/turtlebot4/issues) and [Turtlebot 4 Simulator GitHub Issues](https://github.com/turtlebot/turtlebot4_simulator/issues).

### EvoArm

You can learn more about this robot, what is included in the robotic kit, and the contact information for support by visiting [evodyneacademy's website](https://www.evodyneacademy.com/evoarm-robotic-arm-kit-arduino). Additionally, [EvoArm Flyer](evoarm-kit-insert.pdf) has some helpful information.

#### Step 1:

You should first get connected and drive the robot using remote control on the web interface. Please follow the [tutorial](evoarm_tutorial.md) compiled by the previous teams. You can also refer to the instructions on the flyer that came with the robot to connect to it. You can either connect to the robot via hotspot or `CompSci` wifi.

To connect via hotspot:

1. find the correct robot (EVOARMMINI302, EVOARMMINI324, or EVOARMMINI334) under your computer's networks;
2. go to `10.10.0.1:9072` in your browser;
3. use password `evodyne2020` to log in

To connect via `CompSci` wifi:

1. go to `evoarm.evodyne.co`;
2. enter your robot's access id (the 9 digit number on the white sticker on the robot);
3. use access code `evodyne2020` to log in

#### Step 2:

Once the robot is up and running, you will need to select an autonomous robotic hand manipulation technique to implement for an application of your choice. Feel free to utilize resources provided in the kit. To find ideas you can also go to [evoarm-apps](https://www.evodyneacademy.com/forum/evoarm-apps), create an account, after after you are approved, you will find lots of information from the community, including sample code and information on how to use ROS and HTTP commands to control/program the arm.

#### Other Resources:

- This robot has already been built but if you need to check various parts of the built process, check out [evodyneacademy tutorials](https://www.evodyneacademy.com/tutorials) for all 16 steps of the build process. You will need to use password: `bRtaos40sNd` to access the tutorials.
- For support, you may reach out to Evodyne Robotics Academy `support@evodyneacademy.com`.

## Project Demonstrations

At the beginning of the class session on Thursday, November 17th, each team will be given an opportunity to demonstrate their project. When the class session starts, teams will be given a few minutes to set up their demonstrations and get them running. Then, class members will participate in an interactive demonstration session, where everyone will be able to view each demonstration.

## Assessment

The grade that a student receives on this assignment will have the following components.

- **GitHub Actions CI Build Status [up to 5%]:**: For lab04 repository associated with this assignment students will receive a checkmark grade if their last before-the-deadline build passes.

- **Mastery of Verbal Explanation during the Demonstration [up to 15%]:**: Since the ability to communicate technical details of a project is crucial to building successful software and hardware applications, a portion of students' lab grade will be determined based on the quality of the project demonstration.

- **Mastery of Technical Writing [up to 25%]:**: Students will also receive a part of their grade when the responses to the writing prompts presented in the `report.md` reveal a proficiency of both writing skills and technical knowledge. To receive full points of this component, the submitted writing should have correct spelling, grammar, and punctuation in addition to following the rules of Markdown and providing complete and conceptually and technically accurate answers.

  - Please note that the "Check Spelling" GitHub Actions check may flag proper nouns or other real words if the dictionary it uses does not contain them. If your "Check Spelling" GitHub Actions check is failing due to a correctly spelled word being incorrectly flagged as "unknown" by CSpell, you will need to add the word to the list of words in `.github/cspell.json`.

- **Mastery of Technical Knowledge and Skills [up to 55%]**: Students will receive a portion of their assignment grade when their project design and implementation reveals that they have mastered all of the technical knowledge and skills developed during the completion of this project. Any written programs must be inside `src/` directory. As a part of this grade, the instructor will assess aspects of the project including, but not limited to, the appropriate design of the robot task, the completeness and correctness of the implemented software, effectiveness of experiments, and the use of effective source code comments and Git commit messages.

- **Continuous Progress [up to 40% deducted points]**: To ensure equal team effort and timely troubleshooting, students may lose up to 40% of points from their final deliverable for not demonstrating continuous team effort on this project. Each activity not submitted by the stated deadline in the [Timeline](#timeline) section by ALL team members will result in -10% unless the effected team member or the whole team (if the entire team was effected) can demonstrate circumstances beyond their control (e.g., illness, hardware challenges unsolvable in time, etc.).

All grades for this project will be reported through a student's gradebook GitHub repository.

### GatorGrade

You can check the baseline writing and commit requirements of this project by running department's assignment checking `gatorgrade` tool To use `gatorgrade`, you first need to make sure you have Python installed. If not, please see:

- [Setting Up Python on Windows](https://realpython.com/lessons/python-windows-setup/)
- [Python 3 Installation and Setup Guide](https://realpython.com/installing-python/)
- [How to Install Python 3 and Set Up a Local Programming Environment on Windows 10](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-windows-10)

Then, you need to install `gatorgrade`:

- First, [install `pipx`](https://pypa.github.io/pipx/installation/)
- Then, install `gatorgrade` with `pipx install gatorgrade`

Finally, you can run `gatorgrade`:

`gatorgrade --config config/gatorgrade.yml`

## Assistance

If you are having trouble completing any part of this project, then please talk with the course instructor during the laboratory session. Alternatively, you may ask questions in the Discord channel for this course. Finally, you can schedule a meeting during the course instructor's office hours.
