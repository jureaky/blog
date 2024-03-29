---
title: "Create a Github Blog With Hugo"
date: 2020-04-30T21:15:39+09:00
draft: false
ShowToc: true
resources:
- name: "featured-image"
  src: "images/featured-img.png"
summary: Figure out How I created this blog using Hugo.
tags: ["Blog", "Hugo"]
categories: ["Env"]
---


## 1 What is Hugo?

Hugo is a **Static Site Generator**. Then, what is a static site generator? 

To understand this word, we first need to understand what static, or dynamic on the contrary, is when it comes to websites.

### Static Sites vs Dynamic Sites

Static sites just serve plain HTML files, which means exactly the same page is shown to every single one who accesses.

Otherwise, Dynamic sites work the opposite way. People can see different pages when those pages are called 'dynamic.' Typical example is that the username you see must be different with your friends' usernames after login. Dynamic sites communicate with databases.

We are so accustomed to dynamic sites that someone might say _"How could a website or web service work without databases? :roll_eyes:"_ 

However, it is not always the case that a website needs a database. Blog can actually fall into that case. The main purpose of blog is to show written articles(static contents) to everyone who clicks those articles. 

In this case, static sites can take advantage over dynamic sites in that static sties are much simpler, and faster.

---

## 2 Why Hugo?

There are many static site generators available. '**Hexo**' is one of popular static site generators. '**Jekyll**' seems to be very popular as it works naturally with 'Github Pages'.

However, there are several reasons why I chose to use Hugo as a static site generator:

* Hugo's build time is the fastest among those three.
* Hugo has a pretty detailed documentation as I think.
* Above all, I found a theme that caught my mind. [LoveIt](https://github.com/dillonzq/LoveIt) :heart:

---

## 3 Install Hugo

I use WSL(Windows Subsystem for Linux) Ubuntu 18.04 as my environment.

1. Go to [Hugo Github Releases page](https://github.com/gohugoio/hugo/releases)
2. Select a suitable version and download `hugo_extended_0.xx.x_Linux-64bit.deb`. For me, I downloaded v0.67.1 using `wget`. You can get the download link by right-clicking the package and clicking 'copy link address':

```bash
$ wget https://github.com/gohugoio/hugo/releases/download/v0.67.1/hugo_extended_0.67.1_Linux-64bit.deb
```

3. Extract the package by using following command(Replace version with what you downloaded).

```bash
$ sudo dpkg -i hugo_0.xx.x_Linux-64bit.deb
```

4. Verify the installation is successful.

```bash
$ hugo version
```

---

## 4 Create a Project

### Create a New Directory

Hugo project directory can be made using the command below. The directory has default structure.

```bash
$ hugo new site <project-name> # For me, I created with a name 'blog'
$ cd blog
$ ls -al
total 0
drwxrwxrwx 1 jureaky jureaky 4096 Apr 29 21:39 .
drwxrwxrwx 1 jureaky jureaky 4096 Apr 29 21:39 ..
drwxrwxrwx 1 jureaky jureaky 4096 Apr 29 21:39 archetypes
-rw-rw-rw- 1 jureaky jureaky   82 Apr 29 21:39 config.toml
drwxrwxrwx 1 jureaky jureaky 4096 Apr 29 21:39 content
drwxrwxrwx 1 jureaky jureaky 4096 Apr 29 21:39 data
drwxrwxrwx 1 jureaky jureaky 4096 Apr 29 21:39 layouts
drwxrwxrwx 1 jureaky jureaky 4096 Apr 29 21:39 static
drwxrwxrwx 1 jureaky jureaky 4096 Apr 29 21:39 themes
```

### Create a Git Repository

Two Git repositories are needed. One is for this project directory `blog`. The other is for storing generated static files so that the blog can be exposed by Github Pages. The pages will be stored in `public` directory and this directory will be added as a submodule for this entire project.

Go to Github and make two **empty** repositories(do not include README or any other).

1. `<project name repository>` - For me, `blog`
2. `<username>.github.io` repository - For me, `jureaky.github.io`

Make the project directory as a git repository, and add github repository as origin

```bash
$ git init
$ git remote add origin https://github.com/jureaky/blog.git
```

Add another github repository as a submodule in `public` directory.

```bash
$ git submodule add -b master https://github.com/jureaky/jureaky.github.io.git public
```

{{< admonition tip >}}
For WSL users, to prevent unexpected issues caused by different line ending between Linux and Windows, add below configuration to `.gitattributes` file in each git project root.
```
* text=auto eol=lf
*.{cmd,[cC][mM][dD]} text eol=crlf
*.{bat,[bB][aA][tT]} text eol=crlf
```
For more information, please see [here](https://code.visualstudio.com/docs/remote/troubleshooting#_resolving-git-line-ending-issues-in-containers-resulting-in-many-modified-files).
{{< /admonition >}}

### Install Theme

I found this theme, [LoveIt](https://github.com/dillonzq/LoveIt). It looks neat and simple, but provides lots of plugins and features.
The command below downloads the theme in `themes/LovIt` directory and add it as a submodule, which is a recommended way of installing theme because you can manage theme seperately so it is convenient if there is any update on your theme. 

```bash
$ git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

### Blog Configuration

Blog configuration is done through `config.toml` file in your root directory. Modify this file and add apropriate configurations. There might be a document for this in your theme page.
For LoveIt theme, menu configuration can be found [here](https://hugoloveit.com/theme-documentation-basics/#basic-configuration), and site configuration [here](https://hugoloveit.com/theme-documentation-basics/#site-configuration).
Further, I overrided some css variables to change fonts by creating `config/css/_override.scss` file:
```scss
@import url('https://fonts.googleapis.com/css2?family=Open+Sans&family=Zilla+Slab&display=swap');
$global-font-family: Open Sans, sans-serif;
$header-title-font-family: Zilla Slab, serif;
$header-title-font-size: 1.5rem;
```

---

## 5 Write a Post

Create a post with the command below:

```bash
$ hugo new posts/<post-name>.md
```

New `<post-name>.md` file will be created in `content/posts` directory.
If you look at the file, some front matters are already written. Note the line `draft: true`, which means the post is created as a draft by default. If you want to publish this post, you should change this value to `false`.

---

## 6 Execute Locally

Before publishing online, it's better to confirm everything is done as you expected.
The command below provides local server.

```bash
$ hugo serve -D
```

`-D` option is used when you want to publish draft also. You can check at
`http://localhost:1313`.
I can see this post I wrote has uploaded.

{{< figure src="images/local-screenshot.png" caption="Screenshot from `localhost`">}}

Wow !

---

## 7 Upload

Finally it's time to upload my first post ! :laughing:

Don't forget to change the value of  `draft` from `false` to `true` in the front matter.


```bash
# Execute from the project root directory.
$ hugo -t <theme-name> # For me, hugo -t LoveIt
```

You can find generated `HTML` files in `public` directory. Let's commit them.

```bash
$ cd public
$ git add .
$ git commit -m "Commit message"
$ git push origin master
```

Also, don't forget to commit from the project root directory.

```bash
$ cd .. # move to project root from public directory
$ git add .
$ git commit -m "Commit message"
$ git push origin master
```

---


## 8 Add a Comment Widget (Utterances)

LoveIt theme supports various comment widgets, but unfortunately Utterances is not naturally supported.

However, it's easy to attach Utterances.

You can find it out here: [https://utteranc.es/](https://utteranc.es/)

Utterances looks very neat and clean.

It provides Github like UI, which I think most users are accustomed to, and actually it uses Github Issuses to manage comments.

---

:point_right: **Thank you for reading my post !** :pray:

:point_right: Please leave a comment if you have any ideas to share ! :notes:
