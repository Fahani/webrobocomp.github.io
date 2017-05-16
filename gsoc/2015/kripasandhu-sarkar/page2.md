# _GSoC,_ Computer vision components and libraries management

#2

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