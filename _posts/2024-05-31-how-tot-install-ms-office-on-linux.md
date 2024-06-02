---
title: How to install Microsoft Office 2007/ 2010/ 2016/ 2021/ 365 or any version on Linux
author: wxguy
date: 2024-05-31 21:10:00 +0530
categories: [Tutorial]
tags: [virtual-machine, ms-office, linux]
pin: false
toc: true
comments: true
render_with_liquid: true
img_path: /assets/img/winapps/
---

![MS Office 2021 Word File in Arch Linux](winapps-word-highlight.png){: .shadow }
_MS Office 2021 Word File in Arch Linux_

## Requirement of Microsoft Office

Undoubtedly, Microsoft Office is one of the most popular and most widely used Office suites in the world. Especially, names such as Word, Powerpoint and Excel have become synonymous with office applications/ suits. Based on my experience, I can tell that a new user who shifted to Linux would immediately search for procedures to install Microsoft Office on a Linux OS. As with other software, there are many native alternatives available for Linux. However, the biggest issue here is the compatibility of file types. The Word files or presentations created in Microsoft Office would never open properly in other Office suites, including the de-facto Office suite 'Libre Office'. The myriad of formulas and options available in Excel will never come in other Office suites anytime soon. Therefore, it is a natural choice for users to switch between Microsoft Windows and Linux Desktop for simply using this great piece of software.

## Options available to Install Microsoft Office on Linux

When searching online for installing MS Office on Linux Desktop, most of the tutorials would tell you to follow one of the following methods:-

* Use alternate native software such as Libre Office, WPS Office, Open Office etc.

* Use [wine](https://www.winehq.org) to install MS Office.

* Use [PlayonLinux](https://www.playonlinux.com/en) which under-the-hood uses wine.

* Use Paid software for better compatibility such as [crossover](https://www.crossover.com) which is again the improved version of wine.

* Use Windows Virtual Machine inside Linux Desktop and use it whenever required.

or finally

* Install Windows OS alongside Linux and boot into Windows whenever required.

When you follow any one of the methods listed above, either you will end up using an older version of MS Office or you will still use the software that is not fully compatible with MS Office (except using MS Office directly inside Windows). I have tested all the methods and the best solution I found is the procedure listed in subsequent paragraphs.


## Winapps for Native Windows Software (kind-of) on Linux

So what is so fuzz about Winapps? It is a community software which offers one of the virtualisation solutions to run any Windows applications as if they are installed inside a Linux Desktop. It uses Windows Virtual Machine to install the software and [FreeRDP](https://www.freerdp.com) to access this software from Linux. The beauty of this application is the native integration of the file manager right-click option to open and specify the mime type to open in Windows software.

When I heard about this software two years ago in 2022, I tried it many times and it always gave me some sort of issues which forced me to uninstall. The application was initially developed at <https://github.com/Fmstrat/winapps>. However, the repo did not receive an update for a long time. Therefore, the project was forked and maintained by the community at <https://github.com/winapps-org/winapps>. Recently, I gave Winapps software again a try and did not encounter an issue. I also found it to be too good for running day-to-day utility. Right now, I have installed MS Office 2021 and the application works flawlessly on my Arch Linux desktop.

I will be writing more about the technical know-how on installing and configuring winapps for using MS Office 20xx/ 365 in this blog post. There are many steps involved in configuring the software correctly. Read and execute the commands carefully.

## System Configuration

I will be covering this tutorial on Arch Linux as it is the only desktop I use nowadays. If you use any other desktop, you must install equivalent commands on your own. Here is my system configuration:


```bash
neofetch
                   -`                    sundar@archlinux
                  .o+`                   ----------------
                 `ooo/                   OS: Arch Linux x86_64
                `+oooo:                  Host: TravelMate P214-53 V1.47
               `+oooooo:                 Kernel: 6.6.32-1-lts
               -+oooooo+:                Uptime: 1 hour, 48 mins
             `/:-:++oooo+:               Packages: 1691 (pacman), 15 (flatpak)
            `/++++/+++++++:              Shell: zsh 5.9
           `/++++++++++++++:             Resolution: 1920x1080
          `/+++ooooooooooooo/`           DE: GNOME 46.2
         ./ooosssso++osssssso+`          WM: Mutter
        .oossssso-````/ossssss+`         WM Theme: Adwaita
       -osssssso.      :ssssssso.        Theme: Adwaita [GTK2/3]
      :osssssss/        osssso+++.       Icons: Adwaita [GTK2/3]
     /ossssssss/        +ssssooo/-       Terminal: kitty
   `/ossssso+/:-        -:/+osssso+-     CPU: 11th Gen Intel i7-1165G7 (8) @ 4.700GHz
  `+sso+:-`                 `.-/+oso:    GPU: Intel TigerLake-LP GT2 [Iris Xe Graphics]
 `++:.                           `-/+/   Memory: 11065MiB / 15775MiB
 .`                                 `/
```

## Install Necessary Software

The procedure we are going to follow uses docker for installing and running Windows 11 in the background. Therefore, install all the necessary software:


```bash
sudo pacman -Sy freerdp dnsmasq vde2 bridge-utils openbsd-netcat libguestfs ebtables iptables podman docker docker-compose
```

Add KVM and libvert into to user group:

```bash
sudo usermod -aG $(whoami) libvirt
sudo usermod -aG $(whoami) KVM
```

Enable `podman` and `docker` services using following commands:

```bash
sudo systemctl enable --now podman.socket
sudo systemctl start docker
```

## Install Windows OS Using Docker

Once the system software is installed and configured, we need to install Windows Virtual Machine on the host OS. We will be using the docker for this purpose. The main advantage of this method is to install the Windows OS without user intervention and run it in the background. It also does not require the user to have a Windows OS ISO file as it would automatically download as part of the installation process. All the details about Windows OS are saved in a config file (`*.yml`). For this, we need to create a `compose.yml` file. You can create it anywhere you like, but, ensure to run the docker-compose command in the same location where the `compose.yml` file exists. In my case, I have created the file in home directory at `~/compose.yml`. Update the `~/compose.yml` file with the following content:

```yml
name: "winapps"

services:
  windows:
  image: dockurr/windows
  container_name: windows
  environment:
    VERSION: "tiny11"
    RAM_SIZE: "6G"  # Change RAM size
    CPU_CORES: "4"   # Change core
    USERNAME: "Sundar"          # Change username with yours
    PASSWORD: "xxxxxx"   # Change password with yours
    privileged: true
    ports:
      - 8006:8006
      - 3389:3389/tcp
      - 3389:3389/udp
    stop_grace_period: 2m
    restart: on-failure
    volumes:
      - data:/storage
      -  /mnt/Data:/storage   #  /path/to/mnt/dir:storage_name
```

We will be using the above information to tell the docker-compose command to install the Windows OS with all the specifications listed above. You must edit the file content depending on your hardware and liking. The `compose.yml` is a `yml` file. In our case, we will be installing the Windows 11 Tiny version to save space and resources. It is easy to understand the terminologies of the above file. You can always refer to <https://github.com/dockur/windows> for further tweaking the file content. Once saved, you can run the below command to install the Windows OS in the background:

```bash
docker compose up -d
```

You can also view what is happening to Windows OS from a web browser at http://127.0.0.1:8006. The docker command will install the OS without any user intervention. Once installation is completed, you must logout before proceeding.

> If you wish to access all the files located at different mount partitions, it is recommended to link the mounted partition to your home directory like `ln -s /mnt/34B2CD28B2CCEF82/Data /home/username`.
{: .prompt-tip }

## Install and Configure Winapps


Clone and move into the winapps repo using the following commands:

```bash
git clone https://github.com/winapps-org/winapps.git
cd winapps
```

The winapps configuration is stored at `~/.config/winapps/winapps.conf` and create a file with the following content:

```bash
RDP_USER="Sundar"
RDP_PASS="xxxxxx"    # Replace the password with yours
#RDP_DOMAIN="MYDOMAIN"
RDP_IP="127.0.0.1"
#RDP_SCALE=100
#RDP_FLAGS=""
#MULTIMON="true"
#DEBUG="true"
#FREERDP_COMMAND="xfreerdp"
```

Since you are already there in `winapps` directory, you can check if your Windows 11 is installed correctly using following command:

```bash
bin/winapps check
```

The above command will ask you to accept the key and accept it. Thereafter, it will bring the Windows File Explorer as shown in the following figure.


![Windows Explorer in Arch Linux](windows-explorer-check.png){: .shadow }
_Windows Explorer in Arch Linux_

Now you can navigate to network --> tsclient --> Linux home dir --> copy MS Office installers to --> Documents folder of Windows 11. Then install MS Office which you generally do just by clicking setup.exe --> next --> next --> and done. Once MS Office is installed, you can close the File Explorer. If everything goes well, you can install the actual winapps using the following command:

```bash
./installer.sh
```

Accept all the questions and install for users only. Once all the above procedures are done, you can try to open a document created using MS Office and this time by right click --> Open with --> Choose the 'Microsoft Word' application. It should open in MS Office Word application as shown in the following image.


![Sample IEEE Paper Template in MS Office Word in Arch Linux](ieee-paper-template-in-ms-office-word.png){: .shadow }
_Sample IEEE Paper Template in MS Office Word in Arch Linux_


## Final Tweaking


If all works well, then you can manage (start/stop) using the following docker commands:

```bash
docker compose start  # for starting the machine
docker compose stop   # for stopping the machine
```

The above commands are good to execute but you will get frustrated as it will through your black screen if the Windows OS is not booted before opening MS Office documents. You can solve this issue by booting Windows OS during Arch Linux boot. For this, create a file `/home/usrename/.config/winapps/win11_start.sh` with the following content:

```bash
#!/bin/sh

sleep 3
docker compose up -d  # start the docker in the background
```

Make it executable:

```bash
chmod +x /home/username/.config/winapps/win11_start.sh
```

Next, create a desktop file at `~/.config/autostart/win11-docker.desktop` with the following content:

```bash
[Desktop Entry]
Name=Windows 11 Docker
GenericName=Windows 11
Comment=Start Windows 11 using Docker
Exec=/home/username/.config/winapps/win11_start.sh
Terminal=false
Type=Application
X-GNOME-Autostart-enabled=true
```

That's it. Now onwards, every time you reboot, your Windows OS also will boot and run in the background. This would ensure that your MS Office applications are always available in Arch Linux whenever you open it.

