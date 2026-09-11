# PHASE-15：Lead Agent

> Phase：15  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-14 = DONE  
> 下一阶段：PHASE-16  
> 核心目标：在 CRM 已积累真实线索后，用 AI 辅助判断客户类型、意向、预算和下一步行动，让主理人优先处理高价值线索。

---

# 1. Phase 核心原则

在 CRM 已积累真实线索后，用 AI 辅助判断客户类型、意向、预算和下一步行动，让主理人优先处理高价值线索。

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

## TASK-1501

定义 Lead Agent Input，仅使用完成判断所需 CRM 信息并执行 PII 最小化。

## TASK-1502

定义输出 Schema：customer_type、intent_score、budget_signal、need_summary、recommended_products/artworks、next_action、conversion_probability_range、needs_human_attention。

## TASK-1503

禁止模型自动联系客户、自动承诺价格、自动发送报价或自动关闭线索。

## TASK-1504

建立 lead_ai_assessments，保存任务、版本、建议和人工最终判断。

## TASK-1505

实现新 Lead 手动触发和可配置自动触发，但自动触发只生成建议。

## TASK-1506

结合真实作品、产品库存和客户需求做推荐，推荐结果必须引用真实 ID。

## TASK-1507

不存在匹配产品时允许明确输出“暂无合适推荐”，不得虚构商品。

## TASK-1508

建立 CRM 列表 AI Score 排序与筛选。

## TASK-1509

Lead 详情显示 AI 建议与人工字段对比。

## TASK-1510

OWNER/SALES 可接受部分建议、编辑后接受或完全拒绝。

## TASK-1511

建立至少 20 条脱敏真实/历史线索 Eval 集。

## TASK-1512

评估意向分类、客户类型、推荐有效性、格式合格率，不把模型概率当真实统计概率。

## TASK-1513

记录模型、Prompt Version、延迟、错误和审核结果。

## TASK-1514

实现失败降级：AI 不可用时 CRM 完全可人工使用。

## TASK-1515

执行 E2E：真实 Lead→Agent→WAITING_REVIEW→人工确认→排序→创建下一步跟进任务。

# 4. PHASE-15 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-15 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-15 Completion Report

生成：`docs/reports/PHASE-15-COMPLETION.md`。

# 7. PHASE-15 Gate

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
