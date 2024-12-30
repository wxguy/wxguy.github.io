---
title: Install Ubuntu on VirtualBox and Configure it Properly
author: wxguy
date: 2022-05-17 21:45:00 +0530
categories: [Tutorial]
tags: [arch linux, linux, virtualbox, ubuntu]
pin: false
toc: true
comments: true
render_with_liquid: true
media_subpath: /assets/img/virtualbox/
image:
  path: 48-vbox-ubu-post-inst-auto-adjust-screen.webp
  lqip: data:image/webp;base64,UklGRmIAAABXRUJQVlA4IFYAAADQAwCdASoUAAsAPzmGulQvKSWjMAgB4CcJaF9qRhYgAibVjwgfXgAA/ujwDBnxzdJKwR5oyM2xD93XFQlq5of2i5FjCz3Fl/jP8sa8IpQQA1IAaQAAAA==
  alt: Ubuntu Linux Running inside VirtualBox. (Credit:- Author)

---

-------

## Virtual Machine

As the name suggests, a Virtual Machine is a software that emulates your real hardware. This will enable you to install an Operating System within your Host Operating System. The utility of this Virtual Machine depends on the way you want to use it or configure it. For example, there may be software that only works on Windows-based OS but you have the right to use only Linux-based OS in your organisation. In such a case, you can install a Windows OS within Linux through Virtual Machine software.

The main reason why I use Virtual Machine is due to Micro Soft Office Products. Whenever I work on Word or Presentations which required to be shared with others, I fire up my Windows Virtual Machine, use MS Office, and shut down. In addition, I also use Virtual Machine for testing various Distro and to test my pet projects. There are quite a few options available to choose Virtual Machine software, both paid and free. However, for personal and general-purpose, I recommend using the following:
 -  [VMware Workstation Player](https://www.vmware.com/products/workstation-player.html)
 -  Quick EMUlator ([QEMU](https://www.qemu.org/download/))
 -  [Virt-Manager](https://virt-manager.org/)
 -  [VirtualBox](https://www.virtualbox.org/)


## Why VirtualBox?

I have written an article about Virt-Manager and you can read it [here](https://wxguy.github.io/posts/test-os-using-virtual-machine/). Here are the main reasons I choose VirtualBox over other Virtualisation software:
 -  Cross-platform (Linux, Windows, and Mac OS)
 -  Rapid development cycle to support the latest OSs
 -  Many options are free for personal use
 -  Free
 -  Superior documentation
 -  Help from the community

## What I Intend to Accomplish

The main intent is to install Ubuntu 22.04 as a Guest OS and share directories and files between the Host and Guest OS. Following are the VirtualBox operations I intend to perform in this tutorial:
 -   Install VirtualBox and other additional software on Host OS (Arch Linux)
 -   Allocate more processors
 -   Allocate more memory (RAM)
 -   Manual adjustment of HDD
 -   Bidirectional clipboard sharing
 -   Network sharing between Host and Guest
 -   Boot and install Ubuntu from ISO files
 -   Install necessary software on Guest OS for better performance
 -   Configure Host and Guest for sharing of files between

This tutorial is a lengthy one from installing VirtualBox to enabling file-sharing options. There are lots of screenshots I have posted here for beginners to understand. I believe this one guide is enough to configure VirtualBox software for better performance and get you started with Virtual Machine.

## Install VirtualBox on Host OS

My Host OS is Arch Linux with up-to-date packages. We need to install three core components to make VirtualBox more user-friendly. I use the `linux-lts` kernel on my machine and accordingly, the following three packages are to be installed. While `virtualbox-ext-oracle` can be downloaded directly from the VirtualBox website, I preferred to install it from AUR and I never encountered any issues so far. I used `paru` package manager instead of `pacman` as it would pull AUR packages as well.

```console
$ paru -S virtualbox virtualbox-host-dkms linux-lts-headers
$ paru -S virtualbox-guest-iso
$ paru -S virtualbox-ext-oracle
```

After installing core packages, the virtualbox driver is to be loaded. Execute the  following command in the terminal:

```console
$ sudo modprobe vboxdrv
```

Now add `vboxusers` to the group.

```console
$ sudo usermod -aG vboxusers $(whoami)
```

> We also need to install the `linux-headers` package on Ubuntu Guest OS. That will be done in the latter part of the tutorial.
{: .prompt-tip }

## Download Ubuntu 22.04 ISO

Before we proceed further to tweak the virtualbox, we need to download the latest Ubuntu 22.04 ISO from Ubuntu's official website. Go to [https://ubuntu.com/download/alternative-downloads](https://ubuntu.com/download/alternative-downloads) and scroll down to section `BitTorrent` and download torrent file for `Ubuntu 22.04 LTS`. I recommend this method as the speed of download through torrent is much faster than a direct download. The ISO file downloaded is `ubuntu-22.04-desktop-amd64.iso` and located in the `~/Downloads` directory.

## Create Virtual Machine for Ubuntu

In this section, we will create a new machine and install Ubuntu 22.04 as a Guest OS. Before installing, we need to create a Virtual Machine for our Guest OS.

> If your VirtualBox GUI is looking ugly under KDE dark mode, you can use the `virtualbox -style Fusion %U` command from the terminal.
{: .prompt-tip }

### Before Install

When you open VirtualBox GUI, this is what it looks like.

![](virtualbox-gui.png){: .shadow }
_VirtualBox: GUI_

Clicking on the `New` button will bring a new window as indicated below that will guide you through allocating various hardware to the Virtual Machine. In the first screen, we need to provide the name of the OS, and the location of the machine folder where all Guest OS files will be located. You can manually locate the local directory by clicking on the drop-down option against the `Machine Folder` option. Ensure that you provide a path that has enough space. Then click `Next`.

![](3-virtualbox-create-vm.png){: .shadow }
_VirtualBox: Add New Virtual Machine_

In the next window, allocate the necessary RAM size. Remember that whatever you provide here will not be available for your Host OS. The minimum _recommanded RAM size is 2GB_. But I am providing **8GB** as my system has 16GB RAM.

![](4-virtualbox-vm-mem-size.png){: .shadow }
_VirtualBox: Provide Desired RAM Size_

Next, we need to create a hard disk for our new Virtual Machine. Select the `Create a virtual hard disk now` radio button and click on `Create`.

![](5-virtualbox-vm-hdd-create.png){: .shadow }
_VirtualBox: Create New Virtual Hard Disk_

In the new window, choose the `VDI (VirtualBox Disk Image)` option and click `Next`.

![](6-virtualbox-vm-hdd-type.png){: .shadow }
_VirtualBox: Choose Hard Disk Type_

Now, we need to select how data is to be stored. The option `Dynamically allocated` will not create the whole hard disk at one go and it will grow with Guest OS usage. The other option `Fixed size` will create the whole virtual hard disk file on a local directory which is not recommended. Therefore, we will select the `Dynamically allocated` option and click `Next`.

![](7-virtualbox-vm-hdd-dynamic.png){: .shadow }
_VirtualBox: Select Way to Store Data to Storage_

That will lead to the next option to allocate virtual hard disk space for the machine. The path to the virtual hard disk file will be created automatically by the virtualbox. You can provide virtual hard disk space for the machine. The minimum recommended size is 20GB. I have given 50GB for Ubuntu 22.04. Once the size is provided, click on `Create`. This will create the virtual hard disk file and close the wizard.

![](8-virtualbox-vm-hdd-file-loc-size.png){: .shadow }
_VirtualBox: Provide Desired Hard Disk Space for VM Ubuntu OS_

After the Virtual Machine for Ubuntu 22.04 Guest OS is set up, it will return to VirtualBox GUI. Here you can see that a new entry for Ubuntu 22.04 is made.

![](9-vbox-ubuntu-vm-added.png){: .shadow }
_VirtualBox: View of VirtualBox GUI After Setting Up_

### Tweak Virtual Machine Settings

There are a few more additional settings that need to be tweaked so that we can install Guest OS and improve its performance. Firstly, select Virtual Machine `Ubuntu 22.04` from the left panel and click on the `Settings` option. That will bring up the below settings window. Here we can tweak more options for Guest OS and insert Ubuntu 22.04 ISO for installing from it.

Firstly, under the `General` --> `Basic` tab section, ensure that name of the machine is mentioned as `Ubuntu 22.04`.

![](11-vbox-ubu-settings-gen-basics.png){: .shadow }
_VirtualBox: Settings: General Basic Options_

Under the `General` --> `Advanced` section, we can tweak the clipboard copy-paste between Host and Guest OS. Select `Bidirectional` for both _Shared Clipboard_ and _Drag'n'Drop_ option as shown below.

![](12-vbox-ubu-settings-gen-advanced.png){: .shadow }
_VirtualBox: Settings: General Advanced  Options_

Then go to `System` --> `Motherboard` tab and deselect `Floppy` from the boot order list as we won't use it.

![](13-vbox-ubu-settings-sys-mboard.png){: .shadow }
_VirtualBox: Settings: System Motherboard Options_

Move to `System` --> `Processor` tab and increase the number of the processor to your desired value. If you have 4 then give 2. I have 8 and hence given `4`.

![](14-vbox-ubu-settings-sys-processors.png){: .shadow }
_VirtualBox: Settings: System Processor Options_

Under `Display` --> `Screen`, increase the Video Memory to the desired value. I allocated 64MB to Virtual Machine.

![](15-vbox-ubu-settings-disp-screen.png){: .shadow }
_VirtualBox: Settings: Display Screen Options_

Now we need to provide the path to our downloaded ISO as part of the storage devices. Go to `Storage` --> `Storage Devices` --> `Controller IDE` and click on the `+` symbol embedded in CD icon. That will open a new window for adding a new ISO to VirtualBox.

![](16-vbox-ubu-stor-ide.png){: .shadow }
_VirtualBox: Settings: Storage Controller Options_

In the Optical Disk Selector window, click on the `+` icon to open the file manager navigation window.

![](17-vbox-ubu-stor-ide-op-disc-sel.png.png){: .shadow }
_VirtualBox: Settings: Add An Additional ISO_

Navigate to your download directory where your `ubuntu-22.04-desktop-amd64.iso` is located and click `Open`.

![](18-vbox-ubu-stor-ide-op-disc-sel-iso.png.png.png){: .shadow }
_VirtualBox: Settings: Select ISO File From Local Directory_

Now, you can see a new entry for `ubuntu-22.04-desktop-amd64.iso` that we have selected just now. Select `ubuntu-22.04-desktop-amd64.iso` entry and click `Choose`.

![](19-vbox-ubu-stor-ide-op-disc-choose-iso.png){: .shadow }
_VirtualBox: Settings: Choose Desired ISO_

After selecting the Ubuntu 22.04 ISO, you can see ISO is attached to `Controller: IDE`.

![](20-vbox-ubu-stor-ide-attach-iso.png){: .shadow }
_VirtualBox: Settings: Storage Controller After Attaching ISO_

Now, go to `Network` --> `Adaptor 1` tab and select `NAT`. For simple browsing and common internet-related option, NAT is more than enough. If you want to interact with Guest OS through network connect, then change this value to `Bridged Network` which will directly interact with your Host hardware.

![](21-vbox-ubu-set-net-adaptor.png){: .shadow }
_VirtualBox: Settings: Network Adopter Options_

If you want Guest OS to access your Host USB devices, then go to `USB` and enable the `Enable USB Controller` option. In my case, I have disabled it as it always crashing my VirtualBox.

> If you do not enable the `Enable USB Controller` option, a warning message will display at the bottom of the settings window. You can simply ignore this warning.
{: .prompt-warning }


![](22-vbox-ubu-set-usb-controller.png){: .shadow }
_VirtualBox: Settings: USB Controller Options_

You can also configure your shared folder right now before even booting your Ubuntu 22.04 ISO. For this, go to `Shared Folder` and click on the `+` icon as shown in the below image.

![](23-vbox-ubu-set-share-fold-wo.png){: .shadow }
_VirtualBox: Settings: Shared Folders Options_

That will bring up another helper window where you can provide a path to the Host directory by navigating through the file manager dialog. Provide any name for the Folder Name that will be displayed on Guest OS. Do not forget to select the `Auto Mount` option. Finally, click `Ok`. You can add as many shared folders as you want.

![](24-vbox-ubu-set-share-fold-add-share.png){: .shadow }
_VirtualBox: Settings: Add Shared Folder Local Path_

I have added three two-partitions and the Host's home directory for sharing with Ubuntu 22.04 Guest OS. Once it is completed, you can click `Ok` which will also close the settings window.

![](25-vbox-ubu-set-share-fold-complete.png){: .shadow }
_VirtualBox: Settings: Shared Folders PAth After Setting Up_

That's all. After enabling/ disabling various options, you can start the Virtual Machine by clicking on the `Start` icon as shown below.

![](26-vbox-ubu-start-vm.png){: .shadow }
_VirtualBox: Start Ubuntu 22.04 Guest OS_

## Ubuntu Install Procedures

Installing Ubuntu 22.04 on a Virtual Machine is a straightforward process. However, I will show here how I did on my Virtual Machine.

Starting the Ubuntu 22.04 Virtual Machine will bring in this GRUB Boot message. The select first option `Try or Install Ubuntu` option.

![](27-vbox-ubu-grub.png){: .shadow }
_Ubuntu Install: GRUB Boot Menu_

When Ubuntu boots in for the first time it will offer if you need to install or try live Linux. Select `Install Ubuntu` option.

![](28-vbox-ubu-first-boot.png){: .shadow }
_Ubuntu Install: First Boot To Try or Install_

Select the `English (US)` option for the Keyboard layout and language option. This is not to be confused with the language Indians speak. Though Indians speak UK English, the keyboards we use are from the US.

![](29-vbox-ubu-inst-kb-layout.png){: .shadow }
_Ubuntu Install: Keyboard Layout and Language Selection_

Select `Normal Installation`, disable `Download Updates` option and click `Continue`.

![](30-vbox-ubu-inst-update-options.png){: .shadow }
_Ubuntu Install: Updates and Other Software_

Select `Erase disk and install Ubuntu` option and click `Install Now`.
![](31-vbox-ubu-inst-disk-options.png){: .shadow }
_Ubuntu Install: Installation Type_

Final confirmation will be asked at this stage. Click `Continue`.

![](32-vbox-ubu-inst-disk-erase-warn.png){: .shadow }
_Ubuntu Install: Disk Erase Warning Message_

Select your country by clicking on your country on the map.

![](33-vbox-ubu-inst-country.png){: .shadow }
_Ubuntu Install: Choose Country_

Add your username and password here. You can also enable the auto-login option.

![](34-vbox-ubu-inst-new-user.png){: .shadow }
_Ubuntu Install: New User Options_

Sit and watch till your files are copied to the Guest OS location.

![](35-vbox-ubu-inst-installing.png){: .shadow }
_Ubuntu Install: Install Progress_

Despite not accepting to download files during installation previously, the installer continues to download certain packages. Mostly, they are language-specific. Hence, you can safely select the `Skip` option.

![](36-vbox-ubu-inst-skip-downloads.png){: .shadow }
_Ubuntu Install: Skip Download Files_

Once Ubuntu installation is complete, reboot your system

![](37-vbox-ubu-inst-restart.png){: .shadow }
_Ubuntu Install: First Reboot After Install_

During the reboot, you must enter the `Enter` key to remove installation media.

![](38-vbox-ubu-inst-restart-enter.png){: .shadow }
_Ubuntu Install: Eject Removable Installation Media_

After reboot, ubuntu will welcome the new user with a few options. You can `Skip` the online account option

![](40-vbox-ubu-post-inst-online-skip.png){: .shadow }
_Ubuntu Install: Skip Online Account_

If you wish to help Ubuntu you can select appropriate options and select the `Next` option.

![](41-vbox-ubu-post-inst-feedback-skip.png){: .shadow }
_Ubuntu Install: Option to Help Ubuntu_

You can enable or disable _Location Services_ and click `Next`.

![](42-vbox-ubu-post-inst-privacy-next.png){: .shadow }
_Ubuntu Install: Privacy Option_

Click `Done` to complete the welcome screen.

![](43-vbox-ubu-post-inst-done.png){: .shadow }
_Ubuntu Install: Final Welcome Window_

You must update and upgrade your system to the latest packages before proceeding further. You can use the following commands to update and upgrade your system.

```console
$ sudo apt update -y
$ sudo apt upgrade -y
```

or follow the rest of the guide to set up the best server for your country and update your system.

On my system, I could not update to the latest packages as it always failed with the `index failed to download` error message. So I decided to change my server to the best one. For this open the `Software & Updates` window by searching on the activities search option.

![](43-2-vbox-ubu-post-inst-update-sel-server.png){: .shadow }
_Ubuntu Install: Software And Updates Window_

Now click on the `Download From` drop-down menu and select the `Other` option.

![](43-3-vbox-ubu-post-inst-update-sel-server-others.png){: .shadow }
_Ubuntu Install: Software And Updates Window Other Server_

In the new window, click on the `Select best server` option.

![](43-4-vbox-ubu-post-inst-update-sel-server-best.png){: .shadow }
_Ubuntu Install: Software And Updates Window Best Server_

Wait for a few minutes as it will check for the best server for you.

![](43-5-vbox-ubu-post-inst-update-sel-server-best-progress.png){: .shadow }
_Ubuntu Install: Testing Best Server_

Once the best server is automatically selected, you can select the `Choose server` option.

![](43-6-vbox-ubu-post-inst-update-sel-server-best-choose.png){: .shadow }
_Ubuntu Install: Choose Best Server_

Finally, `Reload` to download a new index from the newly selected repository.

![](43-8-vbox-ubu-post-inst-update-sel-server-best-reload.png){: .shadow }
_Ubuntu Install: Reload Packages from New Repo_

Again search and open the `Software Updater` application.

![](43-9-vbox-ubu-post-inst-update-gui-sel.png){: .shadow }
_Ubuntu Install: Search Software Updater_

and select the `Install Now` option.

![](43-10-vbox-ubu-post-inst-update-gui-install.png){: .shadow }
_Ubuntu Install: Install New Updates_

After updating your system, reboot the system again.

![](43-11-vbox-ubu-post-inst-update-restart.png){: .shadow }
_Ubuntu Install: Reboot System After Update_


## Ubuntu 22.04 Post Install Configurations

Though you have successfully installed the system, it is still not in usable condition. To make it user-friendly, you need to install VirtualBox Guest Addition Package. You can successfully install only if you have kernel header files installed on Guest OS. So we will install it now using the following command.

```console
$ sudo apt install -y build-essential linux-headers-$(uname -r)
```

Now go to the Devices menu in the Virtual Machine window and select the `Insert Guest Addition CD Image` option. This will automatically mount the Guest Addition ISO image into Ubuntu Guest OS.

![](45-vbox-ubu-post-inst-insert-guest-addition-iso.png){: .shadow }
_Ubuntu Post Install: Insert Guest Addition ISO_

Now navigate to the location where the Guest Addition ISO image is mounted and open the location in the terminal.

![](46-vbox-ubu-post-inst-ga-open-terminal.png){: .shadow }
_Ubuntu Post Install: Open Terminal From Mounted ISO_

Now you can install VirtualBox Guest Addition for Linux by executing the following command.

```console
$ sudo ./VBoxLinuxAdditions.run
```

You must reboot the system again to take this effect.

![](47-vbox-ubu-post-inst-ga-install-reboot.png){: .shadow }
_Ubuntu Post Install: Reboot After Installing VB GA_

Finally, you can see that VirtualBox automatically adjusted the Guest OS aspect ratio to the size of the Virtual Machine window. You can also play around with other view modes but remember to shortcut key combos.

![](48-vbox-ubu-post-inst-auto-adjust-screen.png){: .shadow }
_Ubuntu Post Install: Look After Installing VB GA_


## Sharing of Files Using Shared Folder

Though we have added Shared Folder from Host OS and showing up in Guest OS's file manager, you will not be able to use it. When you access it for the first time you will get the following `Permission Error`.

![](49-vbox-ubu-post-inst-sf-mnt-error.png){: .shadow }
_File Share: Permission Error For Shared Folder_

To avoid permission errors, we need to add `vboxsf` to the group. the following command will do that.

```console
$ sudo usermod -aG vboxsf $(whoami)
```

Now onwards, you can start using Ubuntu 22.04 comfortably as we have done a lot of tweaking.

## Closing Thoughts

Though the steps involved in configuring VirtualBox are many, they are straightforward. One of the best parts of VirtualBox is its documentation and Sharing of the Folder option. With this, you will feel as if you are running an Operating System on real hardware. I would highly recommend VirtualBox for your Virtual Machine needs.



------
You can download this article from [here](https://wxguy.github.io/assets/downloads/pdfs/2022-05-15-install-and-configure-ubuntu-on-virtualbox-properly.pdf) for free.

------
