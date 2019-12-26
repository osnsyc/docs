#  生成文档网站简单使用教程

## 1. docsify介绍

docsify 是一个动态生成文档网站的工具。不同于 GitBook、Hexo 的地方是它不会生成将 .md 转成 .html 文件，所有转换工作都是在运行时进行。

这将非常实用，如果只是需要快速的搭建一个小型的文档网站，或者不想因为生成的一堆 .html 文件“污染” commit 记录，只需要创建一个 index.html 就可以开始写文档而且直接部署在 GitHub Pages。

## 2. 引入docsify方式

### 2.1 手动创建`index.html`并引入docsify文件

docsify使用方式很简单，只需要在项目中创建一个`index.html`文件，内容能够如下：

```html
<!DOCTYPE html>
<html>
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta charset="UTF-8">
  <link rel="stylesheet" href="//unpkg.com/docsify/themes/vue.css">
</head>
<body>
  <div id="app"></div>
  <script>
    window.$docsify = {
      //...
    }
  </script>
  <script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
</body>
</html>
```

然后在项目中创建一个`README.md`文件：

```markdown
## 我是首页

这是我的首页介绍
```

如果你的系统安装了Python 的话，可以使用Python来启动一个服务：

```
python -m SimpleHTTPServer 3000

Serving HTTP on 0.0.0.0 port 3000 ...
127.0.0.1 - - [03/Jan/2019 13:45:27] "GET / HTTP/1.1" 200 -
```

![clipboard.png](readme.assets/4212216079-5c2da1dab9f02_articlex.png)
然后在浏览器中输入`http://localhost:3000/`查看浏览效果。

如果没有Python，还是可以使用`http-server`启动服务，
可在终端输入`npm install http-server -g`来安装`http-server`。

```
http-server 

Starting up http-server, serving ./
Available on:
  http://127.0.0.1:8080
  http://172.24.70.142:8080
Hit CTRL-C to stop the server
```

![clipboard.png](readme.assets/1906804078-5c2da2f5be2a4_articlex.png)
然后在浏览器中输入`http://127.0.0.1:8080`查看浏览效果。

![clipboard.png](readme.assets/2496836868-5c2da7a3cdedf_articlex.png)

> ***注意：\***
> 1.在此模式下编辑文件保存后，需要手动刷新浏览器才能看见修改的效果，下面介绍的`docsify-cli`可实现自动查看效果。
> 2.强烈建议把`index.html`文件中的`docsify.min.js`和`vue.css`文件复制到本地项目，然后使用如下方式引入：
>
> ```
> <link rel="stylesheet" href="./vue.css">
> <script src="./docsify.min.js"></script>
> ```
>
> 这样做的好处是不在依赖网络环境了。

### 2.2 使用`docsify-cli`来开发

docsify-cli 工具，可以方便创建及本地预览文档网站。

#### 2.2.1 安装

docsify需要本地先安装`node`, 如果没有安装node，可在node官网选择对应操作系统下载安装：https://nodejs.org/zh-cn/

终端输入`npm i docsify-cli -g`进行全局安装：

```powershell
npm i docsify-cli -g

/usr/local/bin/docsify -> /usr/local/lib/node_modules/docsify-cli/bin/docsify
> fsevents@1.2.4 install /usr/local/lib/node_modules/docsify-cli/node_modules/fsevents
> node install
[fsevents] Success: "/usr/local/lib/node_modules/docsify-cli/node_modules/fsevents/lib/binding/Release/node-v57-darwin-x64/fse.node" already installed
Pass --update-binary to reinstall or --build-from-source to recompile
> docsify@4.8.6 postinstall /usr/local/lib/node_modules/docsify-cli/node_modules/docsify
> opencollective postinstall

                         Thanks for installing docsify 🙏
                 Please consider donating to our open collective
                        to help us maintain this package.

          👉  Donate: https://opencollective.com/docsify/donate

+ docsify-cli@4.3.0
added 456 packages from 206 contributors in 32.827s
```

![clipboard.png](readme.assets/3235988426-5c2a48009e77d_articlex.png)

安装结束后使用`docsify -v`查看是否安装成功：

```shell
docsify -v

docsify-cli version:
  4.3.0
```

> **PowerShell中创建vue项目提示无法加载文件**
>
> **解决方法**
>
> 1. 以管理员身份运行PowerShell
> 2. 执行：get-ExecutionPolicy，如果显示Restricted，表示状态是禁止的
> 3. 执行：set-ExecutionPolicy RemoteSigned
> 4. 选择Y

#### 2.2.2 初始化一个项目

首先需要创建一个项目目录：

```shell
mkdir docsify
```

进入项目目录后，使用`docsify init ./`来初始化一个项目：

```shell
cd docsify

docsify init ./

Initialization succeeded! Please run docsify serve ./
tree -a

.
├── .nojekyll
├── README.md
└── index.html
```

初始化成功后,`docsify`目录会生成如下几个文件：

> 1. `index.html`入口文件
> 2. `README.md`会做为主页内容渲染
> 3. `.nojekyll`用于阻止 GitHub Pages 会忽略掉下划线开头的文件、
>
> `.nojekyll`文件很重要，如果网站部署到`GitHub Pages`时，一定要注意这个文件。

直接编辑 ./README.md 就能更新网站内容，当然也可以添加其他`.md`文件。

#### 2.2.3 启动本地服务

终端输入`docsify serve ./`来启动服务：

```shell
docsify serve ./

Serving /Users/dragon/tmp/docsify now.
Listening at http://localhost:3000
```

然后浏览器打开`http://localhost:3000`就能看见效果。

![clipboard.png](readme.assets/1324477842-5c2a518b7f422_articlex.png)

当修改文件保存后， `docsify serve ./`服务会自动实时更新。

## 3. 关于每个页面和URL路径说明

如果需要创建多个页面，或者需要多级路由的网站，在`docsify`里也能很容易的实现。例如创建一个`guide.md`文件，那么对应的路由就是`/#/guide`。
如果你的目录结构如下：

```
-| ./
  -| README.md
  -| guide.md
  -| zh-cn/
    -| README.md
    -| guide.md
```

那么对应的访问页面将是:

```
./README.md        => http://domain.com
./guide.md         => http://domain.com/guide
./zh-cn/README.md  => http://domain.com/zh-cn/
./zh-cn/guide.md   => http://domain.com/zh-cn/guide
```

## 4. 侧边栏设置

默认情况下，侧边栏会根据当前文档的标题生成目录。

![clipboard.png](readme.assets/3281541690-5c2b29250edfa_articlex.png)

### 4.1 定制侧边栏

首先需要在`index.html`文件中的`window.$docsify`添加`loadSidebar: true,`选项：

```html
<script>
  window.$docsify = {
    loadSidebar: true
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

接着在项目根目录创建`_sidebar.md`文件，内容格式如下：

```
* [home1](home1)
* [home2](home2)
* [bar](bar/)
* [bar/a](bar/a)
```

![clipboard.png](readme.assets/161934219-5c2b346a927ce_articlex.png)

***注：\***配置了`loadSidebar`后就不会生成默认的侧边栏了。

### 4.2 关于侧边栏`_sidebar.md`文件的说明

- 如果只在根目录有一个`_sidebar.md`文件，那么所有页面都将使用这个一个配置，也就是所有页面的侧边栏都一样。
- 如果一个子目录中有`_sidebar.md`文件，那么这个子目录下的所有页面将使用这个文件的侧边栏。
- `_sidebar.md`的加载逻辑是从每层目录下获取文件，如果当前目录不存在该文件则回退到上一级目录。例如当前路径为`/zh-cn/more-pages`则从`/zh-cn/_sidebar.md`获取文件，如果不存在则从`/_sidebar.md`获取。

如果子目录有`_sidebar.md`,但你就想使用根目录的`_sidebar.md`，
可在`index.html`文件中的`window.$docsify`添加`alias`字段：

```html
<script>
  window.$docsify = {
    loadSidebar: true,
    alias: {
      '/.*/_sidebar.md': '/_sidebar.md'
    }
  }
</script>
```

配置`alias`字段后，所有页面都会显示项目根目录`_sidebar.md`文件的配置作为侧边栏，子目录的`_sidebar.md`文件会失效。

### 4.3 显示页面目录(当前页面的标题)

定制的侧边栏仅显示了页面的链接。
还可以设置在侧边栏显示当前页面的目录(标题)。
需要在`index.html`文件中的`window.$docsify`添加`subMaxLevel`字段来设置：

```html
<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 3
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

![clipboard.png](readme.assets/1569030155-5c2b8ffd9eb2a_articlex.png)

subMaxLevel说明：

> subMaxLevel类型是number(数字)，表示显示的目录层级
> **注意：**如果md文件中的第一个标题是一级标题，那么不会显示在侧边栏，如上图所示

| 值   | 说明                                           |
| ---- | ---------------------------------------------- |
| 0    | 默认值，表示不显示目录                         |
| 1    | 显示一级标题(`h1`)                             |
| 2    | 显示一、二级标题(`h1` ~ `h2`)                  |
| 3    | 显示一、二、三级标题(`h1` ~ `h3`)              |
| n    | n是数字，显示一、二、....n 级标题(`h1` ~ `hn`) |

在md文件中标题的写法：

```markdown
# 这是一级标题，对应HTML中<h1>标签

## 这是二级标题，对应HTML中<h2>标签

### 这是三级标题，对应HTML中<h3>标签

#### 这是四级标题，对应HTML中<h4>标签
```

#### 4.3.1 页面的标题不在侧边栏目录显示

***注意：\*** 如果md文件的第一个标题是一级标题，那么默认已经忽略了。

当设置了 subMaxLevel 时，默认情况下每个标题都会自动添加到目录中。如果你想忽略特定的标题，可以给它添加 {docsify-ignore} ：

```
# Getting Started

## Header {docsify-ignore}

该标题不会出现在侧边栏的目录中。
```

要忽略特定页面上的所有标题，你可以在页面的第一个标题上使用 {docsify-ignore-all} :

```
# Getting Started {docsify-ignore-all}

## Header

该页面所有标题都不会出现在侧边栏的目录中。
```

在使用时， {docsify-ignore} 和 {docsify-ignore-all} 都不会在页面上显示。

## 5. 导航栏配置

docsify默认是没有导航栏的，可以通过配置来显示导航栏。

### 5.1 在`index.html`中定义导航栏

如果导航的链接少，则可以直接在`index.html`文件直接定义导航栏，要注意链接要以`#/`开头：

```html
<body>
  <nav>
    <a href="#/">项目</a>
    <a href="#/home1">home1</a>
    <a href="#/bar/a">bar/a</a>
  </nav>
</body>
```

![clipboard.png](readme.assets/1524213589-5c2b98d2c3e53_articlex.png)

### 5.2 通过配置文件定义导航栏

首先需要在`index.html`文件中的`window.$docsify`添加`loadNavbar: true,`选项：

```html
<script>
  window.$docsify = {
    loadNavbar: true
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

接着在项目根目录创建`_navbar.md`文件，内容格式如下：

```
* [home1](home1)
* [home2](home2)
* [bar](bar/)
* [bar/a](bar/a)
```

![clipboard.png](readme.assets/3408446144-5c2b9b0a6f069_articlex.png)

> ***注意：\***
> 1.如果使用配置文件来设置导航栏，那么在`index.html`中定义的导航栏只有在定制的首页才会生效，其他页面会被覆盖。
> 2.如果只在根目录有一个`_navbar.md`文件，那么所有页面都将使用这个一个配置，也就是所有页面的导航栏都一样。
> 3.如果一个子目录中有`_navbar.md`文件，那么这个子目录下的所有页面将使用这个文件的导航栏。
> 4.`_navbar.md`的加载逻辑是从每层目录下获取文件，如果当前目录不存在该文件则回退到上一级目录。例如当前路径为`/zh-cn/more-pages`则从`/zh-cn/_navbar.md`获取文件，如果不存在则从`/_navbar.md`获取。

### 5.3 导航栏嵌套

如果导航内容过多，可以写成嵌套的列表，会被渲染成下拉列表的形式：

```
* 根目录
  * [home1](home1)
  * [home2](home2)
  * [guide](guide)

* bar目录
  * [bar](bar/)
  * [a文件](bar/a)
  * [b文件](bar/b)

* foo目录
  * [one](foo/one)
```

![clipboard.png](readme.assets/1408700234-5c2b9d613eb39_articlex.png)

## 6. 设置封面

docsify默认是没有封面的，默认有个首页`./README.md`。
通过设置`coverpage`参数，可以开启渲染封面的功能。

首先需要在`index.html`文件中的`window.$docsify`添加`coverpage: true`选项：

```html
<script>
  window.$docsify = {
    coverpage: true
  }
</script>
<script src="//unpkg.com/docsify"></script>
```

接着在项目根目录创建`_coverpage.md`文件，内容格式如下：

```
![logo](_media/icon.svg)
# 我的文档网站
## 个人文档网站
> 一个神奇的文档网站生成巩固

* Simple and lightweight (~12kb gzipped)
* Multiple themes
* Not build static html files

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#quick-start)
[Get Started](#quick-start)
```

![clipboard.png](readme.assets/3838518689-5c2ce5ca8385e_articlex.png)

> ***注：\***一份文档只会在根目录下加载封面，其他页面或者二级目录下都不会加载。

#### 6.1 自定义封面背景

目前的背景是随机生成的渐变色，每次刷新都会显示不同的颜色。
docsify封面支持自定义背景色或者背景图，在`_coverpage.md`文档末尾添加：

```
<!-- 背景图片 -->
![](_media/bg.png)

<!-- 背景色 -->
![color](#2f4253)
```

![clipboard.png](readme.assets/1626706737-5c2cea040aa81_articlex.png)

> ***注意：\***
> 1.自定义背景配置一定要在`_coverpage.md`文档末尾。
> 2.背景图片和背景色只能有一个生效.
> 3.背景色一定要是`#2f4253`这种格式的。

#### 6.2 把封面作为首页

配置了封面后，封面和首页是同时出现的，封面在上面，首页在下面。
通过设置`onlyCover`参数，可以让docsify网站首页只显示封面，
原来的首页通过`http://localhost:3000/#/README`访问。

在`index.html`文件中的`window.$docsify`添加`onlyCover: true,`选项：

```html
<script>
  window.$docsify = {
    coverpage: true,
    onlyCover: true,
  }
</script>
<script src="./docsify.min.js"></script>
```

> 通过此配置可以把`./README.md`文件独立出来，当成项目真正的README介绍文件。

#### 6.3 多个封面

如果你的文档网站是多语言的，或许你需要设置多个封面。
例如你的文档目录结构如下

```
.
└── docs
    ├── README.md
    ├── guide.md
    ├── _coverpage.md
    └── zh-cn
        ├── README.md
        └── guide.md
        └── _coverpage.md
```

那么你可以在`index.html`文件中的`window.$docsify`这么配置:

```html
window.$docsify = {
  coverpage: ['/', '/zh-cn/']
};
```

或者具体指名文件名:

```html
window.$docsify = {
  coverpage: {
    '/': 'cover.md',
    '/zh-cn/': 'cover.md'
  }
};
```

## 7. 网站部署到GitHub Pages

GitHub Pages 支持从三个地方读取文件:

> 1、`master`分支
> 2、`master`分支下的`docs`目录
> 3、`gh-pages`分支

> 1、如果你的文档直接是在项目根目录写的，那么可直接把代码推送到master分支上， `GitHub Pages`里选择`master branch`.
> 2.如果你的文档是在master分支下的`docs/`目录下编写的，那么可直接把代码推送到master分支上，`GitHub Pages`里选择`master branch/docs folder`.

本例子项目是直接在根目录中编写的，所以`GitHub Pages`里选择`master branch`的方式部署。

首先在github网站上创建好仓库，然后终端打开项目目录：

```
git init
git add .
git commit -m 'docsify项目初始化'
git remote add origin https://github.com/username/docsify.git
git push --set-upstream origin master
```

代码推送到github上后，打开github的仓库，选择`Settings` -> `GitHub Pages` -> `master branch` -> `save`。

![clipboard.png](readme.assets/2269451939-5c2cf1d93d4b5_articlex.png)

![clipboard.png](readme.assets/1850439453-5c2cf25666eb3_articlex.png)

#### 7.1 使用docsify的例子

[https://spiritree.github.io/n...](https://spiritree.github.io/note/#/)

[https://ripperhe.com/awesome-...](https://ripperhe.com/awesome-mac-html/#/)

## 8. 一些插件

### 8.1 搜索插件

全文搜索插件会根据当前页面上的超链接获取文档内容，在 localStorage 内建立文档索引。默认过期时间为一天，当然我们可以自己指定需要缓存的文件列表或者配置过期时间。

```html
<script>
    window.$docsify = {
      // 完整配置参数
      search: {
        maxAge: 86400000,               // 过期时间，单位毫秒，默认一天
        paths: [],                      // or 'auto'，匹配文件路径
        placeholder: 'Type to search',  // 搜索提示框文字， 支持本地化，例子在下面
        // placeholder: {
        //   '/zh-cn/': '搜索',
        //   '/': 'Type to search'
        // },
        noData: 'No Results!',          // 找不到结果文字提示，支持本地化，例子在下面
        // noData: {
        //   '/zh-cn/': '找不到结果',
        //   '/': 'No Results'
        // },
        depth: 2,                       // 搜索标题的最大程级, 1 - 6
      }
    }
  </script>
  <!-- 引入搜索模块 -->
  <script src="//unpkg.com/docsify/lib/plugins/search.js"></script>
```

### 8.2 评论插件Gitalk

Gitalk：一个现代化的，基于Preact和Github Issue的评论系统。
使用例子：

```html
<link rel="stylesheet" href="//unpkg.com/gitalk/dist/gitalk.css">
<script src="//unpkg.com/docsify/lib/plugins/gitalk.min.js"></script>
<script src="//unpkg.com/gitalk/dist/gitalk.min.js"></script>
<script>
  const gitalk = new Gitalk({
    clientID: 'Github Application Client ID',
    clientSecret: 'Github Application Client Secret',
    repo: 'Github repo',
    owner: 'Github repo owner',
    admin: ['Github repo collaborators, only these guys can initialize github issues'],
    // facebook-like distraction free mode
    distractionFreeMode: false
  })
</script>
```

Gitalk具体使用教程：[https://segmentfault.com/a/11...](https://segmentfault.com/a/1190000018072952)

docsify其他插件：[https://docsify.js.org/#/zh-c...](https://docsify.js.org/#/zh-cn/plugins)

### 8.3 样式插件

在网上找到一个样式：[https://jhildenbiddle.github....](https://jhildenbiddle.github.io/docsify-themeable/#/customization)

使用方法,在HTML文件中引入：

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/docsify-themeable@0/dist/css/theme-simple.css">
```

## 参考资料

docsify中文官网：https://docsify.js.org/#/zh-cn/