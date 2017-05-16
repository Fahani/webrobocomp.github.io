# Debian Packaging 2

12 Jun 2015

##What is a binary package? A binary package in a is an application package which contains (pre-built) executables, as opposed to source code. Basically a binary package is an archive which contains executables some other info like rules on how to install them, dependencies etc. debian binary package is also a type of binary package. You can use a package manger to install these packages.I have explained I have explained it in tutorial [Debian packaging](http://robocomp.github.io/website/2015/05/23/nithin1.html).

##Implementation in Robocomp For binary packages i was left with two options. whether i could use the same cmake script i used for creating source packages or i could use the `CPACK` packaging tool. Finally i decided to go with CPACK because - 1)less code , that means less messing up 2)its an well known tool so is expected to perform better than a script i am writing. CPACK has so many configuration options so i made a seperate cmake file `package_details.cmake` for configuring cpack so that its easier for users to change any configuration. CPACK will add a target `package` for generating binary package. so you could run `make package` for generating the package.

##Source packages and ppa A Personal Package Archive (PPA) s a special software repository for uploading source packages to be built and published as an APT repository by Launchpad.So basically if a software has a ppa then users can just add the pa to their sources and they will be able to install the software package and will also get updates automatically. As alreasy mentioned we can only upload source packages into a ppa, by definition _Source packages provide you with all of the necessary files to compile or otherwise, build the desired piece of software._ now the next question is how can we create source packages. I have explained it in tutorial [Debian packaging](http://robocomp.github.io/website/2015/05/23/nithin1.html).

##Implementation in Robocomp For creating source package for robocomp i wrote a cmake module _source_package_.The module will basically copy the source in to another directory (currently _Debian_ in build folder) and will create the source tar.Then it will create all debian/ files dynamically. The script will be executed when the target `spackage` is made.

After creating the source packages one trouble i faces was in setting up (registering) the PGP keys. Once you have created a launchpad account you should sign the Ubuntu Code of Conduct.Then you can upload the package using `dput` utility.

###NB

*   Unfortunately CPACK has a bug in it, its not changing the control files permission correctly which throws a warning during installation. so i have create a bash script which will fix the control file permissions.

*   You cant upload a package with same name into same ppa again, launchpad will reject it. so you need to change the package name and hence the version, every time you upload. This is automatically taken care off by the script.

*   If there are any changes to the source, then you should upload the whole source into the ppa. Well, any changes to fies in _debian_ folder is not considered a source change. In Robocomp its implemented in such a way that if there is any changes to the source, then you have to change the Robocomp version.

*   Right now we are not generating the changelog automatically. But that is a feature we could add, generating changelog from the git commit messages.

* * *

Nithin Murali