# GSoC, Computer vision components and libraries management -Open Detection

#4

19 Aug 2015

**Experience** It has been a quite a ride through this Google Summer of Code. Writing a completely new library with complete building framework/rest framework for documentation or tutorial/and Doxygen framework for auto documentation-class diagrams was challenging. It was equally exciting as well. Because of this I believe that I have acquired quite a good ‘library maintainer/admin’ skills. I will continue contributing into this library after the finish of this GSoC.

After the challenges in building library support tools, the next major challenge was library design. Since, there was no previous ‘coding flavor/design’ I had to come up with the polymorphic and repeatable class design. There was so much confusion in choosing one alternative among the so many choices of good design. While the progress of the library I had to redesign the framework, and and remove structures, include namespaces and several things. I initially had thought that the design will be pretty much stable after the first month, which was not true. I still feel that the main design may need to be changed in the future to have a more logical structure. I hope this constant effort of keeping a good design will be helpful for the library users, and in the end, won’t end up with an unstructured library like OpenCV. In the future I’ll always keep an eye open for the design in general.

Now coming to the actual work of implementing different algorithm, I had really good experience in getting my hands dirty with variety of popular object detection methods. This was exactly what I had thought from this project. Taking out three months and working on the popular methods of your research topic is probably important and I hope that I’ll make a good us of this in the future.

**Learning** Here I’ll point out some of the tools and resources I have used while building the library.

####CMake building framework: I read and used ideas from the CMakeLists files of **VTK, pcl** and **OpenCV** (not that much); and chose stuffs among them depending what I thought was interesting and also modified them according to my needs. But in general, I used PCL cmake framework as my major reference.

####Library design: I have always found the design of **VTK**, beautiful; so chose it to get the polymorphic design of the classes. Even though fully templated policy based design is faster than polymorphic classes, I chose the polymorphic one as it gives way more flexibility without cluttering the design (ideas taken from vtk). I still am not sure if the performance loss through this design will affect in the future.

####Algorithms: I had a good background in PCL, so Point Cloud based detection was not difficult and used their wonderful 3d_recognition_framework in my backend. Detection based on 2D features is my own contribution and other 2D global detectors (like HOG) was pretty straightforward from OpenCV. One good learning was the implementation of HOG Trainer which I worked on few open source code and modified them according to my own needs (like hard negative training etc).

##Future This project does not ends with the end of GSoC. I plan to do the following in the short term future:

*   Host a website as a homepage and links to documentation/tutorials etc.
*   Identify how is the website automatically updated with a push in previous libraries like PCL etc. They probably use some demon which runs everyday. Learn and implement it.
*   Improve the documentations, write some descriptive tutorials.
*   Properly launch the library, with its website; first among the colleagues of our lab; then in public blogs.

* * *