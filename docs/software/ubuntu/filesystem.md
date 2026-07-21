---
title: Filesystem
parent: Ubuntu
nav_order: 13
---

# Linux Filesystem

The Linux filesystem is organized differently from that of Microsoft Windows in many ways, although here I am going to describe some of the most important differences pertaining to this specific project.

## Important Things to Note

### Filesystem Formatting

The majority of Linux-based operating systems utilize the ext4 format, which **CANNOT** be read by Windows. When transferring files between the two utilizing a USB flash drive, use FAT32 or exFAT formatted drives, as both OSes can read them.

### Line Endings

Windows ends lines in text documents (like this one!) using the terminator characters ```\r\n```, whereas Unix-derived systems (including Linux) utilize just ```\n``` as the line terminator character. In most instances, this does not matter, as Linux *can* read Windows line endings (and vice versa as far as I know), *however* if you are writing bash scripts to automate package bringups or just system setup, note that **files with Windows line endings cannot be executed!** You will have to use a tool such as ```dos2unix``` to convert the files to Unix line endings, *then* they can be executed.

Additional note: make sure a file has *consistant* line endings. I encountered an issue this summer where a Python file was refusing to run due to "inconsistant indentation" despite having consistant indentation throughout the flagged section. Turns out that the problematic section had the Windows line endings, and the extra ```\r``` character was causing the error.

### Executable Permissions

Files are not executable by default on Linux, especially if they're text files (ex. .py or .bash). You can use the following command to rectify that.

```bash
# Will grant executable permissions to the target file
sudo chmod +X <filename>
```

Then you can execute the file by entering ```./``` then the filename into the terminal and pressing enter.

## Home Directory

The home directory, located at ```/home/$USER``` (where $USER is the username), and often abreviated as ```~/```, is where all of the usual user related folders are stored. You can think of it as a rough equivalent to ```C:\Users\<username>``` on modern Windows systems (where <username> is the user). This includes the Desktop, Documents, Videos, Pictures, and Downloads folders, as well as a variety of user configs stored in *hidden files and directories.*

### Hidden Files and Directories

While these exist on Windows as well, the way they are denoted is different on Linux. On Linux, a file or directory starting with '.' acts as a hidden file/directory, and is not shown by default in file viewers or while using the ```ls``` command. They can still be accessed by typing them into the ```cd``` command, *or* by toggling their visability in your file manager (usually CTRL+H).

## Root Directory

Similar to how ```C:\``` denotes the root of the Windows filesystem, ```/``` denotes the root of the Linux filesystem. The majority of important files for the project are stored in ```/home/$USER```, as described above, however a general overview of the Linux root filesystem structure might prove useful for debugging.

For more informarion on the Linux filesystem hierarchy, you can read the Wikipedia article [here](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard).

### /bin and /sbin

These are directories that contain binaries (compiled programs), roughly equivalent to .exe files on Windows. They usually lack a file extention, and usually can be invoked in a terminal from any directory on the system (so long as the directories are part of the $PATH environment variable, which they *should* be on any modern Linux distro by default).

Usually, ```/bin``` contains essential command binaries (ex. cd, ls), whereas ```/sbin``` contains essential *system* binaries (ex. fsck, init) that require root permissions (sudo) to execute.

### /boot

This directory contains the bootloader and boot files, responsbible for actually booting the operating system from the UEFI/BIOS firmware. This directory uses a different disk partition than the rest of the system, as it is formatted as FAT32, not ext4. (This is due to how UEFI/BIOS booting works, the more technical details are not relavant here.) Odds are unless your bootloader is no longer working, you **shouldn't** mess with this directory.

### /dev

The ```/dev``` directory is one of many Linux "virtual" directories, as it doesn't contain files, but rather device entries for devices connected to the system. This includes storage devices, serial devices, and some other system resources. One of the most important subsets of files here is the ```tty``` files, which represent serial devices (think COM on Windows). Usually, USB serial devices show up as either ```/dev/ttyUSB*``` or ```/dev/ttyACM*```, where '*' is any number 0 or greater. Your user will need to be part of the "dialout" group to access serial device files; more about this is explained on the Installation page.

### /etc

This directory contains system-wide configuration files. Not much should be done with these files unless absolutely neccessary.

### /lib and /lib64

Contain libraries utilized by the binaries contained in ```/bin``` and ```/sbin```.

### /media

Root for mount point of removable storage mediums (ex. USB drives).

### /mnt

Often used as a mount point for different storage devices. Often utilized with the ```mount``` command. Empty by default.

### /opt

Storage for optional add on packages. Notably, ROS2 stores its files here (for Jazzy, under ```/opt/ros/jazzy```).

### /proc

Stores process and kernel information as a virtual procfs filesystem. Not too important to get into the finer details here.

### /root

Like ```/home/$USER```, but for the root user of the system.

### /run

Contains runtime variable data, which is information about the running system since last boot (ex. running processes and logged in users).

### /snap

Contains programs installed using the *snap* package format used by many Ubuntu packages.

### /srv

Site-specific data served by this system.

### /tmp

Used by the system for temporary files, and is usually not persistant between boots (i.e. all files stored here are *lost* on reboot/power down.)

### /usr

Acts as a secondary hierarchy for user-readable data. Also contains many similar folders to ```/``` (ex. bin, sbin, lib, lib64). Some distros use *simlinks* (similar to shortcuts on Windows) to point the ```/``` directories to their coresponding ```/usr``` directories (Ubuntu and Arch both seem to do this).

### /var

Directory for variable files, often files that have their contents frequently changed during system operation (ex. logs).

----
