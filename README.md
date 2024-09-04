# crm (Cargo registry manager)

[`crm`](https://github.com/wtklbm/crm) 是一个在终端运行的镜像管理程序，能够对 `Cargo` 镜像源进行简单的添加、修改、删除操作，并能帮助您快速的切换不同的 `Cargo` 镜像源。`crm` 内置了多种国内 (中国) 镜像源，它们分别是：`sjtu`, `tuna`, `ustc`, `rsproxy`，`bfsu`, `nju`, `hit`。


在使用 Rust 语言做开发时，使用 Rust 官方镜像源进行 `cargo build` 的速度非常的慢，可能会因为网络的原因导致依赖下载超时而无法完成编译。为了能够在最少的时间内完成打包操作，一般会使用国内镜像源来代替官方镜像。


通常，大家一般会手动修改 `~/.cargo/config` 文件来完成镜像的切换，手动修改配置文件的工作是繁琐的，它需要手动打开文件所在的目录，还要记住每一个镜像源的地址和配置方式，在不知道哪个国内源的网速最快的时候，我们还需要对镜像的速度进行手动的测速，在使用国内镜像源的过程中，如果当前所使用的国内镜像源也挂了，我们还需要切换到另一个国内镜像源，这就显得非常的棘手。如果您手动配置了国内镜像源，并且还经常的通过 `cargo publish` 发包的话 ，那么在发包之前，还需要将国内镜像源再手动切换为官方镜像。在比如，每一个国内镜像源同步镜像的时间是不一样的，如果您刚发了一个包并且想第一时间应用到您的项目中，但是因为国内镜像源的没有及时的同步镜像，而导致包无法下载，这个时候您还需要切换到官方镜像源来下载最新发布的包。每一次手动切换镜像的操作都是繁琐且耗时的，而 `crm` 就是为了解决上述的问题。



## 安装

### 通过 `cargo` 安装

```bash
# 在终端中执行
# NOTE 最好使用官方镜像源来安装或升级

cargo install crm
```



### 在 `Arch Linux` 上安装

您可以从 Arch Linux 用户仓库 (AUR) 中安装它，感谢 [taotieren](https://github.com/taotieren) 为此做出的努力。

- <https://aur.archlinux.org/packages/crm>

```bash
# 在终端中执行

# `yay` 是一个 AUR 包管理器，可以使用您喜欢的任何包管理器来安装它，例如 `paru`
yay -S crm
```

- <https://aur.archlinux.org/packages/crm-git>

```bash
# 在终端中执行

yay -S crm-git
```

> NOTE：`crm` 和 `crm-git` 都是基于源代码来构建二进制文件的，如果不想通过下载源代码的方式来构建，则可以到 [Github Release](https://github.com/wtklbm/crm/releases) 中下载预构建的二进制版本。



### 手动下载预编译的二进制文件

可以直接从 [Github Release](https://github.com/wtklbm/crm/releases) 下载预编译的二进制版本，将其下载后，保存到任意目录，然后将该目录放到操作系统的 PATH 环境变量中：
 - 如果使用的是 Windows 11 系统，则可以在搜索框中搜索 "**编辑系统环境变量**"，然后找到 "**环境变量**"，找到用户变量中的 "**Path**"，双击它，并添加自定义路径
 - 如果使用的是 macOS 或 Linux 系统，则可以将 `export PATH="$PATH:自定义路径"` 添加到 `.zshrc` 或 `.bashrc` 文件中 (取决于使用的是哪种 Shell)

通过这种安装方式，可以更加自由的管理该软件，半年甚至一年不更新也可以正常使用，可以用 `crm check-update` 来查询新版本，并进行更新。若要将 `crm` 批量安装到多台操作系统的话，这样会更加方便快捷。

在下载二进制文件时，可能会遇到下载失败的情况，为此，可以使用 Github 镜像来加速下载。

```bash
# 在终端中执行

# 使用 `https://hub.gitmirror.com/` 镜像来加速下载
# 下载哪个版本，就将下面的 URL 部分进行相应的替换即可
curl -O https://hub.gitmirror.com/https://github.com/wtklbm/crm/releases/download/v0.2.1/crm_windows_amd64.tar.gz
```



## 使用

`crm` 的原则是使用最小依赖，并尽可能的简化终端操作。您只需要在终端键入 `crm` 即可获得命令帮助信息。

```bash
# 在终端执行
#
# NOTE:
#  - [args] 表示 args 是一个或多个可选参数
#  - <name> 表示 name 是一个必填参数
#
# 下面这些命令在执行时会自动切换为官方镜像，避免了手动切换镜像的麻烦：
#  - `crm install` 对应 `cargo install`
#  - `crm publish` 对应 `cargo publish`
#  - `crm update` 对应 `cargo update`
#
# `crm test` 命令一般用于进行全量测试，而 `crm best` 是切换到最优镜像的快速方式

$ crm

  crm best                    评估网络延迟并自动切换到最优的镜像
    crm best git              仅评估 git 镜像源
    crm best sparse           仅评估支持 sparse 协议的镜像源
    crm best git-download     仅评估能够快速下载软件包的 git 镜像源 (推荐使用)
    crm best sparse-download  仅评估能够快速下载软件包且支持 sparse 协议的镜像源 (推荐使用)
  crm default                 恢复为官方默认镜像
  crm install [args]          使用官方镜像执行 "cargo install"
  crm list                    从镜像配置文件中获取镜像列表
  crm publish [args]          使用官方镜像执行 "cargo publish"
  crm remove <name>           在镜像配置文件中删除镜像
  crm save <name> <addr> <dl> 在镜像配置文件中添加/更新镜像
  crm test [name]             下载测试包以评估网络延迟
  crm update [args]           使用官方镜像执行 "cargo update"
  crm use <name>              切换为要使用的镜像
  crm version                 查看当前版本
  crm check-update            检测版本更新
```


- 根据 [反馈](https://github.com/wtklbm/crm/issues/11)，并不建议您使用 `ustc` 镜像源，它会限制并发
- 以下镜像只能在更新 `git` 镜像仓库时获得加速效果，在下载包时无法获得加速效果：
  - `tuna`
  - `bfsu`
  - `nju`
  - `hit`



## 在项目中使用来自不同镜像源的依赖

`crm` 在配置镜像源时，会默认在 `~/.cargo/config` 中多增加一个 `registries` 属性对象，通过增加该属性对象，您就可以在项目中应用来自于不同镜像源的依赖。比如您在使用官方镜像源时，可以通过在项目的 `Cargo.toml` 文件中指定依赖的 `registry` 属性来使用不同的国内镜像源。如果您已经在使用国内镜像源了，那么也可以通过修改 `registry` 属性的方式来切换到其他的国内镜像源。以下是一个示例。




```toml
# Cargo.toml

# 使用官方镜像源时，`registry` 属性可选的值为： `sjtu`, `tuna`, `ustc`, `rsproxy`, `bfsu`, `nju`, `hit`
# 如果您已经使用 `crm` 切换到了 `rsproxy` 镜像源，那么 `registry` 属性可选的值则为其他国内镜像：`sjtu`, `tuna`, `ustc`, `bfsu`, `nju`, `hit`
# 以此类推
# 值得注意的是，在使用国内镜像源时，您无法直接通过修改 `registry` 属性的方式使用官方镜像源
# 如果您想使用官方镜像源，那么请在终端执行 `crm default` 来切换到官方镜像

[dependencies]
# 使用 `ustc` 国内镜像来下载 `log` 依赖
log = {version = "0.4.12", registry = "ustc"}

# 使用 `sjtu` 国内镜像来下载 `lazy_static` 依赖
lazy_static = {version = "1.4.0", registry = "sjtu"}
```



> NOTE：
> - 如果您刚安装 `crm`，那么请在终端执行一次：`crm default`，然后就可以在项目的 `Cargo.toml` 文件中配置 `registry` 属性了。
> - 从 Rust v1.39.0 版本开始，`~/.cargo/config.toml` 将被推荐使用，如果 `~/.cargo/config` 和 `~/.cargo/config.toml` 同时存在，则优先使用 `~/.cargo/config` 配置。



## 使用 `sparse` 协议

在 `v1.68.0` 版本中，新增了一个 `sparse` 协议。以前我们更新注册表默认都是通过克隆 `git` 仓库来实现的，在克隆 `git` 仓库时会出现明显的延迟，`sparse` 协议的好处就是不用再克隆镜像源仓库了，而是仅通过 HTTPS 从网络上查询和下载您所需要的包。虽然该协议避免了 `git` 克隆的步骤，但是当镜像源的网络不稳定时较容易出错。

按照官方说法，使用该协议有两种方式：

1. 将 `export CARGO_REGISTRIES_CRATES_IO_PROTOCOL=sparse` 加入到 `.bashrc` 或 `.zshrc` 文件中
2. 或者编辑 `~/.cargo/config` 文件：
    ```bash
    [registries.crates-io]
    protocol = "sparse"
    ```

但是，当您使用 `crm` 时，`crm` 自带了一些支持 `sparse` 协议的镜像源，它们是开箱即用的。只须执行 `crm best sparse` 或 `crm best sparse-download` 即可轻松切换到支持 `sparse` 协议的镜像源。

如果您想了解更多的内容，请参考下面的链接：
 - <https://blog.rust-lang.org/2023/03/09/Rust-1.68.0.html#cargos-sparse-protocol>
 - <https://doc.rust-lang.org/cargo/reference/config.html#registriescrates-ioprotocol>
 - <https://github.com/rust-lang/cargo/issues/9069>
 - <https://internals.rust-lang.org/t/call-for-testing-cargo-sparse-registry/16862/20>
 - <https://blog.rust-lang.org/2022/06/22/sparse-registry-testing.html>




## 注意事项

1. `v0.1.0` 版本以下的 `.crmrc` 配置文件和最新版本的配置文件并不能相互兼容，如果您正在使用小于 `v0.1.0` 的版本，当您更新到最新版本时，请手动删除 `~/.crmrc` 文件
2. `crm` 会修改 `~/.cargo/config` 文件来进行镜像源的切换，如果您使用的是小于 `v0.1.3` 的版本，那么当您使用 `crm` 切换镜像时，`~/.cargo/config` 文件中的文档注释会被删除并且永远无法恢复，如果您在 `~/.cargo/config` 文件中保存了笔记或者文档，请尽快更新到最新版，在最新版中，对此进行了优化，不再自动删除文档注释 (除修改的字段外)
3. `crm` 默认会在 `~/.cargo/config` 文件中增加一个 `env.git-fetch-with-cli` 属性，值为 `true`，在使用 `crm` 时您无法删除该选项，如果您不想使用 `Git` 可执行文件进行 `Git` 操作，请手动修改 `~/.cargo/config` 文件并将 `git-fetch-with-cli` 的值修改为 `false`




## 错误处理

以下是 crm 异常结束的情况对照表：
 - 1: 写入的镜像名称有误
 - 2: 写入的镜像地址有误
 - 3: 写入的 `dl` 字段值有误
 - 4: 命令无效
 - 5: `config.json` 配置文件解析失败 (格式有误或字段缺失)
 - 6: 属性名错误
 - 7: 不能删除内置镜像
 - 8: 要删除的镜像不存在
 - 9: 传入的参数错误
 - 10: 要进行下载测试的镜像不存在
 - 11: 要进行连接测试的镜像不存在
 - 12: `.crmrc` 配置文件解析失败 (格式有误或字段缺失)
 - 13: 写入文件失败，请检查权限
 - 14: 配置文件冲突，需手动检查




## Others

### rust-library-chinese

`rust-library-chinese` 是 Rust 核心库和标准库的源码级中文翻译，可以用作 IDE 工具的中文智能提示，也可以基于翻译好的内容生成 Rust 中文 API 文档。

- [从 Github 访问](https://github.com/wtklbm/rust-library-i18n)
- [从 Gitee 访问](https://gitee.com/wtklbm/rust-library-chinese)




## LICENSE

MIT OR Apache-2.0

