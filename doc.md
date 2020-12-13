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

## gorilla/mux 使用指南
* 指定 Methods () 来区分请求方法;在 Gorilla Mux 中，如未指定请求方法，默认会匹配所有方法。
* 请求路径参数和正则匹配 
```
router.HandleFunc("/articles/{id:[0-9]+}", articlesShowHandler).Methods("GET").Name("articles.show")
{id:[0-9]+}
使用 {name} 花括号来设置路径参数
在有正则匹配的情况下，使用 : 区分。第一部分是名称，第二部分是正则表达式
限定了 一个或者多个的数字。如果你访问非数字的 ID ，如 localhost:3000/articles/string 即会看到 404 页面。

func articlesShowHandler(w http.ResponseWriter, r *http.Request) {
    vars := mux.Vars(r)
    id := vars["id"]
    fmt.Fprint(w, "文章 ID："+id)
}

Mux 提供的方法 mux.Vars(r) 会将 URL 路径参数解析为键值对应的 Map，使用以下方法即可读取：
vars["id"]
```
* 命名路由与链接生成
Name() 方法用来给路由命名，传参是路由的名称，接下来我们就可以靠这个名称来获取到 URI：
```
router.HandleFunc("/", homeHandler).Methods("GET").Name("home")
router.HandleFunc("/articles/{id:[0-9]+}", articlesShowHandler).Methods("GET").Name("articles.show")
```
## go module 
[学习](https://learnku.com/courses/go-basic/1.15/dependency-management-go-module/9279)
### 使用原因
* Go Modules 是 Go 语言的代码依赖管理工具。类似于 PHP 中的 Composer、Node.js 中的 npm 。
* Go Modules 由官方维护。自 Go 版本 1.14 开始，官方鼓励所有用户迁移到 Go Modules 以进行依赖项管理。
* 弃用 $GOPATH
Go Modules 出现的目的之一就是为了解决 GOPATH 的问题。
在 $GOPATH 时代，Go 源码必须放置于 $GOPATH/src 下，抛弃 $GOPATH 的好处，是你能在任意地方创建的 Go 项目。
另外，$GOPATH 有非常落后的依赖管理系统。因在执行 go get 时，无法传达任何版本信息。
在构建 Go 应用程序上，我们无法保证其它人与你所期望依赖的第三方库是相同的版本（相同的代码），也就是说无法保证所有人的依赖版本都一致。
### go module 日常使用
* go mod init 初始化 生成 go.mod
* go env -w GOPROXY=https://goproxy.cn 使用 go env -w 来修改 go 相关的环境变量
* go get github.com/julienschmidt/httprouter 安装 httprouter
* 每一次的 go get 都会同时修改 go.mod 和 go.sum 文件。
这两个文件是下载依赖包的主要依据。go.mod 类似于 PHP 中的 composer.json ，而 go.sum 则是 composer.lock。
* 几个参数：
        1. module —— 我们的 goblog 在 Go Module 里也算是一个 Module ；
        2. go —— 指定了版本要求，最低 1.15
        3. require —— 是项目所需依赖
* go.sum 文件保存着依赖包的版本和哈希值
* 需要注意的是，go.sum 里不止会保存直接依赖包的哈希值，间接依赖包的哈希值也会被保存。
* go get github.com/gin-gonic/gin
* 每个模块路径有如下两种哈希：
```
github.com/gorilla/mux v1.7.4 h1:VuZ8uybHlWmqV03+zRzdwKL4tUnIp1MAQtp1mIFE1bc=
github.com/gorilla/mux v1.7.4/go.mod h1:DVbg23sWSpFRCP0SfiEN6jmj59UnW/n46BH5rLB71So=
```
前者为 Go Modules 打包整个模块包文件 zip 后再进行 hash 值，而后者为针对 go.mod 的 hash 值。
由此可见，go.sum 是保证所下载源码 100% 正确的重要依据。如果有恶意用户，将某个 Git 项目的 tag 源码做了修改，这些哈希值将会不匹配并报错。
因为 go.sum 有 100% 保证 build 一致的作用，我们建议开发中将其加入到代码版本控制器中。这里面不止有安全的因素，当同事或者其他人 clone 你的代码，我们也希望代码可以保持一致。
* 注释 indirect 此标志标明这个依赖包还未被使用，如果你在代码的某个地方 import 到的话，VSCode 的 Go 插件就会自动将这个标志去除
* go mod tidy 此命令做整理依赖使用,执行时候会把未使用的 module移除掉
* go clean -modcache 清空本地下载的 go modules 缓存
* 下载依赖,默认情况下 go run 和 go build命令执行的时候,go 会基于go.mod自动拉取依赖,主动拉取依赖:go mod download
* go modules 命令
```
go mod init     生成go.mod 和 go.sum 文件
go mod download 下载 go.mod 中指明的所有依赖
go mod tidy     整理现有依赖
go mod graph    查看现有依赖结构
go mod edite    编辑 go.mod 文件
go mod vendor   导出项目所有依赖到 vendor目录
go mod verify   校验一个模块是否被篡改过
go mod why      查看为什么要依赖某模块
```
### 相关环境变量

* GO111MODULE 因是在 Go1.11 版本添加，故命名为 GO111MODULE。
```
auto：项目包含了 go.mod 文件的话启用 Go modules，目前在 Go1.11 至 Go1.15 中仍然是默认值。
on：启用 Go modules，推荐设置，将会是未来版本中的默认值。
off：禁用 Go modules，不推荐设置。
```
* GOPROXY 此变量用于设置 Go 模块代理（Go module proxy),其作用是拉取源码时能够脱离传统的 VCS 方式，直接通过镜像站点来快速拉取。将其设置为 off ，将会禁止 Go 在后续操作中使用任何 Go 模块代理。 * * * GOPROXY 的值是一个以英文逗号 , 分割的 Go 模块代理列表，可设置多个模块代理。
* direct 标志,意味着从源地址抓取,https://goproxy.cn,direct:则告诉 go get 在获取源码包时先尝试 https://goproxy.cn，如果遇到 404 等错误时，再尝试从源地址抓取。

* GOSUMDB 此值是 Go Checksum Database 的缩写，用于在拉取模块版本时（无论是从源站拉取还是通过 Go Module Proxy 拉取）保证拉取到的模块代码包未经过篡改，若发现不一致将会立即中止。
* GOSUMDB="sum.golang.org" 默认值;在国内同样无法访问，所幸 GOSUMDB 可以被 Go Module Proxy 代理。我们所设置的模块代理 goproxy.cn 支持代理 sum.golang.org。另外，此变量还可设置为 off，会禁止 Go 在后续操作中校验模块哈希。

*  GONOPROXY/GONOSUMDB/GOPRIVATE
```
这三个环境变量都是用在依赖了私有模块，这些模块 GOPROXY 和 GOSUMDB 都无法读取。
GONOPROXY —— 设置不走 Go Proxy 的 URL 规则；
GONOSUMDB —— 设置不检查哈希的 URL 规则；
GOPRIVATE —— 设置私有模块的 URL 规则，会同时设置以上两个变量。
因为 GOPRIVATE 会同时设定以上两个，所以一般私有仓库使用 GOPRIVATE 即可。
以上三个值，都可使用逗号分隔来设置多个选项。
go env -w GOPRIVATE="git.example.com,github.com/name/project"
设置后当 go get 时，前缀为 git.example.com 和 github.com/name/project 的模块都会被认为是私有模块。
利用通配符
go env -w GOPRIVATE="*.example.com"
这样子设置的话，所有模块路径为 example.com 的子域名（例如：git.example.com）都将不经过 Go module proxy 和 Go checksum database，需要注意的是不包括 example.com 本身。
```

## 中间件
* Gorilla Mux 的 mux.Use() 方法来加载中间件
```
// 中间件:强制内容类型为 html
router.Use(forceHTMLMiddleware)

func forceHTMLMiddleware(h http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		// 设置标头
		w.Header().Set("Content-type", "text/html;charset=utf-8")
		// 继续处理请求
		h.ServeHTTP(w, r)
	})
}
```

## 301
* 当请求方式为 POST 的时候，遇到服务端的 301 跳转，将会变成 GET 方式。很明显，这并非所愿，我们需要一个更好的方案。

## strins.TrimSuffix()
*  strings 包提供的 TrimSuffix(s, suffix string) string 函数来移除 / 后缀，如果不带斜杆后缀的话，r.URL.Path 将会被原封不动地返回

## 变量作用域
* 函数和函数直接的变量是不可见的
* 如果两个函数要同时用一个变量,那么可以用包级别的变量来解决
* router := mux.NewRouter() // syntax error: non-declaration statement outside function body    语法错误：函数外无法使用变量赋值语句
原因是包级别的变量声明时不能使用 := 语法，修改为带关键词 var 的变量声明即可：
改正: var router = mux.NewRouter()

## ParseForm PostForm Form FormValue() PostFormValue
* r.ParseForm() 由 http 包提供，从请求中解析请求参数，必须是执行完这段代码，后面 r.PostForm 和 r.Form 才能读取到数据，否则为空数组。
```
err := r.ParseForm()
r.PostForm.Get("title")
r.PostForm 存储了 post put 参数,在使用之前要调用 ParseForm()解析
r.Form Form 存储了 post put get参数,在使用之前用调用 ParseForm()解析
r.Form 比 r.PostForm 多了 URL 参数里的数据。
// 如果不想全部获取的话,而是逐个获取,可以不用 ParseForm(),直接使用r.FormValue()和r.PostFormValue()方法
```

## 字符串长度计数
* 命名是两个中文汉字，使用 len() 函数计数却为 6 个。
* Go 语言的内建函数 len ()，可以用来获取切片、字符串、通道（channel）等的长度。
* 这里的差异是由于 Go 语言的字符串都以 UTF-8 格式保存，每个中文占用 3 个字节，因此使用 len () 获得两个中文文字对应的 6 个字节。
* 如果希望按习惯上的字符个数来计算，就需要使用 Go 语言中 utf8 包提供的 RuneCountInString () 函数来计数。

## 错误处理
* 在 Go 中，一般 err 处理方式可以是给用户提示或记录到错误日志里，这种很多时候为 业务逻辑错误。当有重大错误，或者系统错误时，例如无法加载模板文件，就使用 panic() 。
 
## template包的使用
* 解析 html 变量
```
tmpl, err := template.New("create-form").Parse(html)
tmpl.Execute(w, data)
```
* 解析文件
```
tmpl, err := template.ParseFiles("resources/views/articles/create.gohtml")
if err != nil {
        panic(err)
}
tmpl.Execute(w, data)
```

## html/template 语法
* 发生错误时，也就是 errors 的长度大于零时，我们会把 errors 传参到 HTML 中进行渲染。Go 标准库的 html/template，就是专门为这种场景所设计的
* 双层大括号 {{ }} 是默认的模板界定符。用于在 HTML 模板文件中界定模板语法。模板语法都包含在 {{和}} 中间。
* {{.}} 语句 {{ . }} 中的点表示当前对象。当我们传入一个结构体对象时，我们可以使用 . 来访问结构体的对应字段。同理，当我们传入的变量是 map 时，也可以在模板文件中通过 . 根据 key 来取值。
* with 关键字
```
{{ with pipeline }} T1 {{ end }}
如果 pipeline 为空则不产生输出，否则将 . 设为 pipeline 的值并执行 T1。不修改外面的 .
{{ with pipeline }} T1 {{ else }} T0 {{ end }}
如果 pipeline 为空则不改变 . 并执行 T0，否则 . 设为 pipeline 的值并执行 T1。
with 区块外，{{ . }} 代表传入模板的数据，而在 with 区块内，则代表 pipline 里的数据。
如 {{ with .Errors.title }} 这个区块内，{{ . }} 代表 .Errors.title。

pipeline
pipeline 是指产生数据的操作。比如 {{ . }}、{{ .Name }} 等。Go 的模板语法支持使用管道符号 | 连接多个命令，用法和 Unix 下的管道类似 —— | 前面的命令会将运算结果 (或返回值) 传递给后一个命令的最后一个位置。
注意： 并不是只有使用了 | 才是 pipeline。Go 的模板中，pipeline 的概念是传递数据，只要能产生数据的，都是 pipeline。
```
* 注释  注释，执行时会忽略。可以多行。注释不能嵌套，且须紧贴分界符。`{{/* 这是一个注释 */}}`
* 变量  我们还可以在模板中声明变量，用来保存传入模板的数据或其他语句生成的结果。具体语法如下： `$variable := {{ . }}`;其中 $variable 是变量的名字，在后续的代码中就可以使用该变量了。
* 移除空格 有时会不可避免的引入空格或者换行符，导致模板最终渲染结果不符预期。这种情况可以使用 {{- 语法去除模板内容左侧的所有空白符号， 使用 -}} 去除模板内容右侧的所有空白符号。
```
例如：
{{- .Name -}}
注意： - 要紧挨 {{和}}，同时与模板值之间需要使用空格分隔。
```
* 条件判断 
```
{{if pipeline}} T1 {{end}}
{{if pipeline}} T1 {{else}} T0 {{end}}
{{if pipeline}} T1 {{else if pipeline}} T0 {{end}}
```
* range 遍历
```
range 关键字用以在模板里遍历数据，有以下两种写法，其中 pipeline 的值必须是数组、切片、字典或者通道。
{{range pipeline}} T1 {{end}}
如果 pipeline 的值其长度为 0，不会有任何输出

{{range pipeline}} T1 {{else}} T0 {{end}}
如果 pipeline 的值其长度为 0，则会执行 T0。
```
* 修改默认的标识符
```
Go 标准库的模板引擎使用的花括号 {{和}} 作为标识，而许多前端框架（如 Vue 和 AngularJS）也使用 {{和}} 作为标识符，所以当我们同时使用 Go 语言模板引擎和以上前端框架时就会出现冲突，这个时候我们需要修改标识符，修改前端的或者修改 Go 语言的。这里演示如何修改 Go 语言模板引擎默认的标识符：
template.New("test").Delims("{[", "]}").ParseFiles("filename.gohtml")
```

## MySQL 驱动
* 一是利用 database/sql 接口，直接在代码里硬编码 sql 语句；
* 二是使用 ORM，具体一点是 GORM，以对象关系映射的方式在抽象地操作数据库。

### 安装驱动
* go get github.com/go-sql-dirver/mysql
* 注意导入 mysql 驱动时，在包路径前我们添加 _，这里使用了匿名导入的方式来加载驱动
```
 _ "github.com/go-sql-driver/mysql"
```
* 为什么需要匿名导入？
```
因为引入的是驱动，操作数据库时我们使用的是 sql 库里的方法，而不会具体使用到 go-sql-driver/mysql 包里的方法，当有未使用的包被引入时，Go 编译器会停止编译。为了让编译器能正常运行，需要使用 匿名导入 来加载。

当导入了一个数据库驱动后，此驱动会自行初始化（利用 init() 函数）并注册自己到 Golang 的 database/sql 上下文中，驱动里的 init() 代码如下：
func init() {
    sql.Register("mysql", &MySQLDriver{})
}
```
### mysql 使用
* sql.DB 结构体是 database/sql 包封装的一个数据库操作对象，包含了操作数据库的基本方法，通常情况下，我们把它理解为 连接池对象。 var db *sql.DB
* DSN 信息 Data Source Name，表示 数据源信息，用于定义如何连接数据库。不同数据库的 DSN 格式是不同的，这取决于数据库驱动的实现，下面是 go-sql-driver/sql 的 DSN 格式，如下所示：
```
//[用户名[:密码]@][协议(数据库服务器地址)]]/数据库名称?参数列表
[username[:password]@][protocol[(address)]]/dbname[?param1=value1&...&paramN=valueN]
```
* FormatDSN() 是 mysql.Config 提供的用来生成 DSN 信息的方法，我们可以尝试把其生成的信息打印出来：
* sql.DB 连接池
```
一般而言，我们使用 sql.Open() 函数便可初始化并返回一个 *sql.DB 结构体实例，使用 sql.Open() 函数只要传入驱动名称及对应的 DSN 便可，使用很简单，也很通用：

//driverName  表示驱动名，如 mysql, dataSourceName 为上文介绍的 DSN
func Open(driverName, dataSourceName string) (*sql.DB, error)
```
* 当需要连接不同数据库时，只需修改驱动名与 DSN 即可。
* 需要特别注意的是，调用 sql.Open() 时，并未开始连接数据库，只是为连接数据库做好准备而已。所以一般我们都会跟着一个 db.Ping() 来检测连接状态。

* SetMaxOpenConns 最大连接数
```
设置连接池最大打开数据库连接数，<= 0 表示无限制，默认为 0。
问：应该设置多大呢？
实验表明，在高并发的情况下，将值设为大于 10，可以获得比设置为 1 接近六倍的性能提升。而设置为 10 跟设置为 0（也就是无限制），在高并发的情况下，性能差距不明显。
问：是否越大越好？
需要考虑的是不要超出数据库系统设置的最大连接数。
show variables like 'max_connections';
```
* SetMaxIdleConns 空闲连接数
```
设置连接池最大空闲数据库连接数，<= 0 表示不设置空闲连接数，默认为 2。
实验表明，在高并发的情况下，将值设为大于 0，可以获得比设置为 0 超过 20 倍的性能提升。
这是因为设置为 0 的情况下，每一个 SQL 连接执行任务以后就销毁掉了，执行新任务时又需要重新建立连接。很明显，重新建立连接是很消耗资源的一个动作。
设置空闲连接数，当有新任务进来时，直接使用这些随时待命的连接传输数据，以此达到节约资源，提高执行效率的目的。
问：是不是数值越大越好？
首先此值不能大于 SetMaxOpenConns 的值，大于的情况下 mysql 驱动会自动将其纠正。
其次需要考虑的是，长时间打开大量的数据库连接需要占用系统的内存和 CPU 资源。
还有一个情况是 MySQL 会有一个 wait_timeout 的设置，连接超过这个时间就会被自动关闭，默认情况下是 8 个小时。当 MySQL 关闭连接时，sql.DB 请求到的就是一个坏的连接，虽然 sql 包里已经做了处理，当请求到坏连接时会自动重连。但是在这种情况下，单次请求相当于建立了两次连接，消耗比设置为 0 还大，得不偿失。
所以回答上面的问题，不是越大越好，应根据实际情况选择合理的值。
```
* SetConnMaxLifetime 过期时间
```
设置连接池里每一个连接的过期时间，过期会自动关闭。理论上来讲，在并发的情况下，此值越小，连接就会越快被关闭，也意味着更多的连接会被创建。
设置的值不应该超过 MySQL 的 wait_timeout 设置项（默认情况下是 8 个小时）。
此值也不宜设置过短，关闭和创建都是极耗系统资源的操作。
设置此值时，需要特别注意 SetMaxIdleConns 空闲连接数的设置。假如设置了 100 个空闲连接，过期时间设置了 1 分钟，在没有任何应用的 SQL 操作情况下，数据库连接每 1.6 秒就销毁和新建一遍。
这里的推荐，比较保守的做法是设置五分钟：
```

### 常用
* Exec 方法 `_, err := db.Exec(createArticlesSQL)`
```
我们使用 Exec() 来执行创建数据库表结构的语句。
一般使用 sql.DB 中的 Exec() 来执行没有返回结果集的 SQL 语句。例如 INSERT, UPDATE, DELETE 等语句。语法如下：
func (db *DB) Exec(query string, args ...interface{}) (Result, error)
Exec() 方法的第一个返回值为一个实现了 sql.Result 接口的类型，sql.Result 的定义如下：
type Result interface {
    LastInsertId() (int64, error)    // 使用 INSERT 向数据插入记录，数据表有自增 id 时，该函数有返回值
    RowsAffected() (int64, error)    // 表示影响的数据表行数
}
我们可以用 sql.Result 中的 LastInsertId() 方法或 RowsAffected() 来判断 SQL 语句是否执行成功。
因为我们执行的是 CREATE TABLE IF NOT EXISTS 语句，会被重复执行，所以这里判断返回结果意义不大，主要判断返回的第二个参数 err 是否有问题。
```
* 在数据库安全方面，Prepare 语句是防范 SQL 注入攻击有效且必备的手段。
```
stmt, err = db.Prepare("INSERT INTO articles (title, body) VALUES(?,?)")
```



## 数据库设置
* `CREATE DATABASE goblog CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`
* 编码使用 utf8mb4_unicode_ci 是为了支持存储 Emoji，这在现代化的应用中是必要的。另外支持大小写不敏感（ci 是 Case Insensitive 的缩写）。


## strconv.FormatInt()
* strconv.FormatInt(lastInsertID, 10)
这里我们使用到了 FormatInt() 方法来将类型为 int64 的 lastInsertID 转换为字符串。此方法的第二个参数 10 为十进制

## 多变量声明方式
```
多变量声明的方式与引入多个包使用 import(...) 同出一辙，都是 Go 语言为了让开发者少写代码而提供的简写方式。
// 变量初始化
var (
    id   int64
    err  error
    rs   sql.Result
    stmt *sql.Stmt
)
```

