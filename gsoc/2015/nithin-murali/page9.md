# Till now ... after midterm

08 Aug 2015

Hi all , In this post i will talk about what i have been working on after midterm evaluation. I have spend my time working mostly on packaging supporting libraries for Robocomp.This includes FCL and libccd. FCL is a library for performing three types of proximity queries on a pair of geometric models composed of triangles. libccd is a library for collision detection between two convex shapes.Technically Robocomp is only using FCL but libccd is an dependency of fcl, as i couldn’t find an updated ppa for it i decided to package it too. you can see those packages [here](https://launchpad.net/~imnmfotmal)

Also i added ability for generating robocomp source packages for different distributions. Initially i added the option only for trusty, now we could generate packages for any distribution.I worked a bit on build tools also. Currently if someone created an workspace and made a github repo of it, some one else cant use it as we store the repo names in ~/.config directory, so i added an option to reinit an repo.

well i guess thats all for now

* * *

Nithin Murali