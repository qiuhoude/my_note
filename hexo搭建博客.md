
# Hexo + Github Page 搭建博客

### 在github上创建仓库
1. 创建一个名字为(必须和github名称一样) `username.github.io` 的Repository
2. 配置git 
	- `git config --global user.email "3617150@qq.com"` 
	- `git config --global user.name "qiuhoude"`
3. 生成密钥 `ssh-keygen -t rsa -C "3617150@qq.com"`(无脑回车就ok) 会在 ~/.ssh/目录下生成 ，id_rsa私钥 ，id_rsa.pub公钥
4. github-->settings--> SSH-Keys 添加key(将公钥 id_rsa.pub内填入key中)
5. 测试连接 `ssh -T git@github.com`
6. 添加远程仓库 `git remote add origin git@github.com:qiuhoude/username.github.io.git`


### 安装Hexo,初始化,启动服务
```sh
npm install -g hexo-cli #安装hexo到全局  
hexo init blog #初始化hexo(会下载)  
cd blog  
npm install #安装所依赖的包      
hexo server #本地启动
```
__notice__:  
_天朝的网络下载会报错,所以先设置代理  
全局设置代理：npm config set registry http://registry.npm.taobao.org _

### _config.yml 的配置

```yaml
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site 网站
title: 胖胖de码农 #网站标题
subtitle: #网站副标题 
description:  #给搜索引擎看的，对站点的描述，可以自定义
author: qiuhoude #在站点左下角可以看到
language: zh-CN
timezone: Asia/Shanghai 

# URL 网址
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://qiuhoude.github.io/blog #网址  
root: /blog/                  #网站根目录
permalink: :title/  #默认 :year/:month/:day/:title/
permalink_defaults:

# Directory 目录
source_dir: source #资源文件夹，这个文件夹用来存放内容。
public_dir: public #公共文件夹，这个文件夹用于存放生成的站点文件。
tag_dir: tags  #标签文件夹
archive_dir: archives #归档文件夹
category_dir: categories #分类文件夹
code_dir: downloads/code #Include code 文件夹
i18n_dir: :lang
skip_render:

# Writing 文章
new_post_name: :year-:month-:day-:title.md # File name of new posts,新文章的文件名称 默认
default_layout: post #预设布局 
titlecase: false # Transform title into titlecase 把标题转换为 title case
external_link: true # Open external links in new tab 
filename_case: 0     #把文件名称转换为 (1) 小写或 (2) 大写
render_drafts: false #显示草稿
post_asset_folder: true #启动 Asset 文件夹
relative_link: false  #把链接改为与根目录的相对位址
future: true          #显示未来的文章
highlight:            #代码块的设置
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

# Category & Tag
default_category: uncategorized #默认分类 
category_map: #分类别名 
tag_map:  #标签别名


# Server 不修改
## Hexo uses Connect as a server
## You can customize the logger format as defined in
## http://www.senchalabs.org/connect/logger.html
port: 4000
logger: false
logger_format:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 5  #每页显示的文章量 (0 = 关闭分页功能) default 10
pagination_dir: page #分页目录

# Extensions 这里配置站点所用主题和插件  
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: jacman #当前主题名称。值为false时禁用主题

# Deployment 
## Docs: https://hexo.io/docs/deployment.html
## 需要安装 npm install hexo-deployer-git --save
deploy: #部署部分的设置 
  type: git 
  # repository: https://github.com/qiuhoude/qiuhoude.github.io.git
  # repo:
  # 	github: https://github.com/qiuhoude/qiuhoude.github.io.git,master
  repo: https://github.com/qiuhoude/qiuhoude.github.io.git
  branch: master
  message: # commit message, default commit is Site updated {{ now('YYYY-MM-DD HH:mm:ss') }}
  name: qiuhoude
  email: 36117715@qq.com

# admin 管理插件
## 需要安装 npm install --save hexo-admin 
## 访问 http://yoursite/admin/
admin:
  username: houde
  password_hash: $2a$08$41njAq3E8lx5mVsfjqLYq.PXZEc5VTmVY/nlZ2ZTZ2/ipbEbtPqy6
  secret: a secret something
```

### hexo 写作
#### 创建新的文章
- `hexo new [layout] 'postName'`简写`hexo n`
	+ 其中layout是可选参数默认值为post,可在`__config.yml`配置`default_layout`
	+ layout的默认3种,到`scaffolds`目录下查看,些文件名称就是layout名称,可以添加自己的layout
		* post 对应成文章的路径 `source/_posts`
		* page  `source` 
		* draft(草稿) `source/_drafts`

####  layout的内容的说明

```yaml
#{{}}里面的是hexo变量
layout: {{ layout }} # hexo默认会处理全部markdown和html文件,若不处理设置false 
title: {{ title }} #文章页面上的显示名称,不会出现在URL中
date: {{ date }} #文章生成时间
categories: #文章分类目录，可以为空，注意:后面有个空格
tags:  #文章标签，可空，多标签请用格式[tag1,tag2,tag3]，注意:后面有个空格 
description: #  给搜索引擎看的，对本页的描述 参照__config.yml
photos: # 显示的图片 fancybox(需要配置主题)
-  #要显示的照片
```

#### 文章摘要
```
以上是摘要
<!--more-->
以下是余下全文
```
more以上内容即是文章摘要，在主页显示，more以下内容点击『> Read More』链接打开全文才显示。


### 生成静态文件并发布
- `hexo generate`简写`hexo g`  
	- 选项`-d,--deploy` 文件生成后立即部署网站
	- 选项`-w, --watch` 监视文件变动  
- `hexo deploy`简写`hexo d` 部署网站 
	- `-g, --generate` 部署之前预先生成静态文件
- 


### 本地启动
- `hexo server` 简写 `hexo s`    
	- `-p, --port`	重设端口
	- `-s, --static` 只使用静态文件 
	- `-l, --log` 启动日记记录，使用覆盖记录格式

### 其他命令	  

- `hexo clean` 清除缓存文件 (db.json) 和已生成的静态文件 (public)
- `hexo list` 列出网站资料
- `hexo version` 显示 Hexo 版本
- `hexo publish [layout] <title>` 可以将草稿移动到`source/_posts`文件夹下(和 `new`用法很类似)


### 主题
炫酷的个性博客就靠主题啦！hexo的主题列表[Hexo Themes](https://github.com/A-limon/pacman '很多主题')  

- 主题安装在 `themes`目录下

```sh
git clone <repository> themes/<theme-name>
```

- 修改`blog/_config.yml`中的`theme: <theme-name>`  
 
- 修改主题下的`_config.yml`

```yaml
menu: #配置页头显示哪些菜单
#  Home: /
  Archives: /archives
  Reading: /reading
  About: /about
#  Guestbook: /about

excerpt_link: Read More #摘要链接文字
archive_yearly: false #按年存档

widgets: #配置页脚显示哪些小挂件
  - category
#  - tag
  - tagcloud
  - recent_posts
#  - blogroll

blogrolls: #友情链接
  - bruce sha's Github: http://github.com/qiuhoude
  - bruce sha's oschina blog: http://qiuhoude.oschina.net/

fancybox: true #是否开启fancybox效果

duoshuo_shortname: qiuhoude #多说账号

google_analytics:
rss:
```

- 更新主题

```sh
cd themes/<theme-name>
git pull
```

### 评论
静态博客需要使用第三方评论系统,hexo默认集成的是[Disqus](http://disqus.com/),天朝还是用[多说](http://duoshuo.com/)  
注册多说  
将多说的通用代码贴的`hexo\themes\modernist\layout\_partial\comment.ejs`里面

```js
<% if (config.disqus_shortname && page.comments){ %>
<section id="comment">
  #你的通用代码
<% } %>
```

### 自定义页面

```sh
hexo new page "about"
```
在hexo/source/下会生成about目录，里面有个index.md，直接编辑就可以了，然后在主题的_config.yml中将其配置显示出来。
上述步骤，也可以手工生成，在hexo/source/下手工新建about和index.md也是完全等价的。  

> 因为markdown对table的支持不好，我是在about中直接建立index.html，里面书写页面内容，hexo会帮你加上头和尾。

### 404页面


### 连接
[Hexo中文网站](https://hexo.io/zh-cn/docs/)  
[Github Pages](https://pages.github.com/)  
[Hexo主题列表](https://github.com/hexojs/hexo/wiki/Themes)  
[多说](http://duoshuo.com/)

 
