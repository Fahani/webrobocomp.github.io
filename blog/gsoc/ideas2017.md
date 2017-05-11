to-markdownSource on GitHub
 GitHub Flavored Markdown
HTML

<h1 class="post-title">Google Summer of Code 2017 Ideas</h1>
  <span class="post-date">07 Feb 2017</span>
  <h2 id="general-information-on-applications">General information on applications</h2>

<p>If you are interested in any of the ideas listed below or you think you can propose something interesting to improve RoboComp we encourage you to apply.
It is important to familiarize with the software first.
You can go through the available tutorials and direct your questions to the mailing list or gitter chat (listed below, also see conctact section).
Please read all the information posted in this page before applying.
Make sure you are familiar with the required skills for the idea.
Since several of the mentioned RoboComp tools and components are not explained here to keep this list short we encourage everyone to check the RoboComp documentation linked below.
Mentors and backup mentors are listed right after the idea explanation.
All mentors contact info is at the end of the page.</p>

<p><strong>Where can I start and what to include on my application?</strong> <br>
You are encouraged to go through these steps for a better understading and follow-up of your application.</p>

<ol>
  <li>Download and install RoboComp: <a href="https://github.com/robocomp/robocomp/blob/master/README.md">https://github.com/robocomp/robocomp/blob/master/README.md</a>.</li>
  <li>Follow the tutorials: <a href="https://github.com/robocomp/robocomp/blob/master/doc/README.md">https://github.com/robocomp/robocomp/blob/master/doc/README.md</a>.</li>
  <li>Once you are familiar with RoboComp and the components and tools involved in the particular idea you want to contribute to, try to understand how these components/tools work and, if possible, their design.</li>
  <li>Participate in the mailing list asking any question you might have.</li>
  <li>Create a schedule with the milestones you plan to follow during the GSoC 2017 program.</li>
  <li>Send the schedule and any code you might have written along with your CV and other application materials to the main mentor of your idea, CC the backup mentor.</li>
</ol>

<p>General questions about RoboComp:<br>
<a href="https://groups.google.com/forum/?hl=es#!forum/robocomp-dev">Developers list</a><br>
<a href="https://gitter.im/robocomp/home">Gitter chat</a></p>

<h2 id="ideas-list-for-gsoc-2017">IDEAS LIST for GSoC 2017</h2>

<h3 id="1-javascript-support">1. Javascript support</h3>
<p>NodeJS component code generation.
An interesting diversion from current robotics technologies based on C++ and Python would be the use of Javascript as the language to code some highly concurrent components.
Most components combine push-pull and RPC communication models to talk to other components in their graph of processes.
Additional threads are normally used to handle the components’ internal tasks.
In this activity, the current component code generator of RoboComp will be extended to include Javascript running in a server as a new target language for components.
The main restriction here is that ZeroC releases a version of the Ice middleware running on Node, since the web browser version is already out.</p>

<p>Needed skills: Javascript, Python, Cog(python module)<br>
Mentor: Pablo Bustos<br>
Backup mentor: Luis J. Manso</p>

<h3 id="2-graphic-deployment-tool">2. Graphic deployment tool</h3>
<p>Currently RoboComp has a component deployment tool that has been improved through several years, rcmanager.
This tool has to be provided with an XML file with the configuration of the network of components to be deployed.
Having this XML file created using a graphical tool would help enormously the configuration of new setups for different projects.
In this idea you will improve the current component management graphical tool to allow roboticists generate these deployment configuration files in an easy way.</p>

<p>Needed skills: Python, C++, Qt<br>
Mentor: Ramon Cintas<br>
Backup mentor: Marco A. Gutiérrez</p>

<h3 id="3-connect-robocomp-to-gazebo">3. Connect RoboComp to Gazebo</h3>
<p>RoboComp currently has its own simulation tool RCIS (RoboComp InnerModel Simulator).
Despite it is extremely useful for multiple purposes, it does not incorporate physics simulation.
Additionally, several robot models are already available for Gazebo, which is a more spread simulation environment than RCIS.
Having Gazebo as an option for simulating robots using RoboComp will increase the possibilities that our framework can offer to users and developers.
The idea is to connect RoboComp and Gazebo in a similar way ROS is connected to Gazebo.
As a result, simulations in RoboComp could be launched on both simulators (Gazebo or RCIS).</p>

<p>Needed skills: Gazebo, C++, Robotics simulation <br>
Mentors: Marco A. Gutiérrez<br>
Backup mentor: Pablo Bustos</p>

<h3 id="4-graphic-editor-for-innermodel-worlds">4. Graphic editor for InnerModel worlds</h3>
<p>InnerModel worlds are XML-based files which define the kinematic properties of the robots and their environments.
Writing these files is a tedious and error-prone task if they go beyond a handful of lines. 
Even when InnerModel worlds are done, sometimes modifying its elements or their position becomes cumbersome, and developers end up in a trial-and-error loop until that can take a while.
Having a graphical tool for creating and editing these files would help enormously.</p>

<p>Needed skills: C++, OpenSceneGraph, InnerModel<br>
Mentor: Luis J. Manso<br>
Backup mentor: Marco A. Gutiérrez</p>

<h3 id="5-easing-deployment-reducing-dependencies-dockering-robocomp">5. Easing deployment, reducing dependencies, “dockering” RoboComp</h3>
<p>Docker is a good way to make sure software works independently of the configuration of the system where it is being run.
Having demos of RoboComp enclosed into different docker containers can help with deployment in different machines as well as preserve the integrity of a certain demo when it comes the time to run it.
In addition to writing down the robocomp-docker documentation and related tutorials, you will be in charge of:</p>
<ul>
  <li>Refactoring the CMake structure within RoboComp: it is needed so basic requirements for installation are reduced to a minimum. On the same page, this activity should ease the task for new developers to add new libraries, classes, tools or modules to the framework.</li>
  <li>Facilitate the deployment on different operating systems, a fully functional Docker container for RoboComp will be developed so instant deployment will be available on heterogeneous platforms through the docker tool.</li>
  <li>A semi-automatic procedure to update changes from the repository to the Docker container will be established.</li>
</ul>

<p>Needed skills: Docker, GNU/Linux, C++, Python, CMake<br>
Mentor: Marco A. Gutiérrez<br>
Backup mentor: Ramon Cintas</p>

<h3 id="6-3d-visualization-of-internal-robocomp-structures-in-real-time">6. 3D visualization of internal RoboComp structures in real time</h3>
<p>Many RoboComp components use AGM graph models for cognitive world modeling.
These models are frequently hard to understand, especially when they are big, which they usually are.
Making these graphs easier to read and their information easier to access graphically is a key task that will ease the task of debugging and understanding these cognitive models, and in consequence, the robots’ minds.
Specific research and tests with roboticists should be performed in order to find a way of displaying this information that helps anyone understand the whole graph.
Once specific key assets are detected they should be implemented one by one and properly tested with roboticists to ensure they are widely accepted as a proper enhancement.
Several of these features should be developed until the graph becomes easily readable and can be easily managed through the graphical interface provided in RoboComp.</p>

<p>Needed skills: Python, C++, Qt5, OSG<br>
Mentor: Luis J. Manso<br>
Backup mentor: Marco A. Gutiérrez</p>

<h3 id="7-learning-useful-actions-for-efficient-planning">7. Learning useful actions for efficient planning</h3>
<p>Many RoboComp components us the AGGLPlanner artificial intelligence task planner.
Despite the many advantages the AGGLPlanner planner has, when tasks are complex and the domain is large, plans take too long to be computed in practice.
An interesting way to improve these times is to reduce the domain size, that is, the number of possible actions that the planner takes into account when searching for the optimum plan.
Of course we don’t want to remove useful actions from domain, because it would unable the robots to plan how to achieve their misssions.
Therefore, the goal is to learn which actions are most-likely to be used depending on the mission of the robot and the previous missions and plans.
Lets say the robot has actions related to navigation, actions related to manipulation, and others related to human-robot interaction.
If the robot is aksed to go some place, we would like to try planning such task taking only into account the actions related to navigation.
If the planner cannot find a goal using only those actions, the planning process should be relaunched taking also into account other actions which are more or less likely to be used. 
The task at hand here would be to integrate a machine learning algorithm in AGGLPlanner to learn which actions are most-likely to be used for a target, given information regarding previous planning tasks and their outcomes.</p>

<p>Needed skills: Python, Machine learning<br>
Mentor: Luis J. Manso<br>
Backup mentor: Pablo Bustos</p>

<h3 id="8-redesign-the-learnbot-robot-to-achieve-smaller-footprints">8. Redesign the Learnbot robot to achieve smaller footprints</h3>
<p>Learnbot is a low-cost educational robot designed to develop computational thinking through the use of the 
Python language. 
In this proposal we want to think new ways of co-design between robot behavior and its physical outfit. Using advanced 3D printer based prototyping and embedding electronics in the robot’s chassis, the goal is to create new, smaller and more efficient robot designs. Each prototype will be manufactured in RoboLab’s facilities and tested in experiments involving children learning to program.</p>

<p>Needed skilleds: Python, 3D modeling software<br>
Mentor: Juan Mario Haut<br>
Backup mentor: Pablo Bustos</p>

<h3 id="9-improving-the-rcis-robotics-simulator-with-better-software-engineering-practices-and-naive-physics">9. Improving the RCIS robotics simulator with better software engineering practices and naive physics</h3>
<p>RCIS is the official RoboComp simulator. It has been designed using the OpenSceneGraph engine and has several usueful features that accelerates the development of robotics code everyday. However, the new robots and its applications demand more sophisticated functionalities. This proposal is aimed at the refactoring of RCIS to facilitate the modular extension of new simulated sensors and actuators. Also, the new version will include a basic naive physics engine to account for collisions detection and finite acceleration, and also a more efficient implementation of the core simulation loop. RCIS is similar to a RoboComp component and this refactoring process should not be a hard task.</p>

<p>Needed skills: Python, C++<br>
Mentor: Pablo Bustos<br>
Backup mentor: Juan Mario Haut</p>

<h3 id="10-introducing-robust-code-generation-techniques-in-robocomp">10 Introducing robust code generation techniques in Robocomp</h3>
<p>RoboComp is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. It is based on DSL technology. In this proposal we seek to improve the robustness of the generated code using model checking techniques and good software engineering practices. The student will have to extend the current code generation tool, robocompdsl, to include verification points, asserts, parameter range control, etc independent of the specific functionality of the component. We expect that with this improvement the new generated components will be less prone to accidental crashes and easier to debug and maintain.</p>

<p>Needed skills: Python, C++<br>
Mentor: Juan Mario Haut<br>
Backup mentor: Luis J. Manso</p>

<h3 id="11-social-navigation-with-robocomp">11. Social navigation with Robocomp</h3>
<p>Currently RoboComp allows robots to autonomously navigate in its surroundings. Different RoboComp components and agents concurrently run in order to make possible this navigation: localization, SLAM or path planning algorithms are examples of them. On top of RoboComp, a new Robotics Cognitive Architecture named CORTEX is being developed. Considering the recent use of mobile robots in a more social, human inhabited, context, the use of classical navigation techniques is restricted, since they do not differentiate humans from other ordinary objects in the environment. The task at hand here would be to integrate social behaviors in the existing RoboComp Navigation Agent. A typical case of use would be that of a group of humans walking and interacting with the environment while the robot performs a task. All these simulations would be made using RCIS (RoboComp InnerModel Simulator)</p>

<p>Needed skills: Python, C++<br>
Mentor: Pedro Núñez<br>
Backup mentor: Luis Manso</p>

<h3 id="12-collaborative-decission-making-among-a-herd-of-learnbots">12. Collaborative decission making among a herd of Learnbots</h3>
<p>Herds of robots are a very interesting tool to teach distributed computational thinking. We want to develop new tools to create, communicate and control a herd of our Learnbots, and use them to devise new experiments and experiences for learning kids and teens. Each robot is controlled remotely from a laptop via a Wifi link. This configuration eases the development of high-level tools, such as domain specific languages, that would provide a common language and distributed reasoning for the robots. Experimental results will be tested with groups of students and children.</p>

<p>eeded skills: Python 
Mentor: Pilar Bachiller
Backup mentor: Pablo Bustos</p>

<h3 id="13-domain-specific-language-for-programming-the-learnbot-educational-robot">13. Domain Specific Language for programming the Learnbot educational robot</h3>
<p>LearnBot is a small, low cost educational robot created by the RoboComp team. We are developing tools to faciltate its use by kids from 11 to college. The goal of the robot is to ease the learning curve of textual programming. We want to approach this problem by designing simple DSLs that generate robot behaviors taking cake of aspects like time and events that are hard to code using general purpose languages. The prototype languages will translate into Python and using the existing API that controls the robot. Experimental results will be tested with groups of students and children.</p>

<p>Needed skills: Python 
Mentor: Pilar Bachiller
Backup mentor: Pablo Bustos</p>

<h2 id="mentors-info">Mentors info</h2>
<h3 id="marco-a-gutiérrez">Marco A. Gutiérrez</h3>
<p>marcogATunexDOTes<br>
PhD Student, RoboLab,<br>
University of Extremadura</p>

<h3 id="pablo-bustos">Pablo Bustos</h3>
<p>pbustosATunexDOTes<br>
Professor, RoboLab,<br>
University of Extremadura</p>

<h3 id="luis-j-manso">Luis J. Manso</h3>
<p>lmansoATunexDOTes<br>
Postdoc Researcher, RoboLab,<br>
University of Extremadura</p>

<h3 id="ramon-cintas">Ramon Cintas</h3>
<p>rcintasATunexDOTes<br>
Researcher, Robolab,<br>
University of Extremadura</p>

<h3 id="juan-mario-haut">Juan Mario Haut</h3>
<p>juanmariohautATunexDOTes<br>
Researcher,<br>
University of Extremadura</p>

<h3 id="pedro-núñez">Pedro Núñez</h3>
<p>pnuntruATunexDOTes<br>
Professor, RoboLab,  <br>
University of Extremadura</p>

<h3 id="pilar-bachiller">Pilar Bachiller</h3>
<p>pilarbATunexDOTes<br>
Professor, RoboLab,  <br>
University of Extremadura</p>
Markdown

# Google Summer of Code 2017 Ideas

<span class="post-date">07 Feb 2017</span>

## General information on applications

If you are interested in any of the ideas listed below or you think you can propose something interesting to improve RoboComp we encourage you to apply. It is important to familiarize with the software first. You can go through the available tutorials and direct your questions to the mailing list or gitter chat (listed below, also see conctact section). Please read all the information posted in this page before applying. Make sure you are familiar with the required skills for the idea. Since several of the mentioned RoboComp tools and components are not explained here to keep this list short we encourage everyone to check the RoboComp documentation linked below. Mentors and backup mentors are listed right after the idea explanation. All mentors contact info is at the end of the page.

**Where can I start and what to include on my application?**
You are encouraged to go through these steps for a better understading and follow-up of your application.

1.  Download and install RoboComp: [https://github.com/robocomp/robocomp/blob/master/README.md](https://github.com/robocomp/robocomp/blob/master/README.md).
2.  Follow the tutorials: [https://github.com/robocomp/robocomp/blob/master/doc/README.md](https://github.com/robocomp/robocomp/blob/master/doc/README.md).
3.  Once you are familiar with RoboComp and the components and tools involved in the particular idea you want to contribute to, try to understand how these components/tools work and, if possible, their design.
4.  Participate in the mailing list asking any question you might have.
5.  Create a schedule with the milestones you plan to follow during the GSoC 2017 program.
6.  Send the schedule and any code you might have written along with your CV and other application materials to the main mentor of your idea, CC the backup mentor.

General questions about RoboComp:
[Developers list](https://groups.google.com/forum/?hl=es#!forum/robocomp-dev)
[Gitter chat](https://gitter.im/robocomp/home)

## IDEAS LIST for GSoC 2017

### 1\. Javascript support

NodeJS component code generation. An interesting diversion from current robotics technologies based on C++ and Python would be the use of Javascript as the language to code some highly concurrent components. Most components combine push-pull and RPC communication models to talk to other components in their graph of processes. Additional threads are normally used to handle the components’ internal tasks. In this activity, the current component code generator of RoboComp will be extended to include Javascript running in a server as a new target language for components. The main restriction here is that ZeroC releases a version of the Ice middleware running on Node, since the web browser version is already out.

Needed skills: Javascript, Python, Cog(python module)
Mentor: Pablo Bustos
Backup mentor: Luis J. Manso

### 2\. Graphic deployment tool

Currently RoboComp has a component deployment tool that has been improved through several years, rcmanager. This tool has to be provided with an XML file with the configuration of the network of components to be deployed. Having this XML file created using a graphical tool would help enormously the configuration of new setups for different projects. In this idea you will improve the current component management graphical tool to allow roboticists generate these deployment configuration files in an easy way.

Needed skills: Python, C++, Qt
Mentor: Ramon Cintas
Backup mentor: Marco A. Gutiérrez

### 3\. Connect RoboComp to Gazebo

RoboComp currently has its own simulation tool RCIS (RoboComp InnerModel Simulator). Despite it is extremely useful for multiple purposes, it does not incorporate physics simulation. Additionally, several robot models are already available for Gazebo, which is a more spread simulation environment than RCIS. Having Gazebo as an option for simulating robots using RoboComp will increase the possibilities that our framework can offer to users and developers. The idea is to connect RoboComp and Gazebo in a similar way ROS is connected to Gazebo. As a result, simulations in RoboComp could be launched on both simulators (Gazebo or RCIS).

Needed skills: Gazebo, C++, Robotics simulation
Mentors: Marco A. Gutiérrez
Backup mentor: Pablo Bustos

### 4\. Graphic editor for InnerModel worlds

InnerModel worlds are XML-based files which define the kinematic properties of the robots and their environments. Writing these files is a tedious and error-prone task if they go beyond a handful of lines. Even when InnerModel worlds are done, sometimes modifying its elements or their position becomes cumbersome, and developers end up in a trial-and-error loop until that can take a while. Having a graphical tool for creating and editing these files would help enormously.

Needed skills: C++, OpenSceneGraph, InnerModel
Mentor: Luis J. Manso
Backup mentor: Marco A. Gutiérrez

### 5\. Easing deployment, reducing dependencies, “dockering” RoboComp

Docker is a good way to make sure software works independently of the configuration of the system where it is being run. Having demos of RoboComp enclosed into different docker containers can help with deployment in different machines as well as preserve the integrity of a certain demo when it comes the time to run it. In addition to writing down the robocomp-docker documentation and related tutorials, you will be in charge of:

*   Refactoring the CMake structure within RoboComp: it is needed so basic requirements for installation are reduced to a minimum. On the same page, this activity should ease the task for new developers to add new libraries, classes, tools or modules to the framework.
*   Facilitate the deployment on different operating systems, a fully functional Docker container for RoboComp will be developed so instant deployment will be available on heterogeneous platforms through the docker tool.
*   A semi-automatic procedure to update changes from the repository to the Docker container will be established.

Needed skills: Docker, GNU/Linux, C++, Python, CMake
Mentor: Marco A. Gutiérrez
Backup mentor: Ramon Cintas

### 6\. 3D visualization of internal RoboComp structures in real time

Many RoboComp components use AGM graph models for cognitive world modeling. These models are frequently hard to understand, especially when they are big, which they usually are. Making these graphs easier to read and their information easier to access graphically is a key task that will ease the task of debugging and understanding these cognitive models, and in consequence, the robots’ minds. Specific research and tests with roboticists should be performed in order to find a way of displaying this information that helps anyone understand the whole graph. Once specific key assets are detected they should be implemented one by one and properly tested with roboticists to ensure they are widely accepted as a proper enhancement. Several of these features should be developed until the graph becomes easily readable and can be easily managed through the graphical interface provided in RoboComp.

Needed skills: Python, C++, Qt5, OSG
Mentor: Luis J. Manso
Backup mentor: Marco A. Gutiérrez

### 7\. Learning useful actions for efficient planning

Many RoboComp components us the AGGLPlanner artificial intelligence task planner. Despite the many advantages the AGGLPlanner planner has, when tasks are complex and the domain is large, plans take too long to be computed in practice. An interesting way to improve these times is to reduce the domain size, that is, the number of possible actions that the planner takes into account when searching for the optimum plan. Of course we don’t want to remove useful actions from domain, because it would unable the robots to plan how to achieve their misssions. Therefore, the goal is to learn which actions are most-likely to be used depending on the mission of the robot and the previous missions and plans. Lets say the robot has actions related to navigation, actions related to manipulation, and others related to human-robot interaction. If the robot is aksed to go some place, we would like to try planning such task taking only into account the actions related to navigation. If the planner cannot find a goal using only those actions, the planning process should be relaunched taking also into account other actions which are more or less likely to be used. The task at hand here would be to integrate a machine learning algorithm in AGGLPlanner to learn which actions are most-likely to be used for a target, given information regarding previous planning tasks and their outcomes.

Needed skills: Python, Machine learning
Mentor: Luis J. Manso
Backup mentor: Pablo Bustos

### 8\. Redesign the Learnbot robot to achieve smaller footprints

Learnbot is a low-cost educational robot designed to develop computational thinking through the use of the Python language. In this proposal we want to think new ways of co-design between robot behavior and its physical outfit. Using advanced 3D printer based prototyping and embedding electronics in the robot’s chassis, the goal is to create new, smaller and more efficient robot designs. Each prototype will be manufactured in RoboLab’s facilities and tested in experiments involving children learning to program.

Needed skilleds: Python, 3D modeling software
Mentor: Juan Mario Haut
Backup mentor: Pablo Bustos

### 9\. Improving the RCIS robotics simulator with better software engineering practices and naive physics

RCIS is the official RoboComp simulator. It has been designed using the OpenSceneGraph engine and has several usueful features that accelerates the development of robotics code everyday. However, the new robots and its applications demand more sophisticated functionalities. This proposal is aimed at the refactoring of RCIS to facilitate the modular extension of new simulated sensors and actuators. Also, the new version will include a basic naive physics engine to account for collisions detection and finite acceleration, and also a more efficient implementation of the core simulation loop. RCIS is similar to a RoboComp component and this refactoring process should not be a hard task.

Needed skills: Python, C++
Mentor: Pablo Bustos
Backup mentor: Juan Mario Haut

### 10 Introducing robust code generation techniques in Robocomp

RoboComp is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. It is based on DSL technology. In this proposal we seek to improve the robustness of the generated code using model checking techniques and good software engineering practices. The student will have to extend the current code generation tool, robocompdsl, to include verification points, asserts, parameter range control, etc independent of the specific functionality of the component. We expect that with this improvement the new generated components will be less prone to accidental crashes and easier to debug and maintain.

Needed skills: Python, C++
Mentor: Juan Mario Haut
Backup mentor: Luis J. Manso

### 11\. Social navigation with Robocomp

Currently RoboComp allows robots to autonomously navigate in its surroundings. Different RoboComp components and agents concurrently run in order to make possible this navigation: localization, SLAM or path planning algorithms are examples of them. On top of RoboComp, a new Robotics Cognitive Architecture named CORTEX is being developed. Considering the recent use of mobile robots in a more social, human inhabited, context, the use of classical navigation techniques is restricted, since they do not differentiate humans from other ordinary objects in the environment. The task at hand here would be to integrate social behaviors in the existing RoboComp Navigation Agent. A typical case of use would be that of a group of humans walking and interacting with the environment while the robot performs a task. All these simulations would be made using RCIS (RoboComp InnerModel Simulator)

Needed skills: Python, C++
Mentor: Pedro Núñez
Backup mentor: Luis Manso

### 12\. Collaborative decission making among a herd of Learnbots

Herds of robots are a very interesting tool to teach distributed computational thinking. We want to develop new tools to create, communicate and control a herd of our Learnbots, and use them to devise new experiments and experiences for learning kids and teens. Each robot is controlled remotely from a laptop via a Wifi link. This configuration eases the development of high-level tools, such as domain specific languages, that would provide a common language and distributed reasoning for the robots. Experimental results will be tested with groups of students and children.

eeded skills: Python Mentor: Pilar Bachiller Backup mentor: Pablo Bustos

### 13\. Domain Specific Language for programming the Learnbot educational robot

LearnBot is a small, low cost educational robot created by the RoboComp team. We are developing tools to faciltate its use by kids from 11 to college. The goal of the robot is to ease the learning curve of textual programming. We want to approach this problem by designing simple DSLs that generate robot behaviors taking cake of aspects like time and events that are hard to code using general purpose languages. The prototype languages will translate into Python and using the existing API that controls the robot. Experimental results will be tested with groups of students and children.

Needed skills: Python Mentor: Pilar Bachiller Backup mentor: Pablo Bustos

## Mentors info

### Marco A. Gutiérrez

marcogATunexDOTes
PhD Student, RoboLab,
University of Extremadura

### Pablo Bustos

pbustosATunexDOTes
Professor, RoboLab,
University of Extremadura

### Luis J. Manso

lmansoATunexDOTes
Postdoc Researcher, RoboLab,
University of Extremadura

### Ramon Cintas

rcintasATunexDOTes
Researcher, Robolab,
University of Extremadura

### Juan Mario Haut

juanmariohautATunexDOTes
Researcher,
University of Extremadura

### Pedro Núñez

pnuntruATunexDOTes
Professor, RoboLab,
University of Extremadura

### Pilar Bachiller

pilarbATunexDOTes
Professor, RoboLab,
University of Extremadura
to-markdown is copyright © 2011-15 Dom Christie and is released under the MIT licence.