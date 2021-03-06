# 轻量易用的 Aria2 下载机

此镜像采用 Linuxserver 维护的 alpine 基础镜像构建，使用 [AriaNg](https://github.com/mayswind/AriaNg) 作为 Aria2 的前端管理界面，集成超级轻量的 lighthttpd 网页服务器提供文件索引页面。

预置了 dht.dat 和 dht6.dat 数据包，分别对应 IPv4 和 IPv6 环境的 DHT 网络连接优化。默认未启用 IPv6 支持，如有需要可以手动修改配置文件。

### 创建容器

请使用实际的路径替换以下命令中的 `/DOWNLOAD_DIR` 和 `/CONFIG_DIR`，他们将容器中的下载目录
和配置文件目录映射到你指定的主机路径上，以便于管理下载的文件和 Aria2 的配置。

```
sudo docker run -d \
--name aria2 \
-p 6800:6800 \
-p 6880:80 \
-p 6888:8080 \
-p 51443:51443 \
-p 51443:51443/udp \
-v /DOWNLOAD_DIR:/data \
-v /CONFIG_DIR:/config \
-e SECRET=YOUR_SECRET_CODE \
getnas/aria2
```

- 如果不需要浏览下载目录，则去掉 `-p 6888:8080` 参数。
- 如果希望 aria2 容器随 docker 自动运行，则添加 `--restart=always` 参数。
- 请用随意的一组字符串替换 `YOUR_SECRET_CODE`，在浏览器中管理 aria2 时需要用这个安全代码认证身份，以防他人恶意使用。
- 51443 端口的 TCP 和 UDP 分别监听 BT 和 Tracker 连接。

### 指定用户和组

改用 LinuxServer 的 alpine 基础镜像，可以使用环境变量 `PUID` 和 `PGID` 指定容器和主机
之间映射的用户权限。

### 使用方法

* `http://serverip:6880` 打开 web 管理界面
* `http://serverip:6888` 浏览下载目录

### 涉及的项目

* [Aria2](https://github.com/aria2/aria2)
* [AriaNg](https://github.com/mayswind/AriaNg)


### 特别鸣谢

本项目是在 [aria2-with-webui](https://github.com/XUJINKAI/aria2-with-webui) 的基础上构建而来的，感谢作者 [XUJINKAI](https://github.com/XUJINKAI)。

本项目中的 aria2 配置文件参考了 [P3TERX/aria2.conf](https://github.com/P3TERX/aria2.conf) 项目，感谢作者 [@P3TERX](https://github.com/P3TERX)。