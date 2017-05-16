# Projects

**Publications**

1.  [RoboComp: A Tool-Based Robotics Framework](http://link.springer.com/chapter/10.1007/978-3-642-17319-6_25#page-1)

2.  [Improving a Robotics Framework with Real-Time and High-Performance Features](http://link.springer.com/chapter/10.1007/978-3-642-17319-6_26#page-1)

3.  [Visually-guided Object Manipulation by a mobile Robot](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.187.71&rep=rep1&type=pdf)

4.  [Progress in RoboComp](http://dialnet.unirioja.es/servlet/articulo?codigo=4652542)

5.  [A Novel Robust Scene Change Detection Algorithm for Autonomous Robots Using Mixtures of Gaussians](https://robolab.unex.es/index.php?option=com_remository&Itemid=53&func=startdown&id=141)

6.  [A robotic platform for domestic applications](https://robolab.unex.es/index.php?option=com_remository&Itemid=53&func=startdown&id=135)

7.  [Loki, a Mobile Manipulator for Social Robotics](https://robolab.unex.es/index.php?option=com_remository&Itemid=53&func=startdown&id=126)

8.  [Ursus: a robotic assistant for training of children with motor impairments](https://robolab.unex.es/index.php?option=com_remository&Itemid=53&func=startdown&id=110)

9.  [Improving a Robotics Framework with Real-Time and High-Performance Features](https://robolab.unex.es/index.php?option=com_remository&Itemid=53&func=startdown&id=83)

10.  [Improving the lifecycle of robotics components using Domain-Specific Languages](http://arxiv.org/abs/1301.6022)

[For more publications visit here –>](https://robolab.unex.es/index.php?option=com_storepapers&view=search&Itemid=58&ida=0&idc=0&year=all&limitstart=0)

**Robots using RoboComp**

1.  **Loki**

The robot Loki is an AMM built as a collaboration among several Spanish universities and companies, following design and coordination by UEX. This robot has been built as an advanced social robotics research platform.

The mobile base of Loki holds batteries, energy management electronics, local controllers and a high-end dual-socket Xeon board. Standing on it, a rigid column supports the torso which holds two arms and a expressive head. Each arm has 7 degrees-of-freedom (DOF) in anthropomorphic configuration built with a mixture of Schunck motors in the upper arm and a custom-made 3 DOF forearm, a 6 DOF torque-force sensor in the wrist and a 4 DOF BH8 Barret hand with three fingers.

The head is connected to the torso by a 4 DOF neck and has two orientable Point Grey firewire cameras, an ASUS Xtion range sensor and 5MP Flea3 camera. It also features an articulated yaw and eyebrows for synthesis of facial expressions and microphones and a speaker for voice communication. The total number of DOF is 37.

The control of all these elements is performed by a set of components created with the open source robotics framework RoboComp, which can easily communicate with other frameworks like ROS or Orca2.

The robot currently can execute a basic repertoire of navigation, manipulation, and interaction behaviours that are organized under the new RoboCog architecture, and developed on top of the RoboComp framework.

![Smiley face](https://robolab.unex.es/images/stories/robots/Loki/loki_matb%201.jpg) ![Smiley face](https://robolab.unex.es/images/stories/robots/Loki/loki-01.png) ![Smiley face](https://robolab.unex.es/images/stories/robots/Loki/loki-02.png)

2 **Thor**

RoboLab has finished the robot Thor, the new version of its original robot RobEx. It has been completely redesigned to admit a 150Kg load, offering up to 150Ah/12v of electric power.

Thor has been created to be the base of a high performance mobile manipulator, endowed with a robotic 7dof arm, a hand and an expressive head.

Thor has an embedded Cortex processor running Linux and RoboComp. The basic components running inside compute odometric information and serve laser data and battery state. Additionally several reflexes can be added as new components.

![Smiley face](https://robolab.unex.es/images/stories/robots/thor/thor02_grande.jpg)

3 **Dulce**

Dulce, as part of an educational research experience with 5th grade kids, RoboLab has build a robotic turtle that executes Logo programs. This effort is part of a term long cooperation between math teacher Petri Colmenero of Dulce Chacón Primary School and the RoboLab team.

During this term the fifth graders have received weekly classes on Logo using the KTurtle open software Linux application. After this introduction to the basics of computer programming, today the participants have programmed directly the robot Dulce, commanding it to draw letters and geometric figures on a giant board placed on the floor.

This activity intends to bring the computers and robots closer to the learning process, turning it into an active, “learn by doing” experience.

![Smiley face](https://robolab.unex.es/images/stories/robots/Dulce/dulce01_grande.jpg)

4 **Ursus**

Ursus is an assistive robot developed in the Laboratory of Robotics and Artificial Vision of the University of Extremadura in collaboration with the Virgen del Rocío Hospital. It is designed to propose games to children with cerebral palsy in order to improve their recovery. It will also be used as a tool for their therapists. In order to make it visually pleasant for children, it has a friendly height and has been wrapped into the covering tissue of a teddy bear. Patients can get feedback of the status of the exercise in real-time by looking at the image the robot projects on the wall. This information encourages children to correct themselves.

Robolab is building a new robot in collaboration with the Hospital Virgen del Rocío at Sevilla, within the ACROSS project. It is designed to propose games to children with cerebral palsy in order to improve their recovery. It will also be used as a tool for their therapists. In order to make it visually pleasant for children, it has a friendly height and has been wrapped into the covering tissue of a teddy bear. Patients will get feedback of the status of the exercise in real-time by looking at the image the robot projects on a screen or a tv monitor.

Inside this video stream, the robot injects virtual objects signaling the limits of the movements. This augmented reality features are intended to increase the inmersion sensation in the user and to extend the duration of the physical exercises. To achieve the necessary tracking precision of the user arm, URSUS uses its camera under the neck and augmented reality techniques.

![Smiley face](https://robolab.unex.es/images/stories/robots/Ursus/ursus2_grande.jpg)

5 **SMAR**

SMAR is an outdoor robot designed and built at RoboLab in collaboration with the technological company IaDEX and funded by the Government of Extremadura under grant.

This robot is omni-directional (each wheel can be oriented independently), has a payload of 250 Kgs and includes several sensors for self-localization and mapping. SMAR is a very versatile autonomous base that can be augmented with specific tools.

One of the first applications have been the positioning of a high resolution laser-scanner along a pre-computed trajectory. The point clouds obtained are already registered up to a good initial approximation that can be further refined with bundle adjustment methods.

Other applications that are currently being developed include precision agriculture and hyper-spectral image ground calibration.

![Smiley face](https://robolab.unex.es/images/stories/robots/SMAR/2011-10-14%2012.42.29.jpg)

6 **Muecas**

Muecas is our last robotic head. It is designed to be used in social robots as mean to transmit expressions. The most sophisticated components of the head are the eyes. They are made with hollow nylon spheres holding firewire cameras inside.

The eye ball are actuated by Faulhaber linear motors without gear reduction, and can achieve speeds and accelerations comparable to human eyes. The neck is a 3 dof’s parallel actuator driven by dc motors.

There is also an additional dof for the pan movement oh the head. Additional elements are being developed: eyelids, eyebrows and mouth will complete the basic endowment of this outstanding head.

All electronics will be packed in the head and below the neck, including a dual-core Atom embedded processor running RoboComp. All communications in and out of the head will go through a 1Gb Ethernet interface.

![Smiley face](https://robolab.unex.es/images/stories/robots/Muecas/muecas2_grande.jpg)

<iframe src="https://www.youtube.com/embed/9C8r50JBo3Q" allowfullscreen="" width="420" height="315" frameborder="0"></iframe><iframe src="https://www.youtube.com/embed/9C8r50JBo3Q" allowfullscreen="" width="420" height="315" frameborder="0"></iframe>

7 **Robex**

Robex, our new prototype of mobile robot is ready! It is a evolution of Speedy with a new chassis geometry and a new Wifi-N link. Motion is done by two Maxon DC motors with HP encoders.

They are controlled by custom made 3 Amps PWM amplifiers driven by the AtMega32 microcontroller described bellow. This board computes the two PID loops at 2KHz and reads an electronic compass at 2Hz connected through a I2C bus.

The compass and the motor controllers can be coupled in a higher order loop to follow an externally commanded magnetic direction. In the front we have placed a URG Laser -4 meters range and .5 degrees res.- connected also through a USB port.

Other microncontroller board controls the three servos of the steroscopic head that hold two ISight Firewire cameras. Still another one controls a custom made “vestibular” board.

![Smiley face](https://robolab.unex.es/images/stories/robots/family_robex_grande.jpg)

8 **Tornasol**

Tornasol is our first medium-sized robot. He has an aluminium frame and 4-wheels (not very good idea) carrying all the electronics and computer on board. We designed and built for him the power amplifiers and a microcontroller-based board using a risc 100MHz Hitachi SH3 for PID control of the electrical motors. On top of these elements an AMD K7 processor takes care of image adquisition and processing, and navigation control. In addition, it is connected to the world through a Gbit Ethernet link.

In the upper structure we have placed a robotic stereo head with three degrees of freedom and two Firewire cameras. The robot is able of generating an approximated 3D reconstruction of its environment at a frame rate of 4Hz. Here you can see some snapshots of these results.

![Smiley face](https://robolab.unex.es/images/stories/robots/robot2.jpg)

9 **Speedy**

Speedy is a 1/8 model car based autonomous mobile robot. This robot includes a two-level arquitecture: a mini-ITX board with a C3 x86 compatible processor and 4 ATMega32 based custom boards designed and built in the Lab.

Each of these boards takes care of a low-level behavior or group of behaviors: PID drive motor control, steering servo control and digital compass for autopilot in one of them, an ultrasound tilt-controlled scanner in another one, a set of 3-axis accelerometers for inclination sensing in the third one, and servo control of a pan-tilt camera. Limitations in turning radius suggest porting all hardware to a new robotic base.

We have started new work on Component-based software using the Ice communications framework.

![Smiley face](https://robolab.unex.es/images/stories/robots/speedy1.jpg)

<iframe src="https://www.youtube.com/embed/NLD6y-sA_lI" allowfullscreen="" width="420" height="315" frameborder="0"></iframe>

10 **Pneux**

Pneux is a complete mechanical design of a biped robot created by Sebastian K., a former graduated student in the Lab. It’s actuators are pneumatic muscles, a technology in which we work from time to time -when we get funds. It has 2 dof’s in the ankles, 1 in the knees, 3 in the hips and a 3 dof’s stereovision head.

More research on dynamic walking robots has been pursued in collaboration with Professor Félix Monasterio at the Universidad Politécnica de Madrid.

![Smiley face](https://robolab.unex.es/images/stories/robots/Pneux/05.jpg)

11 **Stereo Head**

Stereo Head: As part of Sebastian’s Thesis, he designed a stereo robotic head that was built and placed on the robot substituting the old one. This new head is faster and more accurate than the old one. The cameras are Sony’s firewire model with controllable zoom, focus and many other parameters.

Here you can see a video of the animated 3D model before its construction.

![Smiley face](https://robolab.unex.es/images/stories/robots/Head/02.JPG)

12 **Insex**

Insex is a hexapod machine built by last year undergraduates. It’s parts are made with a Nylon-Fiberglass compound. Joints are powered by 12 servo motors controlled by three cascaded commertial boards. It’s small brain is C++ coded in a computer outside the body following a fairly complex component-based distributed design. The main limitation is the absence of force-feedback from the feet.

![Smiley face](https://robolab.unex.es/images/stories/robots/bot.jpg)

13 **Miura**

Miura is the stereo version of Pulguita. Five degrees of freeedom with digital PID controlled servos and two ISight Firewire cameras. Specially indicated for the new dual-core processors.

![Smiley face](https://robolab.unex.es/images/stories/robots/miura1.jpg)

14 **Pulguita**

Pulguita is a quick algorithm demonstrator that lives on our desks. It can be connected to any computer and is used to try and debug vision and navigation algorithms. The camera is Apple’s ISight and for the pan-tilt unit we use two digital servos from Futaba. The base is built also on top of two servos an all of them are controlled by microcontroller board and connected to de Pc through a serial link.It’s low cost, fast to assemble and can be used in robotic or vision courses.

The video shows Pulguita under the dynamics of target detection with inhibition of return.

![Smiley face](https://robolab.unex.es/images/stories/robots/pulg_0001.jpg)