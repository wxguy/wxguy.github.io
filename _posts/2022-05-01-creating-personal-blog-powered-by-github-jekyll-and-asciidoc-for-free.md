---
title: Creating a personal blog powered by GitHub, Jekyll, and AsciiDoc for free
author: wxguy
date: 2022-04-30 20:55:00 +0530
categories: [Blogging, Tutorial]
tags: [jekyll,asciidoc,linux,github]
pin: false
toc: true
render_with_liquid: false
---

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

> I also used `Python` and `Bash` scripts to generate PDF files for each post and deployment. 

Install all necessary libraries and packages

```console
sudo paru -Sy ruby base-devel git
```

> The AsciiDoctor framework is not always updated with the phase of latest ruby release. However, Arch Linux always installs latest version of ruby. To avoid incompatibilities, between ruby and AsciiDoctor, it is recommanded to use Ruby Version Manager (rvm) from [https://rvm.io/](https://rvm.io/). Installation instructions also listed on the website. I have installed `ruby 3.0` on my machine.
{: .prompt-tip }

Install gem packages

```console
gem install exec jekyll bundler
```

Clone the [Chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy) repo. Official documentation of Chirpy recommends using `Chirpy Starter`. But it is easy to configure the existing repo, so I went for cloning the complete repository and rename it to your blog URL name.

```console
cd /path/to/your/location
git clone https://github.com/wxguy/jekyll-theme-chirpy.git
mv jekyll-theme-chirpy wxguy.github.io
```

Move into the newly created `wxguy.github.io` blog site directory and ensure that `Gemfile` exists.

```console
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

Start the server using the below `Jekyll` command.

```console
bundle exec jekyll serve --watch
```

If everything goes well, you can access the newly created website through the URL `http://127.0.0.1:4000/` on your favorite web browser. 

## Start Blogging

You need to edit certain files prior to writing a blog post. The documentation section of this theme is pretty good. It is recommended that you read [write new post](https://chirpy.cotes.page/posts/write-a-new-post/) and [text and typography](https://chirpy.cotes.page/posts/text-and-typography/) pages before writing your first blog post. You need to spend some time to ensure to make all the corrections properly. 
