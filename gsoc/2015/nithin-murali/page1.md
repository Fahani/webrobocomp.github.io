# _GSoC,_ Building and deployment system design

23 May 2015

###About Me:

Hi all , I am Nithin Murali and i would like to introduce me a little in this post. I am pursuing my engineering degree on Electrical Engineering from Indian Institute of Technology Bombay. I am working on an Autonomous Underwater Vehicle which we are developing for the RObosub competition. We are developing in ROS framework. That was my first introduction to robotic frameworks. I have read about Robocomop before but a real chance to contribute to this ambitious framework was brought to me by GSOC 2015\. I have Chosen the project _RoboComp Building and deployment system design_.

###About the project

Currently the the build system in Robocomp is not very efficient. It is limited only to the core libraries and some additional tools. So we will have to seprately build all the components one by one. Also as the number of component increases it will be more difficult to manage all of them. So I am planning to come up with an workspace model for Robocomp. It would accompany with various tools which will ease handling of components.

As of now the users have to build robocomp from source for using it. But there may be users who dont want to work on the framework but is only interested in developing new components. For such users it is important that that we should supply an compiled package (preferably debian package). Also package would be more accessible if we could provide an ppa for robocomp. So i am planning to package robocomp and also create a ppa for robocomp.

Currently Robocomp dosent have any tests written nor is it using any testinf framework. So one of my task would be decide on a testing framework/stragery and write tests for existing framework.

* * *

Nithin Murali