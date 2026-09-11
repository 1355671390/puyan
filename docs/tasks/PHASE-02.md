# PHASE-02：大师管理 + 非遗档案

> Phase：02  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-01 = DONE  
> 下一阶段：PHASE-03 作品管理 MVP  
> 核心目标：建立整个项目最核心的文化资产数据库。

---

# 1. Phase 目标

本阶段建立：

- 大师数字档案
- 大师履历
- 荣誉
- 展览
- 媒体
- 师承
- 非遗项目
- 历史
- 石料
- 工具
- 工序
- 技法
- 文献

形成：

> 人 → 非遗 → 工艺 → 资料

基础知识关系。

---

# 2. 禁止事项

禁止：

- Master Agent
- AI 自动整理
- 作品正式模块
- 产品
- CRM
- 内容自动生成

本阶段先把真实数据结构做好。

---

# 3. TASK-0201：大师数据模型

建立：

```text
masters
```

字段至少：

```text
id
heritage_project_id
name
art_name
gender
birth_year
title
heritage_identity
years_of_practice
biography
specialties
status
avatar_asset_id
created_at
updated_at
```

---

# 4. TASK-0202：大师扩展档案

建立：

```text
master_experiences
master_honors
master_exhibitions
master_media
master_lineages
```

分别记录：

- 从艺经历
- 荣誉
- 展览
- 媒体
- 师承关系

---

# 5. TASK-0203：非遗项目

建立：

```text
heritage_projects
```

字段：

```text
id
name
level
project_code
region
applicant
history
culture
description
status
created_at
updated_at
```

架构禁止硬编码“砚台”。

---

# 6. TASK-0204：工艺数据模型

建立：

```text
heritage_materials
heritage_tools
heritage_processes
heritage_techniques
heritage_documents
```

工序支持排序：

```text
sort_order
```

例如：

```text
选料
设计
粗坯
雕刻
打磨
养护
```

---

# 7. TASK-0205：关系模型

支持：

```text
大师 ↔ 非遗项目
大师 ↔ 技法
大师 ↔ 工序
非遗 ↔ 材料
非遗 ↔ 工具
```

为未来知识库做准备。

---

# 8. TASK-0206：Migration

全部模型建立 Migration。

测试：

```text
upgrade
downgrade
upgrade
```

---

# 9. TASK-0207：大师 API

实现：

```http
GET    /api/v1/masters
POST   /api/v1/masters
GET    /api/v1/masters/{id}
PATCH  /api/v1/masters/{id}
```

默认采用 Soft Delete / Archive。

---

# 10. TASK-0208：大师履历 API

实现：

```http
/api/v1/masters/{id}/experiences
/api/v1/masters/{id}/honors
/api/v1/masters/{id}/exhibitions
/api/v1/masters/{id}/media
/api/v1/masters/{id}/lineages
```

---

# 11. TASK-0209：非遗 API

实现：

```http
GET    /api/v1/heritage
POST   /api/v1/heritage
GET    /api/v1/heritage/{id}
PATCH  /api/v1/heritage/{id}
```

---

# 12. TASK-0210：工艺 API

实现：

```text
materials
tools
processes
techniques
documents
```

CRUD。

---

# 13. TASK-0211：权限

新增：

```text
master.read
master.create
master.update

heritage.read
heritage.create
heritage.update
```

MASTER 角色：

允许查看自己的相关资料。

是否允许修改由 OWNER 配置。

---

# 14. TASK-0212：大师列表 UI

新增菜单：

```text
文化资产
 ├─ 大师
 └─ 非遗档案
```

大师列表：

- 姓名
- 身份
- 从艺年限
- 状态
- 最近更新

---

# 15. TASK-0213：大师详情 UI

Tabs：

```text
基础资料
履历
荣誉
展览
媒体
师承
技艺
相关资料
```

---

# 16. TASK-0214：非遗档案 UI

展示：

```text
基础信息
历史源流
地域文化
材料
工具
工序
技法
文献
```

工序支持拖拽排序或明确排序值。

---

# 17. TASK-0215：真实数据录入

必须录入：

```text
当前真实非遗项目 × 1
真实大师 × 2
```

不得只使用 Lorem Ipsum。

两位大师至少完成：

- 基础介绍
- 身份
- 从艺经历
- 核心技艺

---

# 18. TASK-0216：数据完整度

系统计算大师档案完整度：

例如：

```text
68%
```

仅用于内部提醒。

不作为文化价值评分。

---

# 19. TASK-0217：搜索

后台支持：

- 大师姓名
- 非遗名称
- 技法
- 工序
- 材料

初期 PostgreSQL 即可。

---

# 20. TASK-0218：审计

以下操作进入 Audit：

- 修改大师身份
- 修改非遗信息
- 删除/归档资料
- 修改技法
- 修改工序

---

# 21. TASK-0219：测试

至少：

- CRUD
- 权限
- 关系
- 排序
- Archive
- Search

---

# 22. TASK-0220：E2E

流程：

```text
OWNER
→ 创建非遗项目
→ 创建大师 A
→ 创建大师 B
→ 添加履历
→ 添加荣誉
→ 添加石料
→ 添加工具
→ 添加工序
→ 关联技法
→ 搜索
→ 查看完整档案
```

---

# 23. TASK-0221：文档

更新：

```text
DATABASE.md
API.md
CHANGELOG.md
ROADMAP.md
```

---

# 24. TASK-0222：Completion Report

生成：

```text
docs/reports/PHASE-02-COMPLETION.md
```

---

# 25. Phase Gate

- [ ] 真实非遗项目已建档
- [ ] 两位大师已建档
- [ ] 大师履历可管理
- [ ] 荣誉可管理
- [ ] 非遗历史可管理
- [ ] 石料可管理
- [ ] 工具可管理
- [ ] 工序可管理
- [ ] 技法可管理
- [ ] 搜索正常
- [ ] 权限正常
- [ ] 测试 PASS
- [ ] Completion Report 完成

结束：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```