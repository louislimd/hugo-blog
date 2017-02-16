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

I did a lot of research on many static site generators and decided to settle down on **Hugo**. It is written in golang and generates content fast. In addition, the [documentation](https://gohugo.io/overview/introduction/) and [support](https://discuss.gohugo.io/) is really awesome. If you want to build a blog, portfolio, documentaion or site with lots of pages, Hugo will be a nice choice for you. Now I'm going to show how I set up this website, deployed it automatically with **Github Pages** and **Wercker**, and created content with **Prose.io**.
<!--more-->
### Download and extract the binary
First, download the Hugo binary for your platform from Github. For example, I will download binary for 64 bit linux, since I am using Ubuntu 16.04. Next, extract it in a seperate folder in home directory. You can create this folder using the following command line:

	$ mkdir ~/dev

### Setting up an alias
Currently we can call Hugo from any folder by running the following command:

	$ ~/dev/hugo_0.17_linux_amd64/hugo_0.17_linux_amd64

As you can see the command above is not very user friendly. By creating an alias, we can make life much easier. Open up your `.bashrc` file:

	$ nano ~/ .bashrc

Add the following text at the end of this file:

	alias hugo='~/dev/hugo_0.17_linux_amd64/hugo_0.17_linux_amd64'

After saving the file, type the following command:

	$ source ~/ .bashrc

Now we can use any hugo command by simply typing `hugo`. Next, we are going to create a new site.

### Creating your website
Change your working directory into `Desktop` and create the new site by typing following commands in terminal:

	$ cd ~/Desktop
	$ hugo new site geekydoc
	$ cd geekydoc

The above command creates a new website in a folder called `geekydoc` and changes your directory into it. 

### Creating content
You can create a new `about.md` file in `content` folder by typing:

	$ hugo new about.md

Now you can edit the file by adding markdown after the `+++`. If you want to create a blogpost, you can type the following command in your terminal:

	$ hugo new post/first.md

The post will be created in the `content -> post` folder.

### Getting the themes
Themes provide the layout and templates that will be used by Hugo to render your website. There are a lot of [Open-source themes](https://themes.gohugo.io/) available. Themes should be added in the `themes` directory.

	$ cd themes

Now you can clone your favorite theme inside the `themes` directory. For example:

	$ git clone https://github.com/digitalcraftsman/hugo-icarus-theme.git

You can view it on `localhost:1313` by typing the following command:

	$ hugo server

### Building the blog
First of all, we need to edit the `config.toml` file. You can customize it to fit your site. For example, the `config.toml` file used by this blog was modified by the one shipped with icarus theme. See the [source code](https://github.com/louislimd/hugo-blog) of this blog to see what I did with my website. Next, type this command:

	$ hugo

This will generate your site inside `public` directory. To publish the website with Github Pages, you should create `User & Organization Pages` on Github first (use the `username.github.io` naming scheme).  If you git push all the content in the `public` folder into this repository, the website will be built and available at `http(s)://<username>.github.io`. However, don't do that right away.

### Automating the deployment process
To automatically deploy the generated site every time we add an article, set up an account for  [Wercker](http://www.wercker.com/) first. This can be simply done by signing in with your Github account. Next, create a `wercker.yml` file in the root of the site directory. For example:

	box: debian
	build:
	  steps:
	    - script:
	        name: install git
	        code: |
	          apt-get update && apt-get install git -y
	    - arjen/hugo-build@1.13.1:
	        version: "0.17"
	        theme: hugo-icarus-theme
	deploy:
	  steps:
	    - script:
	        name: install git
	        code: |
	          apt-get update && apt-get install git -y
	    - leipert/git-push@0.7.6:
	        gh_oauth: $GIT_TOKEN
	        basedir: public
	        clean_removed_files: true
	        branch: master
	        repo: louislimd/louislimd.github.io

After configuring Wercker, create another repository on Github and choose a name for your project. We will push the source code to this repository in the following steps.

### Setting up version control
Adding git to your project is done by running the following command from the _root directory_ of the project.

	$ git init

We don’t want the `public` directory version controlled however, as we will use Wercker to generate that later on. Empty the `public` directory and add a gitignore file into it:

	$ echo "/public" >> .gitignore

After this we can add everything to the repository. For example:

	$ git add .
	$ git commit -m "first commit"
	$ git remote add origin git@github.com:louislimd/hugo-blog.git
	$ git push -u origin master

### Setting up Wercker
	
