# 技术栈取证（不预设语言与架构）

版权：德贝信息技术部

扫描时不确定本仓用何种语言、何种架构。没有命中的生态**整节省略**。版本只抄文件里的，抄不到写「未见」。禁止用常见项目模板补全。

## 跳过目录

`node_modules`、`dist`、`build`、`bin`、`obj`、`.git`、`vendor`、NuGet `packages`、`__pycache__`、压缩包、第三方还原结果。

## 证据 → 记录什么

| 若仓库里有 | 则记录 | 启动线索（有才写） |
|---|---|---|
| `package.json`、锁文件、`.nvmrc`、`engines` | Node 包管理器、依赖与版本 | `scripts` 里的 `dev` / `start` / `build` / `test` |
| `*.sln`、`*.csproj`、`global.json`、`Directory.Build.props` | TFM、SDK 或旧式 csproj、Web/Worker/WinForms/WPF | 解决方案里**实际启动项目**，不是每个 csproj |
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

## 持久化怎么找

- ORM 实体、`DbContext`、`schema.prisma`、MyBatis XML
- 仓库内 `.sql`
- 手写 SQL：检索 `FROM` / `INSERT INTO`（注意动态拼表名）
- 仅有连接串键、无实体：写「库由配置某键指定；表结构以用户文件为准」

**不要**根据这些证据生成表结构文档。与用户表结构文件比对时，只列出不一致项待确认。

## 风格抽样

在最小地图指出的业务目录里，选 2～3 个**已完成**的同类功能（例如两个列表+保存），不要选空壳生成代码。每个样本记下路径，结论必须能指回这些文件。
