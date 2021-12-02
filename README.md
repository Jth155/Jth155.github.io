# Arch Install

First things first, we need to get the ISO file in order to have an arch linux to install. \
To do that, download BitTorrent Web, and snag the torrent file from the Arch Linux download page. \
Throw that on a flashdrive.\
Next, make a VM pointing to that ISO from the USB, and then go through the process of making the VM. \
Before you turn it on, make sure that you change its memory to 8 gigs. \
Now we turn on the VM. \
Our first goal is to partition the file system into 2 parts. \
Use fdisk -l to see our disks, and then cfdisk [name of disk] to format the disk and start partitioning it. \
Note: This part is for UEFI systems only, you must have an EFI partition. \
Make the first partition a primary partition, starting at the first sector, and then type +512M in order to make it 512 megabytes big. \
This will be our ESP Partition. \
Type t to change the type, and then type ef for an EFI partition. \
With that partition done, we now need to create a root partition. \
Go through the same process in order to make the partition, but this time allocate all the rest of the space. \
We don’t need to change the type for this one, so type w to finish. \
Now it is time to make a filesystem. \
We will make a FAT32 filesystem on the first partition, and a Ext4 filesystem on the second partition. \
Use commands mkfs.fat -F32 /dev/sda1 for the first partition, and mkfs.ext4 /dev/sda2 for the second. \
Then, we will mount the filesystem using mount /dev/sda2 (our root partition). \
Next, we really want to get our arch linux necessities installed. \
Thus, we will use pacman -Syy to sync the pacman repository. \
Then, we can use pacstrap /mnt base linux linux-firmware sudo nano in order to get our sweet, sweet packages. \
Now, we want to use them. \
First, generate a fstab file using the gnfstab -U /mnt >> /mnt/etc/fstab command. \
Finally, we will switch to the installed root partition using arch-chroot /mnt . \
Now, lets make sure we are in the correct time zone. \
Use the ls -l /usr/share/zoneinfo to find the preferred time zone, and then make a symbolic link to set the timezone using ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime, replacing New_York with Chicago in this case. \
Now we will setup a locale, to get languages and settings. \
Use nano /etc/locale.gen to open the file, and then uncomment en_US.UTF-8 UTF-8 and en_US ISO-8859-1 for the US. \
Then save and close the editor (ctrl+o enter, then ctrl+x). \
Next we will generate the locale config file using these commands in order: Locale-gen Echo LANG=en_US.UTF-8 > /etc/locale.conf Export LANG=en_US.UTF-8 \
Next, we will set a hostname using echo [hostname] > /etc/hostname. \
We also need to add that name to /etc/hosts. \
Nano /etc/hosts will open the file, and then add the lines 127.0.0.1 localhost ::1 localhost And 127.0.1.1 [hostname]. \
Make sure that it is tabbed appropriately (all the names are in the same line) \
Set a password for the root account using passwd. \
Now we need to install a bootloader, or our install will not boot up again, and we will have wasted all of this time. \
We will use the GRUB bootloader and the EFI boot manager packages in this case. \
Start with command pacman -S grub efibootmgr os-prober mtools. \
Then we need to make a mount point for our first partition and mount it: Mkdir /boot/efi Mount /dev/sda1 /boot/efi Now we can install our boot loader. \
Grub-install –target=x86_64-efi –bootloader-id=grub_uefi Its all icing from here. \
Use pacman -S xorg to get the xorg display server and then use pacman -S gnome to get the gnome gui. \
Add a network manager real quick: pacman -S networkmanager \
Finally, use these last 3 commands to enable the display manager GDM and the network manager. \
Systemctl start gdm.service Systemctl enable gdm.service Systemctl enable NetworkManager.service \
And we are all done with the innards of our Arch install. \
Now for some icing on the cake. \
I would recommend rebooting real quick in order to fully appreciate our new GUI. \
First, we make the three users using the generic command useradd -g [name] \
Then we go to the sudoers file and add all of them to have the sudo priviledge. \
Make sure that you use passwd [username] to give the users passwords, and then to use chage -d 0 [username] in order to make them reset the password when they login the first time. \
Next, we want to SSH. \
Thus, we type pacman -S openssh \
Now, we want the fish shell. \
Use the pacman -S fish command to snag that. \
Finally, we want some cool aliases to play around with. \
I choose to make mine universal, so I used nano /etc/bash.bashrc and then added Alias ls=’ls -lah’ Alias chmodo=’chmod 777’ Alias chmods=’chmod 744’ Alias ..=’cd ..’ \
Now, when you save and exit it won’t work, you need to run source /etc/bash.bashrc. \
And with that, we have the full arch setup we need.\

# Docker Install

I watched the video given \
I ran the command docker run -d -p 443:443 --name openvas mikesplain/openvas \
I opened https://localhost \
I used admin and admin for user and password \
I ran a quick test on my local machine (don't think it worked? not sure exactly) \
I composerized my run command. \
I submitted :)

# Digital Ocean (Project 3)

I opened the link given in the Powerpoint. \
I followed the instructions to get a droplet setup. \
I SSH'd into the droplet and then followed the given guide for the previous project in order to get docker installed on the droplet. \
From there, I followed the guide found in the Powerpoint in order to get my wireguard set up, the only things I did indepently were to change the time zone to America/Chicago, and to change the IP address to the IP address of my droplet. \
Next, I ran the log and used the QR code to get the VPN on my phone. \
I took the screenshots there (shown here) \

Next, downloaded wireguard on my laptop, took the conf file from the digital ocean drop, and set up the vpn on my laptop, taking the two screenshots shown here. \

Here is the link to the guide that I used. \
https://thematrix.dev/setup-wireguard-vpn-server-with-docker/ \
All in all, I used this powerpoint, and the last docker powerpoint to do this project, following the directions as needed.

