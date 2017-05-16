# State Machine Code Generation in Python

19 Jul 2016

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

