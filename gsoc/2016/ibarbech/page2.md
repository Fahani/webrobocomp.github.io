# State Machine Code Generation in C++

21 Jun 2016

# Definition Language

Hi all. This time I write to explain the work done during these first three weeks. In principle, and after talking with my mentor, I began to study as defined a formal language. Finally after learning the steps to the definition I started building an example of this language. This would be contained in a file .smdsl, this language is a domain-specific language:


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


[]→ optionality *→ Item List

# Parser SMDSL file

I then created the parseSMDSL.py file that parses this code. This code creates and returns node tree with information from the state machine.

To parse the code I have taken into account several things.


```
Machine: *	The machine must have a name. *	The machine must have a mandatory initial state. *	The initial state can not be on the list states. *	The final state can not be on the list states. *	The initial state can not be the same as the final state.
Substates: *	Must have a parent state.

```


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


*   **genericworker.h**

Example genericworker.h:


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



*   **genericworker.cpp**

Example genericworker.cpp:


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


*   **specificworker.h**

Example specificworker.h:


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


*   **specificworker.cpp**

Example specificworke.cpp:


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
