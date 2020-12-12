# goblog 日记

## main包 main函数
* 每一段 Go 程序都 必须 属于一个包。而 main 包在 Go 程序中有特殊的位置。
* 一个标准的可执行的 Go 程序必须有 package main 的声明。
* 如果一段程序是属于 main 包的，那么当执行 go install 或者 go run 时就会将其生成二进制文件，当执行这个文件时，就会调用 main 函数。
* main 包里的 main 函数相当于应用程序的入口。要想生成可执行的二进制文件，必须把代码写在 main 包里，而且其中必须包含一个 main 函数。
*  存放 main 函数的文件名称不一定是 main.go，也可以是任何其他合规的 go 文件名称，例如 app.go、index.go。一般推荐使用 main.go，因为直观。

## go 标准库
以下是 Go 标准库常见的包以及功能介绍：

标准库包名	功能简介
bufio	带缓冲的 I/O 操作
bytes	实现字节操作
container	封装堆、列表和环形列表等容器
crypto	加密算法
database	数据库驱动和接口
debug	各种调试文件格式访问及调试功能
encoding	常见算法如 JSON、XML、Base64 等
flag	命令行解析
fmt	格式化操作
go	Go 语言的词法、语法树、类型等。可通过这个包进行代码信息提取和修改
html	HTML 转义及模板系统
image	常见图形格式的访问及生成
io	实现 I/O 原始访问接口及访问封装
math	数学库
net	网络库，支持 Socket、HTTP、邮件、RPC、SMTP 等
os	操作系统平台不依赖平台操作封装
path	兼容各操作系统的路径操作实用函数
plugin	Go 1.7 加入的插件系统。支持将代码编译为插件，按需加载
reflect	语言反射支持。可以动态获得代码中的类型信息，获取和修改变量的值
regexp	正则表达式封装
runtime	运行时接口
sort	排序接口
strings	字符串转换、解析及实用函数
time	时间接口
text	文本模板及 Token 词法器

## go 编译型语言
 * Go 语言为编译型语言，编译型语言有诸多好处，如：
    1. 部署简单
    2. 提早发现错误
    3, 执行效率高
然而这也意味着代码修改后需重新编译才能看到变更，这为我们本地开发带来了诸多不便。

## 自动重载方案
* air [安装](https://learnku.com/courses/go-basic/1.15/automatic-overloading/8944)
```
go env -w  GOPROXY=https://goproxy.cn
GO111MODULE=on  go get -u github.com/cosmtrek/air
air -v
air //启动
go: cannot find main module, but found .git/config in /Users/v_duanjiawei/go/src/github.com/betterDuanjiawei/goblog
        to create a module there, run:
        go mod init
failed to build, error: exit status 1

运行 go mod init
再运行 air
```
* [关于mac安装air后无法找到air命令](https://learnku.com/go/t/51906)
* 请确保 air 命令行时刻处于运行状态

## Content-Type 标头
Content-Type: 响应标头是告知客户端内容的类型，客户端再根据这个信息将内容正确地呈现给用户。

常见的内容类型有：

text/html —— HTML 文档
text/plain —— 文本内容
text/css—— CSS 样式文件
text/javascript —— JS 脚本文件
application/json—— JSON 格式的数据
application/xml —— XML 格式的数据
image/png —— PNG 图片

## 
