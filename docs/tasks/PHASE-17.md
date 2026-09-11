# PHASE-17：财务经营 V1

> Phase：17  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-16 = DONE  
> 下一阶段：PHASE-18  
> 核心目标：建立经营分析层，回答收入、成本、毛利、订单、客单价、渠道和项目盈利情况，但不替代专业会计/ERP。

---

# 1. Phase 核心原则

建立经营分析层，回答收入、成本、毛利、订单、客单价、渠道和项目盈利情况，但不替代专业会计/ERP。

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

## TASK-1701

明确 Finance V1 边界：经营分析，不做总账、税务申报、应收会计准则或完整三大报表。

## TASK-1702

建立经营收入/成本聚合服务，优先从订单、支付、产品成本、作品成本和 B2B 项目读取。

## TASK-1703

统一金额 Decimal 与币种规则。

## TASK-1704

建立成本类别基础：作品成本、产品标准成本、包装、物流、定制、外协、其他直接成本。

## TASK-1705

建立可选 expense_records 用于录入项目直接费用，并要求凭证/备注。

## TASK-1706

计算订单收入、已收金额、直接成本、毛利和毛利率。

## TASK-1707

计算产品/SKU、作品、客户、渠道、B2B项目维度经营结果。

## TASK-1708

建立 Dashboard Finance 页面：收入、订单、客单价、毛利、渠道转化、B2B项目表现。

## TASK-1709

提供时间范围：本月、季度、自定义。

## TASK-1710

退款/取消必须正确冲减相关经营统计。

## TASK-1711

数据必须可 Drill Down 到订单/费用来源。

## TASK-1712

建立 FINANCE 权限与敏感金额权限。

## TASK-1713

所有人工成本调整和费用修改进入 Audit Log。

## TASK-1714

建立数据一致性检查：Finance 汇总与订单支付合计可对账。

## TASK-1715

执行 E2E：个人作品订单+产品订单+B2B订单+费用→收入/成本/毛利统计正确。

## TASK-1716

生成经营分析文档说明所有指标公式，避免未来口径漂移。

# 4. PHASE-17 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-17 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-17 Completion Report

生成：`docs/reports/PHASE-17-COMPLETION.md`。

# 7. PHASE-17 Gate

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
