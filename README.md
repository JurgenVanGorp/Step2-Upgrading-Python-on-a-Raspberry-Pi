Guide creation date: 27-Oct-2021 
# Step 2: Upgrading the Python version on a Raspberry Pi
This guide explains how you can upgrade Python 3 on a Raspberry Pi.

Previous topic: [Step 1: Initial setup of the Raspberry Pi.](https://github.com/JurgenVanGorp/Step1-Setting-up-the-Raspberry-Pi)

## PREREQUISITES

Before starting here, make sure you have a Raspberry Pi with a basic Raspbian OS installed. You can follow [Step 1: setting up the Raspberry Pi](https://github.com/JurgenVanGorp/Step1-Setting-up-the-Raspberry-Pi) if you still need to do that.

## ASSUMPTIONS

* You have some experience with Raspberry Pi (further also abbreviated to RPi because I'm lazy). Upgrading Python requires you to compile the latest version, and you may need some debugging skills to e.g. find and install missing libraries.
* Beware: this installation instruction was updated on 30-Oct-2021. Times change, and this information may be outdated when you read it.
* This instruction was created using a Raspberry Pi 3 B+. It is expected it will work on RPi models 2 to 4.

## Introduction

The native Raspberry Pi OS may come with an older Python version, while e.g. Home-Assistant is using more recent versions. You will need to upgrade Python, or the HA installation will fail.

**Reminder**: It is expected that you have a RPi installed and ready to use. Enabling SSH on the Raspberry Pi, and connecting to the RPi with SSH or [WinSCP on Windows](https://winscp.net/eng/index.php) makes it easier to copy-paste some of the commands below.

## Pre-Install necessary packages

If you haven't done so yet, please make sure to have the latest patches and packages installed.

```
sudo apt update -y
sudo apt upgrade -y
```

... and if you are of the suspicious type:

```
sudo reboot
```

Install the following packages that are required when compiling a new version of Python 3.

```
sudo apt install libssl-dev -y
sudo apt-get install build-essential libssl-dev libffi-dev python-dev -y
sudo apt-get install libbz2-dev liblzma-dev libsqlite3-dev libncurses5-dev libgdbm-dev zlib1g-dev libreadline-dev libssl-dev tk-dev uuid-dev libffi-dev -y
sudo apt-get install libgdbm-compat-dev -y
```

**Remember**: you may need to install more (or different versions) of the packages. Make sure to verify the logs for warnings and errors when doing the compilation later on.

## Get the latest Python 3 version

**KUDOS**: I freely used information [from this forum](https://www.raspberrypi.org/forums/viewtopic.php?p=1761359#p1761359) to create the following step-by-step guide.

Download the latest Python 3 (!) version from this location: [https://www.python.org/downloads/source/].

**WARNING**: At time of writing Python 3.10.0 was the latest version, but e.g. cryptography was not yet updated to the newest version. Since Home-Assistant uses the cryptography module, this version could not be used yet. In the below example I am therefore reverting to V3.9.7.

Be careful: change the Python version to the one you want to download. The version numbers in folder and file names below may therefore need to be updated, but at time of writing this version worked for setting up Home-Assistant in the next step.

```
wget https://www.python.org/ftp/python/3.9.7/Python-3.9.7.tgz
```

You should see something like.
```python
Python-3.9.7.tgz        100%[============================>]  24.56M  9.57MB/s    in 2.6s

2021-10-29 19:16:27 (9.57 MB/s) - ‘Python-3.9.7.tgz’ saved [25755357/25755357]
```

### Compile and install Python

Unpack the tarball. Again, change the version number if you are installing a different version as the below.

```
tar xf Python-3.9.7.tgz
```

Go into the directory. Optimize for your system, start the compilation and install.

**Beware**: the *make* step includes a lenghty test cycle, and takes *really* long (40 minutes on an RPi 3B+). Go for a coffee.

```
cd Python-3.9.7
./configure --enable-optimizations
make -j 4
sudo make install
```

## Set the new Python 3 version as default

**Kudos**: It took me some time to find this out. My gratitude goes to [the information here](https://linuxconfig.org/how-to-change-from-default-to-alternative-python-version-on-debian-linux) and [some more here](https://stackoverflow.com/questions/62275714/how-to-change-the-default-python-version-in-raspberry-pi). 

The default Raspberry Pi OS already has an older Python version installed. When giving the *python* command, the old version may still be used as the default, so pointers need to be updated.

**Attention**: Don't forget to change the "python3.9" to the version you are installing if it is different.

```
sudo update-alternatives  --install  /usr/bin/python  python  /usr/local/bin/python3.9  1
```

Verify the Python versions with.

```
python --version
python3 --version
pip3 --version
```

All three commands should refer to the new version you have installed. If that is the case ... then congrats: you are ready for the next step.

And just to be on the safe side ...

```
sudo apt update -y
sudo apt upgrade -y
sudo reboot
```

Next topic: [Step 3: Setting up Home Assistant native on the Raspberry Pi.](https://github.com/JurgenVanGorp/Step3-Home-Assistant-on-Raspberry-Pi-Native)
