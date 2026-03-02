# IT-devops (Go + Vue)

本仓库包含 DevOps 相关的三部分：Go 后端服务、备份代理以及前端界面。

## 项目介绍

### devops-admin-go — 后端管理服务

> 详见 [devops-admin-go/README.md](devops-admin-go/README.md)

基于 Go（Gin + GORM）的后端服务。

- **接口层**：REST API（统一入口 `/api`），Handler -> Service -> Repository（GORM）
- **数据层**：MySQL 8.0（业务数据）、Redis（缓存/Token 存储）
- **安全层**：JWT + Redis + Casbin（RBAC 权限控制）
- **可观测**：Zap 日志
- **生态集成**：备份代理回调、对象存储（MinIO）、SNMP 设备管理

核心模块：`sys`（系统基础）、`auth`（认证授权）、`oss`（对象存储）、`ops`（运维资产与备份）、`alert`（告警）、`job`（定时任务）、`snmp`（SNMP 设备管理）

```
devops-ui -> devops-admin-go(/api) -> MySQL/Redis
                     |-> MinIO(对象存储)
                     |-> SNMP 设备
devops-backup-agent --(callback)--> devops-admin-go
```

### devops-backup-agent — 网络设备备份代理

> 详见 [devops-backup-agent/README.md](devops-backup-agent/README.md)

基于 Python 3.12（FastAPI + SSH + MinIO）的备份代理。

- **入口**：HTTP API（`/backup`）接收后端下发的设备清单
- **执行**：按设备型号选择适配器，通过 SSH 拉取配置
- **存储**：上传到对象存储（默认 MinIO，S3 协议）
- **回调**：将结果回调到 `devops-admin-go` 的备份回调接口
- **支持设备**：华为 VRP、H3C ComWare、飞塔 FortiOS、锐捷 RGOS

### devops-ui — 前端界面

> 详见 [devops-ui/README.md](devops-ui/README.md)

基于 Vue 3 + Vite + TypeScript 的管理前端。

- **交互**：通过 `src/service` 调用 `devops-admin-go` REST API（`/api` 前缀）
- **权限**：登录获取 Token，按菜单/角色渲染路由与按钮权限
- **页面模块**：系统管理（sys）、运维资产与备份（ops）、告警管理（alert）、定时任务（job）、日志查询（log）、对象存储（oss）、监控看板（monitor）、SNMP 管理（snmp）

## 目录结构

```
.
├── devops-admin-go/     # 后端服务（Go + Gin）
├── devops-backup-agent/ # 网络设备备份代理（Python + FastAPI）
├── devops-ui/           # 前端界面（Vue 3 + Vite）
├── docker/              # Docker 相关
├── nginx/               # Nginx 配置
└── docker-compose.yaml  # Docker Compose 编排
```

## 快速开始

### 方式一：Docker Compose（推荐）

```bash
# 复制环境变量
cp devops-admin-go/.env.example devops-admin-go/.env

# 启动所有服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

### 方式二：本地开发

#### devops-admin-go

- 需要：Go 1.21+、MySQL 8.0、Redis
- 安装依赖：`go mod download`
- 配置数据库：`mysql -u root -p devops < devops-admin/db/mysql.sql`
- 修改配置：编辑 `devops-admin-go/configs/config.yaml`
- 运行：`cd devops-admin-go && go run cmd/server/main.go`

#### devops-ui

- 需要：Node.js 18+、npm
- 安装依赖：`cd devops-ui && npm install`
- 本地开发：`npm run dev`
- 构建：`npm run build`

#### devops-backup-agent

- 需要：Python 3.12、pip
- 安装依赖：`pip install -r requirements.txt`
- 运行：`python -m backup_agent.main --config-file config.yaml`

## Docker Compose 服务组成

| 服务 | 说明 | 端口 |
|------|------|------|
| mysql | MySQL 8.0 数据库 | 3306 |
| redis | Redis 7.x 缓存 | 6379 |
| devops-admin | Go 后端服务 | 8080 |
| nginx | 前端 + 反向代理 | 80 |

## 配置入口

- Go 后端：`devops-admin-go/configs/config.yaml`
- 前端：`devops-ui/.env.development`、`devops-ui/.env.production`
- 备份代理：`devops-backup-agent/config.yaml`

## 技术栈

### 后端 (devops-admin-go)
- Go 1.21+
- Gin (Web 框架)
- GORM (ORM)
- JWT + Casbin (认证授权)
- Zap (日志)
- Viper (配置)

### 前端 (devops-ui)
- Vue 3
- Vite
- TypeScript
- Element Plus

### 备份代理 (devops-backup-agent)
- Python 3.12
- FastAPI
- Paramiko (SSH)
- MinIO SDK

## License

MIT