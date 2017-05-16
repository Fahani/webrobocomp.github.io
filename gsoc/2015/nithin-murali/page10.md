# Packaging FCL and libccd

21 Aug 2015

I assume that you have an debian package folder in your source directory. If not refer to the intro to debian packaging tutorial.

Now lets assume that you are going to upload the package for the first time into a ppa.

###Creating source package

*   Rename the source directory into <project_name>- <version>eg libccd-2.01</version></project_name>
*   Crate a .tar.gz compressed file of the source directory, and place it outside source directory
*   Rename the compressed file into <project_name>_<version>.orig.tar.gz</version></project_name>
*   Now run `debuild -k<gpg_key> -S -sa` if you want to include the whole source tar in upload
*   Or run `debuild -k<gpg_key> -S -sd` if you don’t want to include the whole source tar in upload

###So when should you upload the source? when you are uploading for the first time (obviously) and whenever you make some changes to your source code. but as launchpad wont allow files with same name, you should increase your version number so that the source tar get a new name. For increasing the version number you should increase it in changelog for example in this case `fcl (1.0-0ppa0) vivid; urgency=low` increase 1.0-0ppa0 to 1.1-0ppa0 also remember to rename all names accordingly (source folder and source tar)

but if you have only changed some file in the debian directory. For example, edited changelog or added a dependency. In those cases you can skip the source upload but in such cases you have to increase your ppa number in your changelog for example in this line `fcl (1.0-0ppa0) vivid; urgency=low` change 0ppa0 to 0ppa1.

###uploading Now once you have generated the .source_changes file use dput to upload


```
dput ppa:<your-lp-id>/<name> <file_name>.source_changes

```


###NB

*   if you want to add any dependency, edit the file debian/control add add it to `Depends` or `Build-Depends` field
*   if you want to change the target generation , edit the distribution name in first line of debian/changelog

* * *

Nithin Murali