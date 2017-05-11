<div class="content container">_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *

<div class="posts">

<div class="post">

# [Write a post for robocomp, A step by step guide.](/website/2015/05/23/post_on_webpage/)

<span class="post-date">23 May 2015</span>

In this tutorial you will be learning about writing a post for robocomp. I assume that you are already familiar with contributing via Github if you are not then you can follow [this article](http://rajathkumarmp.github.io/robocomp/tutorial/2015/05/23/contribute/).

switch to `gh-pages` branch You can do this via github client or on the command line by navigating to the directory and executing the command

<div class="highlighter-rouge">

```
`git checkout gh-pages`

```

</div>

After checking out to the github pages branch in your navigate to the `_posts` directory. Here you will find all the posts.

To write a new post. Create a new file and save it as `XYZ.md`

Note that you will be using Github markdown language.

Once you save the file as `XYZ.md`. It will be saved as draft and not published on to the website.

At the header of every article/post you write. Always add this

<div class="highlighter-rouge">

```
---
layout:
title: 
---

```

</div>

Layout can be `post`, `page` or `default`. Always set the layout as `post`. The title is the title of the post. Categories and Tags should be set accoridingly whichever is applicable. This is helpful in navigating or finding posts on same topic. Description is a short explanation or gist of the entire post.

A sample header looks like this.

<div class="highlighter-rouge">

```
---
layout: post
title: Write a post for robocomp, A step by step guide.
---

```

</div>

After adding the header you can proceed writing the post by using Github Markdown language. Now for the most important step. To publish the post or to change the post from draft to final you will have to rename the file to

<div class="highlighter-rouge">

```
`YYYY-MM-DD-XYZ.md`

```

</div>

Commit and push it to the repo, you would have successfully published a post on to the robocomp’s website.

</div>

<div class="post">

# [Creating a new component with eclipse based RoboComp's DSLEditor](/website/2015/05/23/component_creation_with_DSLEditor/)

<span class="post-date">23 May 2015</span>

We will create now a new component that will connect to the RCIS simulator and run a simple controller for the robot, using the laser data. First we need to install the DSLEditor software that is runtime Eclipse application.

Create another terminal in Yakuake and type:

<div class="highlighter-rouge">

```
cd ~/robocomp/tools
python fetch_DSLEditor.py

```

</div>

Select 32 or 64 bits according to your current linux installation. After a little while the DSLEditor will be installed under the _robocompDSL_ directory:

<div class="highlighter-rouge">

```
cd roboCompDSL/DSLEditor
./DSLEditor

```

</div>

Check that you have a _RoboComp_ tab in the upper bar of the DSLEditor window and that the _robocomp_ directory appears in the Project Explorer (left panel). If it does not, right click inside the _Project Explorer_ panel and select _import_. Then select _General_ and then _Existing Projects into Workspace_. Then select your _robocomp_ directory and push _Finish_.

Now we need to bring up some handy tabs in the lower pane. Select the _Window_ tab in the upper bar, then _Show View_, then _Other_ and again _Other_. Select now _Interfaces_ and double-click on it. Go back to the main window.

Now, in the left panel, unfold the _robocomp_ directory down to _robocomp/components/_ and then click on it with the right button. Select _New Folder_ and enter _mycomponents_ in the folder name. Do it again to create a new folder inside _mycomponents_ named _myfirstcomp_. Select _myfirstcomp_ and then click on the _RoboComp_ tab in the upper bar of the main window. Select _Create CDSL file_ and fill the requested name with _MyFirstComp.cdsl_

The new file will open inside a syntax-sensitive editor in the central panel. Ctrl-space gives you syntactically correct options. You can see the skeleton of a new empty component. Look for the tab _Interfaces_ in the lower bar and select _DifferentialRobot.idsl_. Click on the green cross at the right of the bar to include it and accept when prompted in a pop-up window. You will see something like:

<div class="highlighter-rouge">

```
import "/robocomp/interfaces/IDSLs/DifferentialRobot.idsl";
Component PFLocalizerComp{
    Communications{
        };
        language Cpp;
};

```

</div>

Repeat the same steps to include _Laser.idsl_ and then add a _requires_ statement inside de _Communications_ section. The file now should look like this:

<div class="highlighter-rouge">

```
import "/robocomp/interfaces/IDSLs/DifferentialRobot.idsl";
import "/robocomp/interfaces/IDSLs/Laser.idsl";
Component MyFirstComp{
    Communications{
        requires DifferentialRobot, Laser;
    };
language Cpp;
};

```

</div>

Save the file and click in the upper bar on the _RoboComp_ tab. Select _Generate Code_. After a little while the new source tree for your _MyFirstComp_ component will be created. You can go back now to Yakuake and create a new tab to compile it. Then:

<div class="highlighter-rouge">

```
cd ~/robocomp/components/mycomponents/myfirstcomp
cmake .
make
bin/myfirstcomp --Ice.Config=etc/generic_config

```

</div>

and there it is! your component is running.

What! Dissapointed? Yeah, I know it does nothing, but it runs and it is yours! Now let’s do some real programming.

Stop the component with Ctrl Z and then type:

<div class="highlighter-rouge">

```
killall -9 myfirstcomp

```

</div>

Now start your favorite IDE. KDevelop will do it just fine and you have it already installed. Open it in another tab, from Ubuntu menu or with Alt-F2\. Then:

<div class="highlighter-rouge">

```
Click the *Project* tab in the upper bar
Select *Open/Import Project*
Navigate to ~/robocomp/components/mycomponents/myfirstcomp
Select *Makefile* and open the project

```

</div>

In the _Project_ panel to the left of the screen, navigate to _src_ and there select _specificworker.cpp_ and open it. Open also _specificworker.h_

Now replace the empty _void compute()_ method with this compact version of the classic AVOID-FORWARD-STOP architecture proposed by R. Brooks in the late 80’s:

<div class="highlighter-rouge">

```
void SpecificWorker::compute( )
{
    static	float rot = 0.1f;			// rads/sec
    static float adv = 100.f;			// mm/sec
    static float turnSwitch = 1;
    const float advIncLow = 0.8;		// mm/sec
    const float advIncHigh = 2.f;		// mm/sec
    const float rotInc = 0.25;			// rads/sec
    const float rotMax = 0.4;			// rads/sec
    const float advMax = 200;			// milimetres/sec
    const float distThreshold = 500; 	// milimetres
try
{
    RoboCompLaser::TLaserData ldata = laser_proxy->getLaserData();
    std::sort( ldata.begin(), ldata.end(), [](RoboCompLaser::TData a, RoboCompLaser::TData b){ return     a.dist < b.dist; }) ;
    if( ldata.front().dist < distThreshold) 
    {
        adv = adv * advIncLow; 
        rot = rot + turnSwitch * rotInc;
        if( rot < -rotMax) rot = -rotMax;
        if( rot > rotMax) rot = rotMax;
        differentialrobot_proxy->setSpeedBase(adv, rot);
    }
    else
    {
        adv = adv * advIncHigh; 
        if( adv > advMax) adv = advMax;
        rot = 0.f;
        differentialrobot_proxy->setSpeedBase(adv, 0.f);		
        turnSwitch = -turnSwitch;
    }	
}
catch(const Ice::Exception &ex)
{
    std::cout << ex << std::endl;
}
}

```

</div>

To compile the fancy version of _std::sort_ you will have to first add this line at the end of the file _CMakeListsSpecific.txt_ located in the same _src_ directory:

<div class="highlighter-rouge">

```
ADD_DEFINITIONS( -std=c++11 )

```

</div>

and then type:

<div class="highlighter-rouge">

```
cmake .
make

```

</div>

Hereafter, Press F8 in KDevelop to compile and link. Then, go to Yakuake and restart the component.

Let us take InnerModel _simpleworld.xml_ as an example. Open a new tab in Yakuake and execute

<div class="highlighter-rouge">

```
cd robocomp/files/innermodel
rcis simpleworld.xml

```

</div>

Now you should see 2 windows. Now in Yakuake go back to tab where you had compiled _myfirstcomp_ and run

<div class="highlighter-rouge">

```
bin/myfirstcomp --Ice.Config=etc/generic_config

```

</div>

You should see the robot maneouvring aroung the box. Now is when Robotics begin! Try to modify the code to let the robot go pass the blocking boxes.

</div>

<div class="post">

# [aprilTagsComp, Tutorial to simulate virtual apriltags](/website/2015/05/23/apritagstutorial/)

<span class="post-date">23 May 2015</span>

If you haven’t already, Then do read about aprilTagsComp [here](apriltags.md) for better understanding. In this tutorial you will learn the actual functionality of apriltags.

First make sure you have installed apriltags. Please follow the steps that is given in _INSTALL_APRILTAGS_LIB.TXT_. Then move to the apriltagscomp folder

<div class="highlighter-rouge">

```
cd ~/robocomp/components/robocomp-robolab/components/apriltagsComp

```

</div>

Compile by executing

<div class="highlighter-rouge">

```
cmake.
make

```

</div>

Now that you have compiled the component and have the binary generated. Open a new tab in yakuake or terminal and execute

<div class="highlighter-rouge">

```
cd robocomp/files/innermodel
rcis simpleworld.xml

```

</div>

Here we will considering _simpleworld.xml_ as an example since it has virtual apriltags and a robot with a camera is present for simulation. After execution you should now see two windows, One showing the robot camera’s view pointing at one of the apriltags and the other with the site map showing the robot and two virtual apriltags.

Now go back to the terminal where you had compiled the apriltagsComp and execute

<div class="highlighter-rouge">

```
bin/apriltagscomp --Ice.Config=etc/generic_config

```

</div>

You should now see the following output.

<div class="highlighter-rouge">

```
user@username:~/robocomp/components/robocomp-robolab/components/apriltagsComp$ bin/apriltagscomp --Ice.Config=etc/generic_config
[/home/username/robocomp/classes/rapplication/rapplication.cpp]: Loading [camera:tcp -h localhost -p 10001] proxy at 'CameraProxy'...
18:20:19:421::Info::apriltagscomp.cpp::139::/home/username/robocomp/components/robocomp-robolab/components/apriltagsComp/src/apriltagscomp.cpp::run::CameraProxy initialized Ok!
[/home/username/robocomp/classes/rapplication/rapplication.cpp]: Loading [rgbd:tcp -h localhost -p 10096] proxy at 'RGBDProxy'...
18:20:19:421::Info::apriltagscomp.cpp::150::/home/username/robocomp/components/robocomp-robolab/components/apriltagsComp/src/apriltagscomp.cpp::run::RGBDProxy initialized Ok!
[/home/username/robocomp/classes/rapplication/rapplication.cpp]: Loading [rgbdbus:tcp -h localhost -p 10239] proxy at 'RGBDBusProxy'...
18:20:19:421::Info::apriltagscomp.cpp::161::/home/username/robocomp/components/robocomp-robolab/components/apriltagsComp/src/apriltagscomp.cpp::run::RGBDBusProxy initialized Ok!
18:20:19:423::Debug::genericworker.cpp::53::/home/username/robocomp/components/robocomp-robolab/components/apriltagsComp/src/genericworker.cpp::setPeriod::Period changed100
18:20:19:423::Info::specificmonitor.cpp::56::/home/username/robocomp/components/robocomp-robolab/components/apriltagsComp/src/specificmonitor.cpp::initialize::Starting monitor ...
InputInterface RGBD
AprilTagsFamily tagCodes36h11
ID:0-10 0.17
ID:11-20 0.17
ID:21-30 0.17
InnerModelPath /home/robocomp/robocomp/files/innermodel/simpleworld.xml
RoboCompAprilTagsComp::AprilTagsComp started
InnerModelReader: reading /home/robocomp/robocomp/files/innermodel/simpleworld.xml
InnerModelRGBD: 0.000000 {10096}
"/home/robocomp/robocomp/files/innermodel/simpleworld.xml"   "rgbd" 
FOCAL LENGHT: 480 
  6.45862 fps
  7.17213 fps
  7.09036 fps
  6.94859 fps
  7.09561 fps
  7.01433 fps
  7.09569 fps
  7.02332 fps
  7.24122 fps
  7.17631 fps
  6.85862 fps
  7.00415 fps
  7.14883 fps
  6.92038 fps
  7.03233 fps

```

</div>

</div>

<div class="post">

# [aprilTagsComp, Wrapping E. Olson's AprilTags in RoboComp](/website/2015/05/23/apriltags/)

<span class="post-date">23 May 2015</span>

AprilTags is an augmented reality tag system developed by E. Olson at the U. of Michigan, USA. A complete explanation and related papers can be found [here](http://april.eecs.umich.edu/wiki/index.php/AprilTags). There is a C++ version written by Michael Kaes [here](http://people.csail.mit.edu/kaess/apriltags/) which is the one we use.

April tags are AR tags designed to be easily detected by (robot) cameras. Understand them as a visual fiducial (artificial features) system that uses a 2D bar code style “tag”, allowing full 6 DOF localization of features from a single image. It is designed to encode smaller data (between 4 and 12 bits) and also these tags can be detected by the camera even at odd conditions. When the tag is seen by the camera, the algorithm computes the tag’s complete pose defining its own reference system relative to the camera (i.e Location of the tag is known with high accuracy). This reference system is defined as follows: If we look perpendicularly to a non rotated tag, The Z+ axis comes out towards us from the center of the tag plane, The X+ axis points leftwards and the Y+ axis points upwards (a left-hand reference system). The values computed by _apriltagsComp_ are the translation vector from the camera to the center of the tag’s reference system, and the three Euler angles that encode the relative orientation of the tag’s reference system wrt to the camera reference system.

The _AprilTags.cdsl_ file specifies how _apriltagsComp_ has been generated and how it can be re-generated:

<div class="highlighter-rouge">

```
import "/robocomp/interfaces/IDSLs/GetAprilTags.idsl";
import "/robocomp/interfaces/IDSLs/AprilTags.idsl";
import "/robocomp/interfaces/IDSLs/RGBD.idsl";
import "/robocomp/interfaces/IDSLs/RGBDBus.idsl";
import "/robocomp/interfaces/IDSLs/Camera.idsl";
Component AprilTagsComp{
    Communications{
            requires Camera, RGBDBus, RGBD;
            publishes AprilTags;
            implements GetAprilTags;
    };
    language Cpp;
};

```

</div>

This files tells us that the component requires -will be calling- three RoboComp interfaces: Camera, RGBDBus y RGBD, which are normal and depth camera’s interfaces written in RoboComp’s IDSL language. You can find those files in _~/robocomp/interfaces/IDSLs_. Also, the component will publish the data defined in the _AprilTags_ interface and will implement the _GetAprilTags_ interface. This means that using images provided by a component implementing the camera or RGBD interfaces, it will try to detect any tags in them and compute their 6D pose. Finally, it will publish a vector with all the tags id’s and poses to the Ice’s STORM broker, and also it will attend any direct requests (remote procedure calls) received from other components through the _GetAprilTags_ interface. So it is a rather serviceable and handy component!

To access **apriltagsComp** you need to install from _http://github.org/robocomp_ the repository named _robocomp-robolab_.

<div class="highlighter-rouge">

```
cd ~/robocomp/components
git clone https://github.com/robocomp/robocomp-robolab.git

```

</div>

Once downloaded, _apriltagsComp_ can be found in:

<div class="highlighter-rouge">

```
~/robocomp/components/robocomp-robolab/components/apriltagsComp

```

</div>

First, read the _INSTALL_APRILTAGS_LIB.TXT_ file and follow instructions thereby. Once the library has been installed in /usr/local, we can proceed to compile the component:

<div class="highlighter-rouge">

```
cd ~/robocomp/components/robocomp-robolab/components/apriltagsComp
cmake .
make

```

</div>

We should have a binary now:

<div class="highlighter-rouge">

```
~/robocomp/components/robocomp-robolab/components/apriltagsComp/bin/apriltagscomp

```

</div>

##Configuration parameters As any other component, _apriltagsComp_ needs a _config_ file to start. In

<div class="highlighter-rouge">

```
~/robocomp/components/robocomp-robolab/components/apriltagsComp/etc/generic_config

```

</div>

you can find an example of a configuration file. We can find there the following lines:

<div class="highlighter-rouge">

```
GetAprilTagsComp.Endpoints=tcp -p 12210                     //Port where GetAprilTags iface is served
CommonBehavior.Endpoints=tcp -p 11258                       //Not of use for the user now
CameraProxy = camera:tcp -h localhost -p 10001              //Port where a camera is located
RGBDProxy = rgbd:tcp -h localhost -p 10096                  //Port where a RGBD camera is located
RGBDBusProxy = rgbdbus:tcp -h localhost -p 10239            //Port where a bus of RGBDs is located
AprilTagsProxy = apriltags:tcp -h localhost -p 10261        //Not of use for the user
TopicManager.Proxy=IceStorm/TopicManager:default -p 9999    //Port where STROM broker is located
InnerModelPath=/home/robocomp/robocomp/files/innermodel/simpleworld.xml

InputInterface = RGBD                                       //Current input iface to be used
AprilTagsFamily = tagCodes36h11                             //Tags family. See AprilTags paper
AprilTagsSize = 0.17                                        //Tag default real size in meters
ID:0-10 = 0.17   #tag size in meters                        //Tags numbers 1-10 real size in meters
ID:11-20 = 0.17   #tag size in meters                       //Tags numbers 11-20 real size in meters
ID:21-30 = 0.17   #tag size in meters                       //Tags numbers 21-30 real size in meters

```

</div>

AprilTagsFamily is a set of tags, There are different families like 36h10,25h9,16h5 however _tagCodes36h11_ is recommended. Each tag has an ID that is printed inside the surrounding square using Hamming code. Instructions to print tags and other tag families can be found [here](http://april.eecs.umich.edu/wiki/index.php/AprilTags). The algorithm needs the real size of the tag to estimate its position and orientation in space. We can give the component tags of different sizes, As long as they correspond to different ranges of IDs, as specified in the configuration file above.

##Starting the component To start the component we need a real camera connected to the cameraV4lComp component or the RCIS simulator started with a file that includes virtual tags, such as _simpleworld.xml_, Tutorial can be found [here](virtualapriltagstutorial.md). Once RCIS is up and running, It will provide the RGBD.idsl interface (not Camera.idsl for now) at port 10096, which is what the configuration file states. To avoid changing the _generic_config_ file in the repository, We can copy it to the component’s home directory, So changes will remain untouched by future git pulls:

<div class="highlighter-rouge">

```
cp ~/robocomp/components/robocomp-robolab/components/apriltagsComp
cp /etc/generic_config config

```

</div>

So, to begin we type:

<div class="highlighter-rouge">

```
cd ~/robocomp/components/robocomp-robolab/components/apriltagsComp
bin/apriltagscomp --Ice.Config=config

```

</div>

If the robot’s camera is pointing towards one of the tags, You should see in the terminal lines showing the ID and pose of each visible tag.

</div>

<div class="post">

# [Tutorials Directory](/website/2015/05/23/README/)

<span class="post-date">23 May 2015</span>

[A Brief introduction to Components](components.md)

[Creation of a new component using RoboComp’s Eclipse based DSLEditor](component_creation_with_DSLEditor.md)

[Maintaining your own repository of components](using_github.md)

[How to contribute to RoboComp using the GitHub branching mechanism](contribute/contribute.md)

[Using the new **robocompdsl** component generation command line tool](robocompdsl.md)

[The E. Olson’s AprilTags component](https://github.com/robocomp/robocomp-robolab/blob/master/components/apriltagsComp/README.md)

[Creating a Python component using **robocompdsl** that subscribes to _apriltagsComp_](robocompdsl_python.md)

[InnerModel, RoboComp’s internal representation of reality]

[RoboComp’s robots: Ursus, Loki and the others]

[**robocomp-robolab** components]

[The BodyInverseKinematics (BIK) component]

[The Navigation (TRAJ) component]

[RoboCog, a Cognitive Architecture built with RoboComp]

</div>

</div>

<div class="pagination">[Older](/webrobocomp.github.io/page12) [Newer](/webrobocomp.github.io/page10)</div>

</div>