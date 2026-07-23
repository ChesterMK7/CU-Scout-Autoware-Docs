---
title: Setup Scripts
parent: Software
nav_order: 14
---

# Setup Scripts

The [Scout Setup Scripts](https://github.com/ChesterMK7/autoware-scout-setup-scripts) are a set of ```bash``` scripts used for setting up both the host OS and Autoware Docker container for use with our hardware setup. While many of the scripts have comments explaining what each line does, this page serves to elaborate more on why each command is important.

## Host Scripts

These are bash scripts meant to be run on the host OS, contained within the ```host-scripts``` directory in the repo.

### host-setup.bash

```bash
#/bin/bash

# This script is intended to be used on Ubuntu 22.04
# It may work on 22.04 derivatives or other Ubuntu releases, but it has not been tested on them

# Copy docker setup scripts to home (will be moved into autoware dir later)
cp -r ../docker-scripts ~/
# Everything will be installed to the home directory
cd ~/
```

Comments here are pretty self-explanitory. Note that the first line (#/bin/bash) is important, telling the OS what binary to execute the file with (in this case, bash, the default terminal shell of many Linux distributions).

```bash
# Install Nvidia Drivers
# Commands commands from https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/ubuntu.html
# Repo Setup
sudo apt update
sudo apt upgrade -y
sudo apt install -y linux-headers-$(uname -r)
# If you are on a different Ubuntu release, change '2204' to the respective release version
# Ex. 2404 for Ubuntu 24.04
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt update
# Install driver package (should be version 610)
sudo apt install -y cuda-drivers
```

While Nvidia drivers *are* in the default Ubuntu apt repos, the versions there in 22.04 are too old to work with the CUDA and TensorRT (Nvidia dev libaries for Autoware) that the Docker image requires. This process installs the newest version of Nvidia drivers avaliable, which as of writing is 610.

```bash
# Install Autoware 
# Commands sourced from https://autowarefoundation.github.io/autoware-documentation/main/installation/autoware/docker-installation/
# Pull Autoware repo
sudo apt install -y git
git clone https://github.com/autowarefoundation/autoware.git
cd autoware
# Install Ansible
bash ansible/scripts/install-ansible.sh
# Run Docker Playbook
ansible-galaxy collection install -f -r ansible-galaxy-requirements.yaml
ansible-playbook autoware.dev_env.install_docker
# Also grab artifacts neccessary for simulations
ansible-playbook autoware.dev_env.install_dev_env --tags artifacts -e "data_dir=$HOME/autoware_data/ml_models" --ask-become-pass
# Pull Docker Image
docker pull ghcr.io/autowarefoundation/autoware:universe-cuda-jazzy
```

Installs the Autoware Docker container using Autoware's provided instructions. The repo's cloned directory being placed at specifically ```~/autoware``` **is very important, as it will be used as a shared dirsctory between the host OS and the Docker container.** Same applies to the ```~/autoware_data``` directory, which contains sample files useful for running simulations.

Required dependancies are handled by Autoware's provided [Ansible](https://docs.ansible.com/projects/ansible/latest/getting_started/introduction.html) playbook. Ansible allows for system configurations to be easily implemented across different machines. Note that the ```install-ansible.sh``` script provided assumes the host OS utilizes ```apt``` as its package manager, which Debian and its derivatives (ex. Ubuntu) use, but other Linux distros based on RHEL and Arch do not, hence my reccomendation of Ubuntu or an Ubuntu derivative as a host OS should a reinstall be neccessary.

The Docker image pulled at the bottom is specifically for using Autoware on Ubuntu 24.04 and ROS2 Jazzy **with CUDA.** If you do not have a compatable Nvidia GPU (RTX series), you would want to look into the *mesa* based solutions (on AMD Radeon or Intel Arc) or *nouveau* (for older Nvidia GPUs). Both the Intel NUCs and Nvidia Jetson have modern enough GPUs for this not to be an issue, but do take note of this if future PC replacements are neccessary.

```bash
# ROS2 Workspace Setup
# Note that due to githubusercontent.com timeout issues, the vcs imports will likely fail on ClarksonGuest
mkdir -p src
vcs import src < repositories/autoware.repos
vcs import src < repositories/autoware-nightly.repos
cd src
# Additional drivers
# Robosense
git clone https://github.com/RoboSense-LiDAR/rslidar_sdk.git
cd rslidar_sdk
git submodule init
git submodule update
cd ..
# Scout 2.0 & Scout Mini
git clone https://github.com/westonrobot/ugv_sdk
git clone https://github.com/agilexrobotics/scout_ros2 -b origin/jazzy
# InertialSense uINS Parser (Only Tested on Humble)
git clone https://github.com/ChesterMK7/is_gps_publisher_ros2
# InertialSense uINS UDP Publisher (Clone and Build)
cd ..
git clone https://github.com/arcater/ISRoverNetworkNMEA.git
mkdir ISRoverNetworkNMEA/build
cd ISRoverNetworkNMEA/build
cmake ..
make
```

These lines are pretty simple, just cloning repos for the ROS2 packages for Autoware. Note that the Scout-specific files for Autoware specifically are not included here *yet.* (I will look into getting that setup all automated hopefully by the end of the summer.)

The current "setup" for the uINS UDP Publisher doesn't work currently as it isn't set up properly. You can ignore the cmake error that spawns from it for now, as the Inertialsense isn't being used.

```bash
# Move docker scripts to shared directory
cd ../..
mv ~/docker-scripts/*.bash ./*.bash
# Get required local repos for container
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-ubuntu2404.pin
wget https://developer.download.nvidia.com/compute/cuda/13.0.3/local_installers/cuda-repo-ubuntu2404-13-0-local_13.0.3-580.126.20-1_amd64.deb
wget https://developer.download.nvidia.com/compute/tensorrt/10.13.3/local_installers/nv-tensorrt-local-repo-ubuntu2404-10.13.3-cuda-13.0_1.0-1_amd64.deb
```

Some final setup for the Docker container, moving the container-specific scripts and neccessary repository files to the shared directory such that they can be used within the container.

### run-container.bash

The entirety of ```run-container.bash```, as well as the last line of ```host-setup.bash```, is to run the Autoware Docker container with the relavent parameters set.

```bash
#  Run the docker container
docker run --rm -it   --net host   --privileged   --gpus all   -e DISPLAY=$DISPLAY   -e NVIDIA_DRIVER_CAPABILITIES=all   -e NVIDIA_VISIBLE_DEVICES=all   -e HOST_UID=$(id -u)   -e HOST_GID=$(id -g)   -e QT_X11_NO_MITSHM=1   -v /tmp/.X11-unix:/tmp/.X11-unix:rw   -v $HOME/autoware_data/maps:/home/aw/autoware_data/maps   -v $HOME/autoware_data/ml_models:/home/aw/autoware_data/ml_models   -v $HOME/autoware:/home/aw/autoware   -w /home/aw/autoware   --runtime=nvidia   --device=/dev/ttyUSB0 autoware:universe-cuda-jazzy   bash -c "source /opt/autoware/setup.bash && exec bash"
```

The command itself is quite lengthy, but here's a general description of what some important parts do:

-   ```--rm``` *deletes* the container once it is done running. Since all the important files are stored in shared directories, this isn't a major issue, although dependancy packages **will need to be reinstalled each time.** Once you have a working container setup with depancies preinstalled, you can run the command without this flag to save it.

-   ```--net host``` tells the container to use the host machine's network. **This is important for any ethernet interface devices (ex. LiDAR) to work within the Docker!**

-   ```--gpus all``` and all of the ```-e``` options following it, as well as ```--runtime=nvidia``` later on set up the GPU passthrough for the container, allowing it to utilize the host OS' GPU driver directly.

-   The ```-v``` arguments map the first directory (host system) to the second directory on the Docker container. (ex. ~/autoware on the host system is mounted at /home/aw/autoware on the Docker container)

-   ```-w /home/aw/autoware``` sets the directory to open the container terminal session into when the container is executed to /home/aw/autoware.

-   ```--device=/dev/ttyUSB0``` passes the USB serial device to the container, which is used by the UM982 GPS and Scout CAN connections.

-   ```autoware:universe-cuda-jazzy``` is the name of the docker image used to create the container.

-   ```bash -c "source /opt/autoware/setup.bash && exec bash"``` opens a terminal using bash in the container, running the command following the ```-c``` option.

Realistically, you shouldn't *need* to modify this command (aside from removing the --rm flag to run it one final time before using ```docker start``` instead).


----