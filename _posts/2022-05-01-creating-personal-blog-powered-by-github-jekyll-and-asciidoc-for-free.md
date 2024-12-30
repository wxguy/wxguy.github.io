---
title: Creating a personal blog powered by GitHub, Jekyll, and AsciiDoc for free
author: wxguy
date: 2022-05-01 20:55:00 +0530
categories: [Blogging, Tutorial]
tags: [jekyll,asciidoc,linux,github]
pin: false
toc: true
render_with_liquid: true
media_subpath: /assets/img/jekyll_blog
image:
  path: jekyll.webp
  lqip: data:image/webp;base64,UklGRkIAAABXRUJQVlA4IDYAAADQAgCdASoUAAsAPzmIulQvKSWjMAgB4CcJQBibBaSAAP7qeKYlfBktR/rTDkiZNat5iJtkYAA=
  alt: Landing Page of Jekyll, a Plain Text to Static Website Generator. (Credit:- https://jekyllrb.com)
---

-------

## Motivation

I do keep fiddling with my system to explore new applications and technologies. There are many times, I have to install and reinstall applications and execute the same commands many times. To remember the workflow and commands I used, I generally keep noting down all information in a text file. I also noticed that I do visit various blogs again and again to read the same articles. These articles need not be the same workflow you are using on your system. Therefore, I have decided to create a blog and write it down on it.

## Options for Blogging

There are many blogging platforms are available to consider for personal blogging which include `WordPress`, `Weebly`, `medium`, `blogger`, `Tumblr` etc. I will not go into technical details about each platform. All blogging platforms have their own merits and demerits. However, I have decided to blog on a static site platform so that speed is guaranteed and I need not worry about security issues. Moreover, most of these static sites are generally hosted on code-sharing platforms such as `www.github.com`, `www.gitlab.com` etc. That also solves my issue related to server maintenance and the money required to host it. While talking about static site generators for blogging, there are only quite a few technologies available to choose from. Most common are `Hugo`  and `Jekyll`. After going through their official documentation, available plugins, theme selections, customizations, and support from various blog posts and forums I have decided to choose `Jekyll` for my blogging purpose.

## My Requirements

The starting point for creating my blog was to identify a suitable theme from many themes available on websites listed in [Jekyll Website](https://jekyllrb.com/docs/themes/). Before finalising the theme, I have set myself that it should have the following functionalities and options:-

 - Tags for each post
 - Option to specify each post under a certain category
 - Archive section to choose based on year and month
 - Option to enable Dark and light theme
 - Clean, minimal, and good looking
 - Minimal tweaking
 - Ability to write a post using `AsciiDoc`
 - Provide users to download article in pdf at the end of each page
 - Ability to comment on each post (optional)
 - Search within the website (optional)

## What I found

To be honest, it took some time to finalise the theme. Because most of the themes available for Jekyll are awesome. Also, most of my wishes were also present in famous themes. Since I want a bare minimum blog, I decided to go for this [Pixyll theme](https://github.com/johno/pixyll). `Pixyll` theme looks fantastic but a few important features such as tags and archives were not available in this theme. Later I found this blogpost https://zpbappi.com/jekyll-with-tags-archive-and-comments-in-github-pages/ describing how these options can be enabled manually. So I started writing Python and bash scripts to implement certain additional features in my own way. However, while I was searching for `AsciiDoc` and `Jekyll`, I realised that I have to do much more work on `Pixyll` theme to get what I wanted.

That is the time I found this [Chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy). It has all the features I wanted in a pre-configured way. The final outcome of my blog setup is the one you are reading now. In the rest of the article, I will write about technical stuff that is required to be followed to make this blog live.

## Prerequisite

I have installed all the applications and packages on my [Arch Linux](https://archlinux.org/). The package manager I use is [paru](https://github.com/Morganamilo/paru) as it has the ability to install packages from `aur` repositories. You can also use `pacman` instead. If you are using any other Distribution such as Ubuntu, Open Suse, Debian, etc, you have to use google to find equal command-line options.


Install all necessary libraries and packages

```console
sudo paru -Sy ruby base-devel git
```

> The AsciiDoctor framework is not always updated with the phase of the latest ruby release. However, Arch Linux always installs the latest version of ruby. To avoid incompatibilities, between ruby and AsciiDoctor, it is recommended to use Ruby Version Manager (rvm) from [https://rvm.io/](https://rvm.io/). Installation instructions are also listed on the website. I have installed `ruby 3.0` on my machine.
{: .prompt-tip }

Install gem packages

```console
gem install exec jekyll bundler
```

Clone the [Chirpy theme started](https://github.com/cotes2020/chirpy-starter/generate) repo by clicking on it. Name the repo as `username.github.io` and in my case, I entered `wxguy.github.io`  Once I created the remote repo, I cloned it to the local machine for further modification.

```console
cd /path/to/your/location
git clone https://github.com/wxguy/wxguy.github.io.git
```

Move into the newly created `wxguy.github.io` blog site directory and ensure that `Gemfile` exists.

```console
cd wxguy.github.io
ls Gemfile
```

In addition to markdown, I want Jekyll to render `AsciiDoc` files for blog posts. This is possible only through installing Jekyll plugins. Therefore, I edited the `Gemfile` by appending the following lines at the end.

```console
group :jekyll_plugins do
gem "jekyll-asciidoc"
gem "jekyll-paginate"
gem "jekyll-redirect-from"
gem "jekyll-seo-tag"
gem "jekyll-archives"
gem "jekyll-sitemap"
end
```

Install all plugins required for using the Chirpy theme. It may take a few min, depending on your network speed.

```console
bundle install
```

> Before adding content and start your server, you may consider executing `git config --global --add safe.directory /path/to//wxguy.github.io`  command. This will give you write permission to all files and edit files from dirrent OS.
{: .prompt-tip}

Start the server using the below `Jekyll` command.

```console
bundle exec jekyll serve --watch
```

If everything goes well, you can access the newly created website through the URL `http://127.0.0.1:4000/` on your favorite web browser. However, at this stage, it will show any post as we have not written any post inside the `_posts` directory.

## Modify Site Settings

Before you start your blog post, you need to amend some variables in `_config.yml` file. The comments provided in the file are self-explanatory and modified as per your requirement. I have modified the following variables:-

```console
title:
tagline:
description:
timezone:
url:
github: username:
social :name:
social :email:
social :links :github:
google_analytics :id:
avatar:
paginate:
```

The variable `avatar` is required to display your profile image. You need to provide a path to your profile image for this. I created a directory called `img` under `assets` and placed my profile picture called `avatar.png`. Reloading the site showed my profile image in the sidebar.

If you wish to display the favicon image to be displayed in the current tab, then read this article [https://chirpy.cotes.page/posts/customize-the-favicon/](https://chirpy.cotes.page/posts/customize-the-favicon/).
 

## Change Site Font

I like the way content is presented in [https://beautifuljekyll.com/](https://beautifuljekyll.com/) blog. It uses `Lora` and `Times New Roman` to display the content. To implement the same in my blog, I created a `style.css` file under the `assets/css` directory and added the below content.

```css
---
---

/*
  If the number of TAB files has changed, the following variable is required.
  And it must be defined before `@import`.
*/
$tab-count: {{ site.tabs | size | plus: 1 }}; // plus 1 for home tab

@import "{{ site.theme }}";

/* append your custom style below */

html {
  font-size: 92%
}

body {
  font-family: 'Lora', 'Times New Roman', serif;
  font-size: 130%
}

h1 {
  font-family: 'Lora', 'Times New Roman', serif;
  font-size: 1.9rem;
}

h2 {
  font-family: 'Lora', 'Times New Roman', serif;
  font-size: 1.5rem;
}

h3 {
  font-family: 'Lora', 'Times New Roman', serif;
  font-size: 1.2rem;
}

h4 {
  font-family: 'Lora', 'Times New Roman', serif;
  font-size: 1.15rem;
}

h5 {
  font-family: 'Lora', 'Times New Roman', serif;
  font-size: 1.1rem;
}
```

This ensured that text is changed on the main body and headings to make it uniform.

## Start Blogging

The documentation section of this theme is pretty good. It is recommended that you read [write new post](https://chirpy.cotes.page/posts/write-a-new-post/) and [text and typography](https://chirpy.cotes.page/posts/text-and-typography/) pages before writing your first blog post. You need to spend some time to ensure to make all the corrections properly. 


> Please view your post changes in Google Chrome or Chromium. On my system Firefox did not display the site properly. It is also recommanded to review the generated site on *`Private`* mode to ensure that cookies and cache files are not creating an issue.
{: .prompt-warning }



------
You can download this article from [here](https://wxguy.github.io/assets/downloads/pdfs/2022-05-01-creating-personal-blog-powered-by-github-jekyll-and-asciidoc-for-free.pdf) for free.

------
