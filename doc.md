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

## Web 数据响应
Web 的响应与请求结构是类似的，响应分为三个部分：响应行、响应头部、响应体。

响应行：协议、响应状态码和状态描述，如： HTTP/1.1 200 OK
响应标头：包含各种头部字段信息，如 cookie，Content-Type 等头部信息。
响应体：携带客户端想要的数据，格式与编码由头部的 Content-Type 决定。
响应状态码的有固定取值和意义：

100~199：表示服务端成功客户端接收请求，要求客户端继续提交下一次请求才能完成整个处理过程。
200~299：表示服务端成功接收请求并已完成整个处理过程。最常用就是：200
300~399：为完成请求，客户端需进一步细化请求。比较常用的如：客户端请求的资源已经移动一个新地址使用 302 表示将资源重定向，客户端请求的资源未发生改变，使用 304，告诉客户端从本地缓存中获取。
400~499：客户端的请求有错误，如：404 表示你请求的资源在 web 服务器中找不到，403 表示服务器拒绝客户端的访问，一般是权限不够。
500~599：服务器端出现错误，最常用的是：500

## http.ServeMux 优缺点
* http.ServeMux 的局限性
http.ServeMux 在 goblog 中使用，会遇到以下几个问题：

不支持 URI 路径参数
不支持请求方法过滤
不支持路由命名
* http.ServeMux 的优缺点
优点
标准库意味着随着 Go 打包安装，无需另行安装
测试充分
稳定、兼容性强
简单，高效

缺点
缺少 Web 开发常见的特性
在复杂的项目中使用，需要你写更多的代码
开发效率和运行效率，永远是对立面。
Go 标准库选择 运行效率 高于 开发效率
事实上，标准库最大的优点是 Go 自带。


## CURL
* get 请求 curl http://localhost:3000/articles                              
* post 请求 curl -X POST http://localhost:3000/articles

## 为什么不选择 HttpRouter？
* HttpRouter 是目前来讲速度最快的路由器，且被知名框架 Gin 所采用。
不选择 HttpRouter 的原因是其功能略显单一，没有路由命名功能，不符合我们的要求。
HttpRouter 和 Gin 比较适合在要求高性能，且路由功能要求相对简单的项目中，如 API 或微服务。在全栈的 Web 开发中，gorilla/mux 在性能上虽然有所不及，但是功能强大，比较实用

## 安装 gorilla/mux
```
go env -w GOPROXY=https://goproxy.cn
go mod init
go get -u github.com/gorilla/mux
```

## gorilla/mux 的路由解析采用的是 精准匹配 规则，而 net/http 包使用的是 长度优先匹配 规则。
* 精准匹配 指路由只会匹配准确指定的规则，这个比较好理解，也是较常见的匹配方式。
* 长度优先匹配 一般用在静态路由上（不支持动态元素如正则和 URL 路径参数），优先匹配字符数较多的规则。
* 使用 长度优先匹配 规则的 http.ServeMux 会把除了 /about 这个匹配的以外的所有 URI 都使用 defaultHandler 来处理。
* 而使用 精准匹配 的 gorilla/mux 会把以上两个规则精准匹配到两个链接，/ 为首页，/about 为关于，除此之外都是 404 未找到
* 一般 长度优先匹配 规则用在静态内容处理上比较合适，动态内容，例如我们的 goblog 这种动态网站，使用 精准匹配 会比较方便。

## 