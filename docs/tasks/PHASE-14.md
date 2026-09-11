# PHASE-14：订单系统

> Phase：14  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-13 = DONE  
> 下一阶段：PHASE-15  
> 核心目标：把个人作品、标准产品和后续 B2B 交易统一沉淀为订单，实现从 CRM 成交到收款、发货、完成和售后的业务闭环。

---

# 1. Phase 核心原则

把个人作品、标准产品和后续 B2B 交易统一沉淀为订单，实现从 CRM 成交到收款、发货、完成和售后的业务闭环。

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

## TASK-1401

建立 orders、order_items、payments、shipments、order_status_history 等核心模型。

## TASK-1402

订单类型支持 ARTWORK、PRODUCT、MIXED，并为 B2B 订单预留关联字段。

## TASK-1403

定义状态：DRAFT/PENDING_PAYMENT/PAID/PROCESSING/READY_TO_SHIP/SHIPPED/COMPLETED/CANCELLED/AFTER_SALES。

## TASK-1404

订单项支持唯一 Artwork 与 Product Variant；售出孤品必须防止重复销售。

## TASK-1405

建立金额模型：商品金额、折扣、运费、应收、已收、退款；使用 Decimal，禁止浮点金额。

## TASK-1406

本阶段允许人工登记支付，不强制接第三方在线支付。

## TASK-1407

支付记录必须保存方式、金额、时间、凭证/备注、操作人，并可审计。

## TASK-1408

建立收货地址最小必要数据与隐私权限。

## TASK-1409

实现订单 CRUD、状态转换、支付登记、发货登记 API。

## TASK-1410

实现库存事务：产品支付/确认后按规则预留或扣减，取消时释放；必须与 PHASE-05 流水一致。

## TASK-1411

实现 Artwork 状态联动：RESERVED/SOLD，并处理取消恢复规则。

## TASK-1412

CRM Lead 可转换为订单，保留来源链路。

## TASK-1413

后台建立订单列表、详情、支付、发货、售后基础 UI。

## TASK-1414

建立订单编号规则与唯一索引。

## TASK-1415

实现并发购买孤品和库存并发扣减测试。

## TASK-1416

执行 E2E：CRM线索→订单→支付→库存/作品状态→发货→完成→Dashboard 数据一致。

# 4. PHASE-14 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-14 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-14 Completion Report

生成：`docs/reports/PHASE-14-COMPLETION.md`。

# 7. PHASE-14 Gate

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
