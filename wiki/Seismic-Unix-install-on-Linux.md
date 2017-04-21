This page is intended to be a generic workflow for how to manually install Seismic Unix from Source on a modern Linux System. Specific instructions on [[Seismic Unix install on Ubuntu|installing Seismic Unix on Ubuntu]] are also available. Archlinux users [[Seismic Unix install on Archlinux|may install Seismic Unix]] directly from AUR.

# How to get it

You can download the latest version of the source code from [The CWP Website](http://www.cwp.mines.edu/cwpcodes/) or type in a terminal window:
```sh
wget ftp://ftp.cwp.mines.edu/pub/cwpcodes/cwp_su_all_43R1.tgz
```

# Prepping

## Untar
You need to select where you want to install for this page lets say you want to use /opt/cwp/42/ for this page.
```sh 
tar -xvf cwp_su_all_43R1.tgz -C /opt/cwp/43R1/
```

## Setting Environment Variables
These Instructions are specific to bash, if your a csh or tsh guy some modification might be needed.

### CWPROOT
$CWPROOT needs to be set to the directory where you untarred the code.
```sh
export CWPROOT=/opt/cwp/43R1
```


### PATH

You need to add $CWPROOT/bin to your systems working shell path 
```sh
export PATH=$PATH:$CWPROOT/bin
```

### .bashrc

Adding the above variables to .bashrc should allow you to actually use Seismic Unix next time you start a console window.
Think of it like adding it to the startup routine
```sh
echo "export CWPROOT=/opt/cwp/43R1" >> ~/.bashrc
echo "export PATH=$PATH:$CWPROOT/bin" >> ~/.bashrc
```

## Required Packages

list of packages

Redhat Example
```sh
yum install gcc gcc* libx* openmotif openmotif* freeglut-devel
```


### Ubuntu 11.10

```sh
sudo apt-get update
sudo apt-get install libx11-dev libc6 freeglut3-dev gfortran libmotif4 libxt6 x11proto-print-dev libmotif-dev
```

## Modifying the Makefile
Auto Configure might be coming, on Centos 5 I had to change c++ and cc to g++ and gcc. With Ubuntu I was able to install with no changes.
you can find these settings in Makefile.config


# Compiling
The Main Install Requirements
```sh
cd $CWPROOT/src
make install
make xtinstall
make finstall 
make mglinstall
make utils
make xminstall
make sfinstall
```

# Testing the install

```sh
suplane | suximage title="My First Plot"
```

