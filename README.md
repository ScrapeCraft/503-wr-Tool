<div align="center">
  <img src="./appicon.png" alt="503 微软邮箱工具箱" width="112" />
  <h1>503 微软邮箱工具箱</h1>
  <p>基于 Wails 的 Windows Microsoft 邮箱批处理桌面工具箱</p>

  [![Latest Release](https://img.shields.io/github/v/release/ScrapeCraft/503-wr-Tool?display_name=tag&sort=semver)](https://github.com/ScrapeCraft/503-wr-Tool/releases/latest)
  [![Downloads](https://img.shields.io/github/downloads/ScrapeCraft/503-wr-Tool/total)](https://github.com/ScrapeCraft/503-wr-Tool/releases)
  [![Platform](https://img.shields.io/badge/platform-Windows-0078D6?logo=windows)](#运行要求)
  [![Wails](https://img.shields.io/badge/Wails-v2.12.0-DF0000)](https://wails.io/)

  **[简体中文](#zh-cn) | [English](#english)**
</div>

---

<a id="zh-cn"></a>

## 简体中文

### 项目简介

503 微软邮箱工具箱是一款 Windows 桌面程序，目前整合了两个相互独立的功能：

- **批量授权**：执行 Microsoft OAuth / Refresh Token 批量授权，并通过辅助邮箱接收验证码。
- **刷新令牌和筛号**：刷新已有 Refresh Token，并根据账号状态分类保存结果。

两个功能分别维护配置、运行状态和结果目录。同一时间只运行一个任务，便于后续继续整合其他独立工具。

### 主要功能

#### 批量授权

- 批量读取账号和辅助邮箱。
- 辅助邮箱取件支持 **POP** 与 **IMAP** 自由切换。
- POP/IMAP Host、SSL 和端口独立配置。
- 支持 `http`、`https`、`socks5` 代理协议。
- 支持直接填写代理，或填写返回代理文本的 Proxy API URL。
- 支持并发数、整套流程失败重试和任务停止。
- 提供账号预览、搜索、分页、密码隐藏、实时日志和结果筛选。
- 通过 `处理进度.txt` 自动跳过已处理账号，支持断点继续。
- 历史结果自动加载，可复制单条或当前筛选结果。

#### 刷新令牌和筛号

- 可选择输入文件，也可直接使用“批量授权”的成功结果。
- 支持 **IMAP Scope** 或 **Graph Scope**。
- 支持 1-50 并发；失败记录不自动重试。
- 刷新成功后仅替换原记录中的 Refresh Token，其余字段保持不变。
- 按以下状态分类：`成功`、`封禁`、`风险`、`失效`、`网络错误`、`其他`。
- 每次运行使用独立结果目录，并生成运行信息 `run.json`。
- 提供格式检查、账号预览、搜索、分页、敏感信息隐藏、分类筛选和实时日志。

#### 通用功能

- 启动后通过 GitHub Releases API 静默检查新版本。
- 支持手动检查更新；发现新版本后打开 GitHub Release 页面，不会自动替换 EXE。
- 自定义无边框窗口、最小化、最大化和关闭按钮。
- 内置使用教程、作者联系方式和 QQ 群反馈入口。

### 下载与运行

1. 打开 [Releases](https://github.com/ScrapeCraft/503-wr-Tool/releases/latest)。
2. 在 **Assets** 中下载发布者上传的 EXE 或 ZIP 安装包。
3. 如果下载的是 ZIP，请先解压，再运行其中的 EXE。

> `Source code (zip)` 和 `Source code (tar.gz)` 是 GitHub 自动生成的源码包，不是可直接运行的软件。请从 **Assets** 下载工具文件。

### 运行要求

- Windows 10 / Windows 11
- Microsoft Edge WebView2 Runtime
- 可访问 Microsoft 登录、邮箱服务器和 GitHub API 的网络环境

Windows 10/11 通常已经包含 WebView2 Runtime。如果程序无法显示界面，可从 Microsoft 官方网站安装最新 WebView2 Runtime。

### 文件格式

建议使用 **UTF-8 编码的 TXT 文件**，每行一条记录。字段之间必须使用四个连续连字符：`----`。

| 用途 | 格式 | 示例 |
| --- | --- | --- |
| 批量授权账号 | `邮箱----密码[----其他内容...]` | `user@example.com----password` |
| 辅助邮箱 | `邮箱----密码` | `helper@example.com----password` |
| 刷新令牌和筛号 | `邮箱----密码----client_id----refresh_token[----其他内容...]` | `user@example.com----password----client_id----refresh_token` |
| 代理或代理接口返回内容 | `ip:port` | `1.2.3.4:8080` |

代理输入框支持：

- 直接代理：`ip:port`
- 带协议的代理 URL
- 返回代理 TXT 文本的 HTTP(S) API 地址

> **重要：**辅助邮箱池是消耗式队列。辅助邮箱被取用后会从原辅助邮箱文件中移除，请在运行前保留备份。

### 快速使用

#### 批量授权

1. 准备账号文件和辅助邮箱文件。
2. 选择 POP 或 IMAP，并填写对应邮箱服务器 Host。
3. 填写代理或 Proxy API URL，选择代理协议。
4. 设置并发数和失败重试次数。
5. 点击“开始运行”，在账号预览、运行结果和日志区域查看进度。
6. 中途停止后可再次启动，程序会根据 `处理进度.txt` 断点继续。

#### 刷新令牌和筛号

1. 选择符合格式的账号文件，或点击“使用批量授权成功文件”。
2. 选择 IMAP 或 Graph Scope。
3. 设置并发数并开始运行。
4. 在“分类结果”中查看状态，或打开结果目录读取分类文件。

### 配置与输出

程序会在 EXE 所在目录创建以下结构：

```text
配置/
├─ 批量授权.json
└─ 刷新令牌和筛号.json

结果/
├─ 批量授权/
│  ├─ 授权成功.txt
│  ├─ 授权失败.txt
│  ├─ 绑定后授权失败.txt
│  └─ 处理进度.txt
└─ 刷新令牌和筛号/
   └─ <每次运行目录>/
      ├─ 成功.txt
      ├─ 封禁.txt
      ├─ 风险.txt
      ├─ 失效.txt
      ├─ 网络错误.txt
      ├─ 其他.txt
      └─ run.json
```

刷新令牌功能只会为本次实际出现的分类创建对应 TXT 文件，因此部分分类文件可能不存在。

批量授权成功记录格式：

```text
邮箱----密码----client_id----refresh_token----辅助邮箱----辅助邮箱密码[----原账号其他内容...]
```

### 更新机制

程序访问以下公开接口检查最新正式版本：

```text
https://api.github.com/repos/ScrapeCraft/503-wr-Tool/releases/latest
```

- 启动后自动后台检查一次。
- 顶部“检查更新”按钮可以立即重新检查。
- 只比较语义版本号并展示 Release 信息。
- 不自动下载，也不自动覆盖当前程序。
- GitHub Draft 和 Prerelease 不会被 `/releases/latest` 识别为最新正式版。

### 开发与构建

开发环境：

- Go 1.24.1+
- Node.js 20.19+ 或 22.12+
- npm
- Windows WebView2 Runtime

安装前端依赖并启动开发模式：

```powershell
npm --prefix frontend install
go run github.com/wailsapp/wails/v2/cmd/wails@v2.12.0 dev
```

运行检查：

```powershell
go test ./...
npm --prefix frontend run build
```

构建正式版本：

```powershell
.\build.ps1
```

构建脚本会执行 Go 测试，使用 Wails v2.12.0 进行 clean/trimpath 构建，并使用以下 ldflags：

```text
-s -w -X main.appVersion=<版本号>
```

最终文件会复制到项目根目录：

```text
503微软邮箱工具箱v<版本号>.exe
```

### 发布版本

1. 同步更新 `VERSION`、`wails.json`、`frontend/package.json`、`frontend/package-lock.json` 和版本测试中的版本号。
2. 执行 `.\build.ps1`。
3. 将生成的 EXE 压缩为 ZIP。
4. 创建标签 `v<版本号>` 对应的正式 GitHub Release。
5. 在 Release 页面底部的 **Attach binaries** 区域上传 ZIP，确保文件显示在 **Assets** 中。

```powershell
$version = (Get-Content .\VERSION -Raw).Trim()
Compress-Archive -LiteralPath ".\503微软邮箱工具箱v$version.exe" `
  -DestinationPath ".\503微软邮箱工具箱v$version.exe.zip" -Force
```

### 数据安全

- 输入文件、配置和结果文件可能包含明文密码、Client ID 和 Refresh Token。
- 界面默认隐藏部分敏感字段，但磁盘文件及“复制完整结果”内容不会脱敏。
- 请妥善保管文件，并在不再需要时自行清理。

### 联系与相关链接

- [下载最新版本](https://github.com/ScrapeCraft/503-wr-Tool/releases/latest)
- [加入 QQ 群：952042396](https://qm.qq.com/q/1oYAEbPgco)
- 作者 QQ：`1091687244`
- [获取便宜机房代理](https://socks5.io/?code=6127RZQA)
- [购买微软邮箱](https://cheapemail.cc/)

---

<a id="english"></a>

## English

### Overview

503 Microsoft Mailbox Toolbox is a Windows desktop application that currently combines two independent tools:

- **Batch Authorization**: performs Microsoft OAuth / Refresh Token authorization in batches and receives verification codes through helper mailboxes.
- **Refresh Token & Account Filter**: refreshes existing tokens and saves accounts into status-based categories.

Each tool maintains its own settings, runtime state, and output directory. Only one task can run at a time, keeping the application ready for additional independent tools in future releases.

### Features

#### Batch Authorization

- Loads account and helper-mailbox lists in batches.
- Supports both **POP** and **IMAP** for verification-code retrieval.
- Stores separate Host, SSL, and port settings for POP and IMAP.
- Supports `http`, `https`, and `socks5` proxy schemes.
- Accepts a direct proxy or a Proxy API URL that returns proxy text.
- Supports concurrency, full-flow retries, and task cancellation.
- Includes account preview, search, pagination, password masking, live logs, and result filters.
- Resumes interrupted work by skipping addresses recorded in `处理进度.txt`.
- Loads historical results and supports copying individual or filtered results.

#### Refresh Token & Account Filter

- Accepts a separate input file or the success file generated by Batch Authorization.
- Supports **IMAP Scope** and **Graph Scope**.
- Supports 1-50 concurrent workers; failed records are not retried automatically.
- Replaces only the Refresh Token field after a successful refresh and preserves all other fields.
- Classifies results as `成功` (success), `封禁` (banned), `风险` (risk), `失效` (invalid), `网络错误` (network error), or `其他` (other).
- Creates an independent output directory and a `run.json` manifest for every run.
- Includes format validation, preview, search, pagination, secret masking, category filters, and live logs.

#### Shared Features

- Checks for new versions through the GitHub Releases API after startup.
- Provides a manual update check; new versions open on GitHub and never replace the EXE automatically.
- Uses a custom borderless window with minimize, maximize, and close controls.
- Includes an in-app tutorial, author contact details, and a QQ feedback-group shortcut.

### Download and Run

1. Open the [latest Release](https://github.com/ScrapeCraft/503-wr-Tool/releases/latest).
2. Download the publisher-provided EXE or ZIP package from **Assets**.
3. If you downloaded a ZIP, extract it first, then run the EXE.

> GitHub's automatically generated `Source code (zip)` and `Source code (tar.gz)` archives are source packages, not the runnable application. Download the application file from **Assets**.

### System Requirements

- Windows 10 / Windows 11
- Microsoft Edge WebView2 Runtime
- Network access to Microsoft sign-in services, mailbox servers, and the GitHub API

WebView2 Runtime is normally included with Windows 10/11. Install the latest Microsoft WebView2 Runtime if the application window cannot render correctly.

### Input Formats

UTF-8 encoded TXT files are recommended. Store one record per line and separate fields with exactly four hyphens: `----`.

| Purpose | Format | Example |
| --- | --- | --- |
| Batch Authorization account | `email----password[----extra fields...]` | `user@example.com----password` |
| Helper mailbox | `email----password` | `helper@example.com----password` |
| Refresh Token & Account Filter | `email----password----client_id----refresh_token[----extra fields...]` | `user@example.com----password----client_id----refresh_token` |
| Proxy or Proxy API response | `ip:port` | `1.2.3.4:8080` |

The proxy field accepts:

- A direct `ip:port` proxy
- A complete proxy URL with a scheme
- An HTTP(S) API URL whose TXT response contains a proxy

> **Important:** helper mailboxes are consumed as a queue. Once selected, a helper mailbox is removed from the original helper-mailbox file. Keep a backup before running a task.

### Quick Start

#### Batch Authorization

1. Prepare an account file and a helper-mailbox file.
2. Select POP or IMAP and enter the corresponding mailbox server Host.
3. Enter a proxy or Proxy API URL and select its scheme.
4. Configure concurrency and retry count.
5. Start the task and monitor the preview, results, and live logs.
6. If stopped, start it again to resume from `处理进度.txt`.

#### Refresh Token & Account Filter

1. Select a correctly formatted input file, or choose the Batch Authorization success file.
2. Select IMAP or Graph Scope.
3. Configure concurrency and start the task.
4. Review the categorized results in the UI or open the output directory.

### Settings and Output

The application creates the following structure next to the EXE:

```text
配置/
├─ 批量授权.json
└─ 刷新令牌和筛号.json

结果/
├─ 批量授权/
│  ├─ 授权成功.txt
│  ├─ 授权失败.txt
│  ├─ 绑定后授权失败.txt
│  └─ 处理进度.txt
└─ 刷新令牌和筛号/
   └─ <per-run directory>/
      ├─ 成功.txt
      ├─ 封禁.txt
      ├─ 风险.txt
      ├─ 失效.txt
      ├─ 网络错误.txt
      ├─ 其他.txt
      └─ run.json
```

The refresh/filter tool creates TXT files only for categories encountered during that run, so some category files may be absent.

A successful Batch Authorization record uses this format:

```text
email----password----client_id----refresh_token----helper_email----helper_password[----original extra fields...]
```

### Update Check

The application checks the latest stable release through:

```text
https://api.github.com/repos/ScrapeCraft/503-wr-Tool/releases/latest
```

- One silent check runs after startup.
- The top-bar update button forces a new check.
- The application compares semantic versions and displays Release metadata.
- It never downloads or replaces the current executable automatically.
- Drafts and prereleases are not returned as the latest stable release by GitHub's `/releases/latest` endpoint.

### Development and Build

Requirements:

- Go 1.24.1+
- Node.js 20.19+ or 22.12+
- npm
- Windows WebView2 Runtime

Install frontend dependencies and start development mode:

```powershell
npm --prefix frontend install
go run github.com/wailsapp/wails/v2/cmd/wails@v2.12.0 dev
```

Run checks:

```powershell
go test ./...
npm --prefix frontend run build
```

Build a release executable:

```powershell
.\build.ps1
```

The build script runs Go tests, performs a clean/trimpath Wails v2.12.0 build, and applies:

```text
-s -w -X main.appVersion=<version>
```

The final executable is copied to the repository root as:

```text
503微软邮箱工具箱v<version>.exe
```

### Publishing a Release

1. Synchronize the version in `VERSION`, `wails.json`, `frontend/package.json`, `frontend/package-lock.json`, and the version test.
2. Run `.\build.ps1`.
3. Compress the generated EXE into a ZIP archive.
4. Create a stable GitHub Release using tag `v<version>`.
5. Upload the ZIP through the Release page's **Attach binaries** area and verify that it appears under **Assets**.

```powershell
$version = (Get-Content .\VERSION -Raw).Trim()
Compress-Archive -LiteralPath ".\503微软邮箱工具箱v$version.exe" `
  -DestinationPath ".\503微软邮箱工具箱v$version.exe.zip" -Force
```

### Data Security

- Input, settings, and output files may contain plaintext passwords, Client IDs, and Refresh Tokens.
- The UI masks selected fields by default, but files on disk and copied full results are not redacted.
- Store these files securely and remove them when they are no longer needed.

### Contact and Links

- [Download the latest version](https://github.com/ScrapeCraft/503-wr-Tool/releases/latest)
- [Join QQ group 952042396](https://qm.qq.com/q/1oYAEbPgco)
- Author QQ: `1091687244`
- [Affordable datacenter proxies](https://socks5.io/?code=6127RZQA)
- [Buy Microsoft mailboxes](https://cheapemail.cc/)
