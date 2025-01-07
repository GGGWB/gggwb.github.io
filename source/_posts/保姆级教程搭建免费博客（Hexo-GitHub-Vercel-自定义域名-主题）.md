---
title: 保姆级教程搭建免费博客（Hexo+GitHub+Vercel+自定义域名+主题）
date: 2023-11-21 22:44:54
tags: 个人博客
toc: true # 是否启用内容索引
---

# 0.前言
依照本文搭建博客：

* 高效简洁
* 无需服务器
* 自定义域名

前置条件：GitHub账号/一点点计算机知识/自己的域名（可选）

鉴于很多朋友没有接触过相关知识，我这里做一些简单的介绍，看见不熟悉的内容也不必困惑，简单了解就好。

* **Hexo**是一个高效的静态网站生成框架，它基于**Node.js**，特点是快速，简单且功能强大，是很多人搭建博客的首选框架。编写文章时，hexo运行在本地电脑中；

* **GitHub**用于存储编写好的博客内容以及展示；

* **Vervel**用于快速访问以及自定义域名；

部署好以后，使用方法是本地编写md文件（我是Mac，Win用户只需要替换hexo安装过程即可），推送到GitHub，部署到vercel，浏览器查看！教程分为几大部分：

* Hexo的安装和示例博客的本地查看；
* 编写博客
* Hexo更换漂亮主题；
* GitHub仓库的新建和本地pc的联动配置；
* Vercel部署和自定义域名；
* 常用的Hexo命令

我选了一个简单的主题，如下所示：

![截屏2023-11-20 23.09.19](https://dsm.gwbiao.eu.org:8089/i/202311/202311201700494132.png)

当然也可以有很多绚丽的主题：

![image-20231120233124619](https://dsm.gwbiao.eu.org:8089/i/202311/202311201700494286.png)

话不多说，开始教程。

# 1.Hexo的安装和使用

## 1.1 hexo安装需要node.js以及git环境
* node.js安装很简单，Mac下一条命令即可


  ```bash
  $ brew install node
  ```

* 其余系统的安装可以参考[菜鸟教程](https://www.runoob.com/nodejs/nodejs-install-setup.html)。

* 安装后进行检查，能正常显示版本号说明没有问题：


  ```bash
  $ node -v
  # v20.9.0  '#'开头的是注释或者代码输出结果  '$'开头的是要输入的命令
  $ npm -v
  # 10.2.4
  ```


## 1.2 git安装和配置可参考文章：[Mac-本地git配置](https://zhuanlan.zhihu.com/p/150703905)，Win，Linux平台同理

## 1.3 Mac安装hexo
* 全局安装需要输入密码


  ```bash
  $ sudo npm install -g hexo-cli
  ```

* 新建博客的根文件夹，并初始化


  ```bash
  $ mkdir <你的博客根文件夹> 
  # eg: mkdir myBlog
  $ cd <你的文件夹>
  # eg: cd myBlog
  # 后续命令都需要在这个文件夹中进行
  $ hexo init
  # 博客初始化
  $ npm install
  # 安装必备组件
  ```
* 安装完成后，项目结构如下：


  ```bash
  ├── _config.yml # 网站的配置文件 
  ├── package.json
  ├── scaffolds # 生成文章的模版文件夹
  ├── source  # 资源文件夹，除 _posts 文件，其他以下划线_开头的文件或者文件夹不会被编译打包到 public 文件夹
      |   ├── _drafts # 草稿文件
      |   └── _posts # 文章 Markdowm 文件 
  └── themes  # 主题文件夹
  ```

## 1.4 Hexo的简单使用
* 安装完成之后可以通过以下俩命令进行本地调试，server会给出本地链接


  ```bash
  $ hexo g
  $ hexo s
  # INFO  Validating config
  # INFO  Start processing
  # INFO  Hexo is running at http://localhost:4000/ . Press Ctrl+C to stop.
  ```

* 打开浏览器输入`http://localhost:4000/`，可以看到示例博客，在终端或者cmd中按`Ctrl  C`结束服务

![image-20231121163100246](https://dsm.gwbiao.eu.org:8089/i/202311/202311211700555465.png)


# 2.正式编写博客
* 使用以下命令新建一篇博客，md后缀名不用加


  ```bash
  $ hexo new post "your-first-blog"
  # $ hexo new [layout] <title>
  # 您可以在命令中指定文章的布局（layout），默认为 post，可以通过修改 _config.yml 中的 default_layout 参数来指定默认布局。
  # 创建新的博客以后，可以在文件夹中用markdown编辑器打开进行编写，例如typora，mweb，vscode等等
  ```

* hexo 有三种默认布局，post 、page 和 draft ，它们分别对应不同的路径。draft 是草稿的意思，如果你想写文章，但是又不想被看到，那么就可以


  ```bash
  # 这样会在 source/_draft 中新建一个 newdraft.md 文件
  $ hexo new draft newdraft
  
  # 如果想预览草稿文件
  $ hexo server --draft
  
  # 如果想将草稿文件发表到 post 中
  $ hexo publish draft newdraft
  ```


# 3.hexo更换主题

* hexo官网的[主题链接](https://hexo.io/themes/)，点击不同的链接可以进入主题的GitHub项目或者示例网站。
* 里面有很多花哨的主题，也有简洁风格的，更换主题的方式建议按照所选择主题的GitHub中示例教程，常规操作如下：
1. 下载主题包


  ```bash
  # 进入你的博客根文件夹
  $ git clone https://github.com/GitHub作者名/主题仓库名.git themes/主题仓库名
  # 一般这条命令在主题的GitHub中作者均有提供
  # 命令的意思是将这个主题下载到你的博客根文件夹下的themes文件夹中，这个themes文件夹专门存放各种主题
  ```

2. 编辑博客根文件夹下的'_config.yml'文件，找到themes修改'theme: 主题名'，

  ```bash
  $ vim _config.yml
  ```

3. 查看新主题

  ```bash
  $ hexo cl
  $ hexo g
  $ hexo s
  # 浏览器进入 http://localhost:4000
  ```

# 4.发布到GitHub

* 首先需要新建一个GitHub仓库，存储后续发布的文章，如图所示；

![image-20231121164018989](https://dsm.gwbiao.eu.org:8089/i/202311/202311211700556023.png)

* 要创建一个和你用户名相同的仓库，后面加.github.io，这样在部署到GitHub page的时候，才会被识别，

  也就是 https://xxxx.github.io，其中xxx就是你注册GitHub的用户名。例如我的仓库名：gggwb.github.io

* 这时候需要将Hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开博客根目录下的 _config.yml文件，这是博客的配置文件，在这里你可以修改与博客配置相关的各种信息。

* 修改根目录下的 _config.yml 文件


  ```yaml
  deploy:
  	type: git  # 使用 git 同步代码
  	repository: <your-git-rep-url>  # 你自己的 git 仓库地址
  	branch: master  # 默认采用 master 分支同步代码
  ```

![image-20231121164300209](https://dsm.gwbiao.eu.org:8089/i/202311/202311211700556184.png)


* 安装发布扩展包


  ```bash
  $ npm install hexo-deployer-git
  ```
* 发布代码


  ```bash
  $ hexo cl # 清除之前生成的静态文件
  $ hexo g # 生成静态文件
  $ hexo d # 发布代码到 git 远程仓库
  ```

# 5.部署到Vercel并自定义域名（可选）

* 这个时候对于没有域名的朋友已经可以用了，个人博客地址就是https://用户名.github.io
* 对于有自定义域名需求的朋友，可以继续使用Vercel部署和自定义域名，当然GitHub本身也可以自定义域名，在仓库中设置即可，这个很简单，不多赘述。vercel的话，建议用GitHub登陆vercel.com，先建立一个账号，然后以下是部署的详细的步骤：

1. 在你的本地电脑上，确保已经安装了Vercel CLI。如果没有安装，可以通过运行以下命令来安装：


  ```bash
  $ npm install -g vercel
  ```

2. 在Hexo项目根目录下，运行以下命令来生成静态文件：


  ```bash
  $ hexo generate
  ```

3. 在Hexo项目根目录下，运行以下命令来部署到Vercel：


  ```bash
  $ vercel
  # Vercel CLI 32.5.5   这几行是输出，可以先预览效果
  # 🔍  Inspect: https://vercel.com/gggwb/hexo-blog/Amcc3qKG256Y8Aq4QG9NPWDvZrpX [10s]
  # ✅  Preview: https://hexo-blog-dp5p5juvl-gggwb.vercel.app [10s]
  # 📝  To deploy to production (hexo-blog-pink-eight.vercel.app +1), run `vercel --prod`
  $ vercel --prod
  # 配置到生产
  ```

* 如果是第一次使用Vercel，你需要进行登录和授权操作。按照命令行提示进行相应的操作。

* 在部署过程中，Vercel会询问你一些配置选项，包括项目名称、部署方式等。根据你的需求进行设置。（没有其他需求可以一路回车）

* 部署完成后，Vercel会提供一个部署后的URL，你可以通过该URL访问你的博客，这个URL是vercel提供的一个超长的域名，接下来可以替换为自己的域名。

4. 在博客根目录下新建一个文件'vercel.json'，在'alias'处输入你自己的域名:


  ```bash
  $ vim vercel.json # 新建文件的命令
  # 输入以下内容：
  {
    "public": true,
    "routes": [
      {
        "src": "/(.*)",
        "dest": "public/$1"
      }
    ],
    "alias": "blog.gwbiao.eu.org"
  }
  ```

![image-20231121171101493](https://dsm.gwbiao.eu.org:8089/i/202311/202311211700557865.png)

5. 该域名需要在腾讯云或者阿里云免费进行dns解析，登陆腾讯云/搜索dns/点击云解析dns

   ![image-20231121172001797](https://dsm.gwbiao.eu.org:8089/i/202311/202311211700558405.png)


* 点击控制台

![image-20231121172112156](https://dsm.gwbiao.eu.org:8089/i/202311/202311211700558475.png)


   * 点击添加域名，新建一条cname如下

![image-20231121172225382](https://dsm.gwbiao.eu.org:8089/i/202311/202311211700558548.png)


   * 在vercel中，点击你的博客/Settings/Domains/Add这里添加自己的域名；点击add以后，会自动刷新看该域名是否被解析，也就是需要配置腾讯云的云解析DNS，配置内容也给出了，我这里是给个示例，没有在腾讯云解析，所以报了无效配置，按上面流程来的话，应该是正常配置的。

![image-20231121172716322](https://dsm.gwbiao.eu.org:8089/i/202311/202311211700558839.png)


* 注：vercel也可以在dashboard中直接导入GitHub仓库。

* 部署到Vercel和GitHub Pages有以下几个区别：

  1. 部署方式：Vercel是一个全球性的现代化部署平台，它支持多种项目类型的部署，包括静态网站、单页面应用、服务器端渲染应用等。它提供了更多的灵活性和功能，如自动构建、环境变量管理、预渲染等。而GitHub Pages主要用于托管静态网站，部署方式相对简单。
  2. 自定义域名：Vercel允许你使用自定义域名来访问你的部署项目，包括绑定你自己的域名、配置HTTPS等。而GitHub Pages也支持自定义域名，但需要进行一些额外的配置，如添加CNAME文件和进行DNS解析。
  3. 构建和部署速度：Vercel在构建和部署方面通常更快，特别是对于大型项目或需要进行构建的项目。GitHub Pages的构建和部署可能需要更长的时间，特别是在首次部署时。
  4. 集成与扩展性：Vercel提供了与其他服务（如GitHub、GitLab、Bitbucket等）的集成，可以方便地与代码仓库进行同步和自动部署。它还提供了更多的扩展性，如Webhooks、Serverless Functions等。而GitHub Pages则更紧密地与GitHub仓库集成，可以直接从GitHub仓库进行部署。

  总体而言，Vercel提供了更多的功能和灵活性，适用于更复杂的项目和需求。而GitHub Pages则更适合简单的静态网站部署，特别是对于使用GitHub作为代码仓库的用户来说。选择哪种部署方式取决于你的项目需求和个人偏好。



# 6.一些Hexo的常用命令

## 常用命令

| 命令                        | 说明                                                         |
| :-------------------------- | :----------------------------------------------------------- |
| hexo -v                     | 查看 hexo 版本号                                             |
| hexo cl 等同于 hexo clean   | 清除生成的静态文件                                           |
| hexo g 等同于 hexo generate | 生成静态页面至 public 页面                                   |
| hexo s 等同于 hexo server   | 生成本地预览                                                 |
| hexo d 等同于 hexo deploy   | 部署（需要安装 npm install hexo-deployer-git --save 插件包） |



参考文章：

[Hexo+Github+Netlify博客搭建教程（原创 Leo 程序员Leo 2023-10-14 01:13 发表于江苏）](https://mp.weixin.qq.com/s/Tp1vwolTUYe_Sh03NzPZug)

[使用 Hexo 写个人博客（原创 Alex南山竹  蒲东平  2023-11-21 00:54 发表于上海）](https://mp.weixin.qq.com/s/g8C-jQMgiV_p2O7YOchzxg)



