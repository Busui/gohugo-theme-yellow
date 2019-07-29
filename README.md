[demo](https://busui.github.io)

## 一、安装Yellow


> 安装本主题之前需要你确保你自己已经知道并且会基础使用Hugo。建议先仔细阅读[Hugo getting-started](https://gohugo.io/getting-started/installing/)


在Hugo项目的根目录下，将yellow仓库克隆到themes目录下：
```
git clone git@github.com:Busui/gohugo-theme-yellow.git themes/yellow
```

此刻你的themes目录下应该有一个名为yellow的文件夹：

```
[Busui@Busui HugoBlog]$ ls  ./themes/
yellow
```

## 二、Hugo根目录config.toml配置

在yellow主题下会有一个同名的`config.toml`文件，为了简单，先**不要管它**！将下面的内容复制到你**Hugo项目根目录**下的`config.toml`文件里，然后修改一些参数就可以了。

```
baseURL = "http://example.com"
languageCode = "zh-cn"
defaultContentLanguage = "zh-cn"
title = ""                           # site title  # 网站标题
theme = "yellow"
hasCJKLanguage = true                # has chinese/japanese/korean ? # 自动检测是否包含 中文\日文\韩文
summaryLength = 100
paginate = 5                         # shows the number of articles  # 首页显示文章数量
enableEmoji = true
googleAnalytics =                    # your google analytics id
disqusShortname = ""                 # your discuss shortname

[author]                             # essential                     # 必需
  name = "Busui"

[blackfriday]
  smartypants = true

[[menu.main]]                        # config your menu  # 配置菜单
  name = "首页"
  weight = 10
  identifier = "home"
  url = "/"
  post = "#icon-ziyuan2"
[[menu.main]]
  name = "分类"
  weight = 20
  identifier = "categories"
  url = "/categories/"
  post = "#icon-ziyuan"
[[menu.main]]
  name = "标签"
  weight = 30
  identifier = "tags"
  url = "/tags/"
  post = "#icon-ziyuan4"
[[menu.main]]
  name = "归档"
  weight = 40
  identifier = "archive"
  url = "/archive/"
  post = "#icon-ziyuan5"
[[menu.main]]
  name = "关于"
  weight = 50
  identifier = "about"
  url = "/about/"
  post = "#icon-ziyuan1"

[taxonomies]
  category = "categories"
  tag = "tags"
  archive = "archive"

[params]
  custom_css = ["custom_css/style.css", "custom_css/syntax.css"]

# mathjax
  enableMathJax = true                                                 # enable mathjax  # 是否使用mathjax（数学公式）

  enableSummary = true                                                 # display the article summary  # 是否显示文章摘要
  

[highlight]
  pygmentsUseClasses = true
  pygmentsCodeFences = true     # 开启```languageCode```模式
  pygmentsCodefencesGuessSyntax = true
```

## 三、运行

如果没有什么意外，运行下面命令，访问`http://localhost:1313/`你就会看到一个初始化状态的yellow主题布局：

```
hugo server -D
```

## 四、如何更新到最新主题？

如果主题作者对主题进行了更新，那你可以进入yellow目录下，直接`git pull`就可以更新到最新的主题了,大概如下：

```
[Busui@Busui themes]$ cd yellow/
[Busui@Busui yellow]$ git pull
remote: Enumerating objects: 13, done.
remote: Counting objects: 100% (13/13), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 7 (delta 5), reused 7 (delta 5), pack-reused 0
Unpacking objects: 100% (7/7), done.
From github.com:Busui/gohugo-theme-yellow
   5f95dc9..fb125e3  master     -> origin/master
Updating 5f95dc9..fb125e3
Fast-forward
 layouts/_default/summary.html | 2 +-
 layouts/posts/single.html     | 5 +++++
 2 files changed, 6 insertions(+), 1 deletion(-)
```

## 五、一些markdown语法约定

### 五一、标题

标题可以从标题一到标题六：

```
# This is H1
## This is H2
### This is H3
#### This is H4
##### This is H5
###### This is H6
```

渲染后如下：
# This is H1
## This is H2
### This is H3
#### This is H4
##### This is H5
###### This is H6

如你所见。h5和h6被本主题弃用了。

一般H1是用作标题的。而写文章的时候，尽量用h2, h3和h4标签。这不是硬性要求，但这么做无疑会让你的文章布局显得更加美观。


### 五二、列表样式：

```
- This is line1-1
    - This is line1-2
- This is line2-1
    - This is line2-2
```

渲染后如下：

- This is line1-1
    - This is line1-2
- This is line2-1
    - This is line2-2

### 五三、图片加载

图片加载分为两种。

**一种**是从url加载的。这种情况下图片是放在一个集中的地方，比如你服务器上某个专门存储图片的文件夹，比如某个图床。加载这种图片的语法如下：

```
![](https://.....)
```        

括号里面的`https://.....`可以换成相应的图片资源url。

**另一种**是从[Page Bundles](https://gohugo.io/content-management/page-bundles/)加载。这种情况是将与你写的文章的相关资源统一在一个文件夹里面，也就是和这篇文章相关的图片也会和对应文章放在同一个文件路径内。如果想深入理解还是请看官方[Page Bundles](https://gohugo.io/content-management/page-bundles/)。我举个例子吧：

比如我的about页面就是一个`Page Bundles`：
```
-about
|-- _index.html
|__ genie.png
```

`_index.html`(注意有下划线)说明这是一个`Page Bundles`。因为我的about页面只用到了一张图片资源，所以只是放了一张图片。事实上如果你的about页面有其他资源，比如视频，音乐，你也应该把它们放到about文件夹下。

hugo提供了一些函数和变量来加载`Page Bundles`下的图片（资源），因此本主题自定义了一个名为`Hugoimg`的[shortcodes](https://gohugo.io/content-management/shortcodes/)来加载图片：

```
{{</ Hugoimg genie Fit />}}
```


实际上这个shortcode灵活性很强的。你自己看[源码](https://github.com/Busui/gohugo-theme-yellow/blob/master/layouts/shortcodes/Hugoimg.html)就知道怎么用了。当然，我会更建议你去看[官方](https://gohugo.io/templates/shortcode-templates/#single-named-example-image)的。  