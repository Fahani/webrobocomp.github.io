_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *


# [Build tools](/website/2015/06/26/nithin6/)

26 Jun 2015

###rc_init_ws This will initialize a Robocomp workspace in the current/specified directory.


```
rc_init_ws [path]

```


###rcbuild When invoked form workspace without any arguments if not inside source path, it will build all the non-ignored components inside the workspace, if inside any component source directory it will build only that component. But if a component is specified it will build it.



```
 rcbuild [-h] [-i | --doc | --installdoc] [component]

```


The `doc` will generate documentation, `installdoc` will install the docs to install path, `install` will build and install the components. currently you can only generate docs for one component at a time.

###rccomp This dosent have much functions as of now. `rccomp list` will list all the components.

###rced when invoked as component-name file-name. it will open the the file in the component. if multiple files with same name exists, it will give choices and will ask you to choose one. It uses the editor specified in $EDITOR by default, if not present it will use `vim`.


```
rced [-h] component file

```


###rcrun Using rcrun you can start, stop or force stop any component from anywhere. You can also start a component in debug mode, given you have the required _config file_ in the _etc_ directory. If you have specified a config file then rcrun will use it to start the component. By default rcrun will use the `config` config file in `etc` directory, if not found it will search for `generic_config` if not found it will use any of config files present.If the debug flag is set, it will search for a config file that ends with _.debug_.


```
rcrun [-h] [-s START |-st STOP | -fst FSTOP] [-d | -cf CFILE | -c CONFIG] [-is] [component]

```


###rccd Using this you can cd into the component directory given the component name.


```
rccd component

```


* * *

Nithin Murali


# [_GSoC,_ Till Now..., (Before Midterms)](/website/2015/06/25/rajath1/)

25 Jun 2015

**Summary:** Built a website for robocomp using jekyll.

**Progress:** Took the task of building a website for documenting the open source project _RoboComp_ for the first 4 weeks of Google Summer of Code 2015\. The website should be able to segregate the posts into categories, Make it easy for users to post content and most importantly have a proper flow among the posts so that a new users will find it easy to learn the framework.

Started building the website by using the [dbyll theme](https://github.com/dbtek/dbyll). Messed around with the code a bit and had the website up for robocomp. Website [link](https://rajathkumar.github.io/robocomp). After a few iterations the website was all good. While exploring on the same topic stumbled upon [Type theme](https://rohanchandra.github.io/project/type/). Which was a jekyll based them more clean and elegant than the first one. Ported the entire website to the type theme and currently have shifted the [website](http://robocomp.github.io/website/) to the [robocomp organization on github](https://github.com/robocomp). Currently have tweaked the website based on the suggetsions recieved by the mentors.

Version-1 : https://github.com/robocomp/website/tree/version-1

**Future:** Will implement automatic segregation of posts based on categories the user mention. Extend the features of the website and add analytics, comments for blog posts etc. Give the website its final iterations based on the suggestions recieved.

* * *

Rajath Kumar M.P


# [New build system and workspace model in Robocomp

#2

](/website/2015/06/25/nithin5/)

25 Jun 2015

For managing these components we would need different utilities. So i started compiling a list of different utilities keeping a reference to other frameworks. Finally i have decide on my list

*   rc_init_ws
*   rcbuild
*   rced
*   rccd
*   rcrun
*   rccomp

For more information about the utilities see the [tutorial](http://robocomp.github.io/website/2015/06/26/nithin6.html) on build utilities. All the utilities are implemented using python except `rccd` which is implemented as a shell function, As a subprocess cant affect its parents environment. So i was thinking, as anyway we will need to source a bash script, we could move the exporting of the environment variables also into that script.

##Future work

One useful feature that needs to be implemented is auto complete for the arguments. It would be a very useful feature as we don’t need need to know the exact component name, etc. Also some work on the manifest.xml has to be done. It was planned to contain basic info on packages like name, maintainer, dependencies etc. I didn’t do it right now because i was not really sure about the component dependencies, i will need to discuss about it a bit.

* * *

Nithin Murali



# [_GSoC,_ 2015 ideas](/website/2015/06/22/gsoc15/)

22 Jun 2015

1.- **RoboComp tutorial, social management and documentation**: RoboComp’ sources has been ported to GitHub and we are building a new documentation repository there. We are using GitHub markdown language (GFM) write new docs and turorials. We want to build a set of short tutorials that guide the new users along several interconnected topics, such as component oriented programming, robotics, computer vision, robotics software modules integrating heterogeneous sources, cognitive architectures and testing and validating, all from inside RoboComp. These new tutorials will be developed using RoboComp’s robotics simulator, RCIS, so interactive examples can be created and used in the explanations. This package also includes work on automated installation scripts using CMake. Generic knowledge of linux systems, website and wiki administration is needed. This is a key task for our project as it would bring more attention to it as it will open the development to new people interested in the field.

Required student level: intermediate programming and systems administration.

2.- **Computer vision components and libraries management**: RoboComp is being used to build a new cognitive architecture called RoboCog. Among the different modules already in progress, the object detection module is crucially important. We are pursuing an efficient 2D/3D vision pipeline that, working with the robot body control module, is able to localize, recognize, fit a pre-existing model and track a series of daily objects that the robot might encounter. Grasping would be one target of this pipeline, or even a means to complete recognition. There are currently many components implementing computer vision algorithms and intensive work done in pipeline construction. The key tasks on this idea would be to collaborate in the creation of high level tools to organize, document and test different pipelines for an specific task. These tools will be designed in collaboration with the mentors and tested in RoboComp’s RCIS simulator and real robots.

Required student level: intermediate computer vision knowledge, C++ programming, basic CMake knowledge

3.- **RoboComp Building and deployment system design**: Current CMake building system in RoboComp is limited only to the core libraries, the RCIS simulator and some additional tools. A very useful task would be to come up with a more complex CMake structure that could build the entire system, including all finished components, without breaking current dependencies. Also, a few scripts will have to be built to compile individual components, run tests, search and inspect the source tree efficiently, check dependencies and documentation requirements. This code needs also to take into account the dependencies between components that can be stored in xml-like files -i,e, manifestos- within the component itself.

Required student level: intermediate CMake knowledge, programming in C++, basic knowledge of shell scripting

4.- **Deployment generator and run-time monitoring**: When creating a specific robot architecture, many components have to be brought into a common deployment environment. Each component has its runtime configuration and network parameters that have to be declared in a common deployment file, from where the complete system can be brought to life. This task proposes the design of a domain specific language to facilitate the creation of shellscript deployment files that are syntactically and semantically correct. Once a net of RoboComp components is up and running, additional tools are needed to monitorize their execution through an existing default interface called CommonBehavior.This tool will use the DSL as input and will show a graphical representation of the running system. It will be written in Python and will extend important efforts already made in this direction.

Required student level: intermediate programming with Python and introductory knowledge of formal languages

For any questions, proposals, or comments please contact RoboComp’s org admin at: marcogunex.es



# [Robocomp Workspace Model](/website/2015/06/20/nithin4/)

20 Jun 2015

The Robocomp workspace is for those who are developing components rather than the framework itself. The main advantage of having a workspace is that it will make the work-flow much easier. Workspace basically organizes the development. For example, currently for building or running a component you have to go to its directory, create a build directory and then use cmake, while by using workspace and build tools you could achieve the same in a single command.

I have started the workspace model design keeping in mind the following points.

*   you should be able to build all components at once, if necessary and also separately
*   the source tree should be kept clean
*   It should scalable and also existing components should be easily moved in
*   dependencies should be easily handled

Referring to other similar workspace models i came up with the following model.

###The recommended layout for development is as follows:

![ Robocomp workspace model](https://raw.githubusercontent.com/robocomp/website/gh-pages/img/workspace_model.png "Robocomp workspace model")

##Elements of workspace

###Workspace The workspace is the folder inside which you are going to be actively developing components. Keeping things in a folder with connected development helps keep separation of development models. In simple words, a workspace can be thought of as a group different components, for example Robocomp has some default components, you may as well create some components so in this case you Robocomp’s components can be in a workspace while your components in another. The config file in ~/.config/RoboComp/rc_worksapce.config maintains a list of all the registered workspaces.

###Source space The source space (a folder inside workspace) contains the source code of all the components in the workspace or this is where you will be developing. The source space is the folder where build tools will look for components. This folder is easily identified as it is where the toplevel.cmake is linked Robocomp installed folder and the name `src`. Each component should be in a direct subdirectory. If the directory contains a file named _IGNORE_COMP_ the component will be ignored while building the workspace.

###Build Space The build space is the folder in which cmake is invoked and generates artifacts such as the CMakeCache. This need not be a direct sub directory of workspace, it can be any where. This is basically an _build_ directory of all the components.

###Development Space The development space is where build system generates the binaries and config files which are executable before installation.It will have a septate directory for each components. Each component directory contains a folder `bin` which has the build binaries and a `etc` directory which contain config files. This should be a direct subdirectory of workspace. Currently the `devel space is merged with the source space` as you can seen in the layout graph.

###Install Space This is the default directory in to which the components in current workspace will get installed along with generated docs. This directory contains a file named _.rc_install_ which marks this as an install space. Please note that the robocomp install path _/opt/robocomp_ is also an install space by default.

* * *

Nithin Murali

[Older](/webrobocomp.github.io/page10) [Newer](/webrobocomp.github.io/page8)
