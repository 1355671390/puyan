# PHASE-19：工坊管理

> Phase：19  
> 状态：READY  
> 优先级：P1  
> 前置依赖：PHASE-18 = DONE  
> 下一阶段：PHASE-20  
> 核心目标：在真实出现学徒、村民或生产协作人员后，管理人员、技能等级、培训、生产任务、工时、质量和劳务数据。

---

# 1. Phase 核心原则

在真实出现学徒、村民或生产协作人员后，管理人员、技能等级、培训、生产任务、工时、质量和劳务数据。

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

## TASK-1901

上线前确认存在真实工坊人员或明确生产协作需求；否则 Phase Gate 可 HOLD。

## TASK-1902

建立 workshop_people、workshop_skills、workshop_person_skills、training_records、workshop_tasks、work_logs、quality_checks。

## TASK-1903

技能等级统一 L0-L5，并允许不同工序分别评级。

## TASK-1904

建立可训练工序：选料辅助、粗磨、打磨、包装、简单雕刻、研学协助等，实际由大师确认。

## TASK-1905

禁止系统把大师核心技艺简单量化替代；技能等级仅用于生产/培训管理。

## TASK-1906

建立人员档案最小字段：姓名/编号/联系方式权限化/加入时间/状态/角色。

## TASK-1907

建立培训记录：课程、导师、时间、工序、结果、备注。

## TASK-1908

建立生产任务：作品/产品/批次、工序、数量、负责人、计划时间、状态。

## TASK-1909

任务只能分配给具备相应技能等级的人员，允许 OWNER/MASTER 特批并记录原因。

## TASK-1910

建立工时记录与劳务计件/计时基础字段，但不做工资税务系统。

## TASK-1911

建立质量检查：检查人、标准、结果、返工、报废原因。

## TASK-1912

与 Product/Inventory/B2B 项目建立必要关联。

## TASK-1913

后台建立人员、技能矩阵、培训、任务、质量 UI。

## TASK-1914

MASTER 角色可参与技能确认和质量审核。

## TASK-1915

所有技能升级、质量结论和劳务调整可审计。

## TASK-1916

执行 E2E：新学员→培训→技能L1→分配打磨任务→工时→质检→通过→任务完成。

# 4. PHASE-19 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-19 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-19 Completion Report

生成：`docs/reports/PHASE-19-COMPLETION.md`。

# 7. PHASE-19 Gate

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
