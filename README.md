# 🌐 SNMP MIB 监控平台 (SQLite 版本)

<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Next.js](https://img.shields.io/badge/Next.js-15-black.svg)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue.svg)](https://www.typescriptlang.org/)
[![Go](https://img.shields.io/badge/Go-1.23-00ADD8.svg)](https://golang.org/)
[![SQLite](https://img.shields.io/badge/SQLite-3-003B57.svg)](https://sqlite.org/)
[![Production Ready](https://img.shields.io/badge/Production-Ready-success.svg)](#)

**[🇨🇳 中文](README.md) | [🇺🇸 English](README_EN.md)**

</div>

> 🚀 **企业级SNMP网络监控平台。** 一个轻量级的独立解决方案，采用Go后端和Next.js前端。它无需Docker、PostgreSQL或Redis等任何外部依赖，部署极其简单。

## ✨ 主要特性

- 🚀 **零依赖部署**: 无需Docker，无需外部数据库。作为独立的二进制文件运行。
- 🗃️ **SQLite驱动**: 简单的基于文件的数据库，易于管理和备份。
- 💾 **内存缓存**: 无需Redis即可为关键操作提供高性能缓存。
- 🔧 **强大的工具集**: 包括设备发现、MIB管理、配置生成和批量操作。
- 📊 **监控集成**: 为Prometheus、Categraf和其他监控系统生成配置。
- 📱 **现代化UI**: 使用Next.js和shadcn/ui构建的响应式、直观的Web界面。
- 🌐 **多语言支持**: 支持中英文切换。

## 🛠️ 技术栈

- **前端**: Next.js 15, React 19, TypeScript, Tailwind CSS, shadcn/ui
- **后端**: Go 1.23, Gin, GORM
- **数据库**: SQLite 3 及内存缓存 (LRU)
- **部署**: 独立二进制文件，可选`systemd`服务集成。

## 🏗️ 系统架构

平台作为两个主要进程运行：一个用Go编写的后端API服务器和一个由Next.js驱动的前端服务器。

```mermaid
graph TD
    subgraph "用户设备"
        A[Web浏览器]
    end

    subgraph "服务器"
        B[Next.js 前端 :12300]
        C[Go 后端 API :17880]
        D[SQLite 数据库 (snmp_platform.db)]
        E[内存缓存]
    end

    subgraph "网络设备"
        F[交换机, 路由器等]
    end

    A --> B
    B --> C
    C --> D
    C --> E
    C -- SNMP --> F
```

## 🚀 快速入门

### 📋 环境要求

- **操作系统**: Linux / macOS / Windows
- **Node.js**: `v18.0` 或更高版本
- **Go**: `v1.21` 或更高版本

### ⚡ 安装与部署

1.  **克隆代码库:**
    ```bash
    git clone https://github.com/evan7434/snmp-mib-ui.git
    cd snmp-mib-ui
    ```

2.  **运行简化部署脚本:**
    该脚本将为您安装依赖、构建前端和后端，并创建启动/停止脚本。
    ```bash
    ./deploy-simple.sh
    ```

3.  **启动服务:**
    ```bash
    ./start.sh
    ```

现在，应用程序已成功运行！

### 📱 访问地址

| 服务 | URL | 描述 |
|---|---|---|
| 🌐 **Web界面** | `http://localhost:12300` | 主要的管理用户界面 |
| 🔌 **API服务** | `http://localhost:17880` | 后端API |

### 🔧 服务管理

使用生成的脚本来管理平台：

-   **启动服务**: `./start.sh`
-   **停止服务**: `./stop.sh`
-   **检查状态**: `./status.sh`
-   **查看日志**: `tail -f logs/frontend.log` 或 `tail -f logs/backend.log`

关于更高级的部署选项，例如作为`systemd`服务运行，请参阅[部署指南](docs/DEPLOYMENT.md)。

## 📖 功能概览

该平台为网络管理提供了一整套全面的工具：

-   **设备管理**: 发现、注册和监控启用SNMP的设备。按类型、供应商或位置对它们进行分组。
-   **MIB管理**: 通过用户友好的OID树状图上传、解析和浏览MIB文件。
-   **配置生成**: 自动为`SNMP Exporter`、`Categraf`和`Prometheus`创建配置。
-   **告警规则管理**: 一个强大的编辑器，用于为`Prometheus`和`VictoriaMetrics`创建和部署告警规则。
-   **监控安装器**: 通过SSH在其他主机上远程安装和管理监控代理。
-   **批量操作**: 一次性在多台设备上执行测试连接或更新配置等操作。

## 📚 文档

- **[部署指南](docs/DEPLOYMENT.md)**: 生产环境设置的详细说明，包括`systemd`和反向代理。
- **[API参考](docs/API.md)**: 后端RESTful API的文档。
- **[开发指南](docs/DEVELOPMENT.md)**: 如何设置开发环境。
- **[故障排除指南](docs/troubleshooting.md)**: 常见问题的解决方案。

## 🤝 贡献

我们欢迎各种贡献！有关如何提交拉取请求、报告错误和建议功能的详细信息，请参阅[CONTRIBUTING.md](CONTRIBUTING.md)。

## 📄 许可证

本项目根据[MIT许可证](LICENSE)开源。

---
<div align="center">
⭐ 如果这个项目对您有帮助，请给它一个星标！ ⭐
</div>
