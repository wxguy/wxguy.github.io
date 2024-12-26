---
title: Make RHEL 8.x / Alma Linux 8.x as Perfect Desktop
author: wxguy
date: 2024-12-25 05:30:00 +0530
categories: [Tutorial]
tags: [rhel, linux, alma-linux]
pin: false
toc: true
comments: true
render_with_liquid: true
img_path: /assets/img/rhel_perfect_desktop/
---

## Why Old and Conservative Distribution

From my previous blog posts, one can understand that I use Arch Linux with K Desktop Environment (KDE) as my primary desktop on my Laptop. I also have one more ASUS Laptop which I got in 2018. One day, I realised that I had not used my old Laptop for a very long time. Therefore, I wanted to try some Distro which is not KDE on this. Immediately, Ubuntu was the only Distro that came to my mind due to the excellent performance it gave me on the very same Laptop. Downloaded and tried the recently released Ubuntu 24.04 LTS version. To my surprise, for the first time, it gave me a disastrous performance. It took too much time to install and boot thereafter. I had to wait for at least four to five seconds after clicking any application to open. Updated the packages, including Kernel, to the latest version to check if the issue is gone. But it did not. 

This issue has forced me to drop my plan to use Ubuntu and opt for a lightweight distro. At this point, I also decided to drop the GNOME-based Distro and try some other Desktop Manager that does not compromise on usability as well. After doing some research and from my past experience I decided to go for XFCE4-based Distro.

After deciding on XFCE4 Desktop Manager, then came yet another headache. Which Distro to choose? Everyone from various websites and tech blogs suggested using MX Linux, Linux Mint or XYZ Distro which all boils down to **Debian** + **XFCE4**. However, I also realised at the same time that I already use Red Hat in one of my Virtual Machines and am also required to work on RHEL for my professional work. Therefore, decided to install Alma Linux which is free Enterprise Linux, an alternative to RHEL. Though the Distro is currently under maintenance support with effect from 3 May 2024, I decided to install Alma Linux 8.x, precisely version 8.10, to ensure I use the same version I use for Official purposes.

Therefore, I decided to install **Alma Linux** Version **8.10** with *XFCE4* as Desktop Manager. 

**Note**: What I meant by saying conservative in my heading is that it is **Super Stable**. 

## Install RHEL / Alma Linux 8.x

Due to its popularity, there are tons of tutorials available online. I don't want to repeat the same in this post. However, I want to highlight a few important steps I have taken here.


### Download ISO

The links to various ISO files are [here](https://almalinux.org/get-almalinux). I downloaded **AlmaLinux OS 8.10 Minimal ISO**. Though it is named Minimal, it is a DVD image and its size is around 2GB. It also allows us to install the OS in offline mode.

### Make Live USB

I used `lsblk` to find my USB disk. In my case, `/dev/sdc` was the name of the attached disk. 

```shell
$ sudo dd if=/path/to/alma-linux-8.10.iso of=/dev/sdc status=progress conv=fsync bs=4M && sync
```

Change `/path/to/alma-linux-8.10.iso` with the real path of AlmaLinux OS 8.10 Minimal ISO. 

The other option to make a reliable Live USB is [balenaEtcher](https://etcher.balena.io). 

### Install Options

I had a 500GB SSD disk available on my Laptop and selected the full disk. I also gave automatic partitioning during installation. Post-install, this is what I got:-

```shell
$ df -h
Filesystem                  Size  Used Avail Use% Mounted on
devtmpfs                    7.8G     0  7.8G   0% /dev
tmpfs                       7.8G   91M  7.7G   2% /dev/shm
tmpfs                       7.8G  9.3M  7.8G   1% /run
tmpfs                       7.8G     0  7.8G   0% /sys/fs/cgroup
/dev/mapper/almalinux-root   70G   27G   44G  38% /
/dev/sdb2                  1014M  315M  700M  31% /boot
/dev/sdb1                   599M  5.9M  593M   1% /boot/efi
/dev/mapper/almalinux-home  387G   14G  373G   4% /home
tmpfs                       1.6G   12K  1.6G   1% /run/user/1000
```


## Connecting to the Internet

During the installation process, I noticed that the system took more than 50 min to install. Therefore, I decided to update to check if the same issue persists. This is where I encountered the issue. The recommended way to connect to the internet using the command line is using `nmcli`. However, the application could not activate my wifi at all. Then, I came up with my own solution using a mobile phone. I connected my Android phone to a Wi-Fi network and used a USB cable to provide internet to my Laptop using USB Tethering. This is the only way I could connect to the Internet before bringing up my Desktop. Later, I also used `sudo nmtui` to activate wifi and connect to the Internet.

## Add Additional Repositories

The default repositories enabled in Alma Linux / RHEL are Base OS and Applications. Some of the applications we will be using later are available in third-party repositories. Enable those now.

Install and enable Extra Package for Enterprise Linux (EPEL) repo.

```shell
$ sudo dnf install epel-release
$ sudo dnf --enablerepo=epel group
```

Enable Power Tool repository.

```shell
$ sudo dnf config-manager --set-enabled powertools
```

Install and enable fusion repos. Fusion has free and non-free sections. We will add both of them.

```shell
$ sudo dnf install distribution-gpg-keys -y

$ sudo rpmkeys --import /usr/share/distribution-gpg-keys/rpmfusion/RPM-GPG-KEY-rpmfusion-free-el-$(r
pm -E %rhel)

$ sudo rpmkeys --import /usr/share/distribution-gpg-keys/rpmfusion/RPM-GPG-KEY-rpmfusion-nonfree-el-
$(rpm -E %rhel)

$ sudo dnf --setopt=localpkg_gpgcheck=1 install  https://mirrors.rpmfusion.org/free/el/rpmfusion-fre
e-release-$\(rpm -E %rhel).noarch.rpm https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-
$\(rpm -E %rhel).noarch.rpm

$ sudo dnf --setopt=localpkg_gpgcheck=1 install  https://mirrors.rpmfusion.org/free/el/rpmfusion-fre
e-release-$\(rpm -E %rhel\).noarch.rpm https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release
-$\(rpm -E %rhel\).noarch.rpm

$ sudo dnf --setopt=localpkg_gpgcheck=1 install  https://mirrors.rpmfusion.org/free/el/rpmfusion-fre
e-release-$(rpm -E %rhel).noarch.rpm https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-$(rpm -E %rhel).noarch.rpm
```

Add sublime-text repo.

```shell
$ sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

$ sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo
```

# Install XFCE4 and Enable Auto Login

Next, we will install the XFCE4 graphical desktop. It is available as a group package. Check if the package is available in the repo.

```shell
$ sudo dnf group list | grep -i xfce

Xfce
```

If you see `Xfce` output as shown above, we can go ahead and install the Desktop.


```shell
$ sudo dnf groupinstall "Xfce" "base-x" -y
```

Start the XFCE desktop on reboot.

```shell
$ echo "exec /usr/bin/xfce4-session" >> ~/.xinitrc
$ sudo systemctl set-default graphical
$ sudo reboot
```

For auto login (without the need to provide a password on every reboot), edit `/etc/gdm/custom.conf` file and add the following content.

```shell
[daemon]
AutomaticLogin=sundar
AutomaticLoginEnable=True
```

Where `sundar` is my username and you need to change it as per your username.

## Install Better Shell (zsh)

The first thing I do after installing any distro is to install `zsh` and configure it with a few additional plugins. Apart from aesthetics, it provides increased performance with assistance. Install and change the shell.

```shell
$ sudo dnf install git chsh zsh -y
$ chsh -s $(which zsh)
```

Once the shell is changed, we need to reboot for proper functioning.

```shell
$ sudo reboot
```

When you open Terminal for the first time after enabling zsh, it will ask you questions. Accept recommended options and write the default `.zshrc` file. Before we start installing plugins, we need to download and install the necessary fonts to display symbols in the Terminal. Execute the following lines one by one in the terminal.

```shell
# Create fonts directory
$ mkdir ~/.local/share/fonts

# Download MesloLGS fonts family to 'fonts' directory
$ wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf -P ~/.local/share/fonts

$ wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf -P ~/.local/share/fonts

$ wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf -P ~/.local/share/fonts

$ wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf -P ~/.local/share/fonts

# Also install the famous JetBrain mono fonts
$ wget https://github.com/ryanoasis/nerd-fonts/releases/latest/download/JetBrainsMono.zip -P ~/.local/share/fonts 

# Move into the font directory
$ cd ~/.local/share/fonts 

# Unzip compressed fonts
$ unzip JetBrainsMono.zip 

# Remove the downloaded zip file 
$ rm JetBrainsMono.zip

# Update font cache to make it available to all apps
$ fc-cache -fv
```

Once the fonts are installed, we need to select `MesloLGS NF Regular` in the terminal by navigating to Edit --> Preferences --> Appearance --> Change Font to **MesloLGS NF Regular**. Once the correct font is selected, we can clone plugin repos. We will be using only three plugins viz., `zsh-syntax-highlighting`, `zsh-autosuggestions` and `powerlevel10k`. Please google to find out more about the plugins. Clone one by one as below.

```shell
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

$ git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Update the  `~/.zshrc` file with following content.


```shell
ZSH_THEME="powerlevel10k/powerlevel10k"
```

and

```shell
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
```

Finally, source the `zshrc` file using the `source ~/.zshrc` command to take effect of all changes before closing terminal.

## Install Necessary Software

The term **Necessary** is a relative word. Here, I meant necessary for me to work on a daily basis. Your requirements may be different. I installed the following packages.

### Install Google Chrome

Edit or create `/etc/yum.repos.d/google.chrome.repo` file with the following content.

```shell
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

```

Install the Google Chrome Stable version.

```shell
$ sudo dnf install google-chrome-stable -y
```

### Install Sublime Text

```shell
$ sudo dnf install sublime-text -y
```

Once installed, you may want to follow my  [https://wxguy.in/posts/sublime-text-as-python-ide](https://wxguy.in/posts/sublime-text-as-python-ide) tutorial to make it as perfect IDE for Python.

### Install Documents Software

Alma Linux does not ship with Office suite by default. The Libre Office available in the repo is very old. But I want to install only the latest version. Here comes the **flatpak** for rescue. Enable it and install Libre Office & Artha (excellent offline dictionary).

Install and enable flatpak repo.

```shell
$ sudo dnf install flatpak -y
$ flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

Install the applications.

```shell
$ flatpak install flathub net.sourceforge.artha.Artha

$ flatpak install flathub org.libreoffice.LibreOffice
```
If you wish to run the flatpak application from terminal, following command will do.

```shell
# $ flatpak run application_name
$ flatpak run net.sourceforge.artha.Artha
```

Anytime, you can search for the latest software using the following command.

```shell
$ flatpak search application_name
```


### Install Audio and Video Players

It is completely a personal choice. I use minimal ones viz., `rhythmbox` for audio and `mpv` for video.

```shell
$ sudo dnf install rhythmbox mpv -y
```

### Miscellaneous Software

Some of the additional utilities are required for dealing with files and other packages.

```shell
$ sudo dnf install file-roller makeself util-linux-user -y
```

## Personalise Look and Feel

The XFCE4 desktop installed earlier carries bare bone look. However, there are additional plugins already available in the repo for improving productivity. You can search xfce4 plugins using the following command.

```shell
$ sudo dnf search xfce4-
```

Once installed, the plugin can be added to the panel by ring-clicking on it.
   

## Install Dracula Theme

There are many themes available for Xfce desktop. I prefer to use Dracula which is a complete theme for desktop, terminal, neovim and other applications. Install it by following the steps.

```shell
# Download Dracula theme
$ wget https://github.com/dracula/gtk/archive/master.zip -P ~/Downloads

# Extract the compressed file
$ unzip master.zip

# Rename the directory to Dracula
$ mv gtk-master Dracula

# Create theme directory and move the Dracula directory
$ mkdir ~/.themes
$ mv Dracula ~/.themes

# Clone Dracula xfce-terminal theme
$ git clone https://github.com/dracula/xfce4-terminal.git
$ mkdir -p ~/.local/share/xfce4/terminal/colorschemes
$ cp xfce4-terminal/Dracula.theme ~/.local/share/xfce4/terminal/colorschemes

# Download Dracula icons
$ git clone https://github.com/m4thewz/dracula-icons ~/.icons/dracula-icons
```

Once installed, themes and icons can be changed from xfce Settings Manager --> Appearance


## Install and Configure Conky

Conky is a lightweight desktop theme and widget manager. It uses less memory and makes your desktop look great. Many themes are available on the GitHub platform. But, I prefer to use **conky-grapes** from [https://github.com/popindavibe/conky-grapes](https://github.com/popindavibe/conky-grapes). This theme requires the following system packages.


```shell
$ sudo dnf install conky ffmpeg jq ncal acpi mocp -y
```

Install **conky-grapes** theme and set it up.

```shell
# Clone repo and move inside the cloned repo
$ git clone https://gitlab.nomagic.uk/popi/conky-grapes ~/conky/conky-grapes
$ cd ~/conky/conky-grapes

# Copy font file to local fonts directory
$ cp *.ttf ~/.local/share/fonts
```

There is a bug in the **conky-grapes** package. It searches for wifi network information in `/proc/net/wireless` file. However, this file does not exist in RHEL version 8.x based distros. This problem can be solved by replacing '/proc/net/wireless' with '/proc/net/igmp' in 'create_config.py' file. Edit the file accordingly.

Once done, see the list of options available with this theme package.

```shell
$ python3 ./create_config.py -h
```

I prefer to use black or dark wallpaper. For my wallpaper, I found the following colours looks better.

```shell
$ python3 ./create_config.py -ri red -ti white -te pink
```

We can view the changes immediately by issuing the following command.

```shell
$ conky -q -d -c ~/conky/conky-grapes/conky_gen.conkyrc
```

We can autostart the above conky widget during boot. Create a file at `~/.config/autostart/conky_theme.sh` with the following content.


```bash
#!/bin/sh

# Wait for 3 sec to load Desktop GUI components
sleep 3

conky -q -d -c ~/conky/conky-grapes/conky_gen.conkyrc
```

Make it executable.

```shell
$ chmod +x ~/.config/autostart/conky_theme.sh
```

To auto start create a `~/.config/autostart/conky_grapes.desktop` file with the following content (change `username` with yours). 

```shell
[Desktop Entry]
Name=Conky Grapes Theme
GenericName=Conky Theme
Comment=Start Conky Grapes Theme
Exec=/home/username/.config/autostart/conky_theme.sh
Terminal=false
Type=Application
X-GNOME-Autostart-enabled=true
```

Now onwards, the conky theme will load automatically on every reboot.

## Install and Configure Neovim

I use neovim or nvim occasionally to learn and for fun. I have already working configurations on my Arch Linux and hosted the config file in GitHub at [https://github.com/wxguy/nvim](https://github.com/wxguy/nvim). But making it work under RHEL or Alma Linux version 8.x is little tricky. If you install nvim from the repo, it won't work as it is very old.

Download the latest working version of nvim for older distributions.

```shell
$ wget -c https://github.com/neovim/neovim-releases/releases/latest/download/nvim-linux64.tar.gz
```

Extract the compressed file to the home directory.

```shell
$ tar -xvzf nvim-linux64.tar.gz -C ~
```

Make the extracted directory hidden with a .

```shell
$ mv ~/nvim-linux64 ~/.nvim
```

Create a soft link at bin directory to invoke nvim from the terminal easily.

```shell
$ mkdir -p  ~/.local/bin/nvim

$ ln -s ~/.nvim/bin/nvim ~/.local/bin/nvim
```


Install necessary applications so that configurations work fine.

The pyright plugin we will use requires at least node version 12 or above. Install it by using the following commands.

```shell
# Disable default node
$ sudo dnf module reset nodejs

# Install node version 14
$ sudo dnf module install nodejs:14
```

Some of the nvim plugins use the latest applications that are not even available in the repo. However, they are distributed through `copr` repo. Add and enable some of the repos.

```shell
$ sudo yum install yum-utils -y

$ sudo yum-config-manager --add-repo=https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo

$ sudo dnf copr enable tkbcopr/fd -y

$ sudo dnf copr enable atim/lazygit -y
```

Install necessary applications.

```shell
$ sudo yum install ripgrep fd hunspell-en xclip lazygit -y
```

The application `fzf` is not available from repos. Install it directly from github itself.

```shell
$ git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf

$ ~/.fzf/install
```

All necessary applications are installed. Now, clone my nvim config repo.

```shell
$ git clone https://github.com/wxguy/nvim.git ~/.config/nvim && nvim
```

and follow onscreen instructions. You might get an error locating the dict file. Replace `/usr/share/hunspell/en_GB-large.dic` with `/usr/share/myspell/en_GB.dic` in `~/.config/nvim/lua/plugins/cmp-dictionary.lua` file. 

## Install and Configure Kitty

Install Kitty terminal using the following command.

```shell
$ curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin

$ ln -sf ~/.local/kitty.app/bin/kitty ~/.local/kitty.app/bin/kitten ~/.local/bin/

$ cp ~/.local/kitty.app/share/applications/kitty.desktop ~/.local/share/applications/

$ cp ~/.local/kitty.app/share/applications/kitty-open.desktop ~/.local/share/applications/

$ sed -i "s|Icon=kitty|Icon=$(readlink -f ~)/.local/kitty.app/share/icons/hicolor/256x256/apps/kitty.png|g" ~/.local/share/applications/kitty*.desktop

$ sed -i "s|Exec=kitty|Exec=$(readlink -f ~)/.local/kitty.app/bin/kitty|g" ~/.local/share/applications/kitty*.desktop

$ echo 'kitty.desktop' > ~/.config/xdg-terminals.list
```

You can change themes using the following. I use Dracula to match with the rest of my desktop theme.

```shell
$ kitty +kitten themes
```
## Sync System Clock With NTP

I had an issue with my Laptop clock. It always showed some other time despite setting the correct time zone. Solved this issue by following a few commands.

```shell
$ sudo dnf -y install ntp ntpdate
```

Add 'allow 192.168.1.0/24' in the following `/etc/chrony.conf` file.

```shell
$ sudo echo "allow 192.168.1.0/24" >> /etc/chrony.conf
```

Finally, start the ntpd demon.

```shell
$ sudo systemctl start ntpd
```

Once done, it automatically checks for NTP time and correct the system clock.

## Install MS Office

~~A separate blog post is on its way. Stay tuned....~~.  You can find the details on how to install MS Office on RHEL based distro at [https://wxguy.in/posts/install-ms-office-any-version-on-rhel-alma-linux-based-distros](https://wxguy.in/posts/install-ms-office-any-version-on-rhel-alma-linux-based-distros).

That's it for now. I may update the post if I find some other useful stuff.