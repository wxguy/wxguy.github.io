---
title: Install Arch Linux Easiest Way Through GUI and CLI
author: wxguy
date: 2022-05-07 15:00:00 +0530
categories: [Tutorial]
tags: [arch linux,linux, manjaro]
pin: false
toc: true
comments: true
render_with_liquid: true
media_subpath: /assets/img/arch-install/
image:
  path: arch_linux_screen_shot.webp
  lqip: data:image/webp;base64,UklGRlYAAABXRUJQVlA4IEoAAABwAwCdASoUAAsAPzmGuVOvKSWisAgB4CcJQBOgA8jLpqbWapAA+2fFDQ2d6qqCSIE8rKqhProPikd+/l/Lqj0AlJno3rukLzcwAA==
  alt: Sample Screenshot by Reditt User 'Houston_NeverMind' on 'unixporn' Channel. (Credit:- https://www.reddit.com/)
---

-------

## Update on Availability of `to-arch.sh` Script

The link to the `to-arch.sh` script mentioned in this post is vanished. The user account of the author is suspended at [https://sr.ht](https://sr.ht). Whatever the reason may be, the the usefulness of this script will be useful to others who whats to follow the instructions mentioned in this post. Therefore, I have uploaded a copy of this script in to my blog post. You can download the script from here [https://wxguy.github.io/assets/downloads/files/to-arch.sh](https://wxguy.github.io/assets/downloads/files/to-arch.sh). Since there is no update to this script from the author, I highly recommend to use it on Virtual Box first on a fresh install and on a real hardware thereafter.

## Why Yet Another Arch Linux Install Tutorial?

If I have to say it in two words, the tutorial written here is damn `easy` and `quick`.

I started using [Arch Linux](https://archlinux.org/) somewhat around 2014. It was new to me migrating to Arch from Ubuntu as many procedures required to be configured manually. I referred Arch Wiki's famous _**Beginners Guide**_ to install and configure my system. I had to spend a lot of time reading the wiki to understand and execute commands one by one. It was time-consuming. 

But now it is 2022, there is no more **_Beginners Guide_** in the Arch Wiki. The official installation guide is located at [https://wiki.archlinux.org/title/installation_guide](https://wiki.archlinux.org/title/installation_guide) and is updated with a lot of content to match the changes in newly included Arch Linux software and components. This time, I do not have time to read and install it. Done some search and research on google and finally fixed this procedure.

## Enter Arch Linux Through Backdoor

The procedure we will follow here is not to install Arch Linux. We will not even download Arch Linux ISO. Instead, we will install Manjaro Linux and remove Manjaro OS-specific packages and components from the installed system. A question may arise if it is the right thing to do so? The answer to this question is "Absolutely Yes". 

For this, you need to understand how this Manjaro Linux or any other derivative Linux distributions are built from its parent distribution. Manjaro claims to be easy to use for beginners and advanced users. To achieve these stated objectives, developers behind Manjaro develop a few GUI tools, artworks, and additional configurations to OS components here and there. They also host all the Arch Linux packages on their repository to control update frequency and delivery of packages to you.

If you know what additional packages are built by Manjaro and how to revert to the Arch Linux repo, then you can remove those packages easily and default back to Arch Linux. That is what exactly is done by a little bash utility called `to-arch.sh`.

## What is `to-arch.sh` All About?

`to-arch.sh` is a bash shell script, developed by `kskeigrshi`. Unlike, most open-source software, this bash script is hosted at [https://sr.ht/~kskeigrshi/to-arch.sh/](https://sr.ht/~kskeigrshi/to-arch.sh/), rather than famous code hosting websites such as GitHub, GitLab, Sourceforge, etc. This script is designed to uninstall all software components installed by Manjaro and rebase the repo back into Arch Linux repositories. While doing so, it will ask a series of questions from users to configure the system properly. In the end, you will get your Arch Linux back while preserving the userland and packages which was installed as part of Manjaro OS.

I tested this script around November 2021 (Manjaro KDE) and tried again a few days back (Manjaro Xfce). On both occasions, it worked like a charm. Manjaro installation took around 18 min and reverting to Arch Linux took just 8 min. That is awesome considering hours required for installing Arch Linux manually. In the rest of the article, I will write on steps to be followed to achieve this.

## System and Software Configurations

I have used this script on both real hardware and virtual machine. The script was executed immediately after the installation of Manjaro Linux to avoid potential conflicts at a later stage. Screenshots shared here are taken from Virtual Box, but the is no difference I found under real hardware as well.

The Manjaro Linux I tried was version `21.2.6` and the ISO downloaded was `manjaro-xfce-21.2.6-220416-linux515.iso`. 

The bash script `to-arch.sh` version I used was `8.1.1`. However, it is highly recommended to download the latest version of the script from [https://git.sr.ht/~kskeigrshi/to-arch.sh/refs](https://git.sr.ht/~kskeigrshi/to-arch.sh/refs).

## Convert Manjaro to Arch Linux

> The procedure detailed here is not only applicable to Manjaro. The `to-arch.sh` will also convert [Garuda Linux ](https://garudalinux.org/) and [Endeavour Linux OS](https://endeavouros.com/).
{: .prompt-tip }

There are many better tutorials available on how to install Manjaro Linux. Here, the focus is to convert Manjaro to Arch Linux. Therefore, I assume that you have already installed Manjaro Linux and booted for the first time. Now head over to [https://git.sr.ht/~kskeigrshi/to-arch.sh/refs](https://git.sr.ht/~kskeigrshi/to-arch.sh/refs) and click on latest version no and then click on `to-arch.sh` link which will be available just about `sha256:xxxxxxxx...` text. If everything goes all right, the `to-arch.sh` script should have been downloaded to your `Downloads` directory. 

Ensure that you have downloaded the script.

```console
$ cd ~/Downloads
$ ls
to-arch.sh
```

Execute the script from the Downloads directory.

```console
$ bash to-arch.sh
```

The script will ask you to select OS type as shown below. If you are not running as root, it will ask you to provide a sudo password. 

![Selection of OS](manj-to-arch-sel-os.png){: .shadow }
_`to-arch.sh`: Selection of OS Type and `sudo` Password_

Providing a `sudo` password will update the system and automatically identify and inform the user about the type of OS they are running. It will also ask you if `pamac` is to be removed. Go ahead and say `Y` to this.  

![Confirmation of OS](manj-to-arch-os-confirm.png){: .shadow }
_`to-arch.sh`: Confirmation of OS Type and Remove `pamac`_

Before proceeding further, you need to select the Arch Linux repo. For this, `to-arch.sh` provides you to choose your favorite console-based text editor. I have chosen `nano`, but you may choose `vim` if you are comfortable. Once the text editor is enabled, the script will ask you to uncomment the repo URL from the Arch Linux user repository. Scroll down to your nearest country and enable repo. Once the repo is selected, the script will remove all Manjaro-specific components and upgrade the system.

At last, you will be asked to provide a choice to install `linux` or `linux-lts`. Select your choice.

![Option to select Linux](manj-to-arch-sel-linux.png){: .shadow }
_`to-arch.sh`: User Confirmation to Select Linux Type_

I have selected `linux-lts`. That's it, the system will be upgraded once again, and all the packages will be downloaded from the newly configured Arch Linux repository. Once all packages are installed, you can reboot your system.

## Post Install

After you reboot your system, you can check if you running Arch or Manjaro Linux by issuing the below command.

```console
$ cat /etc/lsb-release
DISTRIB_ID="Arch"
DISTRIB_RELEASE="rolling"
DISTRIB_DESCRIPTION="Arch Linux"
```

There is just one small issue I have seen after converting to Arch Linux. There was no icon set for Application Menu in the bottom panel. You can see the issue in the below screenshot.

![Arch Linux Home](manj-to-arch-home.png){: .shadow }
_`to-arch.sh`: Arch Linux Home After Converting from Manjaro_

This was seen in both `Xfce` and `KDE` editions. I just have set a new icon for the application launcher by right-clicking on it.

That's it. Now onwards, you can proudly say that you are running Arch Linux.

------
You can download this article from [here](https://wxguy.github.io/assets/downloads/pdfs/2022-05-07-install-arch-linux-easiest-way.pdf) for free.

------

