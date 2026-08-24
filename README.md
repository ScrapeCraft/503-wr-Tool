<div align="center">
  <img src="./build/appicon.png" alt="503 微软邮箱工具箱" width="112" />
  <h1>503 微软邮箱工具箱</h1>
  <p>基于 Wails 的 Windows Microsoft 邮箱批处理桌面工具箱</p>

  [![Latest Release](https://img.shields.io/github/v/release/ScrapeCraft/503-wr-Tool?display_name=tag&sort=semver)](https://github.com/ScrapeCraft/503-wr-Tool/releases/latest)
  [![Version](https://img.shields.io/badge/version-v1.0.5-2F80ED)](./VERSION)
  [![Downloads](https://img.shields.io/github/downloads/ScrapeCraft/503-wr-Tool/total)](https://github.com/ScrapeCraft/503-wr-Tool/releases)
  [![Platform](https://img.shields.io/badge/platform-Windows-0078D6?logo=windows)](#运行要求)
  [![Wails](https://img.shields.io/badge/Wails-v2.12.0-DF0000)](https://wails.io/)

  **[简体中文](#zh-cn) | [English](#english)**
</div>

---

<a id="zh-cn"></a>

## 简体中文

### 项目简介

503 微软邮箱工具箱是一款 Windows 桌面程序，目前整合了四个相互独立的功能：

- **批量授权**：执行 Microsoft OAuth / Refresh Token 批量授权，并通过辅助邮箱接收验证码。
- **刷新令牌和筛号**：刷新已有 Refresh Token，并根据账号状态分类保存结果。
- **邮件查看**：使用 Microsoft IMAP OAuth 只读查看 Inbox/Junk 邮件，并自动提取 4/6 位验证码。
- **协议注册**：通过账密代理和 CaptchaRun 注册 Microsoft 邮箱，并可使用临时邮箱继续开启 OAuth2。

各功能分别维护配置、运行状态和结果目录。同一时间只运行一个任务，避免多个批处理流程互相影响。

### v1.0.5 更新

- 新增“协议注册”界面与完整注册流程，注册次数支持有限次数或无限运行。
- 协议注册固定使用 Edge 147 请求标识，TLS 配置保持 `chrome_148`。
- 支持拖入或选择账密代理 TXT；代理一次载入内存并逐行轮换，不修改源文件。
- 接入 CaptchaRun，以及 `mail.cx`、`outlook.tw`、`tempmail.cn` 三个临时邮箱平台。
- 精简注册日志并限制为最近 200 行；成功账号仅保留并显示最近 100 个。

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

#### 邮件查看

- 使用 `outlook.office365.com:993`、TLS 和 `AUTHENTICATE XOAUTH2`，不使用邮箱密码登录。
- 只读查看收件箱和垃圾邮件，邮件列表只取头信息，打开邮件时再读取正文。
- 使用 IMAP IDLE 接收新邮件通知，每 25 分钟自动重入保持连接。
- 支持 multipart、Base64、Quoted-Printable、常见字符集和 RFC 2047 解码。
- 从纯文本和 HTML 可见文本中识别 4/6 位验证码，并对订单号、金额、日期等数字降权。
- 邮件正文仅以纯文本显示，不执行邮件 HTML、脚本或外部资源。
- 可启用仅监听 `127.0.0.1:47865` 的本地验证码 API；GET 请求无需鉴权，默认关闭 CORS。

#### 协议注册

- 自动生成邮箱资料，并通过导入的账密代理逐个执行 Microsoft 注册流程。
- 兼容 `host:port:user:pass`、`user:pass@host:port` 和带协议 URL 等常见代理格式。
- 代理文件一次读取到内存，每次注册取一行代理，不会修改代理源文件。
- 使用 CaptchaRun 完成风险验证；silent、press 与辅助邮箱验证码均采用固定轮询间隔。
- 注册次数支持 `1-10000` 或无限，并发数支持 `1-50`。
- 可选开启 OAuth2，并支持 `mail.cx`、`outlook.tw`、`tempmail.cn` 临时邮箱平台。
- 展示注册、打码与成功率统计；成功账号显示完整数据并仅保留最近 100 个。

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
- 使用协议注册时，还需可访问 CaptchaRun 与所选临时邮箱平台

Windows 10/11 通常已经包含 WebView2 Runtime。如果程序无法显示界面，可从 Microsoft 官方网站安装最新 WebView2 Runtime。

### 文件格式

建议使用 **UTF-8 编码的 TXT 文件**，每行一条记录。字段之间必须使用四个连续连字符：`----`。

| 用途 | 格式 | 示例 |
| --- | --- | --- |
| 批量授权账号 | `邮箱----密码[----其他内容...]` | `user@example.com----password` |
| 辅助邮箱 | `邮箱----密码` | `helper@example.com----password` |
| 刷新令牌和筛号 | `邮箱----密码----client_id----refresh_token[----其他内容...]` | `user@example.com----password----client_id----refresh_token` |
| 邮件查看 | `邮箱----密码----client_id----refresh_token[----其他内容...]` | `user@outlook.com----password----client_id----refresh_token` |
| 批量授权代理或代理接口返回内容 | `ip:port` | `1.2.3.4:8080` |
| 协议注册代理文件 | `host:port:user:pass` | `proxy.example.com:8080:user:password` |

批量授权的代理输入框支持：

- 直接代理：`ip:port`
- 带协议的代理 URL
- 返回代理 TXT 文本的 HTTP(S) API 地址

协议注册只接受本地账密代理 TXT 文件，每行一个代理。兼容：

- `host:port:user:pass`
- `user:pass@host:port`
- `http://user:pass@host:port`、`https://...`、`socks5://...`
- 使用逗号、分号、竖线或空格分隔的四字段格式

代理文件会一次读取到内存并循环取用，程序不会删除或改写其中的行。

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

#### 协议注册

1. 导入每行一个账密代理的 TXT 文件。
2. 填写 CaptchaRun API Token。
3. 选择是否开启 OAuth2；开启时选择一个临时邮箱平台。
4. 设置并发数与注册次数，或选择“无限”。
5. 点击“开始注册”；程序会自动保存并检查配置，然后开始任务。
6. 在右侧查看统计、最近 100 个成功账号和最近 200 行运行日志。

#### 邮件查看与本地 API

1. 选择包含 `email----password----client_id----refresh_token` 的账号文件，或在界面直接粘贴多行账号数据；密码字段仅为兼容现有文件格式，不参与 IMAP 登录。
2. 选择邮箱和 Inbox/Junk，打开 IDLE 开关等待新邮件。
3. 如需脚本取码，启用本地 API 后直接发送 GET 请求。

```http
GET http://127.0.0.1:47865/api/v1/code?email=user%40outlook.com&digits=4,6&max_age=600
```

等待请求到达后产生的新验证码：

```http
GET http://127.0.0.1:47865/api/v1/code/wait?email=user%40outlook.com&timeout=60
```

API 还支持 `folders=inbox,junk`、`sender=`、`subject=` 和 `after=<cursor>`。等待超时和未找到均返回 HTTP 200，并通过 `status` 区分。

`/api/v1/code` 默认只匹配最近 600 秒（10 分钟）的验证码，可通过 `max_age=秒数` 调整，范围为 1 到 86400 秒。每个文件夹最多扫描最近 20 封邮件。

### 配置与输出

程序会在 EXE 所在目录创建以下结构：

```text
配置/
├─ 批量授权.json
├─ 刷新令牌和筛号.json
├─ 邮件查看.json
└─ 协议注册.json

结果/
├─ 批量授权/
│  ├─ 授权成功.txt
│  ├─ 授权失败.txt
│  ├─ 绑定后授权失败.txt
│  └─ 处理进度.txt
├─ 刷新令牌和筛号/
│  └─ <每次运行目录>/
│     ├─ 成功.txt
│     ├─ 封禁.txt
│     ├─ 风险.txt
│     ├─ 失效.txt
│     ├─ 网络错误.txt
│     ├─ 其他.txt
│     └─ run.json
└─ 协议注册/
   ├─ 注册成功.txt
   ├─ 注册成功但OAuth失败.txt
   ├─ 注册失败.txt
   └─ 历史.json
```

刷新令牌功能只会为本次实际出现的分类创建对应 TXT 文件，因此部分分类文件可能不存在。

批量授权成功记录格式：

```text
邮箱----密码----client_id----refresh_token----辅助邮箱----辅助邮箱密码[----原账号其他内容...]
```

协议注册记录格式：

```text
# 未开启 OAuth2，或账号已创建但 OAuth2 失败
邮箱----密码

# OAuth2 成功
邮箱----密码----client_id----refresh_token----使用的临时邮箱
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
- 部分界面默认隐藏敏感字段；协议注册的成功账号区域、磁盘结果文件及“复制完整结果”内容不会脱敏。
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

503 Microsoft Mailbox Toolbox is a Windows desktop application that currently combines four independent tools:

- **Batch Authorization**: performs Microsoft OAuth / Refresh Token authorization in batches and receives verification codes through helper mailboxes.
- **Refresh Token & Account Filter**: refreshes existing tokens and saves accounts into status-based categories.
- **Mail Viewer**: uses Microsoft IMAP OAuth to read Inbox/Junk messages and extract 4/6-digit verification codes.
- **Protocol Registration**: registers Microsoft mailboxes through credentialed proxies and CaptchaRun, with optional OAuth2 setup through temporary mailboxes.

Each tool maintains its own settings, runtime state, and output directory. Only one task can run at a time so batch workflows do not interfere with one another.

### What's New in v1.0.5

- Adds the Protocol Registration UI and complete registration workflow, including finite and unlimited run modes.
- Uses an Edge 147 request identity while retaining the `chrome_148` TLS configuration.
- Imports credentialed proxies from TXT by selection or drag-and-drop, loads them into memory, and never modifies the source file.
- Integrates CaptchaRun and the `mail.cx`, `outlook.tw`, and `tempmail.cn` temporary-mail providers.
- Keeps only the latest 200 log lines and latest 100 successful accounts in the UI.

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

#### Mail Viewer

- Connects to `outlook.office365.com:993` over TLS with `AUTHENTICATE XOAUTH2`; mailbox passwords are not used for IMAP login.
- Reads Inbox and Junk in read-only mode and fetches message bodies only when a message is opened.
- Uses IMAP IDLE for new-message notifications and periodically re-enters IDLE to keep the connection alive.
- Decodes multipart, Base64, Quoted-Printable, common character sets, and RFC 2047 headers.
- Extracts likely 4/6-digit verification codes while down-ranking order numbers, prices, and dates.
- Can expose a local verification-code API bound only to `127.0.0.1:47865`.

#### Protocol Registration

- Generates account details and performs Microsoft registration through imported credentialed proxies.
- Accepts common proxy formats such as `host:port:user:pass`, `user:pass@host:port`, and scheme-prefixed URLs.
- Loads the proxy file once, rotates one proxy per registration, and never modifies the original file.
- Uses CaptchaRun for risk verification with fixed polling intervals.
- Supports `1-10000` registrations or unlimited mode, with `1-50` workers.
- Optionally enables OAuth2 through `mail.cx`, `outlook.tw`, or `tempmail.cn`.
- Shows registration/captcha statistics, the latest 100 full successful-account records, and the latest 200 log lines.

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
- Protocol Registration additionally requires access to CaptchaRun and the selected temporary-mail provider

WebView2 Runtime is normally included with Windows 10/11. Install the latest Microsoft WebView2 Runtime if the application window cannot render correctly.

### Input Formats

UTF-8 encoded TXT files are recommended. Store one record per line and separate fields with exactly four hyphens: `----`.

| Purpose | Format | Example |
| --- | --- | --- |
| Batch Authorization account | `email----password[----extra fields...]` | `user@example.com----password` |
| Helper mailbox | `email----password` | `helper@example.com----password` |
| Refresh Token & Account Filter | `email----password----client_id----refresh_token[----extra fields...]` | `user@example.com----password----client_id----refresh_token` |
| Mail Viewer | `email----password----client_id----refresh_token[----extra fields...]` | `user@outlook.com----password----client_id----refresh_token` |
| Batch Authorization proxy or Proxy API response | `ip:port` | `1.2.3.4:8080` |
| Protocol Registration proxy file | `host:port:user:pass` | `proxy.example.com:8080:user:password` |

The Batch Authorization proxy field accepts:

- A direct `ip:port` proxy
- A complete proxy URL with a scheme
- An HTTP(S) API URL whose TXT response contains a proxy

Protocol Registration accepts only a local TXT file containing one credentialed proxy per line. Supported forms include:

- `host:port:user:pass`
- `user:pass@host:port`
- `http://user:pass@host:port`, `https://...`, and `socks5://...`
- Four fields separated by commas, semicolons, pipes, or spaces

The file is loaded into memory and cycled without deleting or rewriting any source line.

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

#### Protocol Registration

1. Import a TXT file containing one credentialed proxy per line.
2. Enter the CaptchaRun API Token.
3. Choose whether to enable OAuth2 and, if enabled, select a temporary-mail provider.
4. Set worker count and registration count, or select unlimited mode.
5. Click Start Registration; settings are saved and validated automatically.
6. Monitor statistics, the latest 100 successful accounts, and the latest 200 log lines.

#### Mail Viewer and Local API

1. Select or paste records in `email----password----client_id----refresh_token` format. The password field is retained for compatibility and is not used for IMAP login.
2. Select a mailbox and Inbox/Junk, then enable IDLE to wait for new mail.
3. Enable the local API when another local process needs to retrieve verification codes from `127.0.0.1:47865`.

### Settings and Output

The application creates the following structure next to the EXE:

```text
配置/
├─ 批量授权.json
├─ 刷新令牌和筛号.json
├─ 邮件查看.json
└─ 协议注册.json

结果/
├─ 批量授权/
│  ├─ 授权成功.txt
│  ├─ 授权失败.txt
│  ├─ 绑定后授权失败.txt
│  └─ 处理进度.txt
├─ 刷新令牌和筛号/
│  └─ <per-run directory>/
│     ├─ 成功.txt
│     ├─ 封禁.txt
│     ├─ 风险.txt
│     ├─ 失效.txt
│     ├─ 网络错误.txt
│     ├─ 其他.txt
│     └─ run.json
└─ 协议注册/
   ├─ 注册成功.txt
   ├─ 注册成功但OAuth失败.txt
   ├─ 注册失败.txt
   └─ 历史.json
```

The refresh/filter tool creates TXT files only for categories encountered during that run, so some category files may be absent.

A successful Batch Authorization record uses this format:

```text
email----password----client_id----refresh_token----helper_email----helper_password[----original extra fields...]
```

Protocol Registration records use:

```text
# OAuth2 disabled, or the account was created but OAuth2 failed
email----password

# OAuth2 succeeded
email----password----client_id----refresh_token----temporary_email
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
- Some views mask sensitive fields by default. Protocol Registration's successful-account panel, files on disk, and copied full results are not redacted.
- Store these files securely and remove them when they are no longer needed.

### Contact and Links

- [Download the latest version](https://github.com/ScrapeCraft/503-wr-Tool/releases/latest)
- [Join QQ group 952042396](https://qm.qq.com/q/1oYAEbPgco)
- Author QQ: `1091687244`
- [Affordable datacenter proxies](https://socks5.io/?code=6127RZQA)
- [Buy Microsoft mailboxes](https://cheapemail.cc/)
