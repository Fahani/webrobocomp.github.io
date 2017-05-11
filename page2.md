<div class="content container">_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *

<div class="posts">

<div class="post">

# [Cog and PyParsing](/website/2016/07/14/dgallegosWeek7/)

<span class="post-date">14 Jul 2016</span> 

Let’s go to know how RoboCompDSL works.

## PyParsing

One of the most important modules used in RoboCompDSL is pyparsing. It is a Python module whose latest version to date is 2.0.4, which lets you create grammars in a simple way. Thanks to this, we can create Parsers that are in charge of a grammatical analysis of files that follow a grammatical structure. This module is vital in generating the code as it is used to interpret the DSLs defined in RoboComp.

**A simple example of it would be:**

<div class="highlighter-rouge">

```
from pyparsing import Word, alphas
greet = Word( alphas ) + "," + Word( alphas ) + "!"
hello = "Hello, World!"
print (hello, "->", greet.parseString( hello ))

```

</div>

**Output:**

<div class="highlighter-rouge">

```
$ Hello, World! -> ['Hello', ',', 'World', '!']

```

</div>

This module is used in scripts _parseIDSL.py_ and _parseCDSL.py_ to extract the files CDSL and IDSL necessary information that will be used to generate code routine.

## Cog

Cog is a code generator written in Python that allows you to inject Python code within a file so that, after using the generator, we get a result that depends on the injected code. It can be used with any type of file so it does not limit its use to code generation. The RoboCompDSL tool uses this code generator for creating and modifying components of a quick and easy basis of information obtained from CDSL and IDSL files. This tool is able to generate code routine that hindered the creation of complex systems components.

**An example of use is shown in a file either:**

<div class="highlighter-rouge">

```
Static part of our file
[[[cog
import cog
fnames = ['DoSomething', 'DoAnotherThing', 'DoLastThing']
for fn in fnames:
    cog.outl("void %s();" % fn)
]]]
[[[end]]]
Another fragment of our file

```

</div>

**Running Cog:**

<div class="highlighter-rouge">

```
static part of the file
void DoSomething();
void DoAnotherThing();
void DoLastThing();
Another fragment of our file

```

</div>

</div>

<div class="post">

# [Update State Machine Code Generation in C++](/website/2016/07/11/ibarbechWeek6/)

<span class="post-date">11 Jul 2016</span>

# Update State Machine Code Generation in C++

As “void compute;” no longer necessary, now when a component with state machine is generated the portion of “compute” no exist.

This method can be supress because Its funtion is realized by the state machine.

This portion disappears of files generated:

*   **genericworker.h**
*   **genericworker.cpp**
*   **specificworker.h**
*   **specificworker.cpp**

</div>

<div class="post">

# [Progress with RoboCompDSL](/website/2016/07/08/dgallegosWeek6/)

<span class="post-date">08 Jul 2016</span>

Hi! I am back. It’s been weeks since my last post, because there was not much to say. In this post I will summarize the progress with RoboCompDSL. Now I finished the biggest work, we can communicate two Robocomp components using ROS middleware or ICE middleware and it is as simple as has been done so far. We just need write an IDSL(s) with some restrictions and a CDSL for our component.

**What restrictions?** **Services** has two parameters (in/out). Example: void service(int numRequest, out int numResponse); **Topics** has just one parameter. Example: void topic(int numToPublish); **Allowed Types** are structs, sequences(array) and primitives types as string, int, float…

Paying attention to this restrictions we can write an IDSL compatible with ROS and ICE middleware for our components. I am working on a guide to explain all the news and how to use them.

Finally I would like to highlight a further set of additions that I made:

**Written a Syntax Highlighting File** that allow in KDE applications results like:

![](images/week6.png)

**Support QT5** completed and tested (an example of use in the image above).

</div>

<div class="post">

# [Creating test for CNN object detection component in simulation enviornment (rcis)](/website/2016/06/25/HaritWeek5/)

<span class="post-date">25 Jun 2016</span>

# Design Specifications:

The primary goal is to detect objects in simulation environment using CNN. This component gives a button based control panel to move omnirobot around. It also includes a button to grab image from RGB sensor and send to CNNobjectdetection module and get detections.

# CSDL interface file for the component:

<div class="highlighter-rouge">

```
import "/robocomp/interfaces/IDSLs/RGBD.idsl";
import "/robocomp/interfaces/IDSLs/OmniRobot.idsl";
import "/robocomp/interfaces/IDSLs/objectDetectionCNN.idsl";

Component testODCNNsimulation
{
	Communications
	{
		requires RGBD;
		requires OmniRobot;
		requires objectDetectionCNN;
	};
	language Cpp;
	gui Qt(QWidget);
};

```

</div>

# Specific worker compute method:

1.  grab RGB image from simulator camera

rgbd_proxy->getRGB(rgbMatrix,hState,bState);

1.  show camera output

    imshow(“Display window”,src);

# Specific worker constructor:

callback declaration when button pressed.

<div class="highlighter-rouge">

```
connect(pushButtonLeft, SIGNAL(clicked()), this, SLOT(moveleft()));
  connect(pushButtonRight, SIGNAL(clicked()), this, SLOT(moveright()));
  connect(pushButtonFront, SIGNAL(clicked()), this, SLOT(movefront()));
  connect(pushButtonBack, SIGNAL(clicked()), this, SLOT(moveback()));
  connect(pushButtonClock, SIGNAL(clicked()), this, SLOT(rotateclock()));
  connect(pushButtonAnticlock, SIGNAL(clicked()), this, SLOT(rotateanticlock()));
  connect(pushButtondetect, SIGNAL(clicked()), this, SLOT(detectobject()));

```

</div>

# Callback methods:

<div class="highlighter-rouge">

```
void SpecificWorker::moveleft()
{
	///move robot to left and stop		

windowmutex=false;
cout<<"move left"<<endl;

try
{
	omnirobot_proxy->setSpeedBase(-100,00,0);
	sleep(1);	
    omnirobot_proxy->setSpeedBase(0,0,0);
}
catch(const Ice::Exception& ex)
{
	cout << "[" << PROGRAM_NAME << "]: Error in  proxy (base). Waiting" << endl;
	cout << "[" << PROGRAM_NAME << "]: Motivo: " << endl << ex << endl;
} } Similarly other callback methods are defined for other button clicked events. The detect object object button is similar to testobjectdetect for real images execpt the image is coming from rgbdproxy.

```

</div>

# Results:

![](images/week5/week5_Simulation1.png) ![](images/week5/week5_Simulation2.png) ![](images/week5/week5_Simulation3.png)

</div>

<div class="post">

# [Update](/website/2016/06/22/BasilWeek6/)

<span class="post-date">22 Jun 2016</span>

# Update –Week 6

Recently I was coding some slot functions of what happens when we click on the actions like save,Open,exit.. The old code helped me lot in this case.. In between I tried to put a function to select what kind of context menu should be displayed on screen when we right click on QGraphicsScene..I mean the context menu which appears when we right click on node and its background should be different.I made a silly mistake during that and spend 2 hour going through documentations to understand my mistake :D :D..

Now I will be going into drawing the graphic Tree.Since the new nodes are not circular in shape,We can’t distribute the other connected nodes evenly around a node.So I will have to use a new algorithm for that..Basically I will have to use the same spring force system..But putting some limitations on the node connection directions..Once these drawing have finished,I have to enter the communication part..

I haven’t started coding any communication part yet..I had already learned some Basics about the Zeroc Ice for understanding the robocomp framework in the beginning.So it helps me to build the tool in complete independent modules for GUI,Communications and controlling components.

I have also decided to use subclasses of all three main classes in qt Graphics View network..ie now We are using the subclasses of QGraphicsView,QGraphicsScene and QGraphicsItem instead of using direct classes..This will help to add some extra functionalities to the tool in the future..

* * *

Basil M Varghese

</div>

</div>

<div class="pagination">[Older](/webrobocomp.github.io/page3) [Newer](/webrobocomp.github.io)</div>

</div>