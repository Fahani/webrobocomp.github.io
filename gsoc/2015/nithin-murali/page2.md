# Debian Packaging 1

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