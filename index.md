_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *

# [Google Summer of Code 2017 Ideas](/website/2017/02/07/gsoc17ideas/)

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



# [final week updates](/website/2016/08/20/yashWeek16/)

<span class="post-date">20 Aug 2016</span>

# Final Evaluation Updates

In this post I am concluding the work done during my GSOC period. I will outline the basic work done, results achieved and how to use the tools I have developed. Also I will provide a demo of the work done.

**Work Completed**

I have developed and thoroughly tested RCMaster. Also I have changed the C++ and python components to use RCMaster. Also these changes are incorporated into the code generator so that even the future components will use RCMaster by default.

I will list down the features of RCMaster now:

*   All the components can register to RCMaster in which they will be given a free port
*   The queries can be filtered by various parameters. e.g: A component can get the list of all the interfaces of name “test” running on the host 192.168.0.21
*   RCMaster periodically saves the database so that even if the RCMaster crashes it can recover the data on restart and prevent the hassle of restarting the components.
*   RCMaster has a cache implemented so that if a component is crashed it will be assigned the same port on restart. So that all the previous connected components can continue the connection.
*   The components will now wait until the required interfaces are up and if any of the interfaces go down then they will suspend until they are up. This allows us to start the components in any order.
*   RCMaster is designed to produce meaningful exceptions.

**Usage**

Here I will describe how to generate a sample component and how to run it

First write the cdsl description of the component client1.cdsl



```
import "/robocomp/interfaces/IDSLs/RCMaster.idsl";
import "/robocomp/interfaces/IDSLs/Test.idsl";
import "/robocomp/interfaces/IDSLs/ASR.idsl";

Component client1{
	Communications{
    	requires rcmaster;
    	implements ASR,test;
	};
	language Python;
};

```



Now generate the component by running `robocompdsl client1.cdsl ./`

Now generate another component client2.cdsl by running `robocompdsl client2.cdsl ./`



```
import "/robocomp/interfaces/IDSLs/RCMaster.idsl";
import "/robocomp/interfaces/IDSLs/Test.idsl";
import "/robocomp/interfaces/IDSLs/ASR.idsl";

Component client2{
	Communications{
    	requires rcmaster,test;
	};
	language Python;
}; Now edit the client2 to use the test interface implemented by client1\. Open `src/client2.py` file from the client2's directory and change the `@@COMP NAME HERE@@` to client1.

```



Edit the client to `src/specificworker.py` in client 2 and add the below code in the compute function.



```
try:
    self.proxyData["test"]["proxy"].printmsg("hello from " + self.name)
 except Ice.SocketException
       self.waitForComp("test",True)

```



Now edit the printmsg() in `src/specificworker` to add the below line



```
print message

```


Now we can run all the components.

First start the RCMaster and then the components



```
src/rcmaster.py    This will start RCMaster.    Now start client2 , then you can see that the client 2 is waiting for client1

```



src/client2.py –Ice.Config=etc/config

Now start client1



```
    src/client1.py --Ice.Config=etc/config

```



Now you can see the client1 printing “Hello from Client2” periodically. Play around

### Demo video

You can see the video of the above demo [here](https://github.com/robocomp/website/blob/gh-pages/img/rcmaster_demo.mp4)

### Links to commits

[All Commits](https://github.com/robocomp/robocomp/commits?author=yashsanap)

[Pullrequest1](https://github.com/robocomp/robocomp/pull/78)

<a href="">Pullrequest2</a>

<a href="">Pullrequest3</a>

* * *

Regards,

Yash Sanap



# [RoboCompDSL's Tests](/website/2016/08/01/dgallegosWeek9/)

<span class="post-date">01 Aug 2016</span>

Hi!

After develop and following the Software Development Life Cycle:

![](http://xbsoftware.com/wp-content/uploads/2014/10/software-development-life-cycle.png)

I have created an script which contains tests for RoboCompDSL. This script allows generate easy components automatically with test code and run them. Users just have to supervise and continue with next test if everything works.

This work and more information can be found [here.](https://github.com/robocomp/robocomp/tree/highlyunstable/tools/robocompdsl/tests)

## How does it work?

**tests.sh** has a main with 3 options:

1.- Tests for CPP components.

2.- Tests for PYTHON components.

3.- Exit. (You can exit by simply pressing _Ctrl + C_ too)

If you select first or second option, tests.sh will generate and run your components in **/tmp/** directory. Why /tmp/? Simply, this tests are useless if everything works so in /tmp will not bother and will be removed after reboot. if you need to work with them, you can find them there or re-launching the script.

## Is it nice?

Of course, this script works with [Yakuake](https://yakuake.kde.org), and uses it to offer a much more pleasant and familiar test environment. It will create and divide tabs for each test.


# [Information of interest](/website/2016/07/22/dgallegosWeek8/)

<span class="post-date">22 Jul 2016</span>

Hello, in this post I will explain in summary form some technologies used in **RoboCompDSL.**

## Model Driven Engineering MDE

Model driven engineering (Model-driven engineering) is a generic methodology that is based on knowledge of a specific domain to allow creating generic models (meta-models) of a problem to solve. This allows for greater abstraction of the problem and its surroundings, making it easier to find an efficient and complete solution. In this way, you can create models that users unfamiliar with the problem environment, could work in the domain of interest or even facilitate communication between individuals of a team working in various specific domains within the same working environment.

It is very common to use domain-specific languages ​​(Domain-Specific Language) that can be defined as text, diagrams, etc. To deal with these models. Have tools that deal with these DSLs could detect errors in the use of language or provide a user interface to enable work with models a simpler, guided and robust way. MDE use in oriented components can be an even intuitive programming practice because it would be very easy to specify a model for deployment and communications components or specification of the interfaces they use.

## Domain-specific languages

There are many possible solutions that can be given for the same problem, choose one or the other will always depend on the quality of the result and the cost to get it. The domain-specific languages ​​can reduce the cost of carrying out a project without affecting the result. A clear example would be given in many cases where we have component-oriented routine code that could be specified once and reused the rest of the time schedule. DSLs are widely used in many projects such as SQL or HTML5.


# [State Machine Code Generation in Python](/website/2016/07/19/ibarbechWeek7/)

<span class="post-date">19 Jul 2016</span>

# Equality between State Machine Code Generation in C++ and in Python

*   The language used for definition the state machine is **the same**.
*   The file of parsing of file that has the definition of state machine is **the same**.
*   The framework for the code of state machine is **the same**.

# Codification of the state machine in Python

Robocompcdsl used a templates files to generate code written in python. I modified the following:

*   **genericworker.py**
*   **specificworker.py**

*   **genericworker.py**

This file contains the definition of state machine, slot functions, definition of the signals for transitions between states and the connections between signals and funtions.

*   **specificworker.py**

This file containd the code that is executed when the state machine active a state.

# Example

*   **genericworker.py**

Example genericworker.py:



```
#
# Copyright (C) 2016 by YOUR NAME HERE
#
#    This file is part of RoboComp
#
#    RoboComp is free software: you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    RoboComp is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License
#    along with RoboComp.  If not, see <http://www.gnu.org/licenses/>.

import sys
from PySide import *

class GenericWorker(QtCore.QObject):

	kill = QtCore.Signal()
#Signals for State Machine
	test1totest1 = QtCore.Signal()
	test1totest2 = QtCore.Signal()
	test2totest3 = QtCore.Signal()
	test2totest5 = QtCore.Signal()
	test2totest6 = QtCore.Signal()
	test3totest3 = QtCore.Signal()
	test3totest4 = QtCore.Signal()
	test4totest5 = QtCore.Signal()
	test5totest6 = QtCore.Signal()
	test1sub1totest1sub1 = QtCore.Signal()
	test1sub2totest1sub2 = QtCore.Signal()
	test1sub21totest1sub21 = QtCore.Signal()
	test1sub21totest1sub22 = QtCore.Signal()
	test3sub1totest3sub1 = QtCore.Signal()
	test3sub2totest3sub2 = QtCore.Signal()
	test5sub1totest1sub2 = QtCore.Signal()
	test1sub2totest5sub1 = QtCore.Signal()

#-------------------------

	def __init__(self, mprx):
		super(GenericWorker, self).__init__()

		self.jointmotor_proxy = mprx["JointMotorProxy"]

#State Machine
		self.Machine_testcpp= QtCore.QStateMachine()
		self.test2 = QtCore.QState(self.Machine_testcpp)
		self.test4 = QtCore.QState(self.Machine_testcpp)
		self.test5 = QtCore.QState(self.Machine_testcpp)

		self.test6 = QtCore.QFinalState(self.Machine_testcpp)

		self.test3 = QtCore.QState(QtCore.QState.ParallelStates, self.Machine_testcpp)
		self.test1 = QtCore.QState(QtCore.QState.ParallelStates,self.Machine_testcpp)

		self.test1sub1 = QtCore.QState(self.test1)
		self.test1sub2 = QtCore.QState(self.test1)

		self.test1sub21 = QtCore.QState(self.test1sub2)

		self.test1sub22 = QtCore.QFinalState(self.test1sub2)

		self.test3sub1 = QtCore.QState(self.test3)
		self.test3sub2 = QtCore.QState(self.test3)
		self.test3sub3 = QtCore.QState(self.test3)

		self.test5sub1 = QtCore.QState(self.test5)
		self.test5sub2 = QtCore.QState(self.test5)

#------------------
#Initialization State machine
		self.test1.addTransition(self.test1totest1, self.test1)
		self.test1.addTransition(self.test1totest2, self.test2)
		self.test2.addTransition(self.test2totest3, self.test3)
		self.test2.addTransition(self.test2totest5, self.test5)
		self.test2.addTransition(self.test2totest6, self.test6)
		self.test3.addTransition(self.test3totest3, self.test3)
		self.test3.addTransition(self.test3totest4, self.test4)
		self.test4.addTransition(self.test4totest5, self.test5)
		self.test5.addTransition(self.test5totest6, self.test6)
		self.test1sub1.addTransition(self.test1sub1totest1sub1, self.test1sub1)
		self.test1sub2.addTransition(self.test1sub2totest1sub2, self.test1sub2)
		self.test1sub21.addTransition(self.test1sub21totest1sub21, self.test1sub21)
		self.test1sub21.addTransition(self.test1sub21totest1sub22, self.test1sub22)
		self.test3sub1.addTransition(self.test3sub1totest3sub1, self.test3sub1)
		self.test3sub2.addTransition(self.test3sub2totest3sub2, self.test3sub2)
		self.test5sub1.addTransition(self.test5sub1totest1sub2, self.test1sub2)
		self.test1sub2.addTransition(self.test1sub2totest5sub1, self.test5sub1)

		self.test2.entered.connect(self.fun_test2)
		self.test3.entered.connect(self.fun_test3)
		self.test4.entered.connect(self.fun_test4)
		self.test5.entered.connect(self.fun_test5)
		self.test1.entered.connect(self.fun_test1)
		self.test6.entered.connect(self.fun_test6)
		self.test1sub1.entered.connect(self.fun_test1sub1)
		self.test1sub2.entered.connect(self.fun_test1sub2)
		self.test1sub21.entered.connect(self.fun_test1sub21)
		self.test1sub22.entered.connect(self.fun_test1sub22)
		self.test3sub1.entered.connect(self.fun_test3sub1)
		self.test3sub2.entered.connect(self.fun_test3sub2)
		self.test3sub3.entered.connect(self.fun_test3sub3)
		self.test5sub1.entered.connect(self.fun_test5sub1)
		self.test5sub2.entered.connect(self.fun_test5sub2)

		self.Machine_testcpp.setInitialState(self.test1)
		self.test1sub2.setInitialState(self.test1sub21)

#------------------

		self.mutex = QtCore.QMutex(QtCore.QMutex.Recursive)
		self.Period = 30
		self.timer = QtCore.QTimer(self)

#Slots funtion State Machine
	@QtCore.Slot()
	def fun_test2(self):
		print "Error: lack fun_test2 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test3(self):
		print "Error: lack fun_test3 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test4(self):
		print "Error: lack fun_test4 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test5(self):
		print "Error: lack fun_test5 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test1(self):
		print "Error: lack fun_test1 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test6(self):
		print "Error: lack fun_test6 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test1sub1(self):
		print "Error: lack fun_test1sub1 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test1sub2(self):
		print "Error: lack fun_test1sub2 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test1sub21(self):
		print "Error: lack fun_test1sub21 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test1sub22(self):
		print "Error: lack fun_test1sub22 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test3sub1(self):
		print "Error: lack fun_test3sub1 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test3sub2(self):
		print "Error: lack fun_test3sub2 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test3sub3(self):
		print "Error: lack fun_test3sub3 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test5sub1(self):
		print "Error: lack fun_test5sub1 in Specificworker"
		sys.exit(-1)

	@QtCore.Slot()
	def fun_test5sub2(self):
		print "Error: lack fun_test5sub2 in Specificworker"
		sys.exit(-1)

#-------------------------
	@QtCore.Slot()
	def killYourSelf(self):
		rDebug("Killing myself")
		self.kill.emit()

	# \brief Change compute period
	# @param per Period in ms
	@QtCore.Slot(int)
	def setPeriod(self, p):
		print "Period changed", p
		Period = p
		timer.start(Period)

```



*   **specificworker.py**

Example specificworker.py:



```
#
# Copyright (C) 2016 by YOUR NAME HERE
#
#    This file is part of RoboComp
#
#    RoboComp is free software: you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    RoboComp is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License
#    along with RoboComp.  If not, see <http://www.gnu.org/licenses/>.
#

import sys, os, Ice, traceback, time

from PySide import *
from genericworker import *

ROBOCOMP = ''
try:
	ROBOCOMP = os.environ['ROBOCOMP']
except:
	print '$ROBOCOMP environment variable not set, using the default value /opt/robocomp'
	ROBOCOMP = '/opt/robocomp'
if len(ROBOCOMP)<1:
	print 'genericworker.py: ROBOCOMP environment variable not set! Exiting.'
	sys.exit()

preStr = "-I"+ROBOCOMP+"/interfaces/ --all "+ROBOCOMP+"/interfaces/"
Ice.loadSlice(preStr+"JointMotor.ice")
from RoboCompJointMotor import *

class SpecificWorker(GenericWorker):
	def __init__(self, proxy_map):
		super(SpecificWorker, self).__init__(proxy_map)
		self.Period = 2000
		self.timer.start(self.Period)
		self.Machine_testcpp.start()

	def setParams(self, params):
		#try:
		#	par = params["InnerModelPath"]
		#	innermodel_path=par.value
		#	innermodel = InnerModel(innermodel_path)
		#except:
		#	traceback.print_exc()
		#	print "Error reading config params"
		return True
#Slots funtion State Machine
	#
	# fun_test2
	#
	@QtCore.Slot()
	def fun_test2(self):
		pass

	#
	# fun_test3
	#
	@QtCore.Slot()
	def fun_test3(self):
		pass

	#
	# fun_test4
	#
	@QtCore.Slot()
	def fun_test4(self):
		pass

	#
	# fun_test5
	#
	@QtCore.Slot()
	def fun_test5(self):
		pass

	#
	# fun_test1
	#
	@QtCore.Slot()
	def fun_test1(self):
		pass

	#
	# fun_test6	
	#
	@QtCore.Slot()
	def fun_test6(self)
		pass

	#
	# fun_test1sub1
	#
	@QtCore.Slot()
	def fun_test1sub1(self):
		pass

	#
	# fun_test1sub2
	#
	@QtCore.Slot()
	def fun_test1sub2(self):
		pass

	#
	# fun_test1sub21
	#
	@QtCore.Slot()
	def fun_test1sub21(self):
		pass

	#
	# fun_test1sub22
	#
	@QtCore.Slot()
	def fun_test1sub22(self):
		pass

	#
	# fun_test3sub1
	#
	@QtCore.Slot()
	def fun_test3sub1(self):
		pass

	#
	# fun_test3sub2
	#
	@QtCore.Slot()
	def fun_test3sub2(self):
		pass

	#
	# fun_test3sub3
	#
	@QtCore.Slot()
	def fun_test3sub3(self):
		pass

	#
	# fun_test5sub1
	#
	@QtCore.Slot()
	def fun_test5sub1(self):
		pass

	#
	# fun_test5sub2
	#
	@QtCore.Slot()
	def fun_test5sub2(self):
		pass

```



# Emit Signal

For realice a transition between states we has emit the corresponding signal. The signal is emit as follows:



```
self.statesrctostatedst.emit

```


[Older](/webrobocomp.github.io/page2)
