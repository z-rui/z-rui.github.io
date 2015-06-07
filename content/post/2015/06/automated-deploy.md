---
date: 2015-06-07T01:29:57+08:00
title: Hugo的自动化部署
---

今天根据[Hugo网站上的说明][1]，把Hugo的构建和部署工作交给[Wercker]自动完成。

要把一个Hugo站点发布在[GitHub Pages]服务上，需要在GitHub创建两个repo：

1. [**源**repo][z-rui/hugo-site]，用于存放站点源文件。
2. [**目标**repo][z-rui/z-rui.github.io]，用于存放Hugo生成的静态站点。这个repo会被GitHub Pages作为www站点展示。

以前我是按照[另一篇说明][2]上的流程操作的，但是命令行的操作并不方便。具体的办法是：在本机运行hugo，生成静态站点。然后把源文件的改动推送到源repo里，再把生成的`public`目录用Git的submodule或者subtree功能推送到目标repo里边。

使用Wercker自动部署，则工作要轻松许多：当源文件的改动推送到源repo里边以后，Wercker会自动在它的服务器启动构建任务。当`master`分支成功构建后，就自动地把Hugo生成的静态站点推送到目标repo中。

所以目标repo的事情我就不用管了。如果想要改变部署目标（例如发布到其他的托管主机），只需要修改几条Wercker的设置，非常灵活。

使用自动化部署还给我的编辑流程提供了方便：我现在可以直接在GitHub的web界面上编辑内容。保存以后的更改也会自动地推送到GitHub Page上。另一方面，我可以充分地利用Git的分支功能：如果文章写到一半，可以保存到源repo的`draft`分支（或者`staging`分支，看看哪个名字好听而已）里面，而不是保存在本机。这允许我在多个位置进行持续地编辑，才算是有效地体现了GitHub的作用。

Wercker看起来不是很成熟。我读了它的文档，但是看起来十分地简略。最近它推出了一种新的构建模式，使用了[Docker]的技术。只不过我搞了半天也没有搞定在这种模式下怎么进行自动构建和部署。所以目前仍然运行在旧的模式中。

也有人用的是[Travis CI]，一个看起来更加成熟的持续集成工具，在GitHub的项目中非常流行。不过Wercker也有它的好处：用户可以创建自己的构建环境（称为box）和脚本（称为step），然后发布出去和其他人共享。我用来构建和部署Hugo的脚本，就是直接拿了别人的来用的，可以说是非常方便，不需要动什么脑筋。鉴于Wercker的解决方案已经足够方便，我现在就先不折腾了。

[1]: http://gohugo.io/tutorials/automated-deployments/
[2]: http://gohugo.io/tutorials/github-pages-blog/
[GitHub Pages]: https://pages.github.com/
[z-rui/hugo-site]: https://www.github.com/z-rui/hugo-site/
[z-rui/z-rui.github.io]: https://www.github.com/z-rui/z-rui.github.io/
[Wercker]: http://www.wercker.com/
[Docker]: https://www.docker.com/
[Travis CI]: https://travis-ci.org/

<!--more-->

