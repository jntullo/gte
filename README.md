# GTE - Game Theory Explorer

The Game Theory Explorer (GTE) is a graphical user interface that allows the
interactive construction of small to medium size games in extensive and
strategic form, and to compute equilibria of these games.

Please see http://www.gametheoryexplorer.org/

GTE is part of the [Gambit Project](http://www.gambit-project.org) - a library
of game theory software.

It is written by

* [Alfonso](https://github.com/alfongj)
* [Mark Egesdal](https://github.com/megesdal)
* [Martin Prause](https://github.com/trobar)
* [Rahul Savani](https://github.com/rahulsavani)
* [Bernhard von Stengel](https://github.com/stengel) 

This version of GTE is designed to be portable across platforms and runs on
Linux, Mac OS X, and Windows.

The GUI is currently implemented as a Flash program in Actionscript (files
ending in .as); we refer to this as the client.  The game generated by the
client GUI is stored in a file format and sent to the server.

On the server side, that file is read in, and an algorithm started, for
example, to find one or all Nash-Equilibria of the game.  This algorithm maybe
be a program written in some native code, in particular C for the lrs program.
The output of this algorithm is then sent back to the client to be displayed as
a solution.

These issues are addressed as follows:
- a local server is started called Jetty
- the client communicates with that local server on your machine
- all native algorithms also run locally

![GTE Mainwindow](https://github.com/downloads/trobar/gte/gte_small.png)

## Preface

GTE can be installed in three different ways:

1. Compiled version (PRODUCTIVE), ready to run on an installed Jetty Server
2. Client Development version (CLIENT) for changing the GUI written in
   ActionScript
3. Server Development version (SERVER) for changing servlets or intergrating
   algorithms written in Java or native code.

**This README file describes how to setup your environment to develop the SERVER version**

This requires a number of installations:

1. Java for the server code
2. ANT (a make-like utility for compilation)
3. Jetty
4. Flex SDK (for Actionscript)
5. C compiler for the native code.

After all these installations, any update of the project only requires
re-compiling the server and client.

In the instructions that follow you are told to set environment variables like
FLEX_HOME and JETTY_HOME. These should be set LOCALLY on your computer and stay
there in the file build.properties (see example in repository).

If you want to contribute to the gte project, the first thing you need to do is
to follow these instructions on how to build and deploy the war file gte.war.

## 1. Install Java SDK

1. Download Java SDK (at least Java SE 6) at:
   [http://www.oracle.com/technetwork/java/javase/downloads/index.html]
   (http://www.oracle.com/technetwork/java/javase/downloads/index.html "Java Download")
   ===WINDOWS===
   1.1 Download - http://www.oracle.com/technetwork/java/javase/downloads/
   1.2 Add bin directory to path
   1.3 Add JAVA_HOME environment variable (makes sure it points to the SDK, not JRE)
   ===LINUX===
   $ sudo apt-get install sun-java6-jdk
   ===OS X===
   OS X 10.6 and 10.7 with a Java SDK suitable for gte.  
   Confirm you have the latest version by running "Software update..." from the Apple menu.

2. Set the environment variable JAVA_HOME to the installed JAVA SDK

2. Install ANT ##

OS X 10.6 and 10.7 ships with ant (in /usr/bin/ant)

**LINUX/WINDOWS:**

1. Download ANT at: [http://ant.apache.org/](http://ant.apache.org/ "ANT")
2. Extract the downloaded archive
3. Set the environment variable ANT_HOME to the extracted ANT directory
4. Add the **bin** directory to your PATH environment variable. See install
   instructions at: [http://ant.apache.org/manual/install.html]
   (http://ant.apache.org/manual/install.html "Install ANT")


## 3. Install Jetty

**LINUX/OSX:**

1. `JETTY_VERSION=7.6.1.v20120215`
2. `wget http://download.eclipse.org/jetty/$JETTY_VERSION/dist/jetty-distribution-$JETTY_VERSION.tar.gz`
3. `tar xfz jetty-distribution-$JETTY_VERSION.tar.gz`
4. Set the environment variable JETTY_HOME to the extracted JETTY directory
5. Check Jetty with executing in jetty directory: `java -jar start.jar`
6. Access JETTY at: 127.0.0.1:8080
7. Terminate JETTY with CTRL-C

**WINDOWS:**

1. Download JETTY Version 7.6.1 or above at: [http://download.eclipse.org/jetty/]
   (http://download.eclipse.org/jetty/ "Jetty Download")
2. Extract archive
3. Open console and navigate to extracted directory
4. Set the environment variable JETTY_HOME to the extracted JETTY directory
5. Check Jetty with executing in jetty directory: `java -jar start.jar`
6. Access JETTY at: 127.0.0.1:8080
7. Terminate JETTY with CTRL-C

**Applied to SERVER and CLIENT**

## 4. Install Flex SDK (Adobe Flex SDK, NOT Open Source Flex SDK)

1. Download Flex SDK (Version 4.1A) at: [http://sourceforge.net/adobe/flexsdk/wiki/Download%20Flex%204/]
   (http://sourceforge.net/adobe/flexsdk/wiki/Download%20Flex%204/ "FLEX SDK")
2. Extract archive
3. Set the environment variable FLEX_HOME to the extracted FLEX directory
4. Add the **bin** directory to your PATH environment variable.

You may need to complete the following additional steps on Linux/OSX:

4b. Install "gflashplayer"

**Linux**
4b.1 cd ${FLEX_HOME}/runtimes/player/10.1/lnx
4b.2 tar -zxvf flashplayerdebugger.tar.gz
4b.3 ln -s ./flashplayerdebugger ./gflashplayer
4b.4 Add directory from [4b.1] to PATH

**OS X**
In ${FLEX_HOME}/runtimes/player/10.1/mac there is a DMG file,
"Install Adobe Flash Player Debugger 10.1.dmg".
Double-click in Finder to mount the disk image, which contains
an installer, "Install Adobe Flash Player Debugger".  Double-click
the installer to run.

If you have Adobe Flash Player newer than 10.1 installed, the
installer will complain and terminate.  In this instance, the
Flash Player Debugger.app is also present in the folder.
Double-click to unzip, and drag the application the the
Applications folder (or wherever you want to put it).

By now you should have 4 environment variables: `JAVA_HOME, ANT_HOME, JETTY_HOME and FLEX_HOME`
pointing to the corresponding directories.

## 5. Run the Server version

**LINUX:**

Install gcc as a C-compiler (Ubuntu):

* `sudo apt-get clean`
* `sudo apt-get update`
* `sudo apt-get upgrade`
* `sudo apt-get install build-essential`

**MAC:**

tbc

**WINDOWS:**
Download cygwin (setup.exe) from: [http://www.cygwin.com/](http://www.cygwin.com/ "Cygwin").
Start the setup.exe and install the following packages:

* **Devel:** gcc-mingw-g++ (20050522-3)
* **Devel:** gcc-mingw-core (20050522-3)
* **Devel:** libgcc1 (4.5.3-3)
* **Devel:** make (3.82.90-1)
* **Lib:** libgcc1 (4.5.3-3)

Choosing these packages implies that setup.exe detects dependencies and autmatically will install the following packages on its own:

* binutils (2.22.51-2)
* gcc-core (3.4.4-999)
* gcc-g++ (3.4.4-999)
* libintl3 (0.14.5-1)
* mingw-runtime (3.20-1)
* mingw-w32api (3.17-2)
* w32api (3.17-2)

After setup completes add the **bin** directory of your cygwin install dir (e.g. c:\cygwin\bin) to your PATH environment variable.

**LINUX/MAC/WINDOWS:**

1. From the root directory of the respository (which contains build.properties), execute the `compileComplete` ANT-target with

		ant compileComplete

   The target compiles the servlets, FLEX GUI, native Code, packs the compiled code into a War-file and deploys it to JETTY (**webapps**) and starts the JETTY server. (TODO: Add info on other targets such as makeWarFile.)
2. Access GTE at: `127.0.0.1:8080/gte`
3. Stop the JETTY server using the ANT-target **jetty-stop**

If http://127.0.0.1:8080/gte/ ever shows the browser message

    Unable to connect
    Firefox can't establish a connection to the server at
    127.0.0.1:8080.

then the jetty server is down, and you need to start it (in JETTY_HOME) with

     java -jar start.jar

Or if you want to start jetty so that it can easily be stopped then use

	java -DSTOP.PORT=8079 -DSTOP.KEY=secret -jar start.jar &

And then to stop it

	java -DSTOP.PORT=8079 -DSTOP.KEY=secret -jar start.jar --stop

If you get an error while starting JETTY that says you do not have a Java SDK installed,
please follow the instructions here:
[http://wiki.eclipse.org/Jetty/Howto/Configure_JSP](http://wiki.eclipse.org/Jetty/Howto/Configure_JSP
"Jetty No Java SDK Help"). In general it says that you have to
add `-Dorg.apache.jasper.compiler.disablejsr199=true` to the **Jetty.xml** (JETTY 7.5.0 and prior)
or uncomment this line in **start.ini** (JETTY 8.X and later).

## 6. Additional information

Please find additional instructions on how to include your native algorithm into the SERVER version on GitHub.

The CLIENT version is only for developing the GUI. Please find details on developing the CLIENT verison with Eclipse in README-CLIENT.
