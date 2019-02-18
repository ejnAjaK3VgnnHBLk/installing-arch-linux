# Installing Arch Linux
Arch Linux is a flexible and highly customizable Operating System. It has a little bit of something for everyone. A major deterrent for people, however, is that there is no installation GUI. This means that the terminal must be used for this process. In this guide, I will walk you though installing this operating system from the ground up.

## Getting installation media
Navigate to https://www.archlinux.org/download/ and select the mirror closes to you (US is at the bottom.) Select one of these (it doesn't really matter which one). It will take you to a page filled with lots of different hyperlinks. Select the one that says archlinux-x.x.x-x86_64.iso, where x.x.x is the most current date. This will download a ~600MB file, which may take a while.

## Getting set up
I will be using virtualbox for this tutorial, but it should work for physical hard drives as well. I will be using a 20GB hard drive, but please adjust the values to whatever size drive you have.

### Short aside for virtualbox
Once you have created your virtual machine, you will need to go to Settings -> Storage and under Controller: IDE, select the circle with a plus and then "Choose disk". Now select the ISO file that you just downloaded.

### Short aside for installing on physical hardware.
You may need to find the drivers specific for your [Wifi Card](https://wiki.archlinux.org/index.php/Wireless_network_configuration) and Graphics Card ([NVIDIA](https://wiki.archlinux.org/index.php/Wireless_network_configuration), [AMD](https://wiki.archlinux.org/index.php/AMDGPU), and [Intel](https://wiki.archlinux.org/index.php/intel_graphics). If you need any of these drivers, I'd recommend reading the ENTIRE page.)

## Installation
### Starting up
As you start up the system, select "Boot Arch Linux". At the CLI (command line interface), ensure that your ethernet is working, and type `ping www.google.com`. Hit CTRL+C to stop it.
### Paritioning the drives
#### Basic partitioning system
I will be using a single partition for my storage, but you can manage it however you want. A common strategy is to use seperate /tmp, /mnt, and /home paritions. To start, type `fdisk -l` and identify the disk that you want to use. In my case, it is /dev/sda, which is 20GB. You will have a /dev/loop0, but you can ignore that, it is just your installation media. Now I will type `cfdisk` to start a parition manager software. At the menu, use the arrow keys to select "dos" label type. Hit enter one the second menu shows up. At the bottom left, change the value to one GB under what is show. For me, the value shown is 20G, so I will change it to 19G and then hit enter. Select "primary", and then hit the left arrow, and select the "Bootable option". Hit the down arrow, to select the free space, and hit "new" 1023M should show up as the amount. Hit enter and select "primary" again. Go to the right and hit "write", you will have to type "yes" after. Now hit the "quit" button. Type `fdisk -l` again to make sure that the changed actually saved. There should now be a /dev/sda1 and /dev/sda2. To make the partitions usable, type `mkfs.ext4 /dev/sda1` and `mkswap /dev/sda2`. To use the swap partition, type `swapon /dev/sda2`.
#### More advanced partitioning systems
If you want to use more advanced partitioning, such as seperate partitions for /mnt, /tmp, etc., you will need to to follow the following steps, and adjust them to whatever you are doing. I will here setup my partitions to have seperate /mnt, /tmp, /home, and /boot files. `cfdisk`. I will then create the four (don't forget about your swap space!) partitions here with the size that I want, and of course set the bootable flag on the partition that will hold the /boot folder. The [arch wiki](https://wiki.archlinux.org/index.php/installation_guide#Partition_the_disks) has a great guide on the apropriate sizes of various partitions. Then, I will run the `mkfs.ext* /dev/sdXX` (where * denotes whatever file system you are using, and XX is the end of the drive, with i.e. /dev/sda3) command, as well as the `mkswap /dev/sdXX` and `swapon /dev/sdXX` commands to make the swap partition do its job. Now, we have to mount the `/` partition `mount /dev/sdXX /mnt`, and then make any files that you need here. I will run `mkdir /mnt/tmp`, `mkdir /mnt/boot`, `mkdir /mnt/mnt` (this will create a `/mnt` file on the installed system), and finally `mkdir /mnt/home`. Then I will need to mount all of the drives on these files. `mount /dev/sdXX /mnt/{boot,tmp,home,mnt}` should work (where you run this command seperately for the boot,tmp,home, and mnt partitions.) Now, you should be able to continue the installation process normally here.

### The actual installation
Now that we have the partitions setup, we'll install the system. `pacstrap /mnt base base-devel` will do this. Depending on your internet speed and hard drive speed this might take a while. After pacstrap is done running, we'll generate fstab `genfstab -U /mnt >> /mnt/etc/fstab`.
### Finishing touches
To finish up, we'll chroot into our new system. `arch-chroot /mnt /bin/bash`.
#### Setting the clock
To find the available locations, type `ls /usr/share/zoneinfo`. There are the list of Regions. `ls /usr/share/zoneinfo/Europe` will give the list of all the major cities in the region. To set the clock, we type `ln -sf /usr/share/zoneinfo/Europe/Riga /etc/localtime`. Now to set the hardware clock, `hwclock --systohc --utc`.
#### Setting the locale
The locale is the language and keyboard layout. Run `nano /etc/locale.gen` and uncomment (remove the #) your keymap. The standard american keyboard is en_us.UTF-8 UTF-8. `locale-gen` will generate the locale. `echo LANG=en_US.UTF-8 > /etc/locale.conf` will set the language to standard american English. `export LANG=en_US.UTF-8` will set the language again.
#### Hostname
`echo arch-linux > /etc/hostname` will set the hostname to arch-linux.
#### GRUB
`pacman -S grub` will install grub. `grub-install /dev/sda` will allow grub to probe the hard drives to find the bootable partitions. `grub-mkconfig -o /boot/grub/grub.cfg` will generate a grub configuration file that will allow the system to boot.
#### User
Adding a standard user is imperative. `useradd -m -G wheel -s /bin/bash <username>` will add a user. `passwd <username>` will change the root user's password. `password root` changes the root password. You MUST run this, or else your system will be very vulnerable.
#### Networking
`systemctl enable systemd-networkd` will allow the network manager to start at startup. Adding `nameserver <DNS server>` to `/etc/resolv.conf` will allow you to customize your DNS server.
#### Installing a desktop interface
I will show how to install GNOME and XFCE. `pacman -S xorg lightdm lightdm-gtk-greeter` will be used for both, so you can go ahead and run that. `pacman -S xfce4 xfce4-goodies` installs xfce, and `pacman -S gnome gnome-extra` installs GNOME. You will have to run the command `systemctl enable lightdm.service`. 
#### Audio
If you want audio, run `pacman -S alsa-utils pulseaudio-alsa` and then `systemctl enable lightdm.service`.

## Thank you for reading!
