---
date: 2015-06-02T04:09:00+08:00
title: "人生中第一份Pull Request"
---

今天做了几件杂活：

1. 更新Highlight.js到8.6。手动更新似乎是很无聊的工作，如果像MathJax那样提供一个latest链接的话就不需要更新了。
2. 更新Hugo到0.14。这个版本的Hugo deprecate了`.BaseUrl`，并且和我正在使用的主题不能兼容。

本站使用的主题Fork自[chibicode/hugo-theme-shiori](https://github.com/chibicode/hugo-theme-shiori/pulse)。因为更新了Hugo之后不能正常工作，所以我开始自己修改这个主题。其实之前我就改正过Archive页面中的一个逻辑错误，但是我是放在本地的override中的，而没有修改主题本身。这次需要大量的改动，所以干脆修改主题。

转念一想，该主题既然托管在GitHub上，我可以把它做成Pull Request。我以前从来没有做过Pull Request，今天算是实践了一次。

首先在GitHub中点击Fork按钮，几秒钟后，一个本地仓库(repository)就建好了。

接下来要做的就是在本机克隆仓库，设置好远程仓库 *upstream* ，然后创建一个新的分支。

```sh
$ git clone https://github.com/z-rui/hugo-theme-shiori.git
$ git remote add upstream https://github.com/chibicode/hugo-theme-shiori.git
$ git checkout -b patch
```
*upstream* 仓库是被fork的仓库，如果原作者修改了代码，那么我只需要pull upstream就可以及时跟上进度。

然后就是修改其中的内容。修改好了以后提交(commit)，然后推送(push)到自己的仓库中。

```sh
$ git push
```

推送完毕后，在我的GitHub页面上就可以看到生成Pull Request的提示。于是我提交了[人生中的第一份Pull Request](https://github.com/chibicode/hugo-theme-shiori/pull/5)。

我在操作过程中学到了一些知识：

1. 不要在master分支中修改代码。如果这样，则GitHub界面上就没有非常显眼的生成Pull Request的提示。尽管仍然可以生成Pull Request，在master分支中修改代码的主要问题是会带来冲突(conflict)。
2. 每次push的时候git都会请求我输入用户名和密码，在反复push的时候显得很恼人。可以打开git的缓存功能，在全局设置中设置`credential.helper=cache`，就可以只输入一次用户名和密码。

**更新:** git缓存用户名和密码的功能在Windows上并不好用，因为它依赖UNIX文件系统权限。使用[git-credential-winstore](http://gitcredentialstore.codeplex.com/)项目可以让Windows凭据管理器保管git用户名和密码。下载`git-credential-winstore.exe`并复制到合适的目录中，在git的全局设置中设置`credential.helper=winstore`即可。

<!--more-->
