# 技术栈取证（不预设语言与架构）

版权：德贝信息技术部

没有命中的生态整节省略。版本只抄文件里的，抄不到写「未见」。禁止用常见项目模板补全。

证据冲突：锁文件 / 工程文件 > 清单（package.json、csproj）> README。

多路径：前后端分仓按路径分节记录，不合成一套架构。

用户给的若是子目录：可上探一层找 `.sln` / 多个工程；候选多于一个则列出请用户确认，不猜。

## 分析时忽略的目录

`node_modules`、`dist`、`build`、`bin`、`obj`、`.git`、`.vs`、`.idea`、`.svn`、`vendor`、NuGet `packages`、`__pycache__`、`target`、`.next`、`.nuxt`、`coverage`、压缩包、第三方还原结果。

## 证据 → 记录什么

| 若仓库里有 | 则记录 | 启动线索（有才写） |
|---|---|---|
| `package.json`、锁文件、`.nvmrc`、`engines` | Node 包管理器、依赖与版本；Vue2/3 等以锁文件为准 | `scripts` 里的 `dev` / `start` / `build` / `test` |
| `*.sln`、`*.csproj`、`packages.config`、`global.json`、`Directory.Build.props` | TFM；SDK 风格或旧式 csproj / netfx；`OutputType`；Web / MVC / Web API / WebForms / WCF / Worker / WinForms / WPF / IIS 宿主（见到才写） | 解决方案里**实际启动项目**，不是每个 csproj |
| 引用 SqlSugar / Dapper / EF / EasyUI / Layui / DevExpress | 见到才记，不按常见方案补 | — |
| `pom.xml`、`build.gradle`、`settings.gradle` | Java 与构建工具、Spring 等 | 主类、`bootRun` / `mvn spring-boot:run` |
| `go.mod` | Go 模块 | `cmd/` 或 `main.go` |
| `pyproject.toml`、`requirements.txt`、`Pipfile` | Python 与框架 | `manage.py`、`uvicorn`、入口模块 |
| `Cargo.toml` | Rust crate | `[[bin]]`、`src/main.rs` |
| `composer.json`、`*.php` | PHP 与框架 | `artisan`、`public/index.php` |
| `AndroidManifest.xml`、`*.xcodeproj`、`Podfile`、`pubspec.yaml` | 移动端 | applicationId / scheme / `flutter` |
| `CMakeLists.txt`、`*.vcxproj`、`*.pro` | 原生 C/C++ / Qt | 可执行目标名 |
| `Dockerfile`、`compose*.yml`、流水线 yml | 部署形态 | 只记存在，不生成新流水线 |
| 仅有 DLL / so / jar、无对应源码 | 外部依赖 | 标「无源码」，不反编译 |

## 请求入口怎么找（有什么找什么）

不要假设一定是 Vue + WebApi。按已识别的形态找：

- HTTP：`Controller`、`router.`、`@app.`、`FastAPI`、`app.get`、云函数入口
- 桌面：`Program.cs` WinForms/WPF、主窗体、菜单 Click
- 后台：Hangfire / Quartz / Worker / `IHostedService` / Windows Service
- 移动：Activity / 路由表
- 其它见到才写：WCF / SOAP、SignalR、gRPC

## 持久化怎么找

- ORM 实体、`DbContext`、`schema.prisma`、MyBatis XML、SqlSugar / Dapper / EF（见到才写）
- 仓库内 `.sql`
- 手写 SQL：检索 `FROM` / `INSERT INTO`（注意动态拼表名）
- 仅有连接串键、无实体：写「库由配置某键指定」；有用户库表文件才对照，未提供则不反推表

**不要**根据这些证据生成表结构文档。用户提供了库表文件时，只列出不一致项待确认。

## 风格抽样

在最小地图指出的业务目录里，选 2～3 个**已完成**的同类功能（例如两个列表+保存）。不要选空壳，不要选 `*.Designer.cs`、`.g.cs`、T4、Swagger 客户端。每个样本记下**绝对路径**，结论必须能指回这些文件。

## 路径怎么写（产出不在项目根）

- 指向源码、库表、字段解释、启动项：**绝对路径**
- 产出目录内互链：相对文件名（`技术栈.md`、`最小地图.md`、`推断功能.md`）

## 版本管理怎么记（每条项目根单独记）

不要假设前端一定 Git、后端一定 TFS。用户在路径上标明了就用用户的；未标明则：

- 根下有 `.git`：记 Git
- `tf workfold <该根>` 成功或存在 `$tf`：记 TFVC
- 都没有：写「未见」，开发前再问用户

## 改代码时（扫描禁止）

按将改文件所属项目根：

- **Git**：直接改；不要 `tf checkout` / `tf add`；不要在扫描阶段 `git commit` / `push`
- **TFVC**：改已有文件前 `tf checkout <绝对路径>`（不在 PATH 则用 VS Team Explorer 下 `TF.exe` 全路径）。占用：输出含 `locked for check-out by USER`、`checked out by USER in workspace WS`、`锁定`、`TF14098`。把 **USER / 工作区** 告诉用户，**不改该文件**。解析不到人名则贴 `tf` 原文，仍不改。新文件写入后 `tf add`。找不到 `tf.exe` 或不在工作区：说明原因，不假装已签出
