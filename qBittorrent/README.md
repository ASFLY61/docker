## 群晖nas自用：

### 感谢以下项目:

[https://github.com/qbittorrent/qBittorrent](https://github.com/qbittorrent/qBittorrent)   
[https://github.com/c0re100/qBittorrent-Enhanced-Edition](https://github.com/c0re100/qBittorrent-Enhanced-Edition)    
[https://github.com/ngosang/trackerslist]( https://github.com/ngosang/trackerslist)

### 版本：

|名称|版本|说明|
|:-|:-|:-|
|qBittorrent|latest|原版(amd64;arm64v8;arm32v7) 集成Trackers自动更新|
|qBittorrent|qee-latest|qee(amd64;arm64v8;arm32v7) 集成Trackers自动更新|
|qBittorrent|4.3.7|原版(amd64;arm64v8;arm32v7) 集成Trackers自动更新|
|qBittorrent|qee_4.3.7.10|qee(amd64;arm64v8;arm32v7) 集成Trackers自动更新|


### 注意：

1. docker启动会自动修复/config及/Downloads配置文件夹用户权限。请勿将对权限敏感的文件夹映射到此文件夹。

### docker命令行设置：

1. 下载镜像

|版本|命令|
|-|:-|
|原版(amd64;arm64v8;arm32v7)|docker pull johngong/qbittorrent:latest|
|原版(amd64;arm64v8;arm32v7)|docker pull johngong/qbittorrent:4.3.5|
|qee(amd64;arm64v8;arm32v7)|docker pull johngong/qbittorrent:qee-latest|
|qee(amd64;arm64v8;arm32v7)|docker pull johngong/qbittorrent:qee_4.3.5.10|


2. 创建qbittorrent容器

        docker create  \
           --name=qbittorrent  \
           -e WEBUIPORT=8989  \
           -e UID=0  \
           -e GID=0  \
           -e UMASK=022  \         
           -p 6881:6881  \
           -p 6881:6881/udp  \
           -p 8989:8989  \
           -v /配置文件位置:/config  \
           -v /下载位置:/Downloads  \
           --restart unless-stopped  \
           johngong/qbittorrent:latest


3. 运行

       docker start qbittorrent

4. 停止

       docker stop qbittorrent

5. 删除容器

       docker rm  qbittorrent

6. 删除镜像

       docker image rm  johngong/qbittorrent:latest

### 变量:

|参数|说明|
|-|:-|
| `--name=qbittorrent` |容器名|
| `-p 8989:8989` |web访问端口 [IP:8989](IP:8989);(默认用户名:admin;默认密码:adminadmin);此端口需与容器端口和环境变量保持一致，否则无法访问|
| `-p 6881:6881` |BT下载监听端口|
| `-p 6881:6881/udp` |BT下载DHT监听端口
| `-v /配置文件位置:/config` |qBittorrent配置文件位置|
| `-v /下载位置:/Downloads` |qBittorrent下载位置|
| `-e TRACKERSAUTO=YES` |自动更新qBittorrent的trackers,默认开启此功能|
| `-e TZ=Asia/Shanghai` |系统时区设置,默认为Asia/Shanghai|
| `-e WEBUIPORT=8989` |web访问端口环境变量|
| `-e UID=0` |uid设置,默认为0|
| `-e GID=0` |gid设置,默认为0|
| `-e UMASK=022` |umask设置,默认为022|

### 群晖docker设置：

1. 卷

|参数|说明|
|-|:-|
| `本地文件夹1:/Downloads` |qBittorrent下载位置|
| `本地文件夹2:/config` |qBittorrent配置文件位置|

2. 端口

|参数|说明|
|-|:-|
| `本地端口1:6881` |BT下载监听端口|
| `本地端口2:6881/udp` |BT下载DHT监听端口|
| `本地端口3:8989` |web访问端口 [IP:8989](IP:8989);(默认用户名:admin;默认密码:adminadmin);此端口需与容器端口和环境变量保持一致，否则无法访问|

3. 环境变量：

|参数|说明|
|-|:-|
| `TRACKERSAUTO=YES` |自动更新qBittorrent的trackers,默认开启此功能|
| `TZ=Asia/Shanghai` |系统时区设置,默认为Asia/Shanghai|
| `WEBUIPORT=8989` |web访问端口环境变量|
| `UID=0` |uid设置,默认为0|
| `GID=0` |gid设置,默认为0|
| `UMASK=022` |umask设置,默认为022|

### 搜索：

#### 开启：视图-搜索引擎:
##### 说明：

1. 自带 [http://plugins.qbittorrent.org/](http://plugins.qbittorrent.org/) 部分搜索插件
2. 全新安装默认只开启官方自带搜索插件。其它可到 视图-搜索引擎-界面右侧搜索-搜索插件-启动栏(双击)开启
3. 一些搜索插件网站需过墙才能用
4. jackett搜索插件需配置jackett.json(位置config/qBittorrent/data/nova3/engines)，插件需配合jackett服务的api_key。可自行搭建docker版jackett(例如linuxserver/jackett)。

### 其它:

1. Trackers只有一个工作,新增的Trackers显示还未联系，需在qBittorrent.conf里[Preferences]下增加Advanced\AnnounceToAllTrackers=true。
