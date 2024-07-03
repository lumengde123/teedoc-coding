---
title: 开源静态文档网站生成工具 teedoc
keywords: teedoc, 教程, 静态文档网站
desc: 开源静态文档网站生成工具 teedoc
date: 2024-06-15
class: heading_no_counter
tags: 教程, teedoc
---

> **`teedoc`** 是一款开源静态文档网站生成工具，可以在一个工程文件中生成多个不同的文档

官方文档：https://teedoc.github.io/get_started/zh/index.html

>! 注意：修改非文本内容后，建议先`teedoc build`一下再启动HTTP服务查看修改效果；`teedoc serve`只是预览

## 一、安装 teedoc

安装教程：https://teedoc.github.io/get_started/zh/install/index.html

### 1.1 安装 Python3

`teedoc` 是基于 `Python3` 语言开发的软件，需要有这个软件的支持

Linux 可以使用指令
```bash
sudo apt install python3 python3-pip git
```

Windows 和 macOS请到[官网下载](https://www.python.org/downloads/)，然后设置好环境变量并测试是否可用

### 1.2 安装 teedoc

打开终端(`Windows`按`Ctrl+R`输入`cmd`)，输入指令安装 teedoc：
```bash
pip3 install teedoc
```
以后通过以下指令更新 `teedoc`：
```bash
pip3 install -U teedoc
```


## 二、创建工程并启动

### 2.1 新建工程 

进入某个目录内，新建一个工程目录并初始化  [>点此查看原文<](https://teedoc.github.io/get_started/zh/usage/quick_start.html#%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B)
```bash
mkdir teedoc_site
cd teedoc_site
teedoc init
```
然后选择`1`，也就是`minimal`模板生成一份最小工程，会在目录下自动生成一些基础文件

### 2.2 安装插件

执行指令，根据`site_config.json`中的`plugins`的插件设置安装插件

```bash
cd teedoc_site
teedoc install
```
后续也可以在文档中安装一些自己需要的插件。

### 2.3 启动`HTTP`服务

```bash
teedoc serve
```

这个命令会先构建所有`HTML`页面以及拷贝资源文件，然后起一个`HTTP`服务

如果只需要生成页面，使用
```
teedoc build
```

在显示 `Starting server at 0.0.0.0:2333 ....` 后，打开浏览器访问: http://127.0.0.1:2333 就可以看到网站了

同时可以看到目录下多了一个`out`目录，里面就是**生成的静态网站内容**，直接拷贝到服务器使用nginx或者apache进行部署即可

> `teedoc serve` 启动`HTTP`服务后，直接修改`.md`的内容就能在网站上看到修改，但是只是预览效果
> `teedoc build` 后才能真正生成对应的页面

### 2.4 文档目录结构

先阅读下[官方文档](https://teedoc.github.io/get_started/zh/usage/start.html#%E6%96%87%E6%A1%A3%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84)，然后我按个人理解来列出这些文件的作用

以下是一级文件目录的内容
```
teedoc_size
	config
	docs
	out
	pages
	static
	site_config.json
```

- `teedoc_size`：工程文件名，即整个项目的名称
- `config`：存放`config` 模板文件，新建文档后，文档的`config.json`或者`config.yaml`可以`import`这里面的模板
- `docs`：存放文档文件的。每个文档都会在里面有一个独立的文件夹
- `out`：out文件就是最后生成的网站文件
- `pages`：存放页面，包括主页、404页面等等页面
- `static`：静态文件文件夹，比如存放图片
- `site_config.json`：网站配置文件

以下是docs文件目录的内容
```
docs
	doc1
		config.json
		README.md
		sidebar.yaml
		teedoc_notes.md
	doc2
		config.json
		README.md
		sidebar.yaml
```

`teedoc`一个工程文件可以同时包含多个文档网站，并且可以相互跳转。这里的`doc1`和`doc2`就是不同的文档文件。

每个文档文件里包含：
- `config.json`：配置文件
- `README.md`： `README.md` 会被自动转换成index.html，作为文档的首页
- `sidebar.yaml`：用来配置文档侧边栏，需要和.md文件绑定
- `xxx.md`：文档目录如果有内容，需要与对应的`.md`文件绑定，才能显示对应的内容


## 三、基本使用



### 3.1 新建目录和页面

在`docs/doc1`文件夹中，新建一个`first.md`文件

在同级的`sidebar.yaml`中添加一条记录，用来添加文档目录以及绑定目录内容`first.md`的
```yaml
items:
...
-   label: 目录01
    file: first.md
```

### 3.2 侧边栏的使用技巧

每个文档有对应的`sidebar.yaml`来配置侧边栏内容，以下为例子：
```
items:
-   label: Brief
    file: README.md
-   label: 目录01
    file: first.md
-   label: 一级目录
    collapsed: false
    items:
    -   label: 二级目录01
    -   label: 二级目录02
-   label: 目录可以是跳转链接
    url: https://github.com/teedoc/teedoc
    target: _blank
```
- `label`对应侧边栏目录的内容
- `file`为目录对应的`.md`文件，如果没有内容可以删除file字段
- `collapsed: false`表示子目录默认不折叠
- `target`为跳转方式，`_blank`打开新页面，`_self`从当前页面打开

### 3.3 修改文档名称或路径

项目创建后，第一个文档默认为`doc1`，想将它改为自己需要的内容

需要细心修改3个地方，否则可能导致跳转出错


**1. 修改文档菜单和跳转路径**

打开`teedoc_site/config/config.json`文件，将
```json
"items": [
            {
                "url": "/doc1/",
                "label": "Doc1",
                "position": "left"
            },
```
修改为
```json
"items": [
            {
                "url": "/my_notes/",
                "label": "我的笔记",
                "position": "left"
            },
```
- `label`为导航栏菜单中，本文档的名称，随便改没影响
- `url`为点击菜单，跳转到对应的路径
- `position`为菜单向左对齐，可以自己改成 **right**

**2. 修改文档的`route`路由匹配**

打开`teedoc_site/site_config.json`文件，将
```json
"route": {
        "docs": {
            "/doc1/": "docs/Doc1"
        },
```
修改为
```json
"route": {
        "docs": {
            "/my_notes/": "docs/我的笔记"
        },
```
其中，**左侧**为匹配的路径，需要和第一步的`url`对应，否则会出错；**右侧**为对应的文档文件夹位置，需要和文件实际的名称对应，所以需要将文件夹名称由`Doc1`重命名为`我的笔记`，这俩名称要一致

**3. 修改主页按钮跳转路径**

如果主页的按钮涉及到了当前修改的路径，则需要把按钮链接也改了，例如：
```html
<div id="big_btn_wrapper">
    <a class="btn" href="/doc1/">打开笔记</a>
</div>
```

打开`teedoc_site/pages/index/README.md`文件，将`href"/doc1/"`改为`href"/my_notes/"`，否则点击按钮会跳转出错

## 四、一些配置

### 4.1 关闭目录自动编号

在`metadata`里添加配置`class: heading_no_counter`即可

### 4.2 修改刷新延迟

修改项目工程后，会在1秒内刷新，这个时间是可以修改的

打开根目录的`site_config.json`，修改以下内容
```json
"rebuild_changes_delay": 1,
```

### 4.3 `config.json` 模板解析

打开`teedoc_site/config/config.json`

- `"title"`：修改左上角站点名称
- `"logo"`：修改站点名称左侧图标；"alt"为图标名称，"url"为图标地址
- `"home_url"`：点击站点名称/图标后跳转到哪里
- `"items"`：用来设置导航栏菜单的，每个`{}`里代表一个文档
- `"footer"`：底部页脚内容，自己调一下就知道咋用了

.. details::点击查看 footer 示例

    666
    ```json
	"top":[
	    {
	        "label": "第一列链接",
	        "items": [
	            {
	                "label": "我的博客",
	                "url": "https://lumengde.com",
	                "target": "_blank"
	            },
	            {
	                "label": "我的博客",
	                "url": "https://lumengde.com",
	                "target": "_blank"
	            }
	        ]
	    },
	    {
	        "label": "第二列链接",
	        "items": [
	            {
	                "label": "我的博客",
	                "url": "https://lumengde.com",
	                "target": "_blank"
	            }
	        ]
	    }
	],
	```

### 4.4 拓展插件

官方文档：https://teedoc.github.io/get_started/zh/plugins/others.html

- 网站搜索插件：默认装上了，可以把英文替换成中文

打开站点的`config.json`，建议添加以下内容
```yaml
"teedoc-plugin-search":{
    "config": {
        "search_hint": "搜索",
        "input_hint": "输入关键词，多关键词空格隔开",
        "loading_hint": "正在加载，请稍候。。。",
        "download_err_hint": "下载搜索文件失败，请刷新重试或检查网络",
        "other_docs_result_hint": "来自其它文档的结果",
        "curr_doc_result_hint": "当前文档搜索结果"
    }
}
```

> 搜索框搜到的内容，点击跳转到具体页面，可以在当前页面查找所有对应关键词，如果要切换其他文档或页面，按浏览器左上角的返回键即可，不用重新搜索

- gitalk 评论插件：所有的评论数据会放在自己`github`的一个仓库的`issue`上。

- Google 页面翻译插件：在导航栏右侧添加谷歌翻译按钮，点击后一键翻译网页

- 广告或者重要消息全局提示：在顶部显示重要消息或全局提示

- 点赞插件：实现页面显示点赞按钮，可以统计点赞次数

## 五、Markdown语法

详细内容请查看[官方文档](https://teedoc.github.io/get_started/zh/syntax/syntax_markdown.html)

标题自定义id：这里`标题自定义id`的id为 `custom-id`
```
### 标题自定义id {#custom-id}
```

强调、斜体、删除线
```
我们只知道**地球**具有让人类生存的环境，还有*火星*，也许还有~~其它星球~~。
```

分隔符
```
---
***
```

列表：`-`或`*`都可

代码段：第一行是\```+代码语言，第二行开始是代码，最后一行是\```

注释/引用块：可以内嵌`>>`

警告：`>!`

emoji表情：暂不支持，但是可以从 `emoji`表情大全 粘贴进来

上下标：`H~2~O`，`y = x^2^`

[图片](https://teedoc.github.io/get_started/zh/syntax/syntax_markdown.html#%E5%9B%BE%E7%89%87)

视频：使用HTML的 `vedio` 标签
```html
<video src="https://****.com/***.mp4" controls="controls" preload="auto">your brower not support play video</video>
```

iframe：一般视频平台分享的代码直接能使用，可以稍微设置一下宽高
```html
<iframe src="//player.bilibili.com/player.html?aid=52613549&bvid=BV144411J72P&cid=92076022&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" style="width:43vw;height:34vw;min-width: 85%;"> </iframe>
```

引用标记：在文章末尾进行注解
```
我能干饭我自豪。[^干饭人]

[^干饭人]: 老子说道
```

表格
```
| Header 1 | *Header* 2 |
| -------- | -------- |
| `Cell 1` | [Cell 2](http://example.com) link |
| Cell 3 | **Cell 4** |
```

任务列表
```
- [x] 任务1
- [x] 任务2
- [ ] 任务3
- [ ] 任务4
```

标题链接/页内跳转：`[文本](要跳转的标题id)`
```
[iframe 嵌入网页](#iframe-嵌入网页)
```
此外，可以自定义标题的id
```
## iframe 嵌入网页 {#iframe-embed}
```
这里标题：**iframe 嵌入网页** 的id就是 `#iframe-embed`

HTML：能直接在md文件中写HTML

数学：支持`tex`和`Latex`语法，以及`MathML`标签

mermaid：使用 mermaid 可以画很多类型的图表， 详细的语法和支持请看[官网](https://mermaid-js.github.io/)

[标签页/teedoc专用](https://teedoc.github.io/get_started/zh/syntax/syntax_markdown.html#%E6%A0%87%E7%AD%BE%E9%A1%B5%EF%BC%88tabset%EF%BC%89%E6%94%AF%E6%8C%81)


详情页/teedoc专用



## 六、项目不足之处&注意事项

- 搜索功能建议再添加个返回键

- 右侧文章目录无法默认全部展开

- tag无法点击，不能通过tag找到同标签的文章，用处不大

注意事项：

- 搜索功能要先`teedoc build`才能搜到新内容；返回查看其他搜索结果时可以按浏览器左上角返回键