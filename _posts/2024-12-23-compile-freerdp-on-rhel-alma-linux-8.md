---
title: Compile Latest FreeRDP on Red Hat 8.x or Alma Linux 8.x or Any Other RHEL Based Distros
author: wxguy
date: 2024-12-23 15:00:00 +0530
categories: [Tutorial]
tags: [virtual-machine, linux]
pin: false
toc: true
comments: true
render_with_liquid: true
img_path: /assets/img/freerdp/
---

## About FreeRDP

FreeRDP is an Open Source implementation of Microsoft's proprietary Remote Desktop Protocol (RDP) client. It allows users to connect to the remote desktop, which can be Windows, Linux, or macOS. The project is hosted in GitHub and distributed under an Apache licence. The application is feature-rich and has many command-line options to improve the performance of remote desktop access. Due to the popularity of this application, many distributions make it available in their repository. Apart from accessing the remote desktops, I also use this application for accessing MS Office suite, as part of [winapps](https://github.com/winapps-org/winapps),  from my Linux system.


## Why do you need to compile

While a distribution I use already has FreeRDP in its repo, why do you need to compile it? If you use rolling-based distribution, then you need not worry. However, traditional distributions such as Red Hat, Ubuntu, etc., tend to ship only specific versions in their repos. However, FreeRDP is an actively developed application and releases are made with more features in quick succession. The need for compiling FreeRDP arose when I wanted to use winapps on Alma Linux/ RHEL version 8.x. The FreeRDP version available in the repo is 2.11.7 whereas the minimum required by winapps is 3.0. There is also an option to install the latest FreeRDP using flatpak. But it won't work with winapps due to a known bug. Therefore, I was compelled to compile FreeRDP on Alma Linux 8.x. While there are clear instructions on FreeRDP wiki at GitHub [https://github.com/FreeRDP/FreeRDP/wiki/Compilation#compilation](https://github.com/FreeRDP/FreeRDP/wiki/Compilation#compilation), it is not meant for beginners. I have spent a day compiling the software and learned a few additional things that I want to document here. The procedure listed here also works for RHEL 8.x or other RHEL-based distributions and tested with version 8.x only. It may work on the 9.x series as well, but you may need to adjust certain procedures when you encounter errors.

## Install necessary packages

We need the basic development tools to compile software. In RHEL-based distros, it comes in the form of a group package. We can install all packages using the following command:-

```shell
sudo dnf groupinstall 'Development Tools' -y
```
 
FreeRDP is a complex software that deals with network, security, audio, video, filesystems, etc., and hence various additional packages are required to be installed. The following command will do that:-

```shell
sudo dnf install -y ninja-build alsa-lib-devel uriparser-devel cjson-devel SDL2 SDL2_ttf-devel SDL2_image libusb cups-devel libuuid-devel ccache cmake fuse3-devel opus-devel lame-devel docbook-style-xsl ocl-icd-devel pam-devel systemd-devel git-clang-format openssl-devel libX11-devel libXext-devel libXinerama-devel libXcursor-devel libXi-devel libXdamage-devel libXv-devel libxkbfile-devel alsa-lib-devel libusb-devel SDL2-devel pkcs11-helper-devel krb5-devel libwinpr-devel soxr-devel cairo-devel wayland-devel wayland-protocols-devel
```

If you find any missing package from the repo, ensure to enable the `epel` repo.

> If you find a missing `xxx` package during compilation, search for that specific package using `dnf search xxx`. If the search is successful and the package named `xxx-devel` is available, then install it using `sudo dnf install xxx-devel -y`.
{: .prompt-tip }

## Export Paths

During compilation, some libraries would expect config files of other libraries in specific package config locations. These locations can be found using `pkg-config --variable pc_path pkg-config` command. To append the `PKG_CONFIG_PATH` with the search path, use the following command before starting compiling:-

```shell
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:$(pkg-config --variable pc_path pkg-config)
```

## Link Libraries

During compilation, `make` command kept on compiling about missing library `libwinpr-tools.so`. However, I was sure that I installed the exact library earlier. To find what went wrong and what exact name of the library `*.so` file it is searching, use the following command:- 

```shell
ld -lwinpr-tools --verbose  
```

In the long list of output, search for two specific lines, as shown below:-

```shell
SEARCH_DIR("=/usr/x86_64-redhat-linux/lib64"); SEARCH_DIR("=/usr/lib64"); SEARCH_DIR("=/usr/local/lib64"); SEARCH_DIR("=/lib64"); SEARCH_DIR("=/usr/x86_64-redhat-linux/lib"); SEARCH_DIR("=/usr/local/lib"); SEARCH_DIR("=/lib"); SEARCH_DIR("=/usr/lib");
.
.
attempt to open //usr/x86_64-redhat-linux/lib64/libwinpr-tools.so failed
```

Here, the `make` is looking for `libwinpr-tools.so` in the following directories:-

```shell
 /usr/x86_64-redhat-linux/lib64
 /usr/lib64
 /usr/local/lib64
 /lib64
 /usr/x86_64-redhat-linux/lib
 /usr/local/lib
 /lib
 /usr/lib
```

I searched all the above-listed directories and found the required libraries in the following locations:-

```shell
ls /usr/lib64/libwinpr*                                                                                                          

/usr/lib64/libwinpr2.so    /usr/lib64/libwinpr2.so.2.11.7  /usr/lib64/libwinpr-tools2.so    /usr/lib64/libwinpr-tools2.so.2.11.7
/usr/lib64/libwinpr2.so.2      /usr/lib64/libwinpr-tools2.so.2  
```

As you can see, the specific library was available but with a different name. To make it work, I have linked the following two libraries with specific names as shown below:-

```shell
sudo ln -s /usr/lib64/libwinpr-tools2.so.2.11.7 /usr/lib64/libwinpr-tools.so
sudo ln -s /usr/lib64/libwinpr2.so.2.11.7 /usr/lib64/libwinpr.so
```

## Compile FreeRDP

Now we are ready to go ahead and start the compilations. Accordingly, clone the FreeRDP repo to a specific location, and in my case it is in `~/Downloads` directory:-

```shell
git clone --depth 1 https://github.com/freerdp/freerdp.git ~/Downloads/
```

FreeRDP uses `cmake` for compilation. Therefore, create a `build` directory inside the cloned repo and move into it:-

```shell
mkdir -p ~/Downloads/freerdp/build
cd ~/Downloads/freerdp/build
```

By default, the installation location is `/usr/local`. However, I don't want to disturb the default system paths. Accordingly, I choose to install at `/home/sundar/freerdp3`. Here, `sundar` is my username and change it as per your requirement. Compile FreeRDP with the following options, and you can refer to all available options with their meanings from the official link [https://github.com/FreeRDP/FreeRDP/wiki/Compilation#Options](https://github.com/FreeRDP/FreeRDP/wiki/Compilation#Options):-

```shell
cmake .. -DWITH_MBEDTLS=ON -D CMAKE_INSTALL_PREFIX=/home/sundar/freerdp3 D CMAKE_SKIP_INSTALL_RPATH=ON -DWITH_CAIRO=ON -DWITH_SERVER=ON -DWITH_SAMPLE=ON -DUSE_UNWIND=OFF -DWITH_SWSCALE=OFF -DWITH_PLATFORM_SERVER=OFF
```

You have to `CMAKE_INSTALL_PREFIX=/home/sundar/freerdp3` option with yours. If there is no error, go ahead and create executables using the `make` command:-

```shell
make
```

Finally, install the executables and libraries in the custom location that we have mentioned as part of the `cmake` argument:-

```shell
make install
```

If everything goes well, it should not report any errors.

## Append PATH and Test Installation

Check if `xfreerdp` is created at a custom location:-

```shell
ls /home/sundar/freerdp3/bin

freerdp-proxy  freerdp-shadow-cli  sfreerdp  sfreerdp-server  wlfreerdp  xfreerdp
```

The executable is successfully created as intended. Make it available to be executed from the terminal by appending `~/.bashrc`, if you use `bash`  or `~/.zshrc`, if you use `zsh`. The following command is for the `zsh` shell which I use currently:-

```shell
echo "PATH=$PATH:/home/sundar/freerdp3/bin" >> ~/.zshrc
```

If you don't want to close the terminal and reopen it to use `xfreerdp`, source the `~/.zshrc` file:-

```shell
source ~/.zshrc
```

Validate if all are successful using the following:-

```shell
xfreerdp --version
This is FreeRDP version 3.10.4-dev0 (9fe96b4)
```

That's it. Apart from compiling FreeRDP, I have mentioned certain additional steps to debug the compilation process. I hope that it helps someone look for a similar guide.

