# 命令

在终端或命令行窗口中执行 ```go``` 命令，默认会打印 Go 的基本命令：

```bash
$ go           
Go is a tool for managing Go source code.

Usage:

	go <command> [arguments]

The commands are:

	bug         start a bug report
	build       compile packages and dependencies
	clean       remove object files and cached files
	doc         show documentation for package or symbol
	env         print Go environment information
	fix         update packages to use new APIs
	fmt         gofmt (reformat) package sources
	generate    generate Go files by processing source
	get         add dependencies to current module and install them
	install     compile and install packages and dependencies
	list        list packages or modules
	mod         module maintenance
	work        workspace maintenance
	run         compile and run Go program
	test        test packages
	tool        run specified go tool
	version     print Go version
	vet         report likely mistakes in packages

Use "go help <command>" for more information about a command.

Additional help topics:

	buildconstraint build constraints
	buildmode       build modes
	c               calling between Go and C
	cache           build and test caching
	environment     environment variables
	filetype        file types
	go.mod          the go.mod file
	gopath          GOPATH environment variable
	gopath-get      legacy GOPATH go get
	goproxy         module proxy protocol
	importpath      import path syntax
	modules         modules, module versions, and more
	module-get      module-aware go get
	module-auth     module authentication using go.sum
	packages        package lists and patterns
	private         configuration for downloading non-public code
	testflag        testing flags
	testfunc        testing functions
	vcs             controlling version control with GOVCS

Use "go help <topic>" for more information about that topic.
```

Go 命令的基本格式：

```bash
$ go <command> [arguments]
```

| 命令 | 说明 |
| -- | -- |
| bug | 开启 bug 报告 |
| build | 构建编译包和依赖项（编译 Go 源文件） |
| clean | 清除删除对象文件和缓存文件 |
| doc | 查看文档 |
| env | 查看 Go 环境信息 |
| fix | 修复包 |
| fmt | 格式化源文件中的代码 |
| generate | 按处理源生成 Go 文件 |
| get | 添加当前模块中的依赖项并安装它们 |
| install | 安装编译和安装包和依赖项 |
| list | 列出包或模块列表 |
| mod | 操作 Go 模块 |
| work | 管理 Go 工作空间 |
| run | 编译并运行 Go 程序 |
| test | 测试 Go 程序 |
| tool | Go 工具 |
| version | 查看 Go 的版本 |
| vet | 审查报告包中可能的错误 |

使用 ```go help <command>``` 可以查看对应命令的详细使用信息。