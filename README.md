Guide creation date: 27-Oct-2021 
# Step 2: Upgrading the Python version on a Raspberry Pi
This guide explains how you can upgrade Python 3 on a Raspberry Pi.

Previous topic: [Step 1: Initial setup of the Raspberry Pi.](https://github.com/JurgenVanGorp/Step1-Setting-up-the-Raspberry-Pi)

## PREREQUISITES

Before starting here, make sure you have a Raspberry Pi with a basic Raspbian OS installed. You can follow [Step 1: setting up the Raspberry Pi](https://github.com/JurgenVanGorp/Step1-Setting-up-the-Raspberry-Pi) if you still need to do that.

## ASSUMPTIONS

* You have some experience with Raspberry Pi (further also abbreviated to RPi because I'm lazy). Upgrading Python requires you to compile the latest version, and you may need some debugging skills to e.g. find missing libraries that you still need to install.
* Beware: this installation instruction was updated on 30-Oct-2021. Times change, and this information may be outdated when you read it.
* This instruction was created using a Raspberry Pi 3 B+

## Introduction

The native Raspberry Pi OS may come with an older Python version, while e.g. Home-Assistant is using more recent versions. You will need to upgrade Python, or the HA installation will fail.

**Reminder**: It is expected that you have a RPi installed and ready to use. Enabling SSH on the Raspberry Pi, and connecting to the RPi with SSH or [WinSCP on Windows](https://winscp.net/eng/index.php) makes it easier to copy-paste some of the commands below.

## Pre-Install necessary packages

If you haven't done so yet, please make sure to have the latest patches and packages installed.

```
sudo apt update -y
sudo apt upgrade -y
```

... and if you are the suspicious type:

```
sudo reboot
```

Install the following packages that are required when compiling a new version of Python 3.

```
sudo apt install libssl-dev
sudo apt-get install build-essential libssl-dev libffi-dev python-dev
sudo apt-get install libbz2-dev liblzma-dev libsqlite3-dev libncurses5-dev libgdbm-dev zlib1g-dev libreadline-dev libssl-dev tk-dev uuid-dev libffi-dev
sudo apt-get install libgdbm-compat-dev
```

**Remember**: you may need to install more (or different versions) of the packages. Make sure to verify the logs for warnings and errors when doing the compilation later on.

## Get the latest Python 3 version

**KUDOS**: I freely used information [from this forum](https://www.raspberrypi.org/forums/viewtopic.php?p=1761359#p1761359) to create the following step-by-step guide.

Download the latest Python 3 (!) version from this location: [https://www.python.org/downloads/source/]. At time of writing this was version 3.10.0.

**Important**: Make sure to download the tarball.

**Beware**: Change the Python version to the one you want to download. The number in the line below may therefore need to be changed.

```
wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
```

### Compile and install Python

Unpack the tarball. Again, change the version number if you are installing a different version as the below.

```
tar xf Python-3.10.0.tgz
```

Go into the directory. Optimize for your system, start the compilation and install.

```
cd Python-3.10.0
./configure --enable-optimizations
make -j 4
sudo make install
```

**Beware**: the compilation takes *really* long. Go for a coffee.

## Set the new Python 3 version as the default

**Kudos**: It took me some time to find this out. My gratitude goes to [the information here](https://linuxconfig.org/how-to-change-from-default-to-alternative-python-version-on-debian-linux) and [some more here](https://stackoverflow.com/questions/62275714/how-to-change-the-default-python-version-in-raspberry-pi). 

There was already a Python installed on your RPi. When "just" starting Python, it will still use the old version. This now needs to be updated.

**Attention**: Don't forget to change the "python3.10" to the version you are installing if it is different.

```
sudo update-alternatives  --install  /usr/bin/python  python  /usr/local/bin/python3.10   1
```

Verify the Python versions with.

```
python --version
python3 --version
pip3 --version
```

All three commands should refer to the new versions you have installed. If that is the case ... then congrats: you have upgraded to the newest Python version.

And just to be on the safe side ...

```
sudo apt update -y
sudo apt upgrade -y
sudo reboot
```

Next topic: [Step 3: Setting up Home Assistant native on the Raspberry Pi.](https://github.com/JurgenVanGorp/Step3-Home-Assistant-on-Raspberry-Pi-Native)
