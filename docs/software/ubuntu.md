---
title: Ubuntu
parent: Software
nav_order: 10
---

# Ubuntu

[Ubuntu](https://ubuntu.com) is a Linux distribution developed by British comapny Cannonical, used as the basis for many other distros. Our use of it stems from its support for the [ROS2 platform](https://www.ros.org/) that Autoware requires.

Currently, both NUCs use Ubuntu 22.04.5 as their host OS, with the Autoware Docker container utilizing Ubuntu 24.04. Both are currently still supported with updates and security patches, although support for 22.04 will end on May 31st 2027. This *shouldn't* effect anything given we're using containers for our core setup, but if security of the system becomes a concern I'd recommend updating the host to the latest Long Term Support (LTS) release, Ubuntu 26.04, which will recieve support through May of 2031.

## Installing the OS

### Obtaining the OS image

Ubuntu, like most Linux distros, is installed using a bootable medium, most commonly a USB flash drive. Small ones can be purchased relatively cheaply, and realistically a 16GB or 32GB should work just fine.

Firstly, you'll need to download the Ubuntu lvieboot iso file. I would recommend grabbing it from [COSI's Mirror](https://mirror.clarkson.edu/ubuntu-releases/) for faster download speeds, but you can also download it from [Ubuntu's site directly](https://ubuntu.com/desktop).

**MAKE SURE YOU DOWNLOAD THE UBUNTU DESKTOP IMAGE. UBUNTU SERVER WILL WORK, BUT YOU WILL NOT HAVE A GUI.**

### Creating the install medium

There are a variety of ways of creating bootable install mediums from iso images. I will list a few options here.

#### KDE ISO Image Writer (Linux)

On modern Linux distros, you can install KDE's ISO Image Writer tool using your package manager.

```bash
# For Debian-based distros (ex. Debian, Ubuntu, Linux Mint, Zorin OS)
sudo apt install isoimagewriter
# For Arch-based distros (ex. Arch Linux, Manjaro, CachyOS, SteamOS)
sudo pacman -S isoimagewriter
# For RHEL-based distros (ex. Fedora, Bazzite, Red Hat Enterprise Linux)
sudo dnf install isoimagewriter
```

Open the application from your Desktop Environment's start menu (search "iso", it should show up if installed). **Make sure the USB flash drive is inserted.**

Double click on the "ISO Image" box, then navigate to and select the image you want to use. Then check the "USB drive" dropdown box and make sure you have the correct device selected. **THE SELECTED DRIVE WILL BE REFORMATTED, SO MAKE SURE ANY DATA YOU WANT OFF OF IT IS BACKED UP!!!**

Once you've verified the above information, click on "Create", then "Continue", and wait for the process to finish. Note that it will probably take 5-10 minutes.

#### Rufus (Windows)

I'd recommend using [Rufus](https://rufus.ie/en/) for boot medium creation on Windows. You can download it from its website.

Insert your USB flash drive, then launch Rufus. In the window that opens, press the "SELECT" button next to the box below "Boot Selection", then navigate to and select the image you want to use. (If "DOWNLOAD" is displayed instead, click on the dropdown tab next to it and select "SELECT" from the list of options.) Check the "Device" box and make sure you have the correct drive selected. **THE SELECTED DRIVE WILL BE REFORMATTED, SO MAKE SURE ANY DATA YOU WANT OFF OF IT IS BACKED UP!!!**

For compatability with modern systems, I'd also recommend changing the "Partition scheme" to GPT and "Target System" to UEFI (non CSM). There is a possibility that the defaults *will* work on a modern system, however if the system lacks legacy BIOS boot capabilities **only the above options will work.**

Click the "START" button at the bottom. If a menu pops up telling you the image is a ISOHybrid image, make sure "Write in ISO Image mode (recommended)" is selected, then hit OK. Another warning about erasure of the entire disk will then pop up, and again you can click ok to continue. Then wait for the process to finish. Note that it will probably take 5-10 minutes.

Once the medium has been created, remove it from your computer and insert it into the NUC/PC you want to write the new OS to.

### Installing the OS

With the install medium inserted into the target system, reboot and press the correct key to enter the boot menu (usually F9). Select the USB device from the options using the arrow keys, then press enter. You should see the bootloader menu, on which select "Try or Install Ubuntu" and press Enter. The liveboot should start loading (which might take a few minutes depending on the speed of the USB drive).

The installer should load automatically shortly after the Desktop Environment loads in (the GUI). If it doesn't, there should be an application on the desktop titled "Install Ubuntu", which you can run to get into the installer. The first few pages are pretty self-explanitory, just make sure the language and keyboard are set properly.

I would recommend connecting to the internet for the installation to install updates (ClarksonGuest is the public campus WiFi). Then you'll want to make sure "Install Ubuntu" is selected on the next screen. Select "Interactive Install" on the next screen (this process shouldn't happen often enough to warrent making an autoinstall config I hope).

For selecting apps to install, I'd recommend "Default selection", although either would work. On the drivers screen, if you're on Ubuntu 26.04, install both graphics and media drivers. **ON 22.04 AND 24.04, THE DRIVERS IN THE REPOS ARE OLDER THAN THE ONE USED BY THE SETUP SCRIPT, SO THEY MIGHT NOT WORK AND ARE A PAIN TO REMOVE FULLY.**

On the next screen, I would recommend selecting "Manual Installation" to create a custom partition table including a seperate home partition, to eliminate the need for home folder backups when reinstalling the OS. Below is a sample partition table you could use for this purpose.

|Partition #|Size|Filesystem|Mountpoint|Flags|
|-----|-----|-----|-----|-----|
|0|1GB|FAT32|/boot/efi|boot|
|1|80GB|ext4|/|none|
|2|Rest of Disk|ext4|/home|none|

Note: It appears that the Ubuntu 26.04 installer handles the boot partition flags inherently, so those can be ignored if installing that version.

On future installs (once the home partition exists), **only check the format option on the boot and root (/) partitions.** This will leave the home partition unmodified, retaning all the files stored there.

On the next screen, create an account for the new OS. For consistancy, I'd recommend using the same username (required if using an old home partition) and password as the old OS, same with the hostname. I would uncheck "require my password to log in" to allow for automatic login on boot (like the current Ubuntu 22.04 OS setup).

After verifying the timezone is set correctly, hit "Install" and wait for the process to finish.

### User Permissions

An important note for OS reinstalls (and potentially Docker containers as well) is to make sure the user account you are using has the appropriate permissions. Most distros will give the first user account you create the "sudo" permission for running commands as root. This *will* allow you to bypass user-specific permissions for system resource access, however running programs as root *can* cause issues, and outside of apt and system service calls, I'd advise against it in most cases.

For using serial ports (roughly equivalent to COM ports on Windows), make sure the user is part of the "dialout" group (on Ubuntu-based systems), which can be done with the command below.

```bash
sudo usermod -aG dialout $USER
```

Logging out and back in will apply the change.

----
