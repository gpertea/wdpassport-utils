# wdpassport-utils
WD Passport Ultra Complete Utilities for Linux.

<h1> Intro </h1>

This is a python2 script which let you unlock, change password and erase encrypted Western Digital Passport devices on Linux platform.

<h1> Install </h1>

In order to run this script you need to install "lsscsi" command and the "py_sg" module for <b>python2</b>. 

For example, on Arch linux:
```
 yaourt -S python2-pip lsscsi
 sudo pip2 install py_sg
```

On Ubuntu:
```
sudo apt-get install python-pip python-dev lsscsi
sudo pip install py_sg
```


<h1> Usage </h1>
After cloning the repository, make sure the `wdpassport-utils.py` script is executable:
```
cd wdpassport-utils
chmod a+rx wdpassport-utils.py
```
Note: script invocations below should be run as root. (e.g. issue `sudo su` to get a root prompt).
* plug the external drive into an USB port
* unlock the drive (you'll be prompted for the password):
```
./wdpassport-utils.py -u
```
* make the encrypted partition mountable:
```
./wdpassport-utils.py -m
```
* if you O.S. does not mount new drives automatically, check which /dev/sd?1 drive was made available by the `-m` option
(e.g. by running `dmesg | tail` after the previous commant) and mount it manually, with something like this:
```
mkdir /mnt/unlocked
mount /dev/sde1 /mnt/unlocked
```
(assuming that the unlocked drive is /dev/sde1)

<h2>Script options</h2>
```
-h, --help            show this help message and exit
```
Lists all possible arguments.

```
-s, --status          Check device status and encryption type
```
Get device encryption status and cipher suites used.
```
-u, --unlock          Unlock
```
You will be asked to enter the unlock password. If everything is fine device will be unlocked. This command should be 
followed by another run of the script with the `-m` command in order to make the encrypted device actually available to the operating system.

```
-m, --mount           Enable mount point for an unlocked device
```
After unlocking, your operating system still thinks that your device is a strange thing attached to the usb port and it won't know how to manage it. You need this option to force the O.S. to rescan the device and handle it as a normal external usb harddrive (a new /dev/sd?1/ device should appear after this command is issued).

```
-c, --change_passwd   Change (or disable) password
```
This option let you to encrypt your device, remove password protection and change your current password.
If device is "without lock" and you want it to be password protect leave the "OLD password" field empty and choose insert the new password.
If the device is password protected and you want to be as a normal unencrypted device, inser the old password and leave the "NEW password" field empty.
If you only want to change password do it as usual.

```
-e, --erase           Secure erase device
```
"Erase" the device. This will remove the internal key associated to you password and all your data will be unaccessible. You will also lose your partition table and you will need to create a new one (you can use fdisk and mkfs).

```
-d DEVICE, --device DEVICE  Force device path (ex. /dev/sdb). Usually you don't need this option.
```
The script will try to auto detect the current device path of your WD Passport device.
If something is wrong or you want to manually specify the device path yourself you can use this option.

<h1>Disclaimer</h1>
I based my research on Dan Lukes (FreeBSD version) and KenMacD (very simple unlocker) works. 
I'm in no way sponsored by or connected with Western Digital.
Use any of the information contained in this repository at your own risk. I accept no
responsibility.
