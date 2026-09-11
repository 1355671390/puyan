# PHASE-01：用户、权限与系统基础

> Phase：01  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-00 = DONE  
> 下一阶段：PHASE-02 大师管理 + 非遗档案  
> 核心目标：建立后台登录、用户、角色、权限、审计、系统配置等所有业务模块共同依赖的基础能力。

---

# 1. Phase 目标

本阶段完成：

- 用户账户
- 登录/登出
- Session / Token
- 密码安全
- RBAC
- 页面权限
- API 权限
- 审计日志
- 系统设置
- 数据字典
- 编号规则基础
- 管理员基础界面

完成后：

> 系统已经具备安全地让不同角色进入后台并执行不同操作的能力。

---

# 2. 明确禁止

本阶段禁止开发：

- 大师业务
- 非遗档案
- 作品
- 产品
- CRM
- 订单
- AI Agent
- 正式 Dashboard
- 官网业务

---

# 3. TASK-0101：用户数据模型

建立：

```text
users
roles
permissions
user_roles
role_permissions
```

`users` 至少：

```text
id
username
email
phone
password_hash
display_name
status
last_login_at
created_at
updated_at
```

状态：

```text
ACTIVE
DISABLED
LOCKED
```

禁止明文密码。

---

# 4. TASK-0102：RBAC 数据模型

预置角色：

```text
SUPER_ADMIN
OWNER
MASTER
OPERATOR
SALES
WORKSHOP
TEACHER
FINANCE
VIEWER
```

权限格式建议：

```text
resource.action
```

例如：

```text
user.read
user.create
user.update
user.delete

system.read
system.update
```

---

# 5. TASK-0103：Migration

创建正式 Migration。

要求：

- Upgrade
- Downgrade
- Index
- Unique Constraint
- Foreign Key

执行：

```bash
alembic upgrade head
```

必须成功。

---

# 6. TASK-0104：初始化 OWNER

建立 Seed。

只允许通过环境变量或初始化命令创建首个 OWNER/SUPER_ADMIN。

不得在代码中硬编码正式密码。

---

# 7. TASK-0105：认证 API

实现：

```http
POST /api/v1/auth/login
POST /api/v1/auth/logout
GET  /api/v1/auth/me
POST /api/v1/auth/change-password
```

登录失败不得泄露：

- 用户是否存在
- 密码具体错误原因

---

# 8. TASK-0106：密码安全

至少：

- 强密码规则
- 安全 Hash
- 密码修改
- 登录失败限制

推荐：

```text
Argon2id
```

---

# 9. TASK-0107：后台登录页面

建立：

```text
/admin/login
```

包含：

- 用户名/邮箱
- 密码
- 登录
- Loading
- Error

禁止业务菜单未登录访问。

---

# 10. TASK-0108：认证守卫

Admin 所有受保护页面必须经过 Authentication Guard。

未登录：

```text
→ /login
```

API 未认证：

```http
401 Unauthorized
```

---

# 11. TASK-0109：权限守卫

建立：

```text
PermissionGuard
```

前端用于：

- 菜单
- 按钮
- 页面

后端用于：

- API

原则：

> 前端隐藏不是安全措施，最终权限必须由 API 判断。

---

# 12. TASK-0110：用户管理 API

实现：

```http
GET    /api/v1/users
POST   /api/v1/users
GET    /api/v1/users/{id}
PATCH  /api/v1/users/{id}
POST   /api/v1/users/{id}/disable
POST   /api/v1/users/{id}/enable
```

默认不提供物理删除。

---

# 13. TASK-0111：角色权限 API

实现：

```http
GET  /api/v1/roles
GET  /api/v1/permissions
PUT  /api/v1/users/{id}/roles
PUT  /api/v1/roles/{id}/permissions
```

---

# 14. TASK-0112：用户管理 UI

后台：

```text
系统
 ├─ 用户管理
 ├─ 角色管理
 ├─ 权限查看
 └─ 系统设置
```

用户列表：

- 搜索
- 状态
- 角色
- 创建
- 编辑
- 禁用

---

# 15. TASK-0113：审计日志模型

建立：

```text
audit_logs
```

记录：

```text
id
user_id
action
resource_type
resource_id
request_id
ip
user_agent
before_data
after_data
created_at
```

审计日志原则上不可普通删除。

---

# 16. TASK-0114：自动审计

至少记录：

- 登录
- 登录失败
- 修改用户
- 禁用用户
- 修改角色
- 修改权限
- 修改系统设置

---

# 17. TASK-0115：审计日志 UI

支持：

- 时间
- 用户
- 操作
- 资源
- Request ID

过滤和详情查看。

---

# 18. TASK-0116：系统设置

建立：

```text
system_settings
```

管理：

- 项目名称
- 品牌名称
- 联系电话
- 联系邮箱
- 地址
- 默认语言
- 时区
- 作品编号前缀
- 上传限制

敏感设置与普通设置分离。

---

# 19. TASK-0117：数据字典

建立：

```text
dictionaries
dictionary_items
```

未来用于：

- 客户来源
- 作品状态
- 产品分类
- 工艺类型

不要把所有业务枚举写死在前端。

---

# 20. TASK-0118：后台基础布局完善

Admin Shell：

```text
Header
Sidebar
Breadcrumb
Main
User Menu
```

当前菜单只开放：

```text
系统管理
```

未来业务菜单暂不创建。

---

# 21. TASK-0119：安全测试

测试：

- 未登录访问
- 无权限访问
- 禁用账户
- 错误密码
- Token/Session 失效
- 越权 API
- 修改他人权限

---

# 22. TASK-0120：E2E

完整流程：

```text
OWNER 登录
→ 创建 OPERATOR
→ 分配角色
→ OPERATOR 登录
→ 可访问授权页面
→ 无法访问管理员权限
→ OWNER 禁用
→ OPERATOR 无法继续使用
```

---

# 23. TASK-0121：文档

更新：

```text
DATABASE.md
API.md
SECURITY.md
CHANGELOG.md
ROADMAP.md
```

---

# 24. TASK-0122：Completion Report

生成：

```text
docs/reports/PHASE-01-COMPLETION.md
```

必须包含：

- Migration
- API
- UI
- RBAC
- 安全测试
- E2E
- 已知问题

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```

---

# 25. Phase Gate

必须：

- [ ] 登录正常
- [ ] 用户管理正常
- [ ] RBAC 正常
- [ ] API 权限正常
- [ ] 审计正常
- [ ] 系统设置正常
- [ ] Test PASS
- [ ] Build PASS
- [ ] Security PASS
- [ ] Completion Report 完成

OWNER 回复：

```text
APPROVED
```

后才能：

```text
PHASE-01 → DONE
PHASE-02 → READY
```