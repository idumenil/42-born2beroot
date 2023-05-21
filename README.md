# 42-born2beroot
## Introduction

This project consists in creating a virtual machine based on Linux with specific rules. 

You can find more details of the subject here : [born2beroot.pdf](https://github.com/idumenil/42-born2beroot/files/11359931/born2beroot.pdf)

The virtualization software used here is VirtualBox, and the OS chosen is Debian.

### What is virtualization ?

Virtualization refers to the process of creating a virtual version of something, such as an operating system, server, storage device, or network resource. This allows multiple virtual instances of the same resource to be created and managed, which can improve efficiency, flexibility, and cost-effectiveness in computing environments. For example, a single physical server can be partitioned into multiple virtual servers, each with its own operating system and applications. This enables better utilization of hardware resources and reduces the need for additional hardware. Virtualization is widely used in cloud computing, data centers, and enterprise IT environments.

### What is LVM ?

LVM stands for Logical Volume Manager. It is a software component that allows you to manage disk space more flexibly. It allows you to create virtual partitions that can span multiple physical disks and can be resized on-the-fly without the need to reboot the system. LVM is commonly used in Linux systems.

## Walkthrough

### sudo
#### Step 1: Installing sudo
Switch to root and its environment via su -.

$ su -
Password:
#

Install sudo via apt install sudo.

`apt install sudo`

Verify whether sudo was successfully installed via dpkg -l | grep sudo.

`dpkg -l | grep sudo`

#### Step 2: Adding User to sudo Group
Add user to sudo group via adduser <username> sudo.

`adduser <username> sudo`

Verify whether user was successfully added to sudo group via getent group sudo.

`$ getent group sudo`
  
reboot for changes to take effect, then log in and verify sudopowers via sudo -v.

` reboot
<--->
Debian GNU/Linux 10 <hostname> tty1

<hostname> login: <username>
Password: <password>
<--->
$ sudo -v
[sudo] password for <username>: <password>`
  
#### Step 3: Running root-Privileged Commands
From here on out, run root-privileged commands via prefix sudo. For instance:

`$ sudo apt update`

#### Step 4: Configuring sudo
Configure sudo via sudo vi /etc/sudoers.d/<filename>. <filename> shall not end in ~ or contain ..

`$ sudo vi /etc/sudoers.d/<filename>`
  
To limit authentication using sudo to 3 attempts (defaults to 3 anyway) in the event of an incorrect password, add below line to the file.

`Defaults        passwd_tries=3`

To add a custom error message in the event of an incorrect password:

`Defaults        badpass_message="<custom-error-message>"`
  
To log all sudo commands to /var/log/sudo/<filename>:

`$ sudo mkdir /var/log/sudo
<~~~>
Defaults        logfile="/var/log/sudo/<filename>"
<~~~>`
  
To archive all sudo inputs & outputs to /var/log/sudo/:

`Defaults        log_input,log_output
Defaults        iolog_dir="/var/log/sudo"`
  
To require TTY:

`Defaults        requiretty`
  
To set sudo paths to /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin:

`Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"`
