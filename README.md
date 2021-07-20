# Samarium

## 序言

Hi, 这里是下一代 `Edgeless` 加载器——`Samarium` 的源码仓库。

这个项目主要由 `Rust`, `C++`, `TypeScript / Rhai(未定)` 语言组成。



## 构想

### 插件包相关：

> 有很多操作允许只给出软件名，需要实现一个根据软件名查找路径的模块

* 下载
* 保存（保存功能与下载分离，下载后默认进一步执行保存；允许只给出软件名，从临时目录复制到U盘）
* 加载（加载功能与下载分离，如果在PE中，下载后默认进一步执行加载；允许只给出软件名，从临时目录或U盘中加载插件包）
* 更新
* 搜索（默认直接使用文本，支持正则表达式，支持同义词和标签搜索）
* 列出（根据软件名/分类名/作者名等条件）
* 删除
* 信息查询（在线版本号，当前版本号等）
* 属性管理（如是否使用LocalBoost，是否被禁用等，注意判断依据需要有鲁棒性）

### 主题包相关：

> 可以和插件包的逻辑通用，更改具体执行逻辑即可

* 下载
* 设置/保存
* 预览/加载
* 更新
* 搜索
* 列出
* 删除
* 信息查询

### 管理相关：

* 镜像源切换（维护一个 [名称：URL] 列表方便使用时快速切换）
* 索引更新
* 输出版本号
* 输出帮助

## 功能的可配置项

通用参数：

* `-y` 跳过确认
* `--json` 输出Edgeless Hub friendly的JSON格式信息

### 下载

可接收软件名（Chrome）/文件全名（Chrome_1.0.0.0_Cno）作为主参数

* 临时目录路径
* 镜像站名称（通过 [名称：URL] 列表查找并使用对应的URL）
* Aria2线程数
* 完成后操作（默认保存并加载，可以通过字母顺序控制操作流，例如 ls 表示加载并保存，s 表示仅保存，留空表示不操作）

### 保存

可接收软件名（Chrome）/文件全名（Chrome_1.0.0.0_Cno）作为主参数

* 临时目录路径
* 启动盘盘符

### 加载

可接收软件名（Chrome）/文件全名（Chrome_1.0.0.0_Cno）作为主参数

* 查找目录（默认只在临时目录、U盘中找插件包）
* 加载模式（普通/LocalBoost）

### 更新

可接收软件名（Chrome）/文件全名（Chrome_1.0.0.0_Cno）作为主参数；无主参数时检查全部更新

* 作者不一致时是否更新（默认 是）
* 是否多线程（逐个更新还是并发更新，考虑服务器压力默认逐个更新；也可取消此项，不允许并发更新）
* 临时目录路径
* 镜像站名称（通过 [名称：URL] 列表查找并使用对应的URL）
* Aria2线程数
* 完成后操作（默认保存并加载，可以通过字母顺序控制操作流，例如 ls 表示加载并保存，s 表示仅保存，留空表示不操作）

### 搜索

可接收字符串或正则表达式对（软件名_版本号_作者）、分类名进行搜索，允许使用同义词和标签

* 是否展示所有版本（默认使用去重只展示最新版本，历史版本需要访问分类文件夹下的“历史版本”子文件夹，主站没有历史版本，OneDrive有历史版本）

### 列出

可接收 分类名 作为主参数（其他信息使用搜索）

* 是否列出历史版本

### 删除

可接收软件名（Chrome）/文件全名（Chrome_1.0.0.0_Cno）作为主参数，默认遍历全部盘符下的（Edgeless/Resource）并删除所有后缀包含`7z`的文件

* 仅删除指定盘符上的插件包
* 指定后缀名

### 信息查询

可接收软件名（Chrome）/文件全名（Chrome_1.0.0.0_Cno）作为主参数

* 仅查询本地插件包（适用于无网络情景）
* 打印详细情况（打包者、打包时间、文件大小等,注意分别输出在线插件包和本地插件包的信息）

### ~~卸载~~

可接收软件名（Chrome）/文件全名（Chrome_1.0.0.0_Cno）作为主参数

### 属性管理

可接收软件名（Chrome）/文件全名（Chrome_1.0.0.0_Cno）作为主参数

* 目标属性，可以组合字母叠加属性，如 lf 表示禁用的localboost插件包