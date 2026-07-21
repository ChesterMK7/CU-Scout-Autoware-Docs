---
title: Command Line Basics
parent: Ubuntu
nav_order: 12
---

# Command Line Basics

Unlike the Windows family of Operating Systems, Linux is heavilly reliant on usage of the command line for many applications. Getting familiar with some of the basics will be quite useful in working with both this project as well as industry-standard server software (although that is more often than not RHEL or a derivative of it).

## Opening a Terminal Session

On any desktop environment (DE), opening a terminal session involves opening the repsective Terminal application from the apps menu. I would also reccomend pinning it to your apps menu (if the DE has one), which can usually be done through a right-click triggered menu. On most DEs, pressing the "META"/"SUPER" key (Windows key) will bring up the apps menu. On newer GNOME versions (Ubuntu's default DE), this instead brings up the virtual desktops view instead, so you'll have to click the logo in the bottom left corner to open it instead.

On most DEs, the terminal app is simply called "Terminal" or has terminal in its name. A notable exception is **KDE Plasma**, where it is called "Konsole" instead.

## Using apt, the Package Manager

Ubuntu, as a Debian-based distribution, utilizes ```apt``` as its package manager. This is the primary way to install applications on Ubuntu. (There is also a GUI "app store" on modern versions, but most of the time you're better of using the command line interface instead.)

### Usage

```apt``` has four primary options that you will be likely to utilize: *install*, *remove*, *update*, and *upgrade.* Note that **all of these commands require root (sudo) permissions to be executed!**

#### Update

The simplist of the four, the *update* option simply checks the apt sources list and updates the list of avaliable packages that ```apt``` pulls from. This accounts for additional repositories being added (as the ROS2 installation requires), as well as any updates to existing packages that have been applied since the list was last updated. Usage is pretty simple, as it takes no package names as arguments.

```bash
sudo apt update
```

#### Upgrade

Unlike other package managers, which usually combine the *update* and *upgrade* functionality into the same command (or allow them to be run simultantiously as with Arch's ```pacman```), ```apt``` splits these functionalities into two different command options. *Upgrade* simply updates all installed packages to their newest avaliable version, so long as their dependancies can be met. Usage, like with *update*, is pretty simple as no package name argument is neccesary.

```bash
sudo apt upgrade
```

#### Install

A very simple command that installs the target package. For example, if you wanted to install ```htop``` (a TUI Task Manager like utility), you would run the following:

```bash
sudo apt install htop
```

If a package has additional dependancies that are not currently installed, the *install* command will also install those alongside the target package. If ```apt``` cannot resolve a packages' dependancies, it will refuse to install the target package.

Note that ```apt``` package names are not *always* the most intuitive. For example, if a particular command is provided as part of a larger package, the ```apt``` package will likely be named for the set of commands rather than an individual command. For example, if you wante to install the ```ping``` command, you would need to install the *iputils* package.

#### Remove

The opposite of *install,* removing the selected package rather than installing it. For example, if you wanted to *remove* the ```htop``` package, you would run:

```bash
sudo apt remove htop
```

Same package naming note as above applies. With dependancies, if there are packages that depend on the package that is to be removed, **```apt``` will refuse to remove the package unless all dependant packages are also removed.**

## Filesystem Navigation

Below are some basic commands for navigating the filesystem from the command line.

### pwd

When opening a terminal, it is likely you will be in your user's home directory (denoted by a '~'). You can verify this by running the ```pwd``` (print working directory) command:

```bash
# Should expect output of '/home/$USER', where $USER is your username
pwd
```

Using ```pwd``` is also handy for checking what directory you currently have open.

### ls

For viewing the contents of a current directory, use the ```ls``` command.

```bash
# Show files and directories in the current directory
ls
# Include hidden files and directories (start with '.' on Linux systems)
ls -a
```

### cd

For moving into another directory, use ```cd```.

```bash
# Running cd with no arguments changes the active directory to your user's home directory
cd
# Will change the active directory to Documents, if it exists in the current directory
cd Documents
# Alternatively, you can use absolute paths to jump specifically to a given directory
cd ~/Desktop
# Using '..' returns you to the parent directory of the current directory
cd ..
# Using './' before a directory/file name will refrence starting from the current directory
cd ./Pictures
# You can use multiple of the above in combination, like this
cd ./../Music
```

## Other Useful Commands

Here are some additional commands I find useful when working with Linux systems.

### nano

A basic TUI (terminal user interface) text editor. While its not as feature-rich or powerful as other options, its easy to use for beginners (and people who don't want to spend 5 years learning vim).

```bash
# Installation command (should be installed automatically on most distros)
sudo apt install nano
# nano takes a filename as an argument, and will create a file with that name if it doesn't exist
nano example.txt
```

### htop

A TUI application similar to Task Manager on Windows. Shows system resource allocation, as well as a list of all processes currently running on the system.

```bash
# Installation command
sudo apt install htop
# Run htop
htop
```

## Additional Resources

Should you need more information about commands I didn't describe here, or just want to learn more about Linux in general, a simple online search can get you a decent amount of information on whatever package or command you want to use.

Additionally, if you happen to be on Clarkson's main campus, you can stop by the **Clarkson Open Source Insitute (COSI),** located at Science Center 336. Most days during the semester there is usually somebody around that can likely help you out with issues pretaining to Linux.

----
