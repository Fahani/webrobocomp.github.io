_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *


# [_GSoC,_ Symbolic planning techniques for recognizing objects domestic

#4

, Inverse kinematics Graph Generator](/website/2015/08/13/mercedes4/) 

13 Aug 2015

**ikGraphGenerator, an alternative to ik** : As the project has progressed, many improvements have emerged. One of them is the new component, `ikGraphGenerator`. This component has been developed by Professor Luis Manso, and its goal is to remove weight to the inverse kinematics in the process of handling objects with a robotic effector.

One of the problems of the inverse kinematics is that, given a target for a particular end-effector, it can calculate different solutions or values for each motor of the kinematic chain. For example, to pick up a cup, the IK can position the end effector at the target extending his arm more or less forcing or not the elbow or or separating more or less the shoulder. It doesn’t matter that the final position of the chain be more forced or more natural, the goal is reached and the solution is accepted.

This is not convenient, since there is no way to control the trajectory of the arm. The control on the trajectory arm is crucial at certain times, for example when the robot avoids collisions between his arm and another objects (tables, chairs, walls, including his own body). The current `inversekinematics` component does not allow us this control, so that we need the `ikGraphGenerator` component.

The main concept is to create a spatial graph where each node stores the pose of the point, [tx, ty, tz, rx, ry, rz], and the set of angular values for each motor of the kinematic chain (that has 7 DOFs). The links connecting the nodes whose positions are close to each other. In this way we can calculate paths between two different and separate points.

![Alt text](https://github.com/robocomp/robocomp-ursus/blob/master/components/ikGraphGenerator/etc/ikg.jpg?raw=true)

###What does the `ikGraphGenerator` component?

Basically the `ikGraphGenerator` realizes two functions:

1.  It calculates the graph with random poses and their respective angular values. To do this, first it defines a spatial cube that represents the workspace of the robot arm. In this space a set of poses are selected and are sent to the `inversekinematic` component as targets. The poses that are not achievable by the ik are automatically deleted. The resultant graph is stored in a file in order to use it in later calculations.
2.  Once the graph has been calculated and stored, we can send a target to the `ikGraphGenerator`. First the component searches the node `A` whose pose is closest to the initial end effector pose and the node `B` whose pose is closest to the target position (to do this, the `ikGraphGenerator` uses a fast low-dimension k-d tree). Then, the component calculates a path between node A and node B through the graph using Dijkstra’s algorithm, so that the arm moves through the graph from the position marked by node A to the position marked by the node B. Finally, in order to achieve the final target, the `inversekinematic` component is called to compute the final values of the joints, starting from the position marked by the node B.

![Alt text](https://github.com/robocomp/robocomp-ursus/blob/master/components/ikGraphGenerator/etc/GIK.png?raw=true)

In the next post I will describe how the whole system works with all the components, the `inversekinematic`, the `ikGraphGenerator` and the `visualik` component.

Bye!


# [Till now ... after midterm](/website/2015/08/08/nithin9/)

08 Aug 2015

Hi all , In this post i will talk about what i have been working on after midterm evaluation. I have spend my time working mostly on packaging supporting libraries for Robocomp.This includes FCL and libccd. FCL is a library for performing three types of proximity queries on a pair of geometric models composed of triangles. libccd is a library for collision detection between two convex shapes.Technically Robocomp is only using FCL but libccd is an dependency of fcl, as i couldn’t find an updated ppa for it i decided to package it too. you can see those packages [here](https://launchpad.net/~imnmfotmal)

Also i added ability for generating robocomp source packages for different distributions. Initially i added the option only for trusty, now we could generate packages for any distribution.I worked a bit on build tools also. Currently if someone created an workspace and made a github repo of it, some one else cant use it as we store the repo names in ~/.config directory, so i added an option to reinit an repo.

well i guess thats all for now

* * *

Nithin Murali


# [Setting up ppa](/website/2015/07/25/nithin10/)

25 Jul 2015

##Setting up an ppa in launchpad

After creating an launchpad account First you need to create and publish an OPENPGP key

###Generating your key in Ubuntu The easiest way to generate a new OpenPGP key in Ubuntu is to use the Passwords and Encryption Keys tool.

**Step 1** open Passwords and Encryption Keys.

**Step 2** Select File > New, select PGP Key and then follow the on-screen instructions.

Now you’ll see your new key listed in the Passwords and Encryption Keys tool. (it may take some time)

###Publishing your key

Your key is useful only if other people can verify items that you sign. By publishing your key to a keyserver, which acts as a directory of people’s public keys, you can make your public key available to anyone else.Before you add your key to Launchpad, you need to push it to the Ubuntu keyserver.

**Step 1** Open Passwords and Encryption Keys.

**Step 2** Select the My Personal Keys tab, select your key.

**Step 3** Select Remote > Sync and Publish Keys from the menu. Choose the Sync button. (You may need to add htp://keyserver.ubuntu.com to your key servers if you are not using Ubuntu.)

It can take up to thirty minutes before your key is available to Launchpad. After that time, you’re ready to import your new key into Launchpad!

OR you can direclty to go `http://keyserver.ubuntu.com/` on your browser and add the PGP key there

###Register your key in launchpad fire up an terminal and run `gpg --fingerprint` should give you fingerprints of all the keys. copy paste the required fingerprint into launchpad

###Sign Ubunutu Code of Conduct Download the ubuntu code of conduct form launchpad `gpg --clearsign UbuntuCodeofConductFile` will sign the file now copy the contents of the signed file and paste in launchpad

###Wrapping Up Now everything is set up. make sure you have some key in `OPENPGP Keys` section and also the signed code of code of conduct as `Yes` as shown. ![](./launchpad.png)

##Uploading package to ppa

launchpad will only accept source packages and not binary.Launchpad will then build the packages. For building source packages we are using debuild which is a wrapper around the _dpkg-buildpackage + lintian_. so you will need to install debuild and dput on your system;

The source_package.cmake script is used to create debian source package.

The main CMakeLists.txt file defines a target `spackage` that builds the source package in build/Debian with `make spackage`

For uploading the package to ppa, First change the **PPA_PGP_KEY** in [package_details.cmake](../cmake/package_details.cmake#L26) to details to the full-name of the PGP key details registered with your ppa account For more details on setting up the pgp key see the [tutorial](./setting_up_ppa.md).Then create a source package by building the target _spackage_.Once the Source package is build successfully, upload it to your ppa by:


```
cd Debian/
dput ppa:<lp-username>/<ppa-name> <packet->source.changes

```


building of source package can be tested with:


```
cd Debian/robocomp-<version>
debuild -i -us -uc -S

```


If you are uploading a new version of robocomp, change the version number accordingly in the [toplevel cmake](../CMakeLists.txt#L31) before building, and then upload the source package as mentioned.

###Note:

If you want to upload another source package to ppa which doesn’t have any changes in the source but maybe in the debian files. you can build the spackage after commenting out `set(DEB_SOURCE_CHANGES "CHANGED" CACHE STRING "source changed since last upload")` in [package_details.cmake](../cmake/package_details.cmake#L27) so that the the script will only increase the ppa version number and won’t include the source package for uploading to ppa (which otherwise will give an error).

##Installing robocomp from ppa

First you will need to add the ppa in your sources, and then install robocomp package.


```
sudo add-apt-repository ppa:<lp-username>/robocomp
sudo apt-get update
sudo apt-get install robocomp

```


this will install robocomp along with basic components into /opt/robocomp.

* * *

Nithin Murali



# [_GSoC,_ Computer vision components and libraries management

#1

](/website/2015/07/02/kripa1/)

02 Jul 2015

**About me**:Hello, I am Kripasindhu Sarkar, a new PhD student at German Research Center for Artificial Intelligence (DFKI), Kaiserslautern working in the topic of Object Detection in simple and depth images. I am extremely interested in the topic of object detection and computer vision; specifically in solving the problem by using theories from human cognition and perception to simulate human way of visualizing the problem. But for now, I am focused on getting a very good grasp at the existing engineering (mostly) techniques in the field of computer vision and object detection. Before joining here as a PhD student I worked as a Software Engineer at Paypal for 2 years and, prior to that I did my masters and graduation from Indian Institute of Technology Kharagpur (IIT Kharagpur).

##Computer vision components and libraries management

The project is about designing and implementing a system for object detection and recognition in 3D point clouds and 2D images, and come up with a structured library with a good and easy-to-use APIs. There has been a good amount of research in this direction and my work was to cherry-pick important ideas and present them as usable components. I’ll now explain in details the various methods I chose to use as a part of this project.

###Local feature based on 2D images The idea is the find local features (like SIFT/SURF/ORB etc) in images of the object to be detected and the given test image. If enough matches are found between the descriptors of the to images an object is defined to be found. Important assumption is that the object to be detected must have textures. Advantage is that we get the complete 6 DOF of the object which might be useful for grasping. This comes in several flavors.

1.  Planner objects: If we know the object is planner, we can directly compute its tomography (pose) after the match.

2.  Random objects: If the object is of arbitrary shape it is quite difficult to detect an object with its pose but can be done in a tricky offline phase [1]. A 3D reconstruction is performed through bundle adjustments with the object to be detected to find the 2D - 3D correspondences. On the run time, given an input image, If enough matches are found, the object is detected with its full pose by solving PnP problem.

###Dense feature based on 2D images: The idea is the find features over a grid or a region of an image encoding the properties of that region and use that feature in some classification algorithm to perform detection. Naturally, we need to calculate dense feature over all possible region size over the image and apply the classifier; and thus it is bit slow as well. Also object pose is not identified in this type. Few of them are:

1.  HOG based simple classification (well known). Difficulty in implementation: Moderate; HOG implementation with multiscale detector is present in OpenCV; but the training has to be performed separately using 3rd party tool like libsvm/matlab etc (but is straightfwd).

2.  HOG based Part Based Model: This is the famous and legendary and state of art (not anymore) object detector which uses LSVM. Difficulty in implementation: Difficult; OpenCV has the detection code, but not that good. Training LSVM is not straight fwd and we need to use the original Matlab implementation of the authors.

3.  Wevlet based face detector with adaboost: This is also well known face detector algorithm used widely. Difficulty in implementation: Easy; though the concept is not that straight fwd, it is readily avilable in OpenCV.

###Detection/Recognition on Depth Images If we can get the Point Cloud with some laser scan or Kinect, there are plenty of algorithms to detect object with its pose. Again we have local feature based and global feature based algorithms described below:

1.  Direct object with local and global features [4]: Very similar to that of RGB image based algorithm with difference in the types of features. Local features have the advantage that preprocessing steps like segmentation is not required but tends to be slow. On the other hand we need to do segmentation to apply global features in the clusters. But once the segmentation (like identifying planes, etc and different clusters) of the scene is done, we can use the results subsequently. Difficulty in implementation: Easy; components of pipeline is available in PCL.

2.  Object matching using classifiers: Global features readily available in PCL and found it to have similar results to a current benchmark but faster (10 seconds for classification testing in the benchmark [2] which uses sliding window based classification on all scales using HOG like descriptors).

##The library - Open Detection It was decided later to have an independent library for Object Detection instead of integrating everything to Robocomp. The result is the inception of a separate library ‘Open Detection’. The details of the design of the library is discussed in the next blog.

* * *

[1] I. Gordon and D. G. Lowe, “What and where: 3d object recognition with accurate pose,” in Toward Category-Level Object Recognition, ser. Lecture Notes in Computer Science, J. Ponce, M. Hebert, C. Schmid, and A. Zisserman, Eds., vol. 4170\. Springer, 2006, pp. 67–82. [2] MOPED: A Scalable and Low Latency Object Recognition and Pose Estimation System [3] Object Detection with Discriminatively Trained Part Based Models [4] Aldoma, A.; Marton, Zoltan-Csaba; Tombari, F.; Wohlkinger, W.; Potthast, C.; Zeisl, B.; Rusu, R.B.; Gedikli, S.; Vincze, M., “Tutorial: Point Cloud Library: Three-Dimensional Object Recognition and 6 DOF Pose Estimation,” Robotics & Automation Magazine, IEEE , vol.19, no.3,



# [_GSoC,_ Computer vision components and libraries management

#2

](/website/2015/07/02/Kripa2/)

02 Jul 2015

**Open Detection:** Following the idea that it is better to have an independent library for Object Detection than contributing directly to Robocomp, I created the new library ‘Open Detection’. It is available now in the following links

*   Github link: https://github.com/krips89/opendetection

*   Documentation link: http://krips89.github.io/opendetection_docs/index.html

I have tried to document/provide tutorial inside the library whenever possible. So instead of writing everything here in the blog I’ll just post links to the tutorials/documentations.

###Installation Instructions

*   Link: https://github.com/krips89/opendetection/blob/master/doc/tutorials/content/installation_instruction.rst

###Library Design The basic idea was to have a library with common and simple interface giving access to varies detection methods available here. After some thinking I came up with the design explained in the following tutorial:

*   https://github.com/krips89/opendetection/blob/master/doc/tutorials/content/basic_structures.rst

The class diagrams providing a good reference is provided here:

*   http://krips89.github.io/opendetection_docs/inherits.html

###Documentation I did not document extensively till now as building an independent library from the scratch took a long time. The other very important reason is that the design is till little vulnerable to changes. I would wait little bit more for the design to be more concrete before I start documenting extensively.

##Things finished Within this time frame I could finish the following tasks:

1.  Design of the library.

2.  Complete CMake infrastructure for modular building of the library from scratch.

3.  2D feature based object detection (both Training and Detection phase) with demo.

4.  Global feature based object detection (both training and detection phase) with demo.

5.  Auto generated Documentation using Doxygen (http://krips89.github.io/opendetection_docs/index.html).

6.  Sphinx based tutorial section to generate nice pages for tutorials and blogs like that of PCL.

7.  Few other Utility classes which fits the needs and design for the library.

###Milestones and things learnt: In the next blog I’ll add the different sources I used to design and implement the above tasks and the things I learnt in this process.

* * *

Kripasandhu Sarkar


[Older](/webrobocomp.github.io/page9) [Newer](/webrobocomp.github.io/page7)
