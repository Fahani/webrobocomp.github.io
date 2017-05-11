<div class="content container">_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *

<div class="posts">

<div class="post">

# [State Machine Code Generation in C++](/website/2016/06/21/ibarbechWeek3/)

<span class="post-date">21 Jun 2016</span>

# Definition Language

Hi all. This time I write to explain the work done during these first three weeks. In principle, and after talking with my mentor, I began to study as defined a formal language. Finally after learning the steps to the definition I started building an example of this language. This would be contained in a file .smdsl, this language is a domain-specific language:

<div class="highlighter-rouge">

```
name_machine{
  [states name_state *[, name_state];]
  initial_state name_state;
  [end_state name_state;]
  [transition{
      name_state=>name_state *[, name_state];
      *[name_state=>name_state *[, name_state];]
  };]
};

*[ :father_state [parallel]{
  [states name_state *[, name_state];]
  [initial_state name_state;]
  [end_state name_state;]
  [transition{
      name_state=>name_state *[, name_state];
      *[name_state=>name_state *[, name_state];]
  };]
};]

```

</div>

[]? optionality *? Item List

# Parser SMDSL file

I then created the parseSMDSL.py file that parses this code. This code creates and returns node tree with information from the state machine.

To parse the code I have taken into account several things.

<div class="highlighter-rouge">

```
Machine: *	The machine must have a name. *	The machine must have a mandatory initial state. *	The initial state can not be on the list states. *	The final state can not be on the list states. *	The initial state can not be the same as the final state.
Substates: *	Must have a parent state.

```

</div>

If it is parallel:

*   Must have a list states.
*   Mustn’t have initial state.
*   Mustn’t have final state.
*   Mustn’t have transitions.

If it isn’t parallel:

*   Must have initial state.
*   The initial state can not be on the list states.
*   The final state can not be on the list states.
*   The initial state can not be the same as the final state.

If any of these conditions is not met an error is displayed.

To include the file .smdsl I modified the file parsing parseCDSL.py to recognize a line as:

statemachine machine.smdsl;

Once the language has been created and has been parsed, I proceeded to the study of Qt State Machine Framework and the creation of code of the state machine in C ++.

Robocompcdsl used a templates files to generate code written in python. I modified the following:

*   **genericworker.h**
*   **genericworker.cpp**
*   **specificworker.h**
*   **specificworker.cpp**

Then I explain what is contained in each of these files:

*   **genericworker.h**

Complete definition of the state machine: Maquina, states, final states, substates, parallel states.

It also contains the names of the functions executed when entering each state.

Finally it contains the signals used for transitions between states.

*   **genericworker.cpp**

In this file transitions between states are created.

The states will be added to the machine and the initial and final states are assigned to the machine or substate.

the connect between the function of a state or the input signal of a state is created.

*   **specificworker.h**

Executive functions of each state are declared.

*   **specificworker.cpp**

Implementation of the functions executed by each of the states.

# Example

*   **file.smdsl**

Example statemachine.smdsl:

<div class="highlighter-rouge">

```
Machine_testcpp{
    states test2, test3, test4, test5;
    initial_state test1;
    end_state test6;
    transition{
	test1 => test1, test2;
	test2 => test3, test5, test6;
	test3 => test3, test4;
	test4 => test5;
	test5 => test6;
    };
};

:test1 parallel{
    states test1sub1, test1sub2;
    transition{
	test1sub1 => test1sub1;
	test1sub2 => test1sub2;
    };
};

:test1sub2{
    initial_state test1sub21;
    end_state test1sub22;
    transition{
	test1sub21 => test1sub21,test1sub22;
    };
};

:test3 parallel{
    states test3sub1, test3sub2, test3sub3;
    transition{
	test3sub1 => test3sub1;
	test3sub2 => test3sub2;
    };
};

:test5{
    states test5sub2;
    initial_state test5sub1;
    transition{
	test5sub1 => test1sub2;
	test1sub2 => test5sub1;
    };
};

```

</div>

*   **genericworker.h**

Example genericworker.h:

<div class="highlighter-rouge">

```
/*
 *    Copyright (C) 2016 by YOUR NAME HERE
 *
 *    This file is part of RoboComp
 *
 *    RoboComp is free software: you can redistribute it and/or modify
 *    it under the terms of the GNU General Public License as published by
 *    the Free Software Foundation, either version 3 of the License, or
 *    (at your option) any later version.
 *
 *    RoboComp is distributed in the hope that it will be useful,
 *    but WITHOUT ANY WARRANTY; without even the implied warranty of
 *    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *    GNU General Public License for more details.
 *
 *    You should have received a copy of the GNU General Public License
 *    along with RoboComp.  If not, see <http://www.gnu.org/licenses/>.
 */
#ifndef GENERICWORKER_H
#define GENERICWORKER_H

#include "config.h"
#include <QtGui>
#include <stdint.h>
#include <qlog/qlog.h>

#include <qt4/QtCore/qstatemachine.h>
#include <qt4/QtCore/qstate.h>
#include <CommonBehavior.h>
#include <JointMotor.h>

#define CHECK_PERIOD 5000
#define BASIC_PERIOD 100

typedef map <string,::IceProxy::Ice::Object*> MapPrx;

using namespace std;

using namespace RoboCompJointMotor;

class GenericWorker : 
public QObject
{
Q_OBJECT
public:
	GenericWorker(MapPrx& mprx);
	virtual ~GenericWorker();
	virtual void killYourSelf();
	virtual void setPeriod(int p);

	virtual bool setParams(RoboCompCommonBehavior::ParameterList params) = 0;
	QMutex *mutex;

	JointMotorPrx jointmotor_proxy;

protected:
//State Machine
	QStateMachine Machine_testcpp;

	QState *test2 = new QState();
	QState *test3 = new QState(QState::ParallelStates);
	QState *test4 = new QState();
	QState *test5 = new QState();
	QState *test1 = new QState(QState::ParallelStates);
	QFinalState *test6 = new QFinalState();
	QState *test1sub1 = new QState(test1);
	QState *test1sub2 = new QState(test1);
	QState *test1sub21 = new QState(test1sub2);
	QFinalState *test1sub22 = new QFinalState(test1sub2);
	QState *test3sub1 = new QState(test3);
	QState *test3sub2 = new QState(test3);
	QState *test3sub3 = new QState(test3);
	QState *test5sub2 = new QState(test5);
	QState *test5sub1 = new QState(test5);

//-------------------------

	QTimer timer;
	int Period;

public slots:
	virtual void compute() = 0;
//Slots funtion State Machine
	virtual void fun_test2() = 0;
	virtual void fun_test3() = 0;
	virtual void fun_test4() = 0;
	virtual void fun_test5() = 0;
	virtual void fun_test1() = 0;
	virtual void fun_test6() = 0;
	virtual void fun_test1sub1() = 0;
	virtual void fun_test1sub2() = 0;
	virtual void fun_test1sub21() = 0;
	virtual void fun_test1sub22() = 0;
	virtual void fun_test3sub1() = 0;
	virtual void fun_test3sub2() = 0;
	virtual void fun_test3sub3() = 0;
	virtual void fun_test5sub2() = 0;
	virtual void fun_test5sub1() = 0;

//-------------------------
signals:
	void kill();
//Signals for State Machine
	void test1totest1();
	void test1totest2();
	void test2totest3();
	void test2totest5();
	void test2totest6();
	void test3totest3();
	void test3totest4();
	void test4totest5();
	void test5totest6();
	void test1sub1totest1sub1();
	void test1sub2totest1sub2();
	void test1sub21totest1sub21();
	void test1sub21totest1sub22();
	void test3sub1totest3sub1();
	void test3sub2totest3sub2();
	void test5sub1totest1sub2();
	void test1sub2totest5sub1();

//-------------------------
};

#endif

```

</div>

*   **genericworker.cpp**

Example genericworker.cpp:

<div class="highlighter-rouge">

```
/*
 *    Copyright (C) 2016 by YOUR NAME HERE
 *
 *    This file is part of RoboComp
 *
 *    RoboComp is free software: you can redistribute it and/or modify
 *    it under the terms of the GNU General Public License as published by
 *    the Free Software Foundation, either version 3 of the License, or
 *    (at your option) any later version.
 *
 *    RoboComp is distributed in the hope that it will be useful,
 *    but WITHOUT ANY WARRANTY; without even the implied warranty of
 *    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *    GNU General Public License for more details.
 *
 *    You should have received a copy of the GNU General Public License
 *    along with RoboComp.  If not, see <http://www.gnu.org/licenses/>.
 */
#include "genericworker.h"
/**
* \brief Default constructor
*/
GenericWorker::GenericWorker(MapPrx& mprx) :
QObject()
{

//Initialization State machine
	test1->addTransition(this, SIGNAL(test1totest1()), test1);
	test1->addTransition(this, SIGNAL(test1totest2()), test2);
	test2->addTransition(this, SIGNAL(test2totest3()), test3);
	test2->addTransition(this, SIGNAL(test2totest5()), test5);
	test2->addTransition(this, SIGNAL(test2totest6()), test6);
	test3->addTransition(this, SIGNAL(test3totest3()), test3);
	test3->addTransition(this, SIGNAL(test3totest4()), test4);
	test4->addTransition(this, SIGNAL(test4totest5()), test5);
	test5->addTransition(this, SIGNAL(test5totest6()), test6);
	test1sub1->addTransition(this, SIGNAL(test1sub1totest1sub1()), test1sub1);
	test1sub2->addTransition(this, SIGNAL(test1sub2totest1sub2()), test1sub2);
	test1sub21->addTransition(this, SIGNAL(test1sub21totest1sub21()), test1sub21);
	test1sub21->addTransition(this, SIGNAL(test1sub21totest1sub22()), test1sub22);
	test3sub1->addTransition(this, SIGNAL(test3sub1totest3sub1()), test3sub1);
	test3sub2->addTransition(this, SIGNAL(test3sub2totest3sub2()), test3sub2);
	test5sub1->addTransition(this, SIGNAL(test5sub1totest1sub2()), test1sub2);
	test1sub2->addTransition(this, SIGNAL(test1sub2totest5sub1()), test5sub1);

	Machine_testcpp.addState(test2);
	Machine_testcpp.addState(test3);
	Machine_testcpp.addState(test4);
	Machine_testcpp.addState(test5);
	Machine_testcpp.addState(test1);
	Machine_testcpp.addState(test6);

	Machine_testcpp.setInitialState(test1);
	test1sub2->setInitialState(test1sub21);
	test5->setInitialState(test5sub1);

	QObject::connect(test2, SIGNAL(entered()), this, SLOT(fun_test2()));
	QObject::connect(test3, SIGNAL(entered()), this, SLOT(fun_test3()));
	QObject::connect(test4, SIGNAL(entered()), this, SLOT(fun_test4()));
	QObject::connect(test5, SIGNAL(entered()), this, SLOT(fun_test5()));
	QObject::connect(test1, SIGNAL(entered()), this, SLOT(fun_test1()));
	QObject::connect(test6, SIGNAL(entered()), this, SLOT(fun_test6()));
	QObject::connect(test1sub1, SIGNAL(entered()), this, SLOT(fun_test1sub1()));
	QObject::connect(test1sub2, SIGNAL(entered()), this, SLOT(fun_test1sub2()));
	QObject::connect(test1sub21, SIGNAL(entered()), this, SLOT(fun_test1sub21()));
	QObject::connect(test1sub22, SIGNAL(entered()), this, SLOT(fun_test1sub22()));
	QObject::connect(test3sub1, SIGNAL(entered()), this, SLOT(fun_test3sub1()));
	QObject::connect(test3sub2, SIGNAL(entered()), this, SLOT(fun_test3sub2()));
	QObject::connect(test3sub3, SIGNAL(entered()), this, SLOT(fun_test3sub3()));
	QObject::connect(test5sub1, SIGNAL(entered()), this, SLOT(fun_test5sub1()));
	QObject::connect(test5sub2, SIGNAL(entered()), this, SLOT(fun_test5sub2()));

//------------------
	jointmotor_proxy = (*(JointMotorPrx*)mprx["JointMotorProxy"]);

	mutex = new QMutex(QMutex::Recursive);

	Period = BASIC_PERIOD;
	connect(&timer, SIGNAL(timeout()), this, SLOT(compute()));
// 	timer.start(Period);
}

/**
* \brief Default destructor
*/
GenericWorker::~GenericWorker()
{

}
void GenericWorker::killYourSelf()
{
	rDebug("Killing myself");
	emit kill();
}
/**
* \brief Change compute period
* @param per Period in ms
*/
void GenericWorker::setPeriod(int p)
{
	rDebug("Period changed"+QString::number(p));
	Period = p;
	timer.start(Period);
}

```

</div>

*   **specificworker.h**

Example specificworker.h:

<div class="highlighter-rouge">

```
/*
 *    Copyright (C) 2016 by YOUR NAME HERE
 *
 *    This file is part of RoboComp
 *
 *    RoboComp is free software: you can redistribute it and/or modify
 *    it under the terms of the GNU General Public License as published by
 *    the Free Software Foundation, either version 3 of the License, or
 *    (at your option) any later version.
 *
 *    RoboComp is distributed in the hope that it will be useful,
 *    but WITHOUT ANY WARRANTY; without even the implied warranty of
 *    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *    GNU General Public License for more details.
 *
 *    You should have received a copy of the GNU General Public License
 *    along with RoboComp.  If not, see <http://www.gnu.org/licenses/>.
 */

/**
       \brief
       @author authorname
*/

#ifndef SPECIFICWORKER_H
#define SPECIFICWORKER_H

#include <genericworker.h>
#include <innermodel/innermodel.h>

class SpecificWorker : public GenericWorker
{
Q_OBJECT
public:
	SpecificWorker(MapPrx& mprx);	
	~SpecificWorker();
	bool setParams(RoboCompCommonBehavior::ParameterList params);

public slots:
	void compute();

private:

private slots:
//Specification slot funtions State Machine
	void fun_test2();
	void fun_test3();
	void fun_test4();
	void fun_test5();
	void fun_test1();
	void fun_test6();
	void fun_test1sub1();
	void fun_test1sub2();
	void fun_test1sub21();
	void fun_test1sub22();
	void fun_test3sub1();
	void fun_test3sub2();
	void fun_test3sub3();
	void fun_test5sub1();
	void fun_test5sub2();

//--------------------

};

#endif

```

</div>

*   **specificworker.cpp**

Example specificworke.cpp:

<div class="highlighter-rouge">

```
/*
 *    Copyright (C) 2016 by YOUR NAME HERE
 *
 *    This file is part of RoboComp
 *
 *    RoboComp is free software: you can redistribute it and/or modify
 *    it under the terms of the GNU General Public License as published by
 *    the Free Software Foundation, either version 3 of the License, or
 *    (at your option) any later version.
 *
 *    RoboComp is distributed in the hope that it will be useful,
 *    but WITHOUT ANY WARRANTY; without even the implied warranty of
 *    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *    GNU General Public License for more details.
 *
 *    You should have received a copy of the GNU General Public License
 *    along with RoboComp.  If not, see <http://www.gnu.org/licenses/>.
 */
#include "specificworker.h"

/**
* \brief Default constructor
*/
SpecificWorker::SpecificWorker(MapPrx& mprx) : GenericWorker(mprx)
{

}

/**
* \brief Default destructor
*/
SpecificWorker::~SpecificWorker()
{

}

bool SpecificWorker::setParams(RoboCompCommonBehavior::ParameterList params)
{

	Machine_testcpp.start();

	timer.start(Period);

	return true;
}

void SpecificWorker::compute()
{
// 	try
// 	{
// 		camera_proxy->getYImage(0,img, cState, bState);
// 		memcpy(image_gray.data, &img[0], m_width*m_height*sizeof(uchar));
// 		searchTags(image_gray);
// 	}
// 	catch(const Ice::Exception &e)
// 	{
// 		std::cout << "Error reading from Camera" << e << std::endl;
// 	}
}

void SpecificWorker::fun_test2()
{
}

void SpecificWorker::fun_test3()
{
}

void SpecificWorker::fun_test4()
{
}

void SpecificWorker::fun_test5()
{
}

void SpecificWorker::fun_test1()
{
}

void SpecificWorker::fun_test6()
{

}

void SpecificWorker::fun_test1sub1()
{
}

void SpecificWorker::fun_test1sub2()
{
}

void SpecificWorker::fun_test1sub21()
{
}

void SpecificWorker::fun_test1sub22()
{

}

void SpecificWorker::fun_test3sub1()
{
}

void SpecificWorker::fun_test3sub2()
{
}

void SpecificWorker::fun_test3sub3()
{
}

void SpecificWorker::fun_test5sub1()
{
}

void SpecificWorker::fun_test5sub2()
{
}

```

</div>

</div>

<div class="post">

# [Creating test for CNN object detection component](/website/2016/06/21/HaritWeek5/)

<span class="post-date">21 Jun 2016</span>

# Design Specifications:

This components has following tasks: firstly, read an image from command line argument, secondly pass the image to CNNobjectdetection component in raw RGB format, thirdly collect the detection results and finally display them on image.

# CSDL interface file for the component:

<div class="highlighter-rouge">

```
import "/robocomp/interfaces/IDSLs/objectDetection.idsl";

Component testObjectDetectionComp
{
	Communications
	{
		requires objectDetection;
	};
	language Cpp;
	gui Qt(QWidget);
};

```

</div>

# Specific worker compute method:

1.  run once: read image file from argument and pass to objectdetectionCNN module.

    Mat src=imread(inputFile,1);

2.  convert to ColorSeq format (raw RGB)

    int k=0; ColorSeq rgbMatrix; rgbMatrix.resize(src.rows*src.cols); for(int i=0;i<=src.rows-1;i++) { for(int j=0;j<=src.cols-1;j++) { Vec3b intensity = src.at<vec3b>(i,j); rgbMatrix[k].blue = intensity.val[0]; rgbMatrix[k].green = intensity.val[1]; rgbMatrix[k].red = intensity.val[2]; k++; } }</vec3b>

3.  call objectdetectioncnn module and get detections.

objectdetectioncnn_proxy->getLabelsFromImage(rgbMatrix, src.rows, src.cols, result);

1.  Display detection Bounding boxes.

    <div class="highlighter-rouge">

    ```
         for(unsigned int i=0; i < result.size();i++ )
             {  <<result[i].bb.width<<" "<<result[i].bb.height<<endl;
                 Rect r1;
                 BoundingBox bb;
                 bb=result[i].bb;
                 r1.x=bb.x;
     				r1.y=bb.y;
     				r1.width=bb.width;
     				r1.height=bb.height;
     				Scalar rect_color = Scalar( 0, 0, 255 );
                 rectangle( src, r1.tl(), r1.br(), rect_color, 2, 8, 0 );
                 string name=result[i].name;
                 putText(src, name.substr(0, name.find(",")), r1.tl(), CV_FONT_HERSHEY_DUPLEX, 0.5, cvScalar(0,0,250), 1, CV_AA);
             }              
     	imshow("Source image", src ); Results: ============     <img src="images/week5/week5_Real1.png" alt="" width="400"/>  <img src="images/week5/week5_Real2.png" alt="" width="400"/>  <img src="images/week5/week5_Real3.png" alt="" width="400"/> 

    ```

    </div>

</div>

<div class="post">

# [week 6 updates](/website/2016/06/20/yashWeek6/)

<span class="post-date">20 Jun 2016</span>

# MidTerm Updates

Its been a few weeks science i wrote a blog post. Its mainly because there was nothing much to write. In this post, at first i will describe an usage scenario of rcmaster and then i will describe work done till now.

**Usage Scenario**

consider a simple robot which has an camera and an simple robotic arm which has an servo motor to control the movement. The feedback from the camera is used to control the angle to which the motor will rotate. Now if we are using robocomp we will use three components for implementing this scenario, one component for handling camera (cameraComp), another for handling the motion of motor (motorComp) and another for processing the output of the camera and deciding the angle to which the motor should rotate (processingComp).

Now after creating the components, we should first start the rcmaster. The rcmaster component will start on a port as defined in the rcmaster’s configuration file. If this port is already occupied, rcmaster will look for alternate ports and will init on that. This port will be updated in the rcmaster environment variable “RCMasterURI”. Now we can start the components one by one. Suppose we start the cameraComp first, then it will use the environment variable to find the port in which rcmaster is running. Then the cameraComp will invoke an operation on the rcmaster interface to register itself while providing information about the interface it implements which in this case is the cameraComp and its host (localhost). The rcmaster will find an free port for each of the interfaces provided and will return the list of ports. So the cameraComp’s camera interface will start listening on the port returned by rcmaster. Now we can start the motorComp in similar way. Then we can start the processingComp, it will also be assigned a port. Now the processingComp will query the rcmaster for the ports in which the cameraComp and motorComp are listening. This can be an blocking call or non-blocking call. If it is a blocking call the rcmaster will either wait for that component to started by user or will start the component by itself. In case of non-blocking call the operation will return the port if the component is already running else will return nothing. But when that component is started by user it will notify the processingComp. Now all the components are deployed and each others can find each other.

Now suppose cameraComp crashed because of some internal error, if the camerComp was started with the monitor flag, then rcmaster will identify that its crash and will soon relaunch the component. Also based on the cache entry the component will be assigned the same port it was assigned earlier, given that none started listening on that port in between.

**Work Done till now …**

I have finished coding the basic features of rcmaster. The component registration and lookup is completed along with cache features. Also i have implemented a backup mechanism of backing up the registry in to disk periodically so that even if rcmaster crashes it can recover most part of database. But none of the code is tested so i haven’t made any pull request till now. Even though all code is there is my forked repository.

Also while coding the post assignment, one dilemma was whether the rcmasetr should assign the ports or the components should assign listen to a free port and notify the master. when the master is assigning there is a small probability that by the time rcmaster pases that port number to the component the port might not be free. But after a discussion with Pablo we have decided to go with rcmaster assigning the port as the other probability is very small.

* * *

Yash Sanap

</div>

<div class="post">

# [Introduction](/website/2016/06/19/BasilWeek5/)

<span class="post-date">19 Jun 2016</span>

# UPDATES

Its been two weeks since my last blog.Its mainly because I did not have much to write about anything.As I have discussed before, one of the chief Job is Implementing the Graphics View framework.Which was new for me.So most of the time I was spending learning about these. So far I have successfully created a UI for the tool. With all those docked windows for logs,tools… And I have also added the graphics View framework tools like QGraphicsView,QGraphicsScene,QGraphicsItem.

I created a custom made GraphicsItem for this.Before starting I was thinking about something simple and basic.But When I made it looked cool to me.Each item will act as a node in the tree.

I used a QGrpahicsView subclass instead of directly using it.It will make the development of future versions super easy..

Apart from these things I have added the context menus what appear when we right click on components and backgrounds.Attached some signals with its slots.Also have some events like wheel events and right click events. Things are going well now..

I believe that I can do all these basic things in 2-3 days.

The only big challenge before me during that will be making a new algorithm for drawing the tree.Because In the old version,The nodes were round in shape and all connections were from and to its centers.So I have to make a new algorithm for that as the new nodes are rectangle in shape and connects are not from its center but from a extended arrows from the node..

I have not sent any pull request even though I have pushed it into my repo.Since the tool is currently in unstable state,I think I just need to put a version in my github account for backup.

* * *

Basil M Varghese

</div>

<div class="post">

# [Creating CNN object detection component in robocomp](/website/2016/06/18/HaritWeek4/)

<span class="post-date">18 Jun 2016</span>

# Design Specifications:

This component will be online and wait for image to be passed in form of RGB pixel values. The basic idea is to have a asynchronously triggerable remote procedure call. So that when the robot reaches any frontier for example a table where it can find objects, the object detection module is ready. Also this setting provides opportunity to load CNN apriori which is a time consuming process. Thus the object detection requires computing just the forward pass of the network and could be done at 0.3-0.4 Hz that is near real-time. ISDL interface file for the component: ============ module RoboCompobjectDetectionCNN {

<div class="highlighter-rouge">

```
struct BoundingBox
{
   float x;
   float y;
   float width;
   float height;    
};

struct ColorRGB
{
    byte red;
    byte green;
    byte blue;
};

struct Label
{
    string name;
    float belive;
    BoundingBox bb;
};

sequence<ColorRGB> ColorSeq;

sequence<Label> ResultList;

interface objectDetectionCNN
{
	void getLabelsFromImage(ColorSeq image, int rows, int cols, out ResultList result);

}; };

```

</div>

# CSDL interface file for the component:

import “/robocomp/interfaces/IDSLs/objectDetectionCNN.idsl”; Component objectDetectionCNN { Communications { implements objectDetectionCNN;

<div class="highlighter-rouge">

```
};
language Cpp;
gui Qt(QWidget); };

```

</div>

# Specific worker compute method:

void SpecificWorker::compute() { ///initialize caffe model only once ///read the model file and other parameters from “etc/caffe_config”

if(first) { first=false;

<div class="highlighter-rouge">

```
::google::InitGoogleLogging("objectDetectionCNN");
read_caffe_config("etc/caffe_config",model_file, trained_file, mean_file, label_file);
classifier= new Classifier(model_file, trained_file, mean_file, label_file);

```

</div>

}

}

# Specific worker get detections:

**Pipeline:**

1.  Convert ColorSeq to opencv::Mat.
2.  Now use top-hat and biletral filter to get object proposals.
3.  Once we have object proposal create a region of interest and pass it caffe classifier.
4.  Return the detections: bounding boxes, labels and classid.

* * *

Harit Pandya

</div>

</div>

<div class="pagination">[Older](/webrobocomp.github.io/page4) [Newer](/webrobocomp.github.io/page2)</div>

</div>