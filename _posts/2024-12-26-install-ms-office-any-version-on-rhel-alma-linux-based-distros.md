---
title: Install MS Office any Version on RHEL Based Distros
author: wxguy
date: 2024-12-26 10:00:00 +0530
categories: [Tutorial]
tags: [rhel, linux, alma-linux, ms-office]
pin: false
toc: true
comments: true
render_with_liquid: true
media_subpath: /assets/img/ms_office_on_rhel/
image:
  path: redhat-microsoft-puzzle.jpg
  alt: Representative Image Credit to https://ohthehugemanatee.org.
---

## Why Separate Post?

This is a follow-up post of [https://wxguy.in/posts/make-rhel-alma-linux-8-as-perfect-desktop](https://wxguy.in/posts/make-rhel-alma-linux-8-as-perfect-desktop) where I did not explain about installing MS Office on RHEL based distros. The post was a little lengthy and hence I felt that it required a separate post. In addition, after I published the post at [https://wxguy.in/posts/how-tot-install-ms-office-on-linux](https://wxguy.in/posts/how-tot-install-ms-office-on-linux), there have been numerous changes in the winapps application. Just to remind you that we will be installing a tiny version of Windows 11 using Docker and configure to access MS Office using [https://github.com/winapps-org/winapps](https://github.com/winapps-org/winapps). I have tested an MS Office 2021 on Alma Linux 8.10 and the process would be the same for all RHEL-based ditros with version 8.x. If you use any other version, you may have to adjust certain steps accordingly.

## Install and Enable Docker

Winapps, support `docker`, `podman` or `libvirt` for installing Windows Virtual Machine. However, they recommend using either podman or docker.  We will be using docker method in this tutrial. Though, there is version of docker available in repo, we will use the latest stable version from official website of docker.

Enable official docker repo.

```shell
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
```

Install docker with compose plugin

```shell
sudo dnf install docker-ce docker-ce-cli containerd.io
```


Check if docker is successfully installed.

```shell
$ docker --version
Docker version 26.1.3, build b72abbb
```

Also, check the docker-compose through version info.

```shell
$ docker-compose --version
Docker Compose version v2.32.1
```

Add docker to the user group.

```shell
sudo usermod -a -G docker $USER
```

Start and enable docker.

```shell
sudo systemctl start docker
sudo systemctl enable docker

```

When I ran docker compose for the first time, I got a permission error as shown below.

```shell
"permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: "
```

Some technical documentation recommends a reboot before using docker. However, I gave necessary permission with following command that solver the issue.


```shell
sudo chmod 777 /var/run/docker.sock
```

## Install Windows11 Using Docker

The latest version of winapps requires FreeRDP version 3 or above. Before, we install system dependencies, lets check the version information of freerdp package available in repo using following command.

```shell
$ dnf info freerdp 

Available Packages
Name         : freerdp
Epoch        : 2
Version      : 2.11.7
Release      : 1.el8
Architecture : src
Size         : 7.0 M
Source       : None
Repository   : copr:copr.fedorainfracloud.org:mlampe:freerdp
Summary      : Free implementation of the Remote Desktop Protocol (RDP)
URL          : http://www.freerdp.com/
License      : ASL 2.0
Description  : The xfreerdp & wlfreerdp Remote Desktop Protocol (RDP) clients
             : from the FreeRDP project.
             : 
             : xfreerdp & wlfreerdp can connect to RDP servers such as Microsoft
             : Windows machines, xrdp and VirtualBox.

```

It is clearly evedent that the freerdp available in repo will not work in our case. There is an option to install FreeRDP using flatpak. However, due to known bug, it won't work either. The solution is to compile the freerdp programme in our syste. The FreeRDP compilation process is little complecated. I have written a seperate post on the same at [https://wxguy.in/posts/compile-freerdp-on-rhel-alma-linux-8](https://wxguy.in/posts/compile-freerdp-on-rhel-alma-linux-8). 


***YOU MUST COMPILE LATEST FREERDP BEFORE PROCEEDING FURTHER.***

Once freerdp is compiled and installed, we can install other system dependencies.

```shell
sudo dnf install -y dialog iproute libnotify nmap-ncat
```

Clone winapps repo to local filesysem and move to it. I use home directory as it is easy to navigate through directories.

```shell
git clone https://github.com/winapps-org/winapps ~/

cd winapps
```

The complete setup preference of client VM are provided as a configuration file called `compose.yaml`.  This `compose.yaml` is also part of cloned repo. Check if it is avalibale.

```shell
$ ls com*                                                                   ─╯

compose.yaml
```


Edit `compose.yaml` file as per your need. I have only changed **USERNAME** and **PASSWORD** section. Rest all I have kept as it is. Sample of my configuration is as shown. 

```yaml
# For documentation, FAQ, additional configuration options and technical help, visit: https://github.com/dockur/windows

name: "winapps" # Docker Compose Project Name.
volumes:
  # Create Volume 'data'.
  # Located @ '/var/lib/docker/volumes/winapps_data/_data' (Docker).
  # Located @ '/var/lib/containers/storage/volumes/winapps_data/_data' or '~/.local/share/containers/storage/volumes/winapps_data/_data' (Podman).
  data:
services:
  windows:
    image: ghcr.io/dockur/windows:latest
    container_name: WinApps # Created Docker VM Name.
    environment:
      # Version of Windows to configure. For valid options, visit:
      # https://github.com/dockur/windows?tab=readme-ov-file#how-do-i-select-the-windows-version
      # https://github.com/dockur/windows?tab=readme-ov-file#how-do-i-install-a-custom-image
      VERSION: "tiny11"
      RAM_SIZE: "4G" # RAM allocated to the Windows VM.
      CPU_CORES: "4" # CPU cores allocated to the Windows VM.
      DISK_SIZE: "64G" # Size of the primary hard disk.
      #DISK2_SIZE: "32G" # Uncomment to add an additional hard disk to the Windows VM. Ensure it is mounted as a volume below.
      USERNAME: "sundar" # Uncomment to set a custom Windows username. The default is 'Docker'.
      PASSWORD: "Pa$$w0rd" # Uncomment to set a password for the Windows user. There is no default password.
      HOME: "${HOME}" # Set path to Linux user home folder.
    privileged: true # Grant the Windows VM extended privileges.
    ports:
 - 8006:8006 # Map '8006' on Linux host to '8006' on Windows VM --> For VNC Web Interface @ http://127.0.0.1:8006.
 - 3389:3389/tcp # Map '3389' on Linux host to '3389' on Windows VM --> For Remote Desktop Protocol (RDP).
 - 3389:3389/udp # Map '3389' on Linux host to '3389' on Windows VM --> For Remote Desktop Protocol (RDP).
    stop_grace_period: 120s # Wait 120 seconds before sending SIGTERM when attempting to shut down the Windows VM.
    restart: on-failure # Restart the Windows VM if the exit code indicates an error.
    volumes:
 - data:/storage # Mount volume 'data' to use as Windows 'C:' drive.
 - ${HOME}:/shared # Mount Linux user home directory @ '\\host.lan\Data'.
      #- /path/to/second/hard/disk:/storage2 # Uncomment to mount the second hard disk within the Windows VM. Ensure 'DISK2_SIZE' is specified above.
 - ./oem:/oem # Enables automatic post-install execution of 'oem/install.bat', applying Windows registry modifications contained within 'oem/RDPApps.reg'.
      #- /path/to/windows/install/media.iso:/custom.iso # Uncomment to use a custom Windows ISO. If specified, 'VERSION' (e.g. 'tiny11') will be ignored.
    devices:
 - /dev/kvm # Enable KVM.
      #- /dev/sdX:/disk1 # Uncomment to mount a disk directly within the Windows VM (Note: 'disk1' will be mounted as the main drive).
      #- /dev/sdY:/disk2 # Uncomment to mount a disk directly within the Windows VM (Note: 'disk2' and higher will be mounted as secondary drives).

```

After editing the `compose.yaml` file, it is time to install Windows 11 using docker compose.

```shell
docker compose --file ~/winapps/compose.yaml up
```

This will show progress as indicated below.

```shell
[+] Running 7/7
 ✔ windows Pulled                                                                  28.2s 
   ✔ aebe871ddbf7 Pull complete                                                    22.5s 
   ✔ debe2ff5bb35 Pull complete                                                    25.0s 
   ✔ df72794a06d8 Pull complete                                                    25.0s 
   ✔ 3ea98e7a316a Pull complete                                                    25.1s 
   ✔ 3c169cf00d7d Pull complete                                                    25.2s 
   ✔ b1788d24960b Pull complete                                                    25.2s 
[+] Running 3/3
 ✔ Network winapps_default  Created                                                 0.3s 
 ✔ Volume "winapps_data"    Created                                                 0.0s 
 ✔ Container WinApps        Created                                                 1.1s 
Attaching to WinApps
WinApps | ❯ Starting Windows for Docker v4.07...
WinApps | ❯ For support visit https://github.com/dockur/windows
WinApps | ❯ CPU: Intel Core i7 8550U | RAM: 10/16 GB | DISK: 60 GB (xfs) | KERNEL: 4.18.0-553.30.1.el8_10.x86_64...
WinApps | 
WinApps | ❯ Downloading Tiny 11 from archive.org...
WinApps | 
WinApps |      0K ........ ........ ........ ........  0%  277K 3h40m
WinApps |  32768K ........ ........ ........ ........  1%  360K 3h13m
WinApps |  65536K ........ ........ ........ ........  2%  449K 2h52m
.
.
.
```

It took about three hours for me to download Tiny version of Windows 11 that has a size of around 5GB. Probably it may be due to the fact that it is downloading ISO from `archive.org`. You can monitor the Windows 11 installation progress using browser using URL http://127.0.0.1:8006. 

After installing Windows, comment out the following lines in the compose.yaml file by prepending a '#':

```shell
- ./oem:/oem
```

and copy the modified `~/winapps/compose.yaml` file to `~/.config/winapps/compose.yaml`.

```shell
cp ~/winapps/compose.yaml ~/.config/winapps/compose.yaml
```

Finally, ensure that the new configuration is applied by running the following.

```shell
docker compose --file ~/.config/winapps/compose.yaml down
docker compose --file ~/.config/winapps/compose.yaml up
```

If some of the procedure listed above is not working, you can refer to official documentation at [https://github.com/winapps-org/winapps/blob/main/docs/docker.md](https://github.com/winapps-org/winapps/blob/main/docs/docker.md).

## Download, Install and Activate MS Office 2011

Once Windows 11 VM is successfully installed, Install MS Office 20XX in client system. I used following method to install MS Office 2011. The post I referred says it is legal to do that. Please let me know if it violates any sort of license, I will remove this section.


Download copy of 2011 to `~/Downloads` directory from Linux host.

```bash
wget https://c2rsetup.officeapps.live.com/c2r/download.aspx\?ProductreleaseID\=ProPlus2021Retail\&platform\=x64\&language\=en-us\&version\=O16GA -P ~/Downloads -O ms_office_setup.exe
```

Login to Windows 11 and copy `ms_office_setup.exe` file from Linux file system to `Downloads` of Windows 11 and start the installation. This is required as `*.exe` files can be executed only from Windows filesystem. 

The activation code shown below is also taken from another of the GitHub repo. Execute all lines of command at one go in Cmd window with ***Admin privilege*** on Windows 11 VM.


```powershell
if exist "C:\Program Files\Microsoft Office\Office16\ospp.vbs" cd /d "C:\Program Files\Microsoft Office\Office16"
if exist "C:\Program Files (x86)\Microsoft Office\Office16\ospp.vbs" cd /d "C:\Program Files (x86)\Microsoft Office\Office16"
for /f %x in ('dir /b ..\root\Licenses16\ProPlus2021VL_KMS*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%x"
cscript ospp.vbs /inpkey:FXYTK-NJJ8C-GB6DW-3DYQT-6F7TH
cscript ospp.vbs /sethst:kms.msgang.com
cscript ospp.vbs /act
pause
```

Once MS Office is installed on Windows 11 **you must logout**.

## Configure Winapps

Before installing winapps, we must create a configuration file at `~/.config/winapps/winapps.conf` with following content. The username and password values should be exactly what we have used in `compose.yaml` file. In my case, I changed only **RDP_USER** and **RDP_PASS**.

```conf
##################################
#   WINAPPS CONFIGURATION FILE   #
##################################

# INSTRUCTIONS
# - Leading and trailing whitespace are ignored.
# - Empty lines are ignored.
# - Lines starting with '#' are ignored.
# - All characters following a '#' are ignored.

# [WINDOWS USERNAME]
RDP_USER="sundar"

# [WINDOWS PASSWORD]
# NOTES:
# - If using FreeRDP v3.9.0 or greater, you *have* to set a password
RDP_PASS="Pa$$w0rd"

# [WINDOWS DOMAIN]
# DEFAULT VALUE: '' (BLANK)
RDP_DOMAIN=""

# [WINDOWS IPV4 ADDRESS]
# NOTES:
# - If using 'libvirt', 'RDP_IP' will be determined by WinApps at runtime if left unspecified.
# DEFAULT VALUE:
# - 'docker': '127.0.0.1'
# - 'podman': '127.0.0.1'
# - 'libvirt': '' (BLANK)
RDP_IP=""

# [WINAPPS BACKEND]
# DEFAULT VALUE: 'docker'
# VALID VALUES:
# - 'docker'
# - 'podman'
# - 'libvirt'
# - 'manual'
WAFLAVOR="docker"

# [DISPLAY SCALING FACTOR]
# NOTES:
# - If an unsupported value is specified, a warning will be displayed.
# - If an unsupported value is specified, WinApps will use the closest supported value.
# DEFAULT VALUE: '100'
# VALID VALUES:
# - '100'
# - '140'
# - '180'
RDP_SCALE="100"

# [ADDITIONAL FREERDP FLAGS & ARGUMENTS]
# NOTES:
# - You can try adding /network:lan to these flags in order to increase performance, however, some users have faced issues with this.
# DEFAULT VALUE: '/cert:tofu /sound /microphone'
# VALID VALUES: See https://github.com/awakecoding/FreeRDP-Manuals/blob/master/User/FreeRDP-User-Manual.markdown
RDP_FLAGS="/cert:tofu /sound /microphone"

# [MULTIPLE MONITORS]
# NOTES:
# - If enabled, a FreeRDP bug *might* produce a black screen.
# DEFAULT VALUE: 'false'
# VALID VALUES:
# - 'true'
# - 'false'
MULTIMON="false"

# [DEBUG WINAPPS]
# NOTES:
# - Creates and appends to ~/.local/share/winapps/winapps.log when running WinApps.
# DEFAULT VALUE: 'true'
# VALID VALUES:
# - 'true'
# - 'false'
DEBUG="true"

# [AUTOMATICALLY PAUSE WINDOWS]
# NOTES:
# - This is currently INCOMPATIBLE with 'docker' and 'manual'.
# - See https://github.com/dockur/windows/issues/674
# DEFAULT VALUE: 'off'
# VALID VALUES:
# - 'on'
# - 'off'
AUTOPAUSE="off"

# [AUTOMATICALLY PAUSE WINDOWS TIMEOUT]
# NOTES:
# - This setting determines the duration of inactivity to tolerate before Windows is automatically paused.
# - This setting is ignored if 'AUTOPAUSE' is set to 'off'.
# - The value must be specified in seconds (to the nearest 10 seconds e.g., '30', '40', '50', etc.).
# - For RemoteApp RDP sessions, there is a mandatory 20-second delay, so the minimum value that can be specified here is '20'.
# - Source: https://techcommunity.microsoft.com/t5/security-compliance-and-identity/terminal-services-remoteapp-8482-session-termination-logic/ba-p/246566
# DEFAULT VALUE: '300'
# VALID VALUES: >=20
AUTOPAUSE_TIME="300"

# [FREERDP COMMAND]
# NOTES:
# - WinApps will attempt to automatically detect the correct command to use for your system.
# DEFAULT VALUE: '' (BLANK)
# VALID VALUES: The command required to run FreeRDPv3 on your system (e.g., 'xfreerdp', 'xfreerdp3', etc.).
FREERDP_COMMAND=""
```


Once the config file is created, we can download and execute the winapps installer from the host i.e., Alma Linux/ RHEL terminal using following command.

```shell
bash <(curl https://raw.githubusercontent.com/winapps-org/winapps/main/setup.sh)
```

Follow the on-screen instruction. Install winapps to current system is recommended. Now onwards, you can open any Word, Presentation or Excel sheets by right click and choosing appropriate MS Office application names. Anytime, you can access Windows 11 OS from Application --> Others --> Windows (on Xfce).


## Miscellaneous

I have tried winapps on GNOME, KDE and now in XFCE. I found that performance of MS Office is much better in XFCE. It opens all application as intended and faster than other desktop environments. Whenever you reboot your system, Windows 11 should also be booting in background. If something goes wrong, it is most likely that Windows 11 has not booted in the background. You can use following commands to check and control Windows 11 VM using docker commands.

Check if the Windows 11 is booted by checking docker process.

```shell
docker ps
```

and it should show the running process as shown below.

```shell
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                                                                                                                             NAMES
4d4e0f472f61   dockurr/windows   "/usr/bin/tini -s /r…"   7 minutes ago   Up 2 minutes   0.0.0.0:3389->3389/tcp, :::3389->3389/tcp, 0.0.0.0:8006->8006/tcp, 0.0.0.0:3389->3389/udp, :::8006->8006/tcp, :::3389->3389/udp   windows
```

Following actions can be performed whenever you are in need with respect to Windows 11 Virtual Machine.

```shell
docker compose --file ~/.config/winapps/compose.yaml start # Power on the Windows VM
docker compose --file ~/.config/winapps/compose.yaml pause # Pause the Windows VM
docker compose --file ~/.config/winapps/compose.yaml unpause # Resume the Windows VM
docker compose --file ~/.config/winapps/compose.yaml restart # Restart the Windows VM
docker compose --file ~/.config/winapps/compose.yaml stop # Gracefully shut down the Windows VM
docker compose --file ~/.config/winapps/compose.yaml kill # Force shut down the Windows VM
```

That's it for now.
