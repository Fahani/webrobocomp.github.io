# Packaging RoboComp

<span class="post-date">23 May 2015</span>

###deb packages

For creating a robocomp debian package :

<div class="highlighter-rouge">

```
cd ~/robocomp
mkdir build
cmake ..
make package

```

</div>

will create a .deb package in the build directory, which we can install using any packaging application like dpkg. To install the created package, just double click on it(open with Software Center) or in terminal type

<div class="highlighter-rouge">

```
sudo dpkg -i <packagename>.deb

```

</div>

###source packages for ppa

Launchpad will only accept source packages and not binary.Launchpad will then build the packages. For building source packages we are using debuild which is a wrapper around the _dpkg-buildpackage + lintian_. so you will need to install debuild and dput on your system.The source_package.cmake script is used to create debian source package.

The main CMakeLists.txt file defines a target `spackage` that builds the source package in build/Debian with `make spackage`

For uploading the package to ppa, First change the **PPA_PGP_KEY** in [package_details.cmake](../cmake/package_details.cmake#L26) to the contact of the PGP key details registered with your ppa account.Then create a source package by building the target _spackage_.Once the Source package is build successfully, upload it to your ppa by:

<div class="highlighter-rouge">

```
cd Debian/
dput ppa:<lp-username>/<ppa-name> package-source.changes

```

</div>

building of source package can be tested with:

<div class="highlighter-rouge">

```
cd Debian/robocomp-<version>
debuild -i -us -uc

```

</div>

####Note:

If you want to upload another source package to ppa which doesn’t have any changes in the source but maybe in the debian files. you can build the spackage after commenting out `set(DEB_SOURCE_CHANGES "CHANGED" CACHE STRING "source changed since last upload")` in [package_details.cmake](../cmake/package_details.cmake#L27) so that the the script will only increase the ppa version number and wont include the source package for uploading to ppa (which otherwise will give an error).

* * *

Nithin Murali