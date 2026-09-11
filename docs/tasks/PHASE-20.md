# PHASE-20：乡村振兴数据中心

> Phase：20  
> 状态：READY  
> 优先级：P1  
> 前置依赖：PHASE-19 = DONE  
> 下一阶段：PHASE-21 Policy Agent  
> 核心目标：把工坊、培训、就业、劳务、产出、研学和企业合作等真实业务数据汇总为可追溯的乡村振兴成果数据，为政府汇报和后续 Policy Agent 提供可信底座。

---

# 1. Phase 核心原则

把工坊、培训、就业、劳务、产出、研学和企业合作等真实业务数据汇总为可追溯的乡村振兴成果数据，为政府汇报和后续 Policy Agent 提供可信底座。

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

## TASK-2001

定义指标口径文档，任何对外数字必须有明确数据来源和计算公式。

## TASK-2002

建立 rural_metrics 或采用聚合视图，避免手工维护无法追溯的“成果数字”。

## TASK-2003

汇总参与工匠人数、活跃人员、培训人数/人次、培训时长。

## TASK-2004

汇总就业/协作人数、劳务结算金额等可确认指标。

## TASK-2005

汇总工坊生产任务、合格产出、参与工序。

## TASK-2006

汇总研学场次、参与人数、合作学校/机构。

## TASK-2007

汇总产品/作品销售和 B2B 合作中与项目相关的经营数据。

## TASK-2008

建立数据时间维度：月、季度、年度。

## TASK-2009

建立指标 Drill Down，每个数字必须能回到 training_record、work_log、study_event、order、b2b_project 等来源。

## TASK-2010

建立成果 Dashboard：就业、培训、生产、研学、销售、合作。

## TASK-2011

建立政府汇报数据导出，只导出汇总和允许公开的信息。

## TASK-2012

建立“数据说明/口径”展示，防止不同报告使用不同算法。

## TASK-2013

禁止 AI 或人工直接填写夸大成果；人工调整必须注明来源、原因和审核人。

## TASK-2014

建立异常检测基础：负数、重复统计、跨期重复、缺失来源。

## TASK-2015

为 PHASE-21 Policy Agent 提供标准化只读指标 API。

## TASK-2016

执行 E2E：培训+工坊任务+研学+订单+B2B→乡村振兴 Dashboard→逐项追溯原始记录。

# 4. PHASE-20 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-20 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-20 Completion Report

生成：`docs/reports/PHASE-20-COMPLETION.md`。

# 7. PHASE-20 Gate

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
