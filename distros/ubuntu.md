# Xiaomi POCO X3 Pro linux mainline guides

## Installing ubuntu distribution

### Prerequisites

- Working Android

- Rooted Android

- A desire to learn and to solve problems that may show up during the process 

- An already partitioned device, If you dont know what partitions do you need; Follow [this guide](/guides/partitioning.md)

## Notes
> [!WARNING]  
>if you think you have made a mistake or something isn't working as intended, reach us in the [Telegram chat](https://t.me/mainlineonvayu), We will try to help you with everything we can do.

> In this guide we will build our distro from scratch, Maybe in the future ready to flash images will be made, But I didnt manage to make one nor my internet supports uploading such file.

> After following this guide you will have Ubuntu 24.04 LTS aarch64 on your device.

### Preparing the environment needed to build our system
Install [termux application](https://github.com/termux/termux-app/releases/download/v0.118.1/termux-app_v0.118.1+github-debug_arm64-v8a.apk) on your rooted POCO X3 Pro, We will use it throughout this guide, If you have it already you can use it instead of downloading a new one.

#### Installing required packages
Run this to install required packages throughout the guide
```sh
apt update && apt upgrade -y && apt install wget tsu
```
During this command it may ask you some questions, Answer all of them with "y", If it didn't it's not an issue, You can proceed with the guide safely

#### Downloading base rootfs
> This is the rootfs that we will build our system on

Run this to download it
```sh
wget https://cdimage.ubuntu.com/ubuntu-base/releases/24.04/release/ubuntu-base-24.04-base-arm64.tar.gz
```

#### Mounting Ubuntu partition 
> We will now mount Ubuntu ext4 partition we created earlier so that we can build our system there.
```sh
mkdir ubuntu
su -c mount -t ext4 /dev/block/by-name/ubuntu ubuntu
```

### Applying base rootfs 
> We will now apply base rootfs to the Ubuntu partition we mounted.

Run this to enter root shell
```sh
tsu
```
Then this to apply the rootfs
```sh
cd ubuntu
tar xvf ../ubuntu-base-24.04-base-arm64.tar.gz
```
Then you can exit the root shell
```sh
exit
```

### Entering Chroot
> Unmount the mount we made earlier because we will use the chroot script mount instead.
```sh
su -c umount ubuntu && rmdir ubuntu
```

> We will now make a permanent path that will house our chroot mount
```sh
su -c mkdir /data/local/ubuntu
```

Now we will download the script that will lets us enter chroot
```sh
wget https://github.com/gixousiyq/POCO-X3-Pro-Mainline-Guides/releases/download/chroot-scripts/ch -O $PREFIX/bin/ch && chmod +x $PREFIX/bin/ch
```

And now run this anywhere in termux
```sh
ch
```

And you are now inside the Ubuntu system! But we are not finished yet, There's still alot of work to do so stick with me a little bit.

### Basic setup
> We will now run some commands to fix errors and warnings such as:

- apt cannot resolve host
```sh
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "127.0.0.1 localhost
127.0.0.1 xiaomi-vayu" > /etc/hosts
```
- Download is performed unsandboxed as root
```sh
groupadd -g 3003 aid_inet
groupadd -g 3004 aid_net_raw
groupadd -g 1003 aid_graphics
usermod -g 3003 -G 3003,3004 -a _apt
usermod -G 3003 -a root
```
Run these now! Not when the errors shows up.

Now we will tell the system our machine name
```sh
echo "xiaomi-vayu" > /etc/hostname
```

Now you can install packages and update the system

During this command it may ask you some questions, Answer all of them with "y"
```sh
apt update && apt upgrade -y && unminimize 
```
>[!WARNING]
>If you are planning to use grub, Remove u-boot-tools- from the command bellow!!! It breaks grub installation!!!

Install general packages
```sh
apt install u-boot-tools- nano net-tools sudo git bash-completion ssh rmtfs protection-domain-mapper tqftpserv
```

### Creating a user
We will now make a user that will carry your username in Ubuntu 

Now replace "vayu" bellow with the username you want, If you are unsure what username you want you can leave it as is, It does **NOT** need to be lowercase by the way; Make sure to memorize it or write it somewhere if you can't remember it because we will need it later
```sh
export USER=vayu 
```
Now run this to create the user you entered its name, Just paste this in termux and you are good to go
```sh
groupadd storage
groupadd wheel
useradd -m -g users -G wheel,audio,video,storage,aid_inet -s /bin/bash $USER
passwd $USER
```
Now we have to add your user to the sudoers list, Otherwise you wont be able to execute sudo

Type this command
```
nano /etc/sudoers
```
A text editor called nano will show up, Scroll via your fingers until you see the line that says 
```
root ALL=(ALL:ALL) ALL
```
Add a line below it like this

Replace "user" with your username that you typed earlier 
```
user ALL=(ALL:ALL) ALL
```
So at the end it should **FOR EXAMPLE** look like this **if you used the default user, as an example**
```
root ALL=(ALL:ALL) ALL
vayu ALL=(ALL:ALL) ALL
```
Now press CTRL + S to save and CTRL + X to exit

Now enter as the user you created using this command
```sh
su theuseryoucreated
cd
```
Now we will install a desktop environment ~~unless you want your ubuntu to have a black screen~~

### Installing a desktop environment 
We will now install the desktop that we will use everytime we boot Ubuntu, So pick the one you want wisely!

Run this command to install Ubuntu desktop (default Ubuntu distribution desktop)
```sh
sudo apt install -y ubuntu-desktop
```
You can also replace ubuntu-desktop with:
- ubuntu-desktop-minimal

- kubuntu-desktop ```KDE plasma```

- xfce4
And many more

But only "ubuntu-desktop" had been tested so far

### Create fstab
fstab file is mandatory in Linux systems; It tell's the operating system what to mount and where, Without it the system wont boot.

To create it run the command bellow
```sh
echo "PARTLABEL=ubuntu / ext4 errors=remount-ro,x-systemd.growfs 0 1
PARTLABEL=esp /boot/EFI vfat umask=0077 0 1
PARTLABEL=logfs /boot/simpleinit vfat umask=0077 0 1" | sudo tee /etc/fstab
```

### Install firmware
All you need to know is that firmware is the stuff needed to get WiFi, GPU, Bluetooth, Touchscreen, Audio, Fingerprint, Cameras, LTE and calls working, That doesn't mean that they are enough on thier own; They need a driver that loads them into the device and manages them.

Anyways with that being said grab the latest firmware from [here](https://github.com/gixousiyq/POCO-X3-Pro-Mainline-Guides/releases/tag/firmware), We will assume that the firmware.tar.gz is in /storage/emulated/0/Download or in download folder in internal storage 

Run this command 
```
cd /lib/firmware
sudo tar xvf /sdcard/Download/firmware.tar.gz 
```

### Getting the panel model
POCO X3 Pro has 2 panels, One is called Huaxing and one is called Tianma; You need to know which one you have, Picking the definitions of the other panel may be catastrophic and may cause permanent damage!!!!! 

Run this command to determine what panel you have
```sh
echo $(sudo cat /proc/cmdline |  tr " :=" "\n" | grep dsi) | tr " " "\n" | tail -1
```
If it said:
- dsi_j20s_42_02_0b_video_display: Means you have Hauxing panel

- dsi_j20s_36_02_0a_video_display: Means you have Tianma panel

### Installing the mainline kernel
We will now install the thing that allow's us to boot Ubuntu in the first place!

Grab:
- vmlinuz-6.10.0-rc5+

- dtb-huaxing-6.10.0-rc5+ ```If you are Hauxing panel```

- dtb-tianma-6.10.0-rc5+ ```If you are Tianma panel```

From [this link](https://github.com/gixousiyq/POCO-X3-Pro-Mainline-Guides/releases/tag/kernel)

And grab 

- modules.tar.gz

from [this link](https://github.com/gixousiyq/POCO-X3-Pro-Mainline-Guides/releases/tag/modules)

Once again we will assume they are in /storage/emulated/0/Download

Run this command to move them to the correct place
```sh
sudo mv /sdcard/Download/*-6.10.0-rc5+ /boot/EFI
```
If you are Huaxing run this command
```sh
cd /boot/EFI
sudo mv dtb-huaxing-6.10.0-rc5+ dtb-6.10.0-rc5+
```
If you are Tianma run this command
```sh
cd /boot/EFI
sudo mv dtb-Tianma-6.10.0-rc5+ dtb-6.10.0-rc5+
```
And this to unpack modules, The kernel probably won't boot without them!!!
```sh
sudo mkdir /lib/modules
cd /lib/modules
sudo tar xvf /sdcard/Download/modules.tar.gz
```

### Making initrd.img
initrd.img or Initialisation Ram Disk is a piece of software that the bootloader loads into the ram which then initialises our system and mounts the stuff in fstab, It also stores our modules and firmware.

To generate it run this command
```sh
cd /boot/EFI
sudo mkinitramfs -o initrd.img-6.10.0-rc5+ 6.10.0-rc5+
```
If you see ```Possible missing firmware``` or ```/dev/disk/by-partlabel/ubuntu doesn't exist``` or ```cannot check for zstd compression support``` or ```root device does not exist```  They are safe to ignore.


Now we are basically done! We have successfully built our Ubuntu system with its kernel and userspace, But always remember; The kernel cant boot itself, So now we will set up a UEFI or a bootloader that will allow us to boot our system, And even dualboot with android!

## [Next step: Setting up DualBoot](/guides/dualboot.md)

## Credits
Ivon's Blog: https://ivonblog.com/en-us/posts/termux-chroot-ubuntu/
map220v: https://github.com/map220v/ubuntu-xiaomi-nabu
