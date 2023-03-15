# 安装

Go 官方提供的二进制发行版支持 ```Windows```、```MacOS```、```Linux```、```FreeBSD``` 这四种操作系统，如果在你的操作系统上没有可用的二进制发行版，请尝试从源码安装 Go。

选择下载适合您系统的安装包：

| 操作系统 | 架构 | 安装包 |
| ---- | -- | ------ |
| Windows 7 或更高版本 | amd64 | [go1.20.windows-amd64.msi](https://go.dev/dl/go1.20.windows-amd64.msi) |
| macOS 11 或更高版本 |	ARM64 | [go1.20.darwin-arm64.pkg](https://go.dev/dl/go1.20.darwin-arm64.pkg) |
| macOS 10.13 或更高版本 | amd64 | [go1.20.darwin-amd64.pkg](https://go.dev/dl/go1.20.darwin-amd64.pkg) |
| Linux 2.6.32 或更高版本 | amd64 | [go1.20.linux-amd64](https://go.dev/dl/go1.20.linux-amd64.tar.gz) |

如果您想要下载其他版本的 Go 安装包，请前往[官网下载](https://golang.google.cn/dl)。

## 在 ```Windows``` 系统中安装

打开您下载的 ```MSI``` 文件并按照提示安装 Go。

默认情况下，安装程序会将 Go 安装到 ```Program Files``` 或 ```Program Files (x86)```，您可以根据需要更改位置。

确认您已安装 Go。在 Windows 中，单击 ```开始``` 菜单，在菜单的搜索框中，键入 ```cmd```，然后按 ```Enter``` 键，在出现的命令提示符窗口中，执行以下命令：

```go
$ go version
```

如果该命令打印了已安装的 Go 版本，说明安装成功。

## 在 ```MacOS``` 系统中安装

打开您下载的包文件，按照提示安装 Go。

该软件包将 Go 发行版安装到 ```/usr/local/go``` 目录下。该安装包会将 ```/usr/local/go/bin``` 目录放入您的 ```PATH``` 环境变量中。您可能需要重新启动所有打开的终端会话才能使更改生效。

通过打开命令提示符并键入以下命令来验证您是否已安装 Go：

```go
$ go version
```

如果该命令打印了已安装的 Go 版本，说明安装成功。

## 在 ```Linux``` 系统中安装

如果存在 ```/usr/local/go``` 文件夹，请使用 ```rm``` 命令来删除该文件夹，然后将刚刚下载的压缩包解压缩到 ```/usr/local``` 目录下，使用 ```tar``` 命令解压后会自动在 ```/usr/local/go``` 中创建一个新的 Go 树，请执行如下命令：

```go
$ rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.linux-amd64.tar.gz
```

然后将 ```/usr/local/go/bin``` 添加到 ```PATH``` 环境变量中。您可以通过将以下行添加到 ```$HOME/.profile``` 或 ```/etc/profile``` 来执行此操作：

```go
export PATH=$PATH:/usr/local/go/bin
```

::: warning 注意
在您下次登录计算机之前，对配置文件所做的更改可能不会应用。要立即应用更改，请在终端中执行命令 ```source $HOME/.profile``` 或者 ```source /etc/profile```。
:::

通过打开命令提示符并键入以下命令来验证您是否已安装 Go：

```go
$ go version
```

如果该命令打印了已安装的 Go 版本，说明安装成功。