<div class="content container">_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *

<div class="posts">

<div class="post">

# [Introduction](/website/2016/06/02/HaritWeek1/)
 
<span class="post-date">02 Jun 2016</span>

# About myself

Hi all, my name is Harit Pandya.I am 4th year PhD student at Robotic Research Center, IIIT Hyderabad, India. My thesis involves implementing Visual Servoing across object instances for a category, path planning of robotic arm. I am also involved in environment perception for autonomous car. Previously, I worked with Ericsson Global Ltd. Mumbai for 1.5 years, where I was responsible for planning and implementation of 3G(WCDMA) and 4G(LTE) networks for priority customers like Vodafone, Idea and Korea telecom. Please visit [my webpage](http://robotics.iiit.ac.in/people/harit.pandya/) for more details.

# Short description of the project

In this project, I consider the problem of object detection using Convolutional Neural Network (CNN) for Robocomp library. The task comprises of two separate components: Firstly, I will integrate current Robocomp repository to work with Caffe library for object recognition. Secondly I will interface simulation environment of RoboComp with the detection framework. Given an image the primary task is to detect the location of the object that will be given by a bounding box around the object. That is followed by classification task where the objective is to predict the category of the object. Major challenge is object localization in the image. CNNs’ work pretty well for classification tasks, however for object localization edge and contrast based features are used by state-of-art object detection methods that generate object proposals. Another challenge is working with CAD models, since most of the model are low textured hence finding features is difficult for them.

Following are the deliverables for the project:

1.  Caffe integration and real image classification.
2.  Object localization on real images using object proposals.
3.  Support for training/fine tuning network using caffe.
4.  Integration with Robocomp simulation environment and support for CAD models.
5.  Regress testing for both components.
6.  Use cases / samples and documentation for both cases.
7.  CAD models dataset with classification and detection support for each of them.

* * *

Harit Pandya

</div>

<div class="post">

# [Review of current Rcmanager](/website/2016/05/30/BasilWeek2/)

<span class="post-date">30 May 2016</span>

My first milestone is to understand the current code-base. The projected time was 2 weeks.This is the update after one week.

I went through the code of current rcmanager which is written on python.This tool have only limited functionalities.But Later, I came to know that robocomp code was initially hosted on source-forge.It contained an old version of manager in the name of compmanger. I didn’t dig deep into code because right now, we should be planning about which all functionalities should we take from the old manager tools.So spending too much time on old code before coding starts seems useless.

## Current rcmanager written in python

The rcmanager is using a custom made template form to show most of the parts of the UI. we can see that form by opening ui_formanager.py .

The rcmanager tool extensively uses modularity.For example the component classes which takes care of the properties of components in the component tree, is defined in another module named rcmanagerConfig.This module also takes care of reading from the .xml files and writing into them.

rcmanagerEditor.py defines the A QDialog childclass widget which can be called from the rcmanager as well as externally from terminal.It is used to edit the features of components in the component tree,as the name suggest..

Inside the rcmanager, one third of the code is under the class the GraphView which defines how the graph tree should behave.

The Major part of the code is under the class TheThing which defines the mainDialog child class which contain the template form which I mentioned earlier. It also declare the canvas object(Which is an instance of GraphView class I mentioned) which draw the graph and take care of the component tree. It also declare the instances of classes which take care of checking whether the components are up or down while the tool is running(Obviously by multi-threading).

Since this class is parent class of the gui tool(All others become the data members of this class). All the slot function which respond to the signals are defined here. Like Saving to file, Opening new file etc..

## In short

The rcmanager tool written on the python will helps to create a tree of components and control them. Control them in the sense turning them on and off. For controlling the behavior of the components we have to check the corresponding yakuake tab. Its graph tree have a very good animations.We can drag them and it will act like pulling a big floating rope on water. But I don’t find this floating feature that useful considering this tool is supposed to control hundreds of components at a time,and I believe that this feature may require more updating frequency and will take some processing power.

## Old managercomp

This one contain some extra menus like dock, node etc..

The tool have an option to enable docking.But from my previous experience, I think we should avoid the [multiple document interface system](http://www.tutorialspoint.com/pyqt/pyqt_multiple_document_interface.htm) in such tools.Because when we are doing some important works we may accidentally click on one of them and they will get detached from its original position and it becomes a distraction.Its better to have fixed tabs or frames for each parts(like component list, tree) and have [Qsplitter](http://doc.qt.io/qt-4.8/qsplitter.html) widget for adjusting their size.

There are some extra modules like managerNode which I think takes care of nodes in component tree including reading from the .xml file. Then there is 2 managerCompConfig modules which is almost similar to the rcmanagerConfig module in the present tool.

The graph drawing work is completely given to another managerGraph module.What surprise me is that this old tool is already using some functionalities of the GraphicsView framework.which is one of the most important framework I have to include in my project.

This tool also contains a few other modules like mygraphtab, mytreewidget which are most probably used by the importing modules.Since I am not not building upon this same tool but a new one, I decided not to dig deep into these parts right now.We may have to come back and refer these codes once we start making the new component tool.

This tool also contains some sections for Log,attribute and Component list box in the right.

Logging the data seems a cool idea.But as we are planning to embed a console to control the deployed component in our project this won’t be that useful.But still to get a brief about actions(uping of components,notifying when some component went down etc) this should be included in the new tool.

* * *

Basil M Varghese

</div>

<div class="post">

# [week 2 updates](/website/2016/05/29/yashWeek2/)

<span class="post-date">29 May 2016</span>

# conceptualization

I have spent most of my initial time reading about different discovery techniques and implementations. Of all the implementations i have read about ice grid gave a lot of new ideas about the different features that can be implemented in to rcmaster. Also while reading about the Robocomp IDSL, I had a hard time figuring out what are the features of slice which is implemented in IDSL. So for helping other guys looking for the same I wrote an [tutorial](https://github.com/yashsanap/robocomp/blob/master/doc/IDSL.md) about IDSL.

So far I have compiled a list of features that can be implemented in to rcmaster. and also have been experimenting with the robocomp components.

*   **Component registration & lookup** - This is the basic feature of rcmaster. The components can register with the rcmaster and they will be assigned a port. Also any component can query about any other component based of number of criterias.
*   **Component deployment/monitoring** - RCmaster can also launch a set of components at start which can be set in the configuration file.Also when a query comes for a component the rcmaster could launch it if its not running. Also if a component is exits unexpectedly then rcmaster could take some specific actions like restarting that component or notifying any other component etc
*   **Load Balancing/Backup** - RCmaster may allow multiple replica of components for load balancing or backup. It could analyze the load of a component and can distribute the could resolve the future proxies to an low load component
*   **Gui For Master** - This can be achieved by modifying rcmanager to use rcmaster (can remove the tedious config file for rc-manger).
*   **Multiple masetrs** - This is one feature in which some effort and thought is required. we can keep them as backup and keep them in sync, may help in cases of temporary connection loses as it can handle local lookups in local master a master’s can communicate with each other to get remote name lookups. more discussion id needed on this.

Also i am thinking if we need to allow multiple masters in the same machine. This may act as backup or could share the query load

* * *

Yash Sanap

</div>

<div class="post">

# [Introduction](/website/2016/05/21/yashWeek1/)

<span class="post-date">21 May 2016</span>

# INTRODUCTION

Hi ,My name is Yahsh Sanap.I am doing my Btech on Electrical Engineering from Indian Institute of Technology Mumbai.I am a participant on Google Summer of Codes 2016 under the Organization Robocomp.

My Project is writing a new component in RoboComp that will provide a naming and port service. When starting, components will now contact this new service to advertise its name, available interfaces and request a valid port. Other components starting later will be able to query the service for component names and interfaces, and therefore being able to establish their proxy connections in real time, eliminating the need for complex and tedious configuration files.I will also try to explore more features for the componenet by studying various similer projects like Ice Grid, ROS Master etc.

The first few weeks will be dedicated to reading up about the robocomp framework and geeting a deep insight about how things work now. Then i will be reading about simliler projects and take ideas about how their usefull features can be implimented in this project. Once i have a clear idea about ths project and features to impliment i will start by creating a basic component will basic query capabilities. Latter adding new features. The last few weeks are dedicated for testing and complete any documentation left.

I will be working mainly in CDSL, IDSL and python.

* * *

Yash sanap

</div>

<div class="post">

# [Who I am? What will I make?](/website/2016/05/19/ibarbechWeek0/)

<span class="post-date">19 May 2016</span>

Hi! My name is Iván Barbecho Delgado, I’m from Extremadura and I was born on 25.04.1994 and I’m study computer engineering in School of Technology of University of Extremadura. Actually I’m working in a project wich robolab, this consist in making walk a hexapod and put sensors.

I can not start my project, because I have perform exams, but when the exams are finished, I will work on the project thoroughly. For now I will work 1 to 2 hours a day in my project. In June, I will start my project seriously. This is my plan to work.

My project will be divided into two parts, automatic code generation for State Machines once in C++ and other in python. So they can generate a componet with a state machine easily. This is very importar for save time when creating components that using state machines.

I work wich **DSL**(domain-specific language), **RoboComp**, **C++** and **Python.** I will use mainly **Qt State Machine Framework.**

Qt State Machine Framework: http://doc.qt.io/qt-4.8/statemachine-api.html

DSL: https://en.wikipedia.org/wiki/Domain-specific_language

RoboComp’s repository: https://github.com/robocomp

</div>

</div>

<div class="pagination">[Older](/webrobocomp.github.io/page6) [Newer](/webrobocomp.github.io/page4)</div>

</div>