+++
images = [
]
date = "2017-01-19T07:11:59+08:00"
title = "Creating new site using hugo"
disable_comments = true
tags = ["blog"
]
categories = ["coding"
]
menu = ""
description = ""
banner = ""

+++

Since static-generated website eliminates a lot of performance and security concerns, I build this blog with a static site generator. By doing so, I can focus on creating content of the blog without the need to deal with databases and all kinds of hosting platforms such as AWS and Azure.

I did a lot of research on many static site generators and decided to settle down on Hugo. It is written in golang and generates content fast. In addition, the documentation and support is really awesome. If you want to build a blog, portfolio, documentaion or site with lots of pages, Hugo will be a nice choice for you. Now I'm going to show how I set up this website, deployed it automatically with Github pages and wercker, and created content with Prose.io.
<!--more-->
### Download and extract the binary
First, download the Hugo binary for your platform from Github. For example, I will download binary for 64 bit linux, since I am using Ubuntu 16.04. Next, extract it in a seperate folder in home directory. You can create this folder using the following command line:

	$ mkdir ~/dev
