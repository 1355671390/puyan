# PHASE-00：项目初始化与工程地基

> Phase：0  
> 状态：READY  
> 优先级：P0  
> 目标：建立稳定、可重复、可测试、可部署、可长期由 Codex 接管的项目工程基础。  
> 前置依赖：无  
> 下一阶段：Phase 1 用户、权限、系统基础

---

# 1. Phase 目标

本 Phase 不开发任何非遗业务功能。

本阶段只完成整个项目未来开发所依赖的工程地基，包括：

- Monorepo 项目结构
- Web 前台
- Admin 管理后台
- FastAPI API
- Worker
- PostgreSQL
- Redis
- MinIO
- Docker / Docker Compose
- 环境变量
- 数据库 Migration
- 基础健康检查
- 基础日志
- 测试框架
- Lint / Type Check
- CI
- Development / Staging / Production 环境约定
- 项目基础文档

完成后必须做到：

```bash
docker compose up -d
```

即可启动完整开发环境。

---

# 2. 本 Phase 明确禁止开发

本 Phase 禁止开发：

- 用户业务系统
- RBAC 业务权限
- 大师管理
- 非遗档案
- 作品管理
- 产品
- CRM
- 订单
- 官网正式页面
- SEO
- AI Agent
- LLM
- 内容系统
- 研学
- 工坊
- 政策
- 财务
- Dashboard 业务指标

允许建立后续需要的技术基础，但禁止提前实现业务逻辑。

---

# 3. 最终目录目标

```text
project/
├── apps/
│   ├── web/
│   └── admin/
│
├── services/
│   ├── api/
│   ├── worker/
│   └── ai/
│
├── packages/
│   ├── ui/
│   ├── types/
│   └── config/
│
├── infra/
│   ├── docker/
│   ├── nginx/
│   └── scripts/
│
├── docs/
│   ├── tasks/
│   └── reports/
│
├── migrations/
├── tests/
│
├── ARCHITECTURE.md
├── DEVELOPMENT_WORKFLOW.md
├── CODEX_RULES.md
├── ROADMAP.md
├── CHANGELOG.md
│
├── .env.example
├── .gitignore
├── docker-compose.yml
├── pnpm-workspace.yaml
├── package.json
└── README.md
```

允许 Codex 根据框架规范增加必要配置文件，但不得改变总体架构。

---

# 4. TASK-0001：开发环境检查

## 目标

确认当前开发机器满足项目要求。

## 检查

```text
Git
Docker
Docker Compose
Node.js LTS
pnpm
Python 3.12+
```

## 输出

记录版本信息。

建议写入：

```text
docs/ENVIRONMENT.md
```

## 验收标准

所有必要工具均可正常执行。

如果缺失依赖：

停止后续任务并报告 OWNER。

---

# 5. TASK-0002：初始化 Monorepo

## 目标

建立统一代码仓库结构。

## 创建

```text
apps/
services/
packages/
infra/
migrations/
tests/
```

建立：

```text
pnpm-workspace.yaml
package.json
```

## 要求

前端相关项目统一使用：

```text
pnpm
```

禁止同时出现：

```text
npm lock
yarn lock
pnpm lock
```

最终只保留：

```text
pnpm-lock.yaml
```

## 验收标准

```bash
pnpm install
```

执行成功。

---

# 6. TASK-0003：初始化 Web 前台

## 目标

建立未来官网/数字文化馆的前端基础。

## 技术

```text
Next.js
React
TypeScript
Tailwind CSS
```

目录：

```text
apps/web
```

## 当前只实现

基础首页：

```text
Non-Heritage Platform
Web Service Running
```

以及：

```text
/api/health
```

或等效健康检查。

## 禁止

不要开始设计正式官网 UI。

## 验收

开发环境可以访问 Web。

页面无 Console Error。

---

# 7. TASK-0004：初始化 Admin 管理后台

## 目标

建立未来主理人管理系统前端。

目录：

```text
apps/admin
```

技术：

```text
Next.js
React
TypeScript
Tailwind CSS
shadcn/ui
```

## 当前只实现

基础 Shell：

```text
Header
Sidebar
Main Content
```

Sidebar 暂时仅显示占位：

```text
Dashboard
System
```

不要提前加入业务菜单。

## 验收

Admin 可独立启动并访问。

---

# 8. TASK-0005：初始化 FastAPI API

## 目标

建立核心后端 API 服务。

目录：

```text
services/api
```

技术：

```text
Python 3.12+
FastAPI
Pydantic
SQLAlchemy
Alembic
```

## API

至少实现：

```http
GET /api/v1/health
```

返回：

```json
{
  "status": "ok"
}
```

建议同时实现：

```http
GET /api/v1/health/ready
```

用于检查依赖是否就绪。

## 验收

FastAPI 可以正常启动。

Swagger/OpenAPI 可以访问。

---

# 9. TASK-0006：PostgreSQL 基础设施

## 目标

建立项目主数据库。

Docker Service：

```text
postgres
```

要求：

- 数据持久化
- 用户名从 ENV 读取
- 密码从 ENV 读取
- 数据库名称从 ENV 读取
- Healthcheck

## API

FastAPI 使用 SQLAlchemy 连接 PostgreSQL。

## 验收

API 可以成功连接数据库。

Ready Health Check 能检测 PostgreSQL 状态。

---

# 10. TASK-0007：Alembic Migration

## 目标

从项目第一天开始建立规范数据库迁移体系。

初始化：

```text
Alembic
```

## 本 Phase

不要创建业务表。

允许创建：

- Migration 基础设施
- 必要系统测试表（如确实需要）

最好保持业务 Schema 为空。

## 验收

以下操作成功：

```bash
alembic upgrade head
```

Migration 可重复执行。

---

# 11. TASK-0008：Redis

## 目标

建立缓存、队列和未来 Agent 基础设施。

Docker Service：

```text
redis
```

## 当前

只建立：

- 服务
- 连接
- Healthcheck

不要实现业务缓存。

## API Ready Check

能够检测 Redis。

## 验收

Redis Ping 成功。

---

# 12. TASK-0009：MinIO

## 目标

建立本地 S3 兼容对象存储。

Docker Service：

```text
minio
```

用途：

未来存储：

- 图片
- 视频
- 音频
- 文档
- 大师资料
- 作品素材

## 当前

只实现：

- MinIO Server
- Bucket 初始化
- ENV 配置
- Healthcheck

例如 Bucket：

```text
heritage-assets
```

## 验收

可以完成：

```text
Upload
Download
Delete
```

测试文件。

---

# 13. TASK-0010：Worker 基础服务

## 目标

建立后台异步任务运行环境。

目录：

```text
services/worker
```

推荐：

```text
Celery
Redis
```

## 当前只实现

测试任务：

```text
health_task
```

例如：

```text
input: ping
output: pong
```

不要实现 AI 任务。

## 验收

API/CLI 可以发送测试任务。

Worker 可以消费。

任务正常完成。

---

# 14. TASK-0011：AI Service 占位

## 目标

为未来 AI 模块预留独立边界。

目录：

```text
services/ai
```

本阶段：

**不接任何真实模型。**

只建立：

```text
README
interfaces
placeholder
```

定义未来大致接口：

```text
generate()
vision()
transcribe()
embed()
```

但不得实现 OpenAI、Claude、Gemini 等具体 Provider。

## 验收

项目架构中 AI Service 边界明确。

无实际模型依赖。

---

# 15. TASK-0012：共享 Packages

建立：

```text
packages/ui
packages/types
packages/config
```

## ui

未来 Web/Admin 共用组件。

当前只需基础结构。

## types

未来共享 TypeScript 类型。

## config

统一：

- ESLint
- TypeScript
- 公共配置

## 验收

Web/Admin 能正确引用 Workspace Package。

---

# 16. TASK-0013：环境变量体系

建立：

```text
.env.example
```

至少规划：

```text
APP_ENV

POSTGRES_HOST
POSTGRES_PORT
POSTGRES_DB
POSTGRES_USER
POSTGRES_PASSWORD

REDIS_HOST
REDIS_PORT

MINIO_ENDPOINT
MINIO_ACCESS_KEY
MINIO_SECRET_KEY
MINIO_BUCKET

API_URL
WEB_URL
ADMIN_URL
```

## 原则

禁止：

- 密钥硬编码
- 密码进入 Git
- Production 密钥进入 `.env.example`

`.env.example` 只放示例值。

## 验收

全新环境：

```bash
cp .env.example .env
```

修改必要配置后即可启动。

---

# 17. TASK-0014：Docker 化

为以下服务建立 Dockerfile：

```text
web
admin
api
worker
```

必要时：

```text
ai
```

## 要求

镜像：

- 尽量小
- 使用明确版本
- 不使用不必要工具
- Production 不运行 Development Server

## 验收

每个镜像都可以独立 Build。

---

# 18. TASK-0015：Docker Compose

统一编排：

```text
web
admin
api
worker
postgres
redis
minio
```

AI Service 如无实际运行需求，可以暂不启动。

## 必须包含

- Network
- Volume
- Healthcheck
- Restart Policy
- ENV

## 核心验收

项目根目录执行：

```bash
docker compose up -d
```

能够启动完整开发环境。

---

# 19. TASK-0016：统一 Health Check

建立：

```text
/api/v1/health
/api/v1/health/ready
```

建议：

`health`

只判断 API 本身。

`ready`

检查：

```text
PostgreSQL
Redis
Object Storage
```

返回示例：

```json
{
  "status": "ready",
  "services": {
    "database": "ok",
    "redis": "ok",
    "storage": "ok"
  }
}
```

## 验收

任意依赖停止后，Ready 状态能反映异常。

---

# 20. TASK-0017：基础日志

## API

使用结构化日志。

至少：

```text
timestamp
level
service
message
request_id
```

## Worker

任务日志至少：

```text
task_id
task_name
status
duration
```

## 禁止

日志中输出：

- 密码
- Token
- Secret
- 完整敏感配置

---

# 21. TASK-0018：Request ID

每个 API 请求生成：

```text
request_id
```

并：

- 返回 Response Header
- 写入日志

方便未来排查：

```text
Web
→ API
→ Worker
→ AI
```

整条链路。

---

# 22. TASK-0019：基础错误处理

统一 API Error Schema。

例如：

```json
{
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "Internal server error",
    "request_id": "..."
  }
}
```

开发环境允许详细错误。

生产环境禁止返回 Stack Trace。

---

# 23. TASK-0020：测试框架

## Python

推荐：

```text
pytest
```

## Frontend

推荐：

```text
Vitest
```

必要时：

```text
React Testing Library
```

## E2E

预留：

```text
Playwright
```

本 Phase 至少测试：

- API Health
- DB Connection
- Redis
- MinIO
- Worker Task

---

# 24. TASK-0021：Lint / Format / Type Check

## TypeScript

配置：

```text
ESLint
Prettier
tsc
```

## Python

推荐：

```text
Ruff
```

必要时：

```text
mypy
```

## 验收

必须存在统一命令，例如：

```bash
pnpm lint
pnpm typecheck
pnpm test
```

Python 同样提供明确命令。

---

# 25. TASK-0022：CI

建立 GitHub Actions。

触发：

```text
push
pull_request
```

执行：

```text
Install
Lint
Type Check
Unit Test
Build
```

## 本阶段

不要自动部署 Production。

## 验收

CI 全部：

```text
PASS
```

---

# 26. TASK-0023：Nginx 基础配置

目录：

```text
infra/nginx
```

预留路由：

```text
/
→ web

/admin
→ admin

/api
→ api
```

实际 Admin 是否最终使用独立子域名可在部署阶段调整。

## 本阶段

建立基础配置即可。

不要做复杂 CDN。

---

# 27. TASK-0024：开发脚本

建立：

```text
infra/scripts/
```

建议：

```text
dev-up
dev-down
dev-reset
db-migrate
test
```

目标：

减少主理人和 Codex 重复输入命令。

---

# 28. TASK-0025：README

项目根目录 `README.md` 必须写明：

- 项目是什么
- 技术栈
- 目录结构
- 环境要求
- 第一次启动
- 停止
- Migration
- 测试
- 常见问题

新开发者/Codex 只看 README 应能启动项目。

---

# 29. TASK-0026：基础安全检查

检查：

- `.env` 是否被 Git Ignore
- Secret 是否硬编码
- Docker 是否暴露不必要端口
- PostgreSQL 是否使用默认弱密码
- MinIO 是否使用默认凭据
- CORS 是否无限开放
- Production Debug 是否关闭

生成：

```text
docs/SECURITY.md
```

---

# 30. TASK-0027：Phase 0 集成测试

执行完整重建测试。

建议流程：

```bash
docker compose down -v

cp .env.example .env

docker compose build

docker compose up -d
```

然后检查：

```text
Web
Admin
API
PostgreSQL
Redis
MinIO
Worker
```

执行：

```text
Migration
Unit Test
Integration Test
Lint
Type Check
Build
```

---

# 31. TASK-0028：Phase 0 文档更新

完成后更新：

```text
README.md
ROADMAP.md
CHANGELOG.md
docs/ENVIRONMENT.md
docs/SECURITY.md
```

ROADMAP：

```text
Phase 0
READY → ACTIVE → WAITING_REVIEW
```

此时：

**不要标记 DONE。**

只有 OWNER APPROVED 后才能标记 DONE。

---

# 32. TASK-0029：生成 Phase Completion Report

生成：

```text
docs/reports/PHASE-00-COMPLETION.md
```

格式：

```text
# Phase 0 Completion Report

## Phase 目标

## 已完成

## 未完成

## 新增模块

## 修改文件

## 数据库变更

## API

## UI

## 基础设施

## 测试结果

## CI 状态

## 安全检查

## 已知问题

## 技术债

## 启动方式

## OWNER 验收步骤

## 下一阶段建议

STATUS: WAITING_FOR_OWNER_APPROVAL
```

---

# 33. Phase 0 最终验收清单

必须全部满足：

- [ ] Git 仓库正常
- [ ] Monorepo 正常
- [ ] Web 正常
- [ ] Admin 正常
- [ ] FastAPI 正常
- [ ] PostgreSQL 正常
- [ ] Redis 正常
- [ ] MinIO 正常
- [ ] Worker 正常
- [ ] Migration 正常
- [ ] Health Check 正常
- [ ] Ready Check 正常
- [ ] `.env.example` 完成
- [ ] Dockerfile 完成
- [ ] Docker Compose 完成
- [ ] Test PASS
- [ ] Lint PASS
- [ ] Type Check PASS
- [ ] Build PASS
- [ ] CI PASS
- [ ] README 完成
- [ ] SECURITY 完成
- [ ] CHANGELOG 更新
- [ ] ROADMAP 更新
- [ ] Completion Report 完成

---

# 34. Phase 0 核心业务验收

在一台满足基础环境要求的机器上，从代码仓库开始：

```bash
git clone <repository>

cd <project>

cp .env.example .env

docker compose up -d
```

无需人工安装 PostgreSQL、Redis、MinIO。

启动完成后：

```text
Web       → 正常
Admin     → 正常
API       → 正常
Database  → Ready
Redis     → Ready
Storage   → Ready
Worker    → Ready
```

然后测试：

```bash
lint
typecheck
test
build
```

全部通过。

---

# 35. Phase 0 不追求的事情

本阶段不评价：

- UI 是否漂亮
- 官网是否完整
- 是否有大师数据
- 是否能卖砚
- 是否有 AI
- 是否有 CRM
- 是否有 Dashboard

Phase 0 只回答一个问题：

> **“我们是否拥有一个可靠的工程底座，可以从这里连续开发未来数年的项目？”**

答案为 YES，Phase 0 才算成功。

---

# 36. Phase 0 完成后的操作

Codex 完成所有任务后：

1. 不进入 Phase 1。
2. ROADMAP 设置：

```text
Phase 0: WAITING_REVIEW
```

3. 输出：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```

4. 停止执行。

只有 OWNER 明确回复：

```text
APPROVED
```

才允许：

```text
Phase 0 → DONE
Phase 1 → READY
```

并开始拆解：

```text
PHASE-01.md
```