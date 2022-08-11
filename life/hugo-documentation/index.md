# hugo documentation




# hugo documentation

<font color="#4194C7">GETTING STARTED</font>

## 基础使用

Hugo的CLI有许多特性，但即使对于不太熟悉代码的人，也可以轻松使用。

<font size=2>接下来将介绍一些在开发Hugo项目中最常用的命令。更完整的介绍见：Command Line Reference</font>>



### 安装测试

请确保Hugo安装在PATH中，你可以用`help`命令检测Hugo是否正确安装：

```shell
hugo help
```

你的命令行输出应该与以下内容相似：

```shell
hugo is the main command, used to build your Hugo site.

Hugo is a Fast and Flexible Static Site Generator
built with love by spf13 and friends in Go.

Complete documentation is available at https://gohugo.io/.

Usage:
  hugo [flags]
  hugo [command]

Available Commands:
  completion  为指定的 shell 生成自动完成脚本
  config      打印站点配置
  convert     将您的内容转换为不同的格式
  deploy      将您的站点部署到云提供商
  env         打印 Hugo 版本和环境信息
  gen         几个有用的生成器的集合
  help        关于任何命令的帮助
  import      从其他人那里导入您的网站
  list        列出各种类型的内容
  mod         各种 Hugo 模块助手
  new         为您的网站创建新内容
  server      高性能网络服务器
  version     打印 Hugo 的版本号

Flags:
  -b, --baseURL string             主地址，比如：http://spf13.com/
  -D, --buildDrafts                包含标记为草稿的最暖死
  -E, --buildExpired               包含过期的内容
  -F, --buildFuture                包含发布日期在未来的内容
      --cacheDir string            缓存的目录路径。默认为: $TMPDIR/hugo_cache/
      --cleanDestinationDir        删除stastic路径中的文件
      --config string              配置文件 (默认为config.yaml|json|toml)
      --configDir string           配置路径 (默认 "config")
  -c, --contentDir string          内容目录的系统路径
      --debug                      调试输出
  -d, --destination string         目标文件写到哪里
      --disableKinds strings       禁用不同类型的页面(home, RSS etc.)
      --enableGitInfo              将 Git 修订、日期、作者和 CODEOWNERS 信息添加到页面
  -e, --environment string         构建环境
      --forceSyncStatic            static有变化时复制所有文件。
      --gc                         启用以在构建后运行一些清理任务（删除未使用的缓存文件）
  -h, --help                       help for hugo
      --ignoreCache                忽略缓存目录
      --ignoreVendorPaths string   忽略与给定 Glob 模式匹配的模块路径的任何 _vendor
  -l, --layoutDir string           布局目录的文件系统路径
      --log                        启用日志记录
      --logFile string             日志文件路径（如果设置，则自动启用日志记录）
      --minify                     缩小任何支持的输出格式（HTML、XML 等）
      --noChmod                    不同步文件的权限模式
      --noTimes                    不同步文件的修改时间
      --panicOnWarning             第一个警告日志出现时panic
      --poll string                将此设置为轮询间隔，例如 --poll 700ms，以使用基于轮询的方法来监视文件系统更改
      --printI18nWarnings          打印缺失的翻译
      --printMemoryUsage           每隔一段时间将内存使用情况打印到屏幕上
      --printPathWarnings          在重复的目标路径等上打印警告
      --printUnusedTemplates       在未使用的模板上打印警告
      --quiet                      以安静模式构建
      --renderToMemory             渲染到内存（仅对基准测试有用）
  -s, --source string              文件系统路径以读取相对于的文件
      --templateMetrics            显示有关模板执行的指标
      --templateMetricsHints       结合使用--templateMetrics时计算一些改进提示 
  -t, --theme strings              要使用的主题（位于 /themes/THEMENAME/）
      --themesDir string           主题目录的文件系统路径
      --trace file                 将跟踪写入文件（通常没有用）
  -v, --verbose                    详细输出
      --verboseLog                 详细日志记录
  -w, --watch                      监视文件系统的更改并根据需要重新创建

Use "hugo [command] --help" for more information about a command.
```



### `hugo`命令

最常用的方法是将当前目录作为输入目录运行`hugo`。

默认情况下，你的网站将生成到`public/`目录，同时你也可以通过更改字段`publishDir`来自定义网站的输出目录。

使用`hugo`命令将渲染你的网站到`public/`目录，此时你便可以将其部署到你的Web服务器：

```shell
hugo
0 draft content
0 future content
99 pages created
0 paginator pages created
16 tags created
0 groups created
in 90 ms
```



### 草稿、未来发布和过期内容

下面三种情况的文章，Hugo不会渲染发布：

1.   指定未来发布时间`pulishdate`
2.   草稿`draft: true`
3.   指定过期时间`expirydata`

上面三种情况可以通过添加指令来覆盖，或者直接改变他们的值：

1.   `--buildFuture`
2.   `--buildDrafts`
3.   `--buildExpired`



### 实时重载LiveReload

Hugo有内建`LiveReload`。可以在运行`hugo server`的同时修改网站并观察一下路径内容的添加、删除或更改：`/static/*` `/content/*` `/data/*` `/i18n/*` `/layouts/*` `/themes/<CURRENT-THEME>/*` `config`，每当有更改，Hugo会重建网站。

#### 自动定位到刚刚更新的页面

`--navigateToChanged`

#### 禁用LiveReload

```shell
hugo server --watch=false
# or
hugo server --disableLiveReload
```



## 目录结构

```shell
.
├── archetypes
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```

`content`：网站的所有内容位于该文件夹中，其中的每个顶级文件夹被视为一个内容部分，比如，如果网站包含三个主要部分--blog、articles和tutorials，那么将会有以下三个目录--content/blog、content/articles、content/tutorials。

`static`：存储所有静态内容：图像、CSS、JavaScript等。



## 配置Hugo

### 配置文件

可以有多个配置文件，通过命令行指定使用

```shell
hugo --config debugconfig.toml
hugo --config a.toml, b.toml, c.toml
```


