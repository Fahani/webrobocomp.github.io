# _GSoC,_ Computer vision components and libraries management

#1

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