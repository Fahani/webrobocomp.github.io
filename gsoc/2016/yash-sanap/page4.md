# week 6 updates

20 Jun 2016

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