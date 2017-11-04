---
layout:     post
title:      "如何在CentOS下使用Jekyll编译博客，并上传到仓库"
subtitle:   ""
date:       2015-06-07 12:00:00
author:     "Deqiang Qin"
header-img: "img/post-jekyll.jpg"
---


>   使用Github+Jekyll搭建个人博客的时候，每一次博客的修改都是需要重新编译整个项目的，编译完后再上传到Github的仓库中。具体流程如下：
<p>
    
</p>

#### 1、搭建Jekyll的编译环境
建议在CentOS下搭建一套Jekyll的环境，我在一篇文章中专门的介绍了如何基于CentOS7搭建Jekyll的，总之，很简单啦，如Ruby等等。同样可以参考另外一篇文章：
http://blog.csdn.net/u014015972/article/details/50497254 
<br>

#### 2、拉取仓库
`git clone https://github.com/qindeqiang/bigdataresource.git`
<p>使用git clone 拉取到本地</p>

#### 3、上传Jekyll模板
网上一搜一大堆的Jekyll模板,也可以去clone现有的博主的模板，感觉好用拿过来用就是了，不要谦虚。一个好的模板，大家用多了，开发者才会注意到，才会去升级，不断的更新。
<br>当然如果，你自己很牛逼，可以完全自己去研究Jekyll，然后做一套模板供大家使用。不过，我只是用户，没必要去研究这么多东东。
<br>把找到的模板上传到仓库的本地目录下。

#### 4、启动Jekyll服务
在仓库目录下，启动Jekyll服务。启动命令：`jekyll server` <br>
Jekyll会自动的对目录下的全部文件进行编译。以后，无论是任何修改，如果要想使修改生效，都必须重启Jekyll服务，重新编译。

#### 5、Git提交
使用git实现对仓库的提交，每一次修改后都要提交到远程仓库后，才可以生效。提交命令如下：
```
git add -A
git commit -m "Update Jekyll"
git push 
```
<br>
<br>
