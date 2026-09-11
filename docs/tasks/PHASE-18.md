# PHASE-18：研学管理

> Phase：18  
> 状态：READY  
> 优先级：P1  
> 前置依赖：PHASE-17 = DONE  
> 下一阶段：PHASE-19  
> 核心目标：在出现真实研学业务后，管理学校、课程、场次、人数、分组、物料、人员、签到、证书和报价，并自动生成执行清单。

---

# 1. Phase 核心原则

在出现真实研学业务后，管理学校、课程、场次、人数、分组、物料、人员、签到、证书和报价，并自动生成执行清单。

本 Phase 必须继续遵守：

```text
业务先跑通
→ 数据结构化
→ 工作流标准化
→ AI/自动化辅助
→ 人工审核关键动作
```

# 2. 本 Phase 禁止事项

- 禁止跨 Phase 提前开发后续模块。
- 禁止为了“以后可能需要”扩大当前 Scope。
- 禁止删除或覆盖原始业务数据。
- 禁止绕过 RBAC、Audit、Migration 和测试。
- AI 相关 Phase 禁止模型未经人工审核直接执行高风险业务动作。

# 3. 详细 Task

## TASK-1801

上线前确认存在真实或明确即将落地的研学业务；若无，Phase Gate 可 HOLD，不得为了 Roadmap 强行开发。

## TASK-1802

建立 courses、study_events、study_groups、study_participants/aggregate、study_materials、study_staff、study_certificates。

## TASK-1803

课程模板包含年龄段、时长、人数范围、目标、流程、材料、安全要求和收费参考。

## TASK-1804

建立“一方砚的诞生”等首个真实课程模板。

## TASK-1805

场次字段：学校/机构、日期、人数、年级、时长、地点、联系人、报价、状态。

## TASK-1806

支持按人数自动生成分组建议、材料数量、工具数量和人员需求。

## TASK-1807

建立场次流程：INQUIRY/QUOTED/CONFIRMED/PREPARING/IN_PROGRESS/COMPLETED/CANCELLED。

## TASK-1808

建立签到机制；未成年人数据遵循最小化原则，优先按学校/分组统计，非必要不收集个人敏感信息。

## TASK-1809

建立证书编号与批量生成元数据，不强制收集学生完整身份信息。

## TASK-1810

建立教师/大师/助理排班与冲突检查。

## TASK-1811

建立材料采购/准备清单，但不开发复杂采购 ERP。

## TASK-1812

建立研学报价与 B2B/订单关联。

## TASK-1813

后台建立课程、场次、日历、执行清单 UI。

## TASK-1814

实现自动资源计划：输入120人/五年级/3小时→分组、材料、人员、流程建议；规则优先，AI可后续辅助。

## TASK-1815

场次完成后记录人数、收入、反馈、素材、成果，为乡村振兴数据中心提供来源。

## TASK-1816

执行 E2E：学校咨询→课程→120人场次→报价→确认→准备→签到→完成→证书/总结。

# 4. PHASE-18 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-18 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-18 Completion Report

生成：`docs/reports/PHASE-18-COMPLETION.md`。

# 7. PHASE-18 Gate

- [ ] 本 Phase 全部 P0 Task 完成
- [ ] 数据 Migration 可重复执行
- [ ] 权限与 Audit 正常
- [ ] Unit / Integration Test PASS
- [ ] 专项 E2E PASS
- [ ] 核心旧业务链未被破坏
- [ ] 真实业务数据验证完成（适用时）
- [ ] P0/P1 Bug 已解决
- [ ] 文档更新完成
- [ ] Completion Report 完成


---

# 开发与验收通用要求

## 数据库

所有 Schema 变化必须通过 Alembic Migration：

```text
design
→ migration
→ upgrade
→ downgrade test
→ upgrade
```

禁止手工修改生产数据库。

## API

所有新增 API 必须具备：

- Pydantic Schema
- 参数校验
- RBAC
- 统一错误结构
- Request ID
- OpenAPI
- Unit/Integration Test

## UI

所有新增页面必须处理：

- Loading
- Empty
- Error
- Permission
- Responsive
- Confirmation（高风险操作）

## 审计

关键业务变更必须进入 Audit Log。

## 测试

至少执行：

```text
Unit Test
Integration Test
相关 E2E
Lint
Type Check
Build
```

任何 Phase 不允许破坏核心链：

```text
大师
→ 作品
→ 素材
→ 官网
→ 客户
→ CRM
→ 订单
→ Dashboard
```

## 文档更新

按实际模块更新：

```text
DATABASE.md
API.md
ARCHITECTURE.md
AGENTS.md
SECURITY.md
CHANGELOG.md
ROADMAP.md
```

不得为了完成清单修改无关文档。

## Completion Report

生成：

```text
docs/reports/PHASE-XX-COMPLETION.md
```

报告必须包含：

```text
Phase 目标
已完成
未完成
新增/修改模块
数据库 Migration
API
UI
AI/Workflow（适用时）
权限
测试结果
E2E
安全检查
真实业务数据验证
已知问题
技术债
部署/迁移注意事项
OWNER 验收步骤
下一 Phase 建议
```

最后必须：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```

## Phase Gate

只有满足本 Phase 核心业务验收、P0/P1 Bug 已处理、测试通过、文档完成，才可进入 WAITING_REVIEW。

OWNER 明确回复：

```text
APPROVED
```

之后才允许：

```text
当前 PHASE → DONE
下一 PHASE → READY
```

未获得 APPROVED：

> 禁止 Codex 开始下一 Phase。
