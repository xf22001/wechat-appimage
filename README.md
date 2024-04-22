# 微信 AppImage

使用 AppImage 运行 Linux 原生微信

- 无需安装
- 规避原微信的不可卸载特性
- 无需更改系统的 `lsb-release` 等 `/etc` 内的文件
- 无需 sudo
- 限制读写目录，以防在操作系统里到处乱放文件

## 使用前提

- 需要安装 bwrap（常见发行版都有）
- 需要有 XDG 用户目录（常见发行版都有）
  - 查看 `~/.config/user-dirs.dirs`（必须有这个文件）
  - 里面要有 `XDG_DESKTOP_DIR`、`XDG_DOWNLOAD_DIR` 和 `XDG_DOCUMENTS_DIR`
  - 运行时，在用户目录下，微信只能读写这三个目录、微信数据库文件（放在 `${XDG_DOCUMENTS_DIR}/xwechat_files` 下，原生微信是 `~/xwechat_files`）、微信配置文件（`~/.xwechat`）和字体配置文件。

## 获取 AppImage 文件

### 从 release 下载

在本仓库 release 下载 x86_64 版，该版本基于[吾爱破解](https://www.52pojie.cn/thread-1896902-1-1.html)给出的 238 版构建。下载后记得给执行权限。

### 自行构建

以下构建方式仅测试过 238 的 x86_64 版。

下载本仓库，同时下载一个微信 Linux 版本的 deb 包。

解包 deb，移植文件：

```bash
# pwd 为仓库根目录
dpkg -X your_wechat.deb /tmp/out
cp -r /tmp/out/opt src         # 大部分文件
cp -r /tmp/out/usr/lib src/usr # 应该只有 libactivation.so
```

安装 appimagetool（自己上网搜）

打包 appimage：

```bash
# pwd 为仓库根目录上一级
appimagetool ./wechat-appimage
```

可以取得 `wechat-x86_64.AppImage`

## 运行方法

已经测试在 Debian 和 kali 运行过。

注：实际测试中有极少时候第一次扫码之后没反应，此时可以关闭重新启动扫码。

### 简单运行

直接双击此文件或者命令行启动即可。

### 安装运行

```bash
sudo mv wechat-x86_64.AppImage /usr/bin/wechat
sudo wechat --install # 安装图标
```

然后你可以在桌面添加类似的启动器图标。

### 更多功能

```bash
# 帮助
wechat-x86_64.AppImage --help
# 进入 bwrap 的 shell(bash)
wechat-x86_64.AppImage --debug
# 关闭微信进程（早期版本无法退出，现在应该没了）
wechat-x86_64.AppImage --kill
# 安装图标
sudo wechat-x86_64.AppImage --install
```

## 声明

本仓库仅供学习交流使用，且未经过严格测试，使用本仓库造成的一切后果由使用者负责。

## 参考

参考：[依云's Blog - 使用 bwrap 沙盒](https://blog.lilydjwg.me/2021/8/12/using-bwrap.215869.html)

参考：[wechat-beta-bwrap](https://github.com/lfift/wechat-beta-bwrap)
