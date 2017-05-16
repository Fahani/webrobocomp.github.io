_**RoboComp** is an open-source Robotics framework providing the tools to create and modify software components that communicate through public interfaces. Components may require, subscribe, implement or publish interfaces in a seamless way. Building new components is done using two domain specific languages, IDSL and CDSL. With IDSL you define an interface and with CDSL you specify how the component will communicate with the world. With this information, a code generator creates C++ and/or Python sources, based on CMake, that compile and execute flawlessly. When some of these features have to be changed, the component can be easily regenerated and all the user specific code is preserved thanks to a simple inheritance mechanism._

* * *



# [Chroot environment](/website/2015/05/23/How_To_Make_Chroot_Environment/)

23 May 2015

A chroot is a way of isolating applications from the rest of your computer, by putting them in a jail. This is particularly useful if you are testing an application which could potentially alter important system files, or which may be insecure. A chroot is basically a special directory on your computer which prevents applications, if run from inside that directory, from accessing files outside the directory. In many ways, a chroot is like installing another operating system inside your existing operating system. The following are some possible uses of chroots:

1.  Isolating insecure and unstable applications
2.  Running 32-bit applications on 64-bit systems
3.  Testing new packages before installing them on the production system
4.  Running older versions of applications on more modern versions of Ubuntu
5.  Building new packages, allowing careful control over the dependency packages which are installed

This manual will follow the steps specified in the [official page of Ubuntu](https://help.ubuntu.com/community/BasicChroot). And the system we will install as tutorial is Ubuntu 14.04 Trusty amd64.

##Brief Explanation Imagine you have your Robocomp version well installed and working really fine in your system (i.e. Ubuntu 14.04 amd64), but you need to upgrade your ICE or OpenCV or PCL or whatever third-party library to a new version. You don’t want to risk your well functional version of Robocomp and it’s dependencies removing the current version and installing the new one (this usually affects other packages and libraries), and you don’t have time enough to make a whole fresh installation in other partition or virtual machine, so the fastest solution is to create a jail containing the same distribution of your main system (Ubuntu 14.04 amd64) with chroot and test Robocomp with the new version of the library you need without touching your fine Robocomp installation. Realize that creating a chrooted environment in your machine makes your system believe that your root directory (“/”) is in another place than the actual root of the system (like I explain on the wiki, the process in which you launch chroot believes that the root directory is in / while actually it is in /var/chroot/trusty_x64/, not letting you touch anything outside that directory and therefore not risking your current installation). Another practical use for chroot is to test an especific program or library in a different distribution or architecture. For example, if you are working in Ubuntu 14.04 amd64 and you want to test if a library that you are using works fine in Debian Wheezy or Ubuntu 14.10 or Ubuntu 14.04 i386.

##Creating a chroot

1.  First of all we need to install the tools to make a chroot in out system.

    `sudo apt-get install debootstrap schroot`

2.  Create a folder where the chroot is going to be installed. You need to make the folder using administrator permission (with _sudo_ i.e). We will put the chroot up in _/var/chroot/trusty_x64_

    `sudo mkdir /var/chroot && sudo mkdir /var/chroot/trusty_x64`

3.  Create a configuration file for schroot. For our example, we will create a file named trusty_x64.conf in _/etc/schroot/chroot.d/_

    `sudo gedit /etc/schroot/chroot.d/trusty_x64.conf`

    And write the following inside: (Change the <username>to actual username, example "root-users=abhi"</username>

    

    ```
     [trusty_x64]
     description=Ubuntu trusty 14.04 for amd64
     directory=/var/chroot/trusty_x64
     root-users=<USERNAME>
     type=directory
     users=testuser

    ```

    

    ```
         - The first line is the name of the chroot thatis going to be created.
         - **description** is a short description of the chroot.
         - **directory** the path where the chroot is going to be installed. Note that is the same path that we specified in step 2.
         - **root-users** list of users that are allowed in our chroot without password.
         - **type**  The type of the chroot. Valid types are ‘plain’, ‘directory’, ‘file’, ‘block-device’ and ‘lvm-snapshot’. If empty or omitted, the default type is ‘plain’.
         - **users** list of users that are allowed access to the chroot.

    ```

    
    see [schroot.config](http://manpages.ubuntu.com/manpages/hardy/man5/schroot.conf.5.html) for further information.

4.  Run Debootstrap. This step will download and unpack a basic ubuntu or debian system to the chroot directory we created in step 2.

    `sudo debootstrap --variant=buildd --arch amd64 trusty /var/chroot/trusty_x64 http://archive.ubuntu.com/ubuntu`

    In our example, we are creating a chroot of an Ubuntu 14.04 64-bit distribution, but this command allows some different commands that can satisfy our needs, for instance, if we want to install the same distribution but the 32-bit version, we have to type:

    `sudo debootstrap --variant=buildd --arch i386 trusty /var/chroot/trusty http://archive.ubuntu.com/ubuntu`

    Note that we have to do the proper changes creating a different schroot configuration file (i.e. _/etc/schroot/chroot.d/trusty_) and a different folder for the new chroot (i.e. _/var/chroot/trusty_)

    If we want to create a chroot for a Debian version (i.e. Debian Wheezy (stable)) we have to type:

    `sudo debootstrap --variant=buildd --arch amd64 wheezy /var/chroot/wheezy_x64 http://ftp.debian.org/debian`

5.  Checking the chroot. To be sure that everything went ok, we can type the following command, that will list all the available chroot enviroments in out system.

    `schroot -l`

    If trusty_x64 appears, we can start working in our chrooted environment typing:

    `schroot -c trusty_x64 -u root`

    The prompt of the chrooted environment should be like:

    `(trusty_x64)root@abhi-Inspiron-7520:/home/abhi#`

    **NOTE** This step is not mandatory. **NOTE** For convenience, the default schroot configuration rebinds the /home directory on the host system so that it appears in the chroot system. This could be unexpected because it means that you can accidentally delete or otherwise damage things in /home on the host system. To change this behaviour we can run the following command in the host system:

    `sudo gedit /etc/schroot/default/fstab`

    And comment the /home line:

    
    ```
    # fstab: static file system information for chroots.
    # Note that the mount point will be prefixed by the chroot path
    # (CHROOT_PATH)
    #
    # <file system> <mount point>   <type>  <options>       <dump>  <pass>
    /proc           /proc           none    rw,bind        0       0
    /sys            /sys            none    rw,bind        0       0
    /dev            /dev            none    rw,bind         0       0
    /dev/pts        /dev/pts        none    rw,bind         0       0
    #/home          /home           none    rw,bind         0       0
    /tmp            /tmp            none    rw,bind         0       0

    ```

    
And that’s it! Now we have a whole very basic system in which we can test out programs and libraries.

##Troubleshooting

*   If you get locale warnings in the chroot like **“Locale not supported by C library.”** or **“perl: warning: Setting locale failed.”**, then try one or more of these commands:


```
    sudo dpkg-reconfigure locales
    sudo apt-get install language-pack-en
    sudo locale-gen en_US.UTF-8
    sudo dpkg-reconfigure locales

```


if the problem persist check out this [page](http://perlgeek.de/en/article/set-up-a-clean-utf8-environment).

*   To get access to the intertet within the chroot, you have to type:

    `sudo cp /etc/resolv.conf /var/chroot/trusty_x64/etc/resolv.conf`

*   You might want to have the proper sources.list in order to be able to install packages from Ubuntu official repositories like universe or multiverse, and the security updates. If you make a chroot installation, the sources.list will be the most basic one, like:

    `deb http://archive.ubuntu.com/ubuntu trusty main`

    You can generate a more complete sources.list file in this pages [Ubuntu](http://repogen.simplylinux.ch/) and [Debian](http://debgen.simplylinux.ch/)

##External Links

[Ubuntu official chroot manual](https://help.ubuntu.com/community/BasicChroot)

[Ubuntu official deboostrap manual](https://help.ubuntu.com/community/DebootstrapChroot)

[PerlGeek troubleshooting](http://perlgeek.de/en/article/set-up-a-clean-utf8-environment)

[Schroot conf manual](http://manpages.ubuntu.com/manpages/hardy/man5/schroot.conf.5.html)

[Debootstap manual](http://manpages.ubuntu.com/manpages/trusty/en/man8/debootstrap.8.html)

[Sources.list for Ubuntu](http://repogen.simplylinux.ch/)

[Sources.list for Debian](http://debgen.simplylinux.ch/)


# [Using robocompdsl, The command line component generator](/website/2015/05/23/robocompdsl/)

23 May 2015

**robocompdsl** is the new tool used in RoboComp to automatically generate components and modify their main properties once they have been generated (e.g., communication requirements, UI type). It is one of the core tools of the framework so, if you installed RoboComp, you can start using it right away.

This new version can only be used from the command line, but the languages used to define components and their interfaces remain mostly the same: **CDSL** to specify components and **IDSL** to specify interfaces. The only difference with the old RoboCompDSLEditor tool is that the reserved keywords (are now case independent). Take a look to the tutorial [“a brief introduction to Components”](components.md) for an introduction to the concept of component generation and the languages involved.

There are three tasks we can acomplish using **robocompdsl**:

*   generating a CDSL template file
*   generating the code for a previously existing CDSL file
*   regenerating the code for an already generated component.

## Generating a CDSL template file

Even tough writing CDSL files is easy –their structure is simple and the number of reserved words is very limited– robocompdsl can generate template CDSL files to be used as a guide when writing CDSL files.


```
$ robocompdsl path/to/mycomponent/mycomponent.cdsl

```


This will generate a CDSL file with the following content:


```
import "/robocomp/interfaces/IDSLs/import1.idsl";
import "/robocomp/interfaces/IDSLs/import2.idsl";

Component CHANGETHECOMPONENTNAME
{
	Communications
	{
		implements interfaceName;
		requires otherName;
		subscribesTo topicToSubscribeTo;
		publishes topicToPublish;
	};
	language Cpp;
	gui Qt(QWidget);
};

```


The CDSL language is described in the tutorial [“A brief introduction to Components”](components.md). Just don’t forget to change the name of the component.

## Generating a component given a CDSL file

Once we have our CDSL file we can generate the component’s source code running robocompdsl with the CDSL file as first argument and the directory where the code should be placed as the second argument.

From the component’s directory: $ cd path/to/mycomponent $ robocompdsl mycomponent.cdsl .

Or somewhere else: $ robocompdsl path/to/mycomponent/mycomponent.cdsl path/to/mycomponent

These commands will generate the C++ or Python code in the specified directory.

## Updating the source code of a component after modifying its CDSL file

Once we generated our component we might change our mind and decide to add a new connection to another interface or to publish a new topic. In these cases we can regenerate the code of the component just by changing the _.cdsl_ file and executing again the command.

As you might have learned from the tutorial [“A brief introduction to Components”](components.md) RoboComp components are divided in specific code (files where you write your code) and generic code (autogenerated code which doesn’t need to be edited). Running robocompdsl again on the same directory will ony overwrite these generic files. To ensure robocompdsl doesn’t overwrite the changes you made to the specific files these are left unchanged, so the component might not compile after regeneration (e.g., you might need to add new methods).


# [Packaging RoboComp](/website/2015/05/23/nithin3/)

23 May 2015

###deb packages

For creating a robocomp debian package :


```
cd ~/robocomp
mkdir build
cmake ..
make package

```


will create a .deb package in the build directory, which we can install using any packaging application like dpkg. To install the created package, just double click on it(open with Software Center) or in terminal type


```
sudo dpkg -i <packagename>.deb

```


###source packages for ppa

Launchpad will only accept source packages and not binary.Launchpad will then build the packages. For building source packages we are using debuild which is a wrapper around the _dpkg-buildpackage + lintian_. so you will need to install debuild and dput on your system.The source_package.cmake script is used to create debian source package.

The main CMakeLists.txt file defines a target `spackage` that builds the source package in build/Debian with `make spackage`

For uploading the package to ppa, First change the **PPA_PGP_KEY** in [package_details.cmake](../cmake/package_details.cmake#L26) to the contact of the PGP key details registered with your ppa account.Then create a source package by building the target _spackage_.Once the Source package is build successfully, upload it to your ppa by:


```
cd Debian/
dput ppa:<lp-username>/<ppa-name> package-source.changes

```


building of source package can be tested with:


```
cd Debian/robocomp-<version>
debuild -i -us -uc

```


####Note:

If you want to upload another source package to ppa which doesn’t have any changes in the source but maybe in the debian files. you can build the spackage after commenting out `set(DEB_SOURCE_CHANGES "CHANGED" CACHE STRING "source changed since last upload")` in [package_details.cmake](../cmake/package_details.cmake#L27) so that the the script will only increase the ppa version number and wont include the source package for uploading to ppa (which otherwise will give an error).

* * *

Nithin Murali


# [_GSoC,_ Building and deployment system design](/website/2015/05/23/nithin2/)

23 May 2015

###About Me:

Hi all , I am Nithin Murali and i would like to introduce me a little in this post. I am pursuing my engineering degree on Electrical Engineering from Indian Institute of Technology Bombay. I am working on an Autonomous Underwater Vehicle which we are developing for the RObosub competition. We are developing in ROS framework. That was my first introduction to robotic frameworks. I have read about Robocomop before but a real chance to contribute to this ambitious framework was brought to me by GSOC 2015\. I have Chosen the project _RoboComp Building and deployment system design_.

###About the project

Currently the the build system in Robocomp is not very efficient. It is limited only to the core libraries and some additional tools. So we will have to seprately build all the components one by one. Also as the number of component increases it will be more difficult to manage all of them. So I am planning to come up with an workspace model for Robocomp. It would accompany with various tools which will ease handling of components.

As of now the users have to build robocomp from source for using it. But there may be users who dont want to work on the framework but is only interested in developing new components. For such users it is important that that we should supply an compiled package (preferably debian package). Also package would be more accessible if we could provide an ppa for robocomp. So i am planning to package robocomp and also create a ppa for robocomp.

Currently Robocomp dosent have any tests written nor is it using any testinf framework. So one of my task would be decide on a testing framework/stragery and write tests for existing framework.

* * *

Nithin Murali


# [Debian Packaging 1](/website/2015/05/23/nithin1/)

23 May 2015

##What is a package By definition _Debian packages are standard Unix ar archives that include two tar archives optionally compressed with gzip (zlib), Bzip2, lzma, or xz (lzma2): one archive holds the control information and another contains the program data._ All debain packages should follow certain conventions. The root source directory should contain a directory named _debian_. This directory contains files which stores info about the package. These are the required files under the debian directory

*   **rules**
    This is the maintainer script for the package building. This script is run by the packaging application to build and install the source into a _tmp_ directory in the debian folder. It has the following Targets

    *   _clean target_ : to clean all compiled, generated, and useless files in the build-tree.

    *   _build target_ : to build the source into compiled programs and formatted documents in the build-tree.

    *   _build-arch target_ : to build the source into arch-dependent compiled programs in the build-tree.

    *   _build-indep target_ : to build the source into arch-independent formatted documents in the build-tree.

    *   _binary target_ : to create all binary packages (effectively a combination of binary-arch and binary-indep targets)

    *   _binary-arch target_ : to create arch-dependent (Architecture: any) binary packages in the parent directory.

    *   _binary-indep target_: to create arch-independent (Architecture: all) binary packages in the parent directory.

*   **changelog**
    This file contains the project changelog along with the project name , version and distribution and urgency of your package.

*   **compact**
    The compact file defines the debhelper compatibility level.

*   **debian/control**
    This file contains various values which dpkg, dselect, apt-get, apt-cache, aptitude, and other package management tools will use to manage the package. The control file describes the source and binary package, and gives some information about them, such as their names, who the package maintainer is, build and run dependencies and so on.

*   **copyright**
    This file contains information about the copyright and license of the upstream sources

*   **(pre/post)(inst/rm)**
    This are the scipts which are run before or after installation or removal of package.

Now once you have the source directory in the prescribed format. you will need a _.tar.gz_ archive of the source in the same folder.Then we can create a debian binary package using



```
debuild -i -us -uc -b

```


Or a debian source package using


```
debuild -i -us -uc -S

```


* * *

Nithin Murali
[Older](/webrobocomp.github.io/page13) [Newer](/webrobocomp.github.io/page11)