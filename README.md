# WebUI-Aria2 | Aria2网页UI

![Main interface](/screenshots/overview.png?raw=true)

The aim for this project is to create the worlds best and hottest interface to interact with aria2. aria2 is the worlds best file downloader, but sometimes the command line brings more power than necessary. The project was initially created as part of the GSOC scheme, however it has rapidly grown and changed with tremendous support and feedback from the aria2 community.

该项目的目标是创建世界上最好和最热门的界面与aria2交互。aria2是世界上最好的文件下载器，但有时命令行带来的功能超出了需要。该项目最初是作为GSOC计划的一部分创建的，但是在aria2社区的大力支持和反馈下，它已经快速增长并发生了变化。

Very simple to use, no build scripts, no installation scripts. First start aria2 in the background either in your local machine or in a remote one. You can do that as follows:

使用非常简单，没有构建脚本，也没有安装脚本。首先在本地机器或远程机器的后台启动aria2。你可以这样做:

```bash
aria2c --enable-rpc --rpc-listen-all
```

> 或使用我们提供的[Aria2](https://github.com/ChenYFan/Aria2_for_Windows.git),直接运行`Hiderun.vbs`

If aria2 is not installed in your local machine then head on to https://aria2.github.io/ and follow the instructions there.

如果您的本地计算机中没有安装aria2，则直接安装到 <https://aria2.github.io/> 然后按照那里的说明去做。

Then to use the WebUI-Aria2,

然后使用Aria2网页UI，

- You can either download this repository and open index.html from `docs` folder.
您可以下载这个存储库并从`docs`文件夹打开index.html。
- Or you could just head on to https://ziahamza.github.io/webui-aria2 and start downloading files! Once you have visited the URL thanks to [Progressive Web Apps](https://developers.google.com/web/progressive-web-apps/) you can open the same URL even when you are offline.
或者直接访问https://ziahamza.github.io/webui-aria2 开始下载文件!多亏了[Progressive Web Apps](https://developer.google.com/web/progressive-Web-apps/)，一旦你访问了这个URL，即使你离线，你也可以打开相同的URL。
        
- Or you can also use NodeJS to create simple server by using the following command from the project folder.
或者也可以使用NodeJS从项目文件夹中使用以下命令创建简单的服务器。

```bash
node node-server.js
```

# Tips | 提示

1. You can always select which files to download in case of torrents or metalinks. Just pause a download and a list icon should appear next to the settings button. To select which files to download before starting the download, give the flag --pause-metadata to aria2. 

你总是可以选择下载哪些文件，以防洪流或金属油墨。只要暂停下载，设置按钮旁边就会出现一个列表图标。若要在开始下载之前选择要下载的文件，请在命令行时输入`--pause-metadata`

See [link](https://aria2.github.io/manual/en/html/aria2c.html#cmdoption--pause-metadata)

看 [官方帮助文档](https://aria2.github.io/manual/en/html/aria2c.html#cmdoption--pause-metadata)获得更多知识.
# Configuration | 配置

Read and edit [configuration.js](src/js/services/configuration.js).

你可以在[configuration.js](src/js/services/configuration.js)看到能修改的选项进行修改

## DirectURL | 直链

This feature allows users to download files that they download from aria2 directly from the webui dashboard. If you are familiar with how webservers work, setup a http server that points at the configured aria2 download directory, check permissions. Then Specify a full url: `http://server:port/` in the webui directURL configuration.

该特性允许用户从webui仪表板直接从aria2下载文件。如果您熟悉web服务器的工作方式，请设置一个指向已配置的aria2下载目录的http服务器，检查权限。然后指定一个完整的url: `http://server:port/` 在webui直接配置。

If the above is not obvious, keep reading what this is about in [directurl.md](directurl.md)

如果以上不明显，请继续阅读这篇文章[directurl_zh-cn.md](directurl_zh-cn.md)


# Dependencies | 依赖关系

Well, you need aria2. And a web browser (if that even counts!)

你需要ari2。还有一个web浏览器(如果这算的话!)

# Docker support | Docker支持

There is two Dockerfile in this project, one is a common Dockerfile, which can be use for **testing purpose**.<br>
The second is a **production ready** Dockerfile for arm32v7 plateforms (which includes raspberry).

在这个项目中有两个Dockerfile，一个是通用的Dockerfile，可以用于**测试目的**.<br>

第二个是用于arm32v7平台表单(其中包括raspberry)的**production ready** Dockerfile。

### 测试目的

You can also try or use webui-aria2 in your LAN inside a Docker sandbox.

您还可以在Docker沙箱内的LAN中尝试或使用webui-aria2。

Build the image

创建图片

```bash
sudo docker build -t yourname/webui-aria2 .
```

..and run it! It will be available at: `http://localhost:9100`

..然后运行它!它会在`http://localhost:9100`出现.

```bash
sudo docker run -v /Downloads:/data -p 6800:6800 -p 9100:8080 --name="webui-aria2" yourname/webui-aria2
```

`/Downloads` is the directory in the host where you want to keep the downloaded files.


`/Downloads` 这是您希望将下载的文件保存在主机中的目录.

### Production ready (ARM platform) | 开始使用(ARM平台)

This image contains both aria2 and webui-aria2.

此图像包含aria2和webui-aria2。

Build it (may take several hours due to the aria2 compilation process. Don't panic and grab a coffee).

构建它(由于aria2编译过程，可能需要几个小时。不要惊慌，去喝杯咖啡)。

```
docker build -f Dockerfile.arm32v7 -t yourname/webui-aria2 .
```

This command will ends up building three images:

这个命令最终将构建三个映像:

- The first one is just about compiling aria2 and goreman binaries. It MUST be deleted each time the `ARIA2_VERSION` is changed in the Dockerfile, otherwise you won't benefit from the update.
- The second is about building and downloading some go dependencies (goreman and gosu).
- The second one is the acutal aria2 container, the one you must use.

<br />

-第一个是关于编译aria2和goreman二进制文件。每次在Dockerfile中更改' ARIA2_VERSION '时都必须删除它，否则您将无法从更新中获益。
-第二个是关于建立和下载一些go依赖(goreman和gosu)。
-第二个是acutal aria2容器，您必须使用它。

Prepare the host volume:
This image required few file to be mounted in the container.

准备主机卷:
这个映像只需要很少的文件就可以挂载到容器中。

```
/home/aria/aria2/session.txt  (empty file)
/home/aria/aria2/aria2.log    (empty file)
/home/aria/aria2/aria2.conf   (aria2 configuration file, not webui-aria2 conf) must contains at least `enable-rpc=true` and `rpc-listen-all=true`
/data/downloads/        (where the downloaded files goes)
```

Run it

运行它

```
docker run --restart=always \
        -v /home/<USER>/data/aria2/downloads:/data/downloads \
        -v /home/<USER>/data/aria2/.aria2:/home/aria/.aria2 \
        -p 6800:6800 -p 9100:8080 \
        --name="webui-aria2" \
        -d yourname/webui-aria2
```

# Contributing | 贡献

Checkout [contributor's guide](CONTRIBUTING.md) to know more about how to contribute to this project.

# Deploy to Heroku | 快速部署到Heroku

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

# Support | 支持

For any support, feature request and bug report add an issue in the github project. [link](https://github.com/ziahamza/webui-aria2/issues)

对于任何支持，特性请求和bug报告都在github项目中添加一个[issue](https://github.com/ziahamza/webui-aria2/issues)。

# License | 许可证

Refer to the LICENSE file (MIT License). If the more liberal license is needed then add it as an issue
