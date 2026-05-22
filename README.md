# KeyRun

KeyRun 是一个面向 JetBrains 系列 IDE 的静态分发项目，包含网页入口、跨平台脚本、`ja-netfilter` 运行文件、插件配置和预生成的产品授权文件。

> 重要声明：本项目涉及 JetBrains 产品授权相关文件和本机 IDE 配置修改。请仅在你明确拥有合法授权、测试授权或研究许可的前提下使用。商业使用请购买 JetBrains 官方许可证。仓库维护者需要自行承担分发、使用和合规风险。

## 项目结构

```text
.
├── index.html                  # GitHub Pages 首页和使用入口
├── ps1                         # Windows PowerShell 引导脚本
├── scripts/
│   ├── install.ps1             # Windows 主安装脚本
│   └── install.sh              # macOS / Linux 主安装脚本
├── ja-netfilter/
│   ├── ja-netfilter.jar        # ja-netfilter 主程序
│   ├── config/                 # ja-netfilter 配置文件
│   └── plugins/                # ja-netfilter 插件 jar
├── licenses/                   # 各 JetBrains 产品的 .key 文件
└── test_script.ps1             # PowerShell 测试脚本副本
```

## 当前能力

- 提供一个静态网页入口，适合通过 GitHub Pages 访问。
- 提供 Windows PowerShell 引导脚本，会下载并执行 `scripts/install.ps1`。
- 提供 macOS / Linux Shell 脚本，会检测平台、语言和必要依赖。
- 脚本会下载 `ja-netfilter` 相关 jar、配置文件和插件文件到用户目录。
- 脚本会扫描本机 JetBrains 配置目录，处理已安装产品的 `.vmoptions` 文件。
- 脚本会清理部分历史环境变量和旧工具残留，并为已有产品写入对应 `.key` 文件。

## 支持产品

脚本中的产品列表包括：

- IntelliJ IDEA
- CLion
- PhpStorm
- GoLand
- PyCharm
- WebStorm
- Rider
- DataGrip
- RubyMine
- AppCode
- DataSpell
- RustRover

## 平台与依赖

### Windows

- 入口文件：`ps1`
- 主脚本：`scripts/install.ps1`
- 运行环境：PowerShell
- 脚本会在需要时请求管理员权限。

### macOS / Linux

- 主脚本：`scripts/install.sh`
- 运行环境：Bash
- 依赖工具：`curl`、`jq`
- macOS 下脚本会尝试通过 Homebrew 安装缺失依赖；Linux 下会根据发行版尝试使用 `apt`、`dnf`、`yum` 或 `pacman`。

## 脚本行为说明

执行安装脚本前，需要关闭正在运行的 JetBrains IDE。脚本会对本机环境做以下修改：

1. 创建或刷新工作目录 `~/.jb_run`。
2. 下载 `ja-netfilter.jar`、配置文件和插件 jar。
3. 清理部分 JetBrains 相关环境变量。
4. 备份并修改部分 Shell 配置文件。
5. 扫描 JetBrains 缓存目录和配置目录。
6. 清理目标 `.vmoptions` 中已有的 `-javaagent` 配置。
7. 写入新的 `-javaagent` 配置。
8. 下载并写入对应产品的 `.key` 文件。
9. 打开项目网页入口。

这些行为会影响本机 JetBrains IDE 的启动配置。维护或测试时建议先在干净虚拟机、测试账户或可回滚环境中验证。

## 静态部署

仓库当前没有构建步骤，`index.html` 和静态资源可以直接通过 GitHub Pages 托管。

默认脚本中写死的静态资源地址为：

```text
https://liyangxu1.github.io/keyrun
```

如果仓库名、GitHub 用户名或 Pages 域名发生变化，需要同步检查并更新：

- `index.html`
- `ps1`
- `scripts/install.ps1`
- `scripts/install.sh`

## 维护注意事项

- 更新 `ja-netfilter` 相关文件后，建议同步更新脚本中的来源说明和完整性校验记录。
- 新增或删除产品时，需要同时检查脚本产品列表和 `licenses/` 目录。
- 修改静态域名时，要全局搜索旧域名，避免引导脚本和主脚本地址不一致。
- `scripts/install.sh` 和 `scripts/install.ps1` 的逻辑应保持一致，避免不同平台行为分叉。
- 项目注释请使用中文，除非第三方协议、命令输出或必要英文标识无法翻译。

## 风险提示

- 脚本会修改用户目录下的 IDE 配置文件。
- 脚本会写入本机 Shell 配置文件备份。
- 脚本会清理已有的 `-javaagent` 配置，可能影响其他工具。
- 脚本会从静态站点下载二进制 jar 和授权文件，维护者需要自行校验文件来源和完整性。
- 授权相关文件的分发可能受到软件许可协议、平台规则和当地法律约束。

## 许可证

仓库内包含第三方二进制文件、插件和产品授权相关文件。当前仓库未提供统一开源许可证。分发或复用前请先确认各文件的原始授权、版权归属和许可边界。
