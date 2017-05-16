# Creating test for CNN object detection component in simulation enviornment (rcis)

25 Jun 2016

# Design Specifications:

The primary goal is to detect objects in simulation environment using CNN. This component gives a button based control panel to move omnirobot around. It also includes a button to grab image from RGB sensor and send to CNNobjectdetection module and get detections.

# CSDL interface file for the component:


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


# Specific worker compute method:

1.  grab RGB image from simulator camera

rgbd_proxy->getRGB(rgbMatrix,hState,bState);

1.  show camera output

    imshow(“Display window”,src);

# Specific worker constructor:

callback declaration when button pressed.


```
connect(pushButtonLeft, SIGNAL(clicked()), this, SLOT(moveleft()));
  connect(pushButtonRight, SIGNAL(clicked()), this, SLOT(moveright()));
  connect(pushButtonFront, SIGNAL(clicked()), this, SLOT(movefront()));
  connect(pushButtonBack, SIGNAL(clicked()), this, SLOT(moveback()));
  connect(pushButtonClock, SIGNAL(clicked()), this, SLOT(rotateclock()));
  connect(pushButtonAnticlock, SIGNAL(clicked()), this, SLOT(rotateanticlock()));
  connect(pushButtondetect, SIGNAL(clicked()), this, SLOT(detectobject()));

```


# Callback methods:


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


# Results:

![](images/week5/week5_Simulation1.png) ![](images/week5/week5_Simulation2.png) ![](images/week5/week5_Simulation3.png)