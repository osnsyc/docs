# Linux配置

## Linux创建应用图标及默认程序设置

在Linux中，通过软件市场等方式安装的应用，会默认创建好应用的快捷方式。
 但是对于自己下载的应用或者源码编译的应用，就需要自己创建应用图标。
 在 Linux 中，一个 `.desktop` 文件就是一个用来运行程序的快捷方式。
 没有 `.desktop` 的话，你的应用就不会在应用菜单中显示了。

`.desktop` 文件基本上就是一个包含程序信息的纯文本文件，通常根据是自己可见还是所有用户可见的不同而放在`~/.local/share/applications`或者 `/usr/share/applications/`目录中。

### 实例

在`/usr/share/applications/`中创建一个文件`typora.desktop`.内容如下：

```
[Desktop Entry]
Type=Application
Terminal=false 
Name=Typota
Categories=Development
Icon=/home/panking/opt/Typora/logo.png
Exec=/home/panking/opt/Typora/Typora %f  # 这个%f很重要，没有的话，双击打开文件时，会是空白文档
MimeType=text/markdown  # 这个配置很重要，用来说明这个程序支持text/markdown文件的打开
```

### 关键词说明

```
[Desktop Entry] 文件头
Encoding    编码
Name    应用名称
Name[xx]    不同语言的应用名称
GenericName 描述
Comment 注释
Exec    执行的命令
Icon    图标路径
Terminal    是否使用终端
Type    启动器类型
Categories  应用的类型（内容相关）
MimeType  指定了文件的类型名称、描述、图标信息，同时通过与.desktop应用程序描述文件整合，指定了文件的打开方式。
```

其中 Exec 常用的参数有：`%f` `%F` `%u` `%U`
 `%f`：单个文件名，即使选择了多个文件。如果已选择的文件不在本地文件系统中（比如说在HTTP或者FTP上），这个文件将被作为一个临时文件复制到本地，％f将指向本地临时文件；
 `%F`：文件列表。用于程序可以同时打开多个本地文件。每个文件以分割段的方式传递给执行程序。
 `%u`：单个URL。本地文件以文件URL或文件路径的方式传递。
 `%U`：URL列表。每个URL以分割段的方式传递给执行程序。本地文件以文件URL或文件路径的方式传递。



## Clash For Linux

### 下载

1. GitHub项目地址:[https://github.com/Dreamacro/clash/releases](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FDreamacro%2Fclash%2Freleases)
    下载适合的 Clash 二进制文件,一般个人的64位电脑下载 clash-linux-amd64.tar.gz 即可。

### 配置

1. 在合适路径下建立Clash(by:也可用其他名字，只要后续使用时路径正确)文件夹，用于存放解压后的文件及配置文件,将下载的压缩文件解压至此，得到一个可执行文件“ clash-linux-amd64 ”。

2. 在终端中执行该文件“ clash-linux-amd64 ”，此时自动生`/home/当前用户ID/.config/clash`文件夹，其中包含两个文件 config.yml 和 Country.mmdb . 如提示权限不足，请执行 `chmod +x  clash-linux-amd64`

3. 编辑/home/当前用户ID/.config/clash下的 config.yml配置文件，内容为自己的服务器及规则等信息(有些商家会提供相应的yml文件，下载后将内容copy至该文件)，保存更改后复制该文件至先前创建的Clash文件夹(by:这两个文件夹不要弄混，一个是手动建立的，一个是自动创建的，都需要.yml文件)。
    `sudo vim /home/userid/.config/clash/config.yml`

4. 重启 clash-linux-arm64 以加载配置文件。

5. 保持 clash-linux-amd64 运行，通过浏览器访问 [http://clash.razord.top](https://links.jianshu.com/go?to=http%3A%2F%2Fclash.razord.top) 进行策略组设置。首次进入时会要求输入IP和端口，端口和口令按yml文件中的`external-controller`内容输入即可，IP为`127.0.0.1`。

6. 保持clash运行，在 设置>网络>系统代理>手动 中按照yml配置文件中设置的端口进行配置。至此便可通过Clash上网。

   

## git最新版的安装

You need to **add the Git Maintainers repository** in order to get the latest Git version.

```shell
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git
```

```shell
git --version
```
It's **not always necessary to remove the existing Git before upgrading** it, but if you run into any problems, do the following and then repeat the steps mentioned above:

```shell
sudo apt-get remove git
```



## Chrome密码不同步

关了Chrome

`~/.config/google-chrome/Default`

删掉里面的`Login Data`

重开

> mac 下该文件的路径是
>
>  `/Library/Application Support/Google/Chrome/Profile 1/Login Data`
>
> windows下是 `C:\Users\Administrator\AppData\Local\Google\Chrome\User Data\Default\Login Data`



## VSCode

### 安装

```shell
sudo add-apt-repository ppa:ubuntu-desktop/ubuntu-make
sudo apt-get update
sudo apt-get install ubuntu-make
```

```shell
umake ide visual-studio-code
```


- 点击图标无法打开

```shell
cd ~/.config
sudo rm -rf ./Code/
```

-  Visual Studio Code is unable to watch for file changes in this large workspace" (error ENOSPC)

```shell
cat /proc/sys/fs/inotify/max_user_watches
sudo vi /etc/sysctl.conf
#加入 fs.inotify.max_user_watches=524288
sudo sysctl -p #生效
```



## docker安装微信

https://www.runoob.com/docker/ubuntu-docker-install.html

```shell
sudo apt-get update
```

```shell
sudo apt-get install \
apt-transport-https \
ca-certificates \
curl \
gnupg-agent \
software-properties-common
```

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```shell
sudo apt-key fingerprint 0EBFCD88
```

```shell
sudo add-apt-repository \
 "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"
```

```shell
sudo apt-get update
```

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

```shell
sudo docker run hello-world
```



> Permission denied while trying to connect to the Docker daemon socket
> 
> ```shell
> sudo cat /etc/group | grep docker
> sudo groupadd -g 999 docker
> sudo usermod -aG dockerroot usrname
> sudo usermod -aG docker usrname
> sudo gpasswd -a usrname docker
> ```

