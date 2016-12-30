---
layout: default
title: 使用GitHub搭建自己的博客
---

# 使用GitHub搭建自己的博客

以这个blog为例，说明如何使用GitHub搭建一个自己的blog网站。

## 注册GitHub账号

## 本地创建项目
1. 在本地创建一个目录，用以存放blog工程
```Shell
mkdir DManTalk
```
对该目录进行git初始化
```Shell
cd DManTalk
git init
```
根据github的规定，只有名字为gh-pages分支中的页面，才会生成网页文件。
所以创建一个没有父节点的分支gh-pages：
```Shell
git checkout --orphan gh-pages
```

2. 根据以下目录树建立模板文件：
```Shell
DManTalk/
├── _config.yml
├── _drafts
├── _includes
│   ├── disqus_comments.html
│   ├── footer.html
│   ├── head.html
│   └── navigation.html
├── index.html
├── _layouts
│   └── default.html
├── _posts
└── _site
```

其中，_config.yml是jekyll的设置文件，最简单的内容如下，具体解释以及其他选项可以参考[官方网页](http://jekyllrb.com/docs/configuration/)
```Shell
baseurl: /DManTalk
```

在_layouts/default.html中添加如下代码，作为Blog的默认模板：
```HTML
<!DOCTYPE html>
<html>
<head>
　　<meta http-equiv="content-type" content="text/html; charset=utf-8" />
　　<title>{{ page.title }}</title>
</head>
<body>

　　{{ content }}

</body>
</html>
```

Jekyll使用Liquid模板语言，{{ page.title }}表示文章标题，{{ content }}表示文章内容，详细信息请参考[官方文档](http://jekyllrb.com/docs/variables/)。

_posts目录就是用来存放所有日志文章的，文件名必须为"年-月-日-文章标题.后缀名"的格式。
每篇文章的头部，必须有一个[yaml文件头](http://jekyllrb.com/docs/frontmatter/)，用来设置这篇文章的元数据信息：用"---"标记开始和结束，中间写入一个元数据信息。
	1. "layout:default"，表示该文章的模板使用_layouts目录下的default.html文件；
	2. "title: 你好，世界"，表示该文章的标题是"你好，世界"。如果不设置这个值，默认使用嵌入文件名的标题，即"hello world"。

```language
---
layout: default
title: 使用GitHub搭建自己的blog
---
```

3. 创建一个首页。
在根目录中，index.html文件即为blog的首页，填入以下内容：
```HTML
---
layout: default
title: 我的Blog
---

<h2>{{ page.title }}</h2>
<p>最新文章</p>
<ul>
	{% for post in site.posts %}
　　<li>{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
　　{% endfor %}
</ul>
```
其中，{% for post in site.posts %}，表示对所有帖子进行遍历。Liquid模板语言规定，输出内容使用两层大括号，单纯的命令使用一层大括号。

4. 发布到github上
在github上新建一个仓库，名字为DManTalk，然后将本地的内容push到github仓库中

```Shell
git add .
git commit -m "init my blog"
git remote add origin https://github.com/DennisGao/DManTalk.git
git push origin gh-pages
```
通过浏览器访问 https://dennisgao.github.io/DManTalk/ 就可以看到首页了。



