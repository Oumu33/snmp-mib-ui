# 🌐 SNMP MIB Monitoring Platform (SQLite Edition)

<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Next.js](https://img.shields.io/badge/Next.js-15-black.svg)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue.svg)](https://www.typescriptlang.org/)
[![Go](https://img.shields.io/badge/Go-1.23-00ADD8.svg)](https://golang.org/)
[![SQLite](https://img.shields.io/badge/SQLite-3-003B57.svg)](https://sqlite.org/)
[![Production Ready](https://img.shields.io/badge/Production-Ready-success.svg)](#)

**[🇨🇳 中文](README.md) | [🇺🇸 English](README_EN.md)**

</div>

> 🚀 **Enterprise-Grade SNMP Network Monitoring Platform.** A lightweight, standalone solution using a Go backend and Next.js frontend. It requires zero external dependencies like Docker, PostgreSQL, or Redis, making it incredibly easy to deploy.

## ✨ Key Features

- 🚀 **Zero-Dependency Deployment**: No Docker, no external databases. Runs as standalone binaries.
- 🗃️ **SQLite Powered**: Simple, file-based database. Easy to manage and back up.
- 💾 **In-Memory Cache**: High-performance caching for key operations without needing Redis.
- 🔧 **Powerful Tooling**: Includes device discovery, MIB management, configuration generation, and batch operations.
- 📊 **Monitoring Integration**: Generates configurations for Prometheus, Categraf, and other monitoring systems.
- 📱 **Modern UI**: A responsive, intuitive web interface built with Next.js and shadcn/ui.
- 🌐 **Multi-language Support**: Switch between English and Chinese.

## 🛠️ Technology Stack

- **Frontend**: Next.js 15, React 19, TypeScript, Tailwind CSS, shadcn/ui
- **Backend**: Go 1.23, Gin, GORM
- **Database**: SQLite 3 with in-memory caching (LRU)
- **Deployment**: Standalone binaries, with optional `systemd` service integration.

## 🏗️ Architecture

The platform runs as two main processes: a backend API server written in Go and a frontend server powered by Next.js.

```mermaid
graph TD
    subgraph "User's Machine"
        A[Web Browser]
    end

    subgraph "Server"
        B[Next.js Frontend :12300]
        C[Go Backend API :17880]
        D[SQLite Database (snmp_platform.db)]
        E[In-Memory Cache]
    end

    subgraph "Network Devices"
        F[Switches, Routers, etc.]
    end

    A --> B
    B --> C
    C --> D
    C --> E
    C -- SNMP --> F
```

## 🚀 Quick Start

### 📋 Prerequisites

- **OS**: Linux / macOS / Windows
- **Node.js**: `v18.0` or newer
- **Go**: `v1.21` or newer

### ⚡ Installation & Deployment

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/evan7434/snmp-mib-ui.git
    cd snmp-mib-ui
    ```

2.  **Run the simple deployment script:**
    This script will install dependencies, build the frontend and backend, and create start/stop scripts for you.
    ```bash
    ./deploy-simple.sh
    ```

3.  **Start the services:**
    ```bash
    ./start.sh
    ```

The application is now running!

### 📱 Access URLs

| Service | URL | Description |
|---|---|---|
| 🌐 **Web Interface** | `http://localhost:12300` | The main management UI. |
| 🔌 **API Server** | `http://localhost:17880` | The backend API. |

### 🔧 Service Management

Use the generated scripts to manage the platform:

-   **Start services**: `./start.sh`
-   **Stop services**: `./stop.sh`
-   **Check status**: `./status.sh`
-   **View logs**: `tail -f logs/frontend.log` or `tail -f logs/backend.log`

For more advanced deployment options, such as running as a `systemd` service, see the [Deployment Guide](docs/DEPLOYMENT.md).

## 📖 Features Overview

This platform provides a comprehensive set of tools for network management:

-   **Device Management**: Discover, register, and monitor SNMP-enabled devices. Group them by type, vendor, or location.
-   **MIB Management**: Upload, parse, and browse MIB files through a user-friendly OID tree.
-   **Configuration Generation**: Automatically create configurations for `SNMP Exporter`, `Categraf`, and `Prometheus`.
-   **Alert Rule Management**: A powerful editor to create and deploy alert rules for `Prometheus` and `VictoriaMetrics`.
-   **Monitoring Installer**: Remotely install and manage monitoring agents on other hosts via SSH.
-   **Batch Operations**: Perform actions like testing connectivity or updating configurations across many devices at once.

## 📚 Documentation

- **[Deployment Guide](docs/DEPLOYMENT.md)**: Detailed instructions for production setup, including `systemd` and reverse proxies.
- **[API Reference](docs/API.md)**: Documentation for the backend RESTful API.
- **[Development Guide](docs/DEVELOPMENT.md)**: How to set up a development environment.
- **[Troubleshooting Guide](docs/troubleshooting.md)**: Solutions for common issues.

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to submit pull requests, report bugs, and suggest features.

## 📄 License

This project is open source under the [MIT License](LICENSE).

---
<div align="center">
⭐ If this project helps you, please give it a Star! ⭐
</div>
