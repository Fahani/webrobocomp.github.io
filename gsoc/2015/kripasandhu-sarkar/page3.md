# GSoC, Computer vision components and libraries management - Open Detction

#3

19 Aug 2015

**Contributions after Mid-term** Following are the contributions towards the project _after_ the mid-term evaluations:

*   HOG based detection (with demo).

*   Face-recognition: training as well as recognition (with demo).

*   Cascade based detection (with demo).

*   Functionality for confidence in a detection (to handle uncertainty).

That concluded the functionality of whatever planned for this summer of code. Other than the planned work, I included some other new functionality which came in between and I thought was required. The other additions are:

*   Addition of two types of detection functions for every detection class.

    a. _detectOmni_ : This is the function whose purpose is to detect an object in an entire scene. Thus, other than the type of detection we also have information about the _location_ of the detection w.r.t. the scene. This was the only function previously implemented.

    b. _detect_ : The purpose of this function is to perform detection on a segmented scene or an ‘object candidate’. i.e. the entire scene is considered as an ‘object’ or an detection. This function was an extra addition for most of the classes. For global descriptor based classes like ODHOGDetector this function came very naturally.

*   **Standardized the training database**:. Each detection class identifies its own database (which is a unique folder as of now). Training data of all classes are now put together under a single ‘trained_data’ directory and the rest is resolved by the class automatically.

*   The class **ODDetectorMultiAlgo**: This is one of the most interesting class and interesting contribution in this project. The idea is to run multiple detection algorithm on a same segmented or unsegmented scene (i.e. run both _detect_ and _detectOmni_ function) and provide the result. That is, using this class one can do object detection/recognition using multiple algorithms and provide outcome of detections (eg. people detected by HOG, face detected by Cascade, bottle detected PnPRansac in a same image). Because of the nice polymorphic design of the detection classes, one can use the concept easily with any number of detection classes with their own parameters, but with _ODDetectorMultiAlgo_ one can only take benefit of the default parameters of the included classes.

*   HOG training: There are no standard training module for training HOG (in OpenCV or in other standard library) and people are mostly confused in training svm with the HOG detector available from OpenCV. This formed a motivation for completing the HOG pipeline and provide a training module. The crucial contribution includes _hard negative_ training mode where the training is repeated with hard examples or false positive windows as negative windows. I hope that this will be really a helpful contribution for the computer vision community.

## Design changes, learning resources, and overall learning experiences:

In the next blog I’ll add the different sources I used to design and implement the above tasks and the things I learnt in this process.

* * *