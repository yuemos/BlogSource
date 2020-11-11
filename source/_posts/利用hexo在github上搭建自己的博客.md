---
title: 【小白都能看懂的安装教程】利用hexo在github上搭建自己的博客
date: 2020-11-10 15:48:54
tags:
---

从高中起养成的写博客的习惯到大学之后就渐渐扔掉了，倒不是说没有什么值得记录的东西了，而是生活中的诱惑多了起来：美食、游戏、旅游……一晃就是三年过去，现在我已经大四，作为一名六年的老程序猿还没有一个自己的博客，真的是天大的罪过。

之前高中的时候用过博客园和csdn来记录我的OI生涯，但是还是搭建一个自己的博客会令人更有成就感。买域名的价格还好说，租服务器的价格就是我这种咸鱼屌丝承担不起的费用了，感谢github——世界上最大的同性交友基地可以给我充当一个小型的服务器。

__使用github搭建个人博客的优点__

1. 访问速度快，仓库里只需要放打包好的静态页面

2. 构建快，代码push到远程仓库后只需要过几分钟就能访问

3. 免费（最主要的原因啊有木有）
4. 易于管理（github的分支和版本回退等）
5. 可以绑定自己的域名（毕竟域名便宜而服务器不便宜）

常用的博客生成和管理工具有两种，分别是hexo和jekyll。当我还租得起服务器的时候搭建博客用的是jekyll，在接触hexo后我深感jekyll的语法和配置相对复杂。利用hexo搭建博客只需要node环境就可以了（直接下载安装即可），下面我们来进入搭建的步骤。

### 需要用到的工具/软件

+ Github
+ Git
+ hexo
+ node.js

### 预先的准备工作

+ 注册（或回想起）一个Github账号，注册地址：[Github](https://github.com/)

+ 安装git，可以参考教程如下:[git](https://www.liaoxuefeng.com/wiki/896043488029600)

+ 安装node，全称默认安装即可，下载地址：[node](https://git-scm.com/)

+ 【可选】花钱购买一个自己的域名

  好了，前期的准备工作就到这里了，下面我们开始进入安装配置阶段。

### 安装hexo

git的安装教程可以参考廖雪峰的官方网站（见上方），node下载安装包后直接安装即可，安装完后在命令行界面输入node -v如果出现版本号则安装成功。

确认git和node安装成功后我们就进入到下一步了。

1. 首先我们需要在自己喜欢的地方新建文件夹，命名为blog（文件名任意），然后进入该文件夹。

2. 在blog文件夹中点击鼠标右键，然后选择“Git Bush Here"，到这一步前最后已经在本地Git上绑定了github账号，如果没有还是参考廖雪峰的官方网站。

3. 输入`npm install hexo -g` 安装hexo，其中-g的效果为全局安装hexo。安装完成如下

   ![avatar](D:\document\blog\source\photo\hexo_install.png)

4. 验证安装成功`hexo -v`

   ![avatar](D:\document\blog\source\photo\hexo-v.png)

5. 执行`npm init`初始化blog文件夹，效果如下

   初始化成功后查看blog文件夹：

   ![avatar](D:\document\blog\source\photo\document.png)

6. 打开source\\_posts可以看到文件夹下有一个名为`hello-world.md`的文件，这就是初始化blog后生成的样例博客。

7. 接下来执行`hexo gerenate`或`hexo g`，这条指令的作用是将我们的初始化博客打包成静态页面，这样可以加快打开速度。

8. 执行`hexo server`或`hexo s`启动本地的服务器（仅在本步使用，用于确定能否正常生成博客，后续会将博客挂到github上），之后打开浏览器输入`http://localhost:4000`可以看到hexo已经为你生成了一篇博客。（ps：如果界面进不去，可能4000端口被占用，按Ctrl+c结束服务器进程，执行以下指令更改端口号`hexo server -p 5000(5000为自己指定的端口号)`，然后再次访问时网址后的4000记得换成自己指定的端口号）

9. 之后在我们想要写博客的时候，只要打开Git Bush界面，输入`hexo new "文件名"`就可以新建一个名为“文件名”的md文件，可以在source\\_posts中利用记事本打开编辑（不会使用MarkDown的小伙伴需要自己再学习一下了）。然后我们输入`http://localhost:4000`就可以看到已经生成了一篇新文章“文件名”（我是多脑残才会用这个名字做实例QAQ）

   ![avatar](D:\document\blog\source\photo\hexo.png)

至此，我们已经在本地安装好hexo并且搭建好我们博客的基础框架了。

### 链接远程库

#### 第一步 将本地的库与github联系起来

1. 进入blog文件夹，右击，“Git Bush Here”，执行`cd ~/.ssh`用于检查.ssh文件是否存在

2. 如果不存在，执行`ssh -keyben -t -rsa -C "github邮箱"`

3. 进入文件夹C:\user\Administrator\\.shh会看到两个文件：id_rsa和id_rsa.pub

4. 在git Bush界面执行`eval "$(ssh-agent -s)"`添加秘钥到ssh-agent中

5. 执行`ssh -add ~/.ssh/id_rsa`添加生成的ssh key到ssh-agent中

6. 利用记事本本打开id_rsa.pub，全选复制文件中的内容

7. 登录github账号，点击头像选择->Settings->SSH and CPG keys，选择右上角的“New SSH key”，将复制的内容粘贴到key中。点击"Add SSH key"按钮

8. 执行`ssh -T git@github.com`,如果看到以下内容则说明添加ssh成功

   ![avatar](D:\document\blog\source\photo\ssh.png)

#### 第二步 配置本地文件

1. github新建repository，注意repository name只能填`用户名.github,io`，同时要确保仓库中至少要有一个文件才能访问

2. 复制仓库的ssh链接

   ![avatar](D:\document\blog\source\photo\copy_ssh.png)

3. 利用记事本打开blog文件夹下的_config.yml文件，翻到最下方，将内容修改为

   `deploy ` 
   	`type: git`
   	`repository: 复制的仓库的SSH链接`
   	`branch: master`

#### 将本地文件推到github

1. 将本地的博客推送到github我们需要先在github上新建一个仓库，点击右上角"+"号，选择New repository（同上一步第一条）

2. 这里需要注意两个问题，一个是repository name只能命名为`用户名.github,io`，另外一个是确认一下下边的主分支是否命名为master

   ![avatar](D:\document\blog\source\photo\repository.png)

3. 将本地的博客推送到github上，我们需要使用到hexo下的deploy，这里我们要先安装，在Git Bush Here窗口中输入

   `npm install hexo-deployer-git --save`
   
   现在我们可以向github推送我们的博客了。

4. 为了方便访问，我们会将静态网页推送到网站上，首先我们要将md文件生成静态网页，然后再部署到github上

   `hexo g //g gerenate 生成 将md文件生成静态网页 `  
   `hexo d //d deploy 部署 将静态网页部署到github`  
   当然以上两条指令也可以简写成下边两句之一  

   `hexo d -g `  

   `hexo g -d`    

   每次修改完本地文件或新建博客后都要执行这两句，也可以在这两句前再加上`hexo clean`先清除本地缓存再生成部署。

5. 现在，我们就可以通过`用户名.github.io`来访问我们的个人博客了！

### 解析域名

到上一步为止，我们的个人博客已经可以正常运行，但是通过`用户名.github.io`访问总是让人觉得很麻烦，远不如使用自己的域名来的方便。

1. 首先你需要准备一个域名

2. 如果想要使用自己的域名，我们需要将自己的域名解析的到github的服务器，我们可以通过打开命令行窗口，使用`ping 用户名.github.io`来获取github服务器的IPV4地址

   ![avatar](D:\document\blog\source\photo\IPV4.png)

3. 接下来我们需要进入我们购买的域名的运营商提供的域名控制台，将我们的域名解析到我们获得的IPV4地址上，如下图所示

   ![avatar](D:\document\blog\source\photo\yuming1.png)

   ![avatar](D:\document\blog\source\photo\yuming2.png)

4. 之后我们需要在我们的本地blog文件夹下新建一个名为CNAME的文件（没有后缀，通过记事本打开），在其中输入我们自己购买的域名，然后`hexo d -g`重新部署到github上

5. 现在我们就可以通过自己的域名来访问自己的个人博客了！

### 记录搭建个人博客的过程中踩的坑

1. 如果在将本地文件推到github的过程中出现问题，可以检查一下blog文件夹里的_config.yml最后更改的几行，这里的冒号后边必须有一个空格。
3. 写博客一定要记得随手保存！随手保存！一个网络波动我写了一个小时的博客就要重新写一遍，心态炸裂。

