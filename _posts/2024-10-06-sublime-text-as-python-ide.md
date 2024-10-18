---
title: How to Make Sublime Text as a Perfect Python IDE
author: wxguy
date: 2024-10-05 21:10:00 +0530
categories: [Tutorial]
tags: [python, ide, text-editor]
pin: false
toc: true
comments: true
render_with_liquid: true propo
img_path: /assets/img/sublime_text/
---

## IDE or Text Editor for Python Development?

I have been using Python for more than a decade now and love for the language is still growing. As Python is strict in their approach while writing syntax, even a extra space will result in error, leading to crashing of application. This is where the use of helper software comes in. These helper software are famously known as Integrated Development Environment (IDE) and Text Editor. Python has plenty of them in each category. Both the software provide different set of guidance to users to perform better coding. There are plenty of debate going around these software regarding should you use IDE or Text Editor? It all depends on what user has and want. It is no denying that IDE such as PyCharm offer much better development options and experience than any other Text Editor. However, to avail all the features offered by such an IDE, you must be working on larger project and have a better hardware resource. Otherwise, in most cases, decent Text Editor with suitable plugins would do the job.

## Why Choose Sublime Text

I have tried many of the text editors such as VS Code, Kate, Neovim and most recently, Zed. However, none comes close to Sublime Text. It is more elegant, small in size, have good sets of Plugins and most importantly, it is faster than any other editors out there. I have been using Sublime Text editor for many years, but have never paid attention to its plugin ecosystem in details. I did try a few plugins in recent past and result is truly amazing. Therefore, penning down here for my future use and for the benefits of others. Some of the plugins I am using are very specific to my need and hence you may need to tweak or skip those accordingly.

## Operating System Details

I am working on Arch Linux with KDE Desktop Environment. Details of my system is appended below.

`````console
neofetch
                   -`                    sundar@archlinux
                  .o+`                   ----------------
                 `ooo/                   OS: Arch Linux x86_64
                `+oooo:                  Host: TravelMate P214-53 V1.47
               `+oooooo:                 Kernel: 6.6.52-1-lts
               -+oooooo+:                Uptime: 17 mins
             `/:-:++oooo+:               Packages: 1706 (pacman)
            `/++++/+++++++:              Shell: zsh 5.9
           `/++++++++++++++:             Resolution: 1920x1080
          `/+++ooooooooooooo/`           DE: Plasma 6.1.5
         ./ooosssso++osssssso+`          WM: kwin
        .oossssso-````/ossssss+`         Theme: Lightly [Plasma], Breeze-Dark [GTK2], Breeze [GTK3]
       -osssssso.      :ssssssso.        Icons: lightly [Plasma], Dracula [GTK2/3]
      :osssssss/        osssso+++.       Terminal: kitty
     /ossssssss/        +ssssooo/-       CPU: 11th Gen Intel i7-1165G7 (8) @ 4.700GHz
   `/ossssso+/:-        -:/+osssso+-     GPU: Intel TigerLake-LP GT2 [Iris Xe Graphics]
  `+sso+:-`                 `.-/+oso:    Memory: 8118MiB / 15775MiB
 `++:.                           `-/+/
 .`                                 `/

`````

## Install Necessary System Packages

The details on how to install Sublime Text itself on any Linux Distro is listed in [Official Repo](https://www.sublimetext.com/docs/linux_repositories.html). If you wish to install it directly from Arch aur repo, you can use the following command:-

```console
paru -Sy sublime-text-4
```

Before we need to proceed, there are few other additional packages required to be installed on the host system. Following command will do that:-

```console
paru -Sy pyright ruff nodejs nodejs-cspell python-jedi
```

If you want beautiful font for editor, you can install _Victor Mono_ font from aur with following command:-

```console
paru -Sy ttf-victor-mono-nerd
```

## Install Sublime Text Plugins

### Auto Completion (Through Language Server Protocol [LSP])

The first and most important duty of IDE or Text Editor meant for software development is to provide better auto completion. Now a days, this feature is mostly provided by a technique called Language Server Protocol (LSP). More on this is listed at [https://microsoft.github.io/language-server-protocol](https://microsoft.github.io/language-server-protocol) and [https://langserver.org/](https://langserver.org). Details about LSP for Sublime Text is provided in the link at [https://lsp.sublimetext.io](https://lsp.sublimetext.io) and list of supported Language Server is listed at [https://lsp.sublimetext.io/language_servers](https://lsp.sublimetext.io/language_servers). To provide better auto completion, linting and formatting, we will install four packages such as `LSP`, `LSP-pyright`, `LSP-ruff` and `Jedi - Python Auto Completion` from Package Control. To do that open Command Palette either from `Tool` --> `Command Palette` or simply pressing `Ctrl` + `Shift` + `P` then type install packages. In the package control bar, type each of the package names and hit enter for install. Restart the editor to take effect. Now you can open any Python file with `.py` extension and start importing and code few lines. You will find how good the editor turned it to be.

![Auto Complete Suggestion by LSP Client](lsp-autocomplete.png){: .shadow }
_Auto Complete Suggestion by LSP Client on Python File_

> Installing `LSP-pyrignt` itself good enough to get auto completion in Sublime Text. However, bracket completion `()` for methods and functions such as simple `print` statement is not provided by pyright. To make it working, we have to install `Jedi` plugin as well. 
> {: .prompt-tip }

It is important to note that `Jedi - Python Auto Completion` from Package Control is an old plugin and almost no commit has been made in the last two years. Since LSP is the future I made an attempt to implement [`jedi-language-server`](https://github.com/pappasam/jedi-language-server) for Sublime Text as an independent plugin. The plugin is hosted at [https://github.com/wxguy/LSP-jedi](https://github.com/wxguy/LSP-jedi). It works perfectly fine on my system and found that it is much better than `Jedi` plugin. If you wish to install Jedi Language Server plugin, you can disable or uninstall `Jedi` plugin and install `LSP-jedi` using following command:-

```bash
git clone https://github.com/wxguy/LSP-jedi.git ~/.config/sublime-text/Packages/LSP-jedi
```

### AllAutoComplete

If you wish to get auto completion help from already opened buffers (files for dummies) then [All AutoCompletion](https://packagecontrol.io/packages/All%20Autocomplete) is the plugin that you find it to be useful. While it is very good in providing suggestions, sometimes I found it to be annoying as it provides too many suggestions and what you look for is buried under the long list of suggestions.

![Additional Auto Complete Suggestion by AllAutocomplete](allautocomplete.png){: .shadow }
_Additional Auto Complete Suggestion by AllAutocomplete_

### AutoDocstring

[AutoDocstring](https://packagecontrol.io/packages/AutoDocstring) plugin enable users to easily create a doc string by using key combination. After you install the plugin, go to class or function or method line (where `:` is written) and press key combinations `ctrl` + `alt` + `'`. This will automatically create the doc string template for you. You just have to move from one to another field by using tab.

![AutoDocstring in Action on a Dummy Function](autodocstring.png){: .shadow }
_AutoDocstring in Action on a Dummy Function_

### Conda Management

I use multiple conda environments for different projects. Every time you open the Sublime Text editor, if you are not in correct Environment, you will get lot of complaints about missing module etc. This is mainly due to non detection of Conda environment by Sublime Text. To manage this you have a `Conda` plugin [https://packagecontrol.io/packages/Conda](https://packagecontrol.io/packages/Conda). After installation, you need to edit the Conda configuration file from `Preference` --> `Package Settings` --> `Conda` --> `Settings`. In the new window, enter the following details (modify the path appropriately as per yours):-

```yaml
{
  "executable": "~/miniconda3/bin/python",
  "environment_directory": "~/miniconda3/envs",
}
```

Now onwards anytime you need to change Conda environment, you can type in Command Palette `Conda` and select what you need to do as shown below:-

![Conda Plugin in Action](conda_plugin.png){: .shadow }
_Conda Plugin in Action_

### SidebarEnhancement

The [SidebarEnhancement](https://packagecontrol.io/packages/SideBarEnhancements) provides side bar menu for Sublime Text. Once installed you can use `ctrl` + `KB` combination to reveal and retreat the side bar. The plugin is good but it can be made beautiful by installing another plugin called [A File Icon](https://packagecontrol.io/packages/A%20File%20Icon). You can see how beautiful the Side Bar now looks after instaling both the plugins:-

![SidebarEnhancement with A File Icon](sidebarenhancement.png){: .shadow }
_SidebarEnhancement with A File Icon_

### AutoFileName

Imaging you are working in a directory and need to insert the path of certain file. Normally, either you remember the path to file and type of use terminal to get the path and insert to the file. However, you can get the file path without leaving Sublime Text using [AutoFileName](https://packagecontrol.io/packages/AutoFileName) plugin. I have discovered this plugin very late but found it to be extremely useful.

### DictionaryAutoComplete

Apart from coding, you might be working on a project that require a lot of writing. In such case, automatic completion of workds from a dictionary would be a bless. [DictionaryAutoComplete](https://packagecontrol.io/packages/DictionaryAutoComplete) does exactly that. It is actually a VS code plugin ported to Sublime Text. You can see plugin action from official link [https://packagecontrol.io/packages/DictionaryAutoComplete](https://packagecontrol.io/packages/DictionaryAutoComplete) to find how useful it is.

### Asciidoctor and Asscidoc

[Asciidoctor](https://packagecontrol.io/packages/Asciidoctor) and [AsciiDoc](https://packagecontrol.io/packages/AsciiDoc) plugins are more personal in nature as I work on some of the project using AsciiDoctor. It provides better syntax highlighting.

### Colour Schemes

The default themes shipped with Sublime Text are good. However, I do have a habit of chaning themes based on my Desktop theme. Accordingly, I do keep on switching between Dracula, Adaptive, Bracket and Gravity. It is purely a personal choise and there are so many to choose from Package Control to suite your liking.

## Shortcut Keys for Quick Navigation

Like any other text editors and IDEs, Sublime Text also offer set of key bindings to quickly navigate within various UI of the editor. Most important key bindings are listed at [https://shortcuts.design/tools/toolspage-sublimetext/](https://shortcuts.design/tools/toolspage-sublimetext/) and [https://www.shortcutfoo.com/app/dojos/sublime-text-3-win/cheatsheet](https://www.shortcutfoo.com/app/dojos/sublime-text-3-win/cheatsheet). If you wish to remap key combination with any other action, you can do so from `Preference` --> `Key Bindings`. Here are the following one key bindings (`LSP Code Action` and `Super Tab`)I have enabled for quick movements:-

```yaml
[
  // to expand quick action of LSP code action
  { "keys": ["ctrl+shift+l"], "command": "lsp_code_actions" },

  // Setup super tab for auto completion. In the auto completion prompt, you can use 
  // tab and shift+tab to navigate up and down. Brought this habit from NeoVim editor.
  { "keys": ["tab"], "command": "move", "args": {"by": "lines", "forward": true}, "context": [
    {
      "key": "auto_complete_visible",
      "operand": true,
      "operator": "equal"
    }
  ] },
  { "keys": ["shift+tab"], "command": "move", "args": {"by": "lines", "forward": false} },
]
```
