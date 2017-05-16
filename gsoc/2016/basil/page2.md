# Review of current Rcmanager

30 May 2016

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