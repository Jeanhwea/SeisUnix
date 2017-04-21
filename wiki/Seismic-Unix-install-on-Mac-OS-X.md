Seismic Unix install on Mac OS X

Installing Seismic Unix on Mac OS X is similar to that on any UNIX platform. However, the typical installation of Mac OS X for most users may not include some necessary components. A few Mac OS X routines do not support options that are standard on other systems. The instructions below have been tested on Mac OS X 10.7.2 Lion but they should also apply to other versions of the OS.
Contents

    1 Install Xcode
    2 Download SU from CWP website
        2.1 Local installation
        2.2 Global installation
    3 Edit the SU configuration file
    4 Install the desired components
    5 Test the installation

Install Xcode

On the Mac OS X system, all of the developer materials (make, gcc, gcc libraries, etc..) are part of a developer package called Xcode. Xcode is a suit of software developing tools for Mac OS X. Mac OS X install DVDs typically contain Xcode. However, most new machine purchases do not include the DVDs. You can download Xcode free of charge directly from Apple's website. You have to register as a developer to do the download. Once Xcode is installed, run the Xcode utility, and go to ​Preferences → Downloads → Components​. Install ​Command Line Tools​, which include make, gcc and the necessary libraries.

Check that the Xcode has been installed correctly by launching a Terminal window and typing
```sh
gcc -v
```

Alternatively, you may type
```sh
which make
```

to see if the make utility is present.
Download SU from CWP website

In what follows Seismic Unix will be downloaded and installed into the current user's home directory. It is possible to install SU globally using ​sudo​ but it is not recommended. If you prefer to install SU globally, follow the instructions below.
Local installation

First, download the compressed tar-ball and unpack it to the home directory.
```sh
curl ftp://ftp.cwp.mines.edu/pub/cwpcodes/cwp_su_all_43R1.tgz -o ~/Downloads/cwp_su_all_43R1.tgz
mkdir ~/cwp/43R1
gunzip ~/Downloads/cwp_su_all_43R1.tgz
tar -xvf ~/Downloads/cwp_su_all_43R1.tar -C ~/cwp/43R1/
```
Add new path variables to the configuration file and make the new settings active. To do that type in Terminal:
```sh
echo "export CWPROOT=~/cwp/43R1/" >> ~/.bashrc
echo "export PATH=$PATH:~/cwp/43R1/bin" >> ~/.bashrc
source ~/.bashrc
```
Global installation

To set the system up as it is set up at CWP, you may do the following
```sh
sudo mkdir /usr/local
sudo mkdir /usr/local/cwp
sudo chown yourusername /usr/local/cwp
```
After the directories have been created, download the gzipped tar archive of the CWP/SU materials into /usr/local/cwp and unbundle that there via
```sh
gunzip cwp_su_all_43R1.tgz
tar -xvf cwp_su_all_43R1.gz
```
Add the following lines to the file .profile that is in your home directory
```sh
export CWPROOT=/usr/local/cwp
export PATH=$PATH:$CWPROOT/bin:.
```
Note that under bash, usually there are two files .bashrc and .bash_profile, or .profile. This path information may need to exist in one or all of these files.
Edit the SU configuration file

Edit the file located at $CWPROOT/src/Makefile.config. The following is an example. You may wish to use other settings.
```sh
SHELL = /bin/sh
LN = /bin/ln
ROOT = $(CWPROOT)
ENDIANFLAG = -DCWP_LITTLE_ENDIAN
LARGE_FILE_FLAG = 
LINEHDRFLAG = 
OPTC = -g  -std=c89
POSTLFLAGS = 
include $(CWPROOT)/src/Rules/gnumake.rules
CC = cc
CFLAGS = -I$I $(OPTC) $(LARGE_FILE_FLAG) $(ENDIANFLAG) $(XDRFLAG) $(LINEHDRFLAG)
C++ = c++
C++FLAGS = -I$I $(OPTC) $(LARGE_FILE_FLAG) $(ENDIANFLAG) $(XDRFLAG)
LD_LIBRARY_PATH += $(CWPROOT)/lib
AR = ar
ARFLAGS = rv
RANLIB = ranlib
RANFLAGS = 
ICHMODLINE = chmod 664 $@
MCHMODLINE = chmod 775 $@
CPP = /usr/bin/cpp
OPTF = -g
FC = gfortran
include $(CWPROOT)/src/Rules/abbrev.rules
IX11 = /usr/X11/include
LX11 = /usr/X11/lib
IMOTIF = /usr/X11/include
LMOTIF = /usr/X11/lib
include $(CWPROOT)/src/Rules/cflags.rules
include $(CWPROOT)/src/Rules/suffix.rules
include $(CWPROOT)/src/Rules/misc.rules
```
# Another possibility for Mac OS X - Lion 
```sh
include $(CWPROOT)/src/Rules/gnumake.rules
include $(CWPROOT)/src/Rules/abbrev.rules
include $(CWPROOT)/src/Rules/cflags.rules
include $(CWPROOT)/src/Rules/suffix.rules
include $(CWPROOT)/src/Rules/misc.rules
include $(CWPROOT)/src/Rules/opengl.rules


LINEHDRFLAG = 
XDRFLAG = -DSUXDR
ENDIANFLAG = -DCWP_LITTLE_ENDIAN
#LARGE_FILE_FLAG = -D_FILE_OFFSET_BITS=64 -D_LARGEFILE64_SOURCE -DGNU_SOURCE
LARGE_FILE_FLAG =

CWP_FLAGS = $(LARGE_FILE_FLAG) $(ENDIANFLAG) $(XDRFLAG) $(LINEHDRFLAG)

SHELL = /bin/sh
ROOT = $(CWPROOT)

LN = ln  # this needs to be changed to cp for FAT32 filesystems
AR = ar
ARFLAGS = rv
RANLIB = ranlib
RANFLAGS = 
ICHMODLINE = chmod 644 $@
MCHMODLINE = chmod 755 $@
#-----------------------------------------------------------------------
# use both X11 path conventions
#-----------------------------------------------------------------------

IX11 = /usr/X11/include
LX11 = /usr/X11/lib
IMOTIF = /usr/X11R6/include
LMOTIF = /usr/X11R6/lib

LD_LIBRARY_PATH += $(CWPROOT)/lib:${LX11}:${LMOTIF}

#-----------------------------------------------------------------------
# Gnu compilers by default just because they are the most common
#-----------------------------------------------------------------------

CPP = cpp

CC = /usr/bin/gcc
OPTC = -g -c90  -m64 -Wall -ansi  -Wno-

FC = gfortran
FOPTS = -g 
FFLAGS = $(FOPTS) -ffixed-line-length-none

C++FLAGS = -I$I $(OPTC) $(CWP_FLAGS) long-long 
CFLAGS = -I$I $(OPTC) $(CWP_FLAGS)


# fortran, if gfortran is installed

FC = gfortran
FOPTS = -g 
FFLAGS = $(FOPTS) -ffixed-line-length-none

C++FLAGS = -I$I $(OPTC) $(CWP_FLAGS)
```

Install the desired components
```sh
cd $CWPROOT/src
make install 
make xtinstall 
make utils
make xminstall
```
Test the installation

Run X11, which is typically located in /Applications/Utilities. Execute the following command:
```sh
suplane | suximage title="My first test" &
```
If a figure appears then the installation has been done correctly. If you run this command in Terminal then X11 will start automatically, but it will take a few seconds longer.
v · d · e Seismic Unix install
Platform	

    Linux (Ubuntu, Archlinux) Mac OS X Solaris Windows

Categories: Installation | Mac OS X

    Log in

    Page
    Discussion

    Read
    View source
    View history

Navigation

    Main page
    Community portal
    Developers portal
    Recent changes

User support

    Official documentation
    How to install
    List of programs
    The guide

Toolbox

    What links here
    Related changes
    Special pages
    Printable version
    Permanent link

    This page was last modified on 24 December 2012, at 01:09.
    This page has been accessed 7,306 times.
    Most content is available under Creative Commons Attribution Share Alike, Some Restrictions Apply.
