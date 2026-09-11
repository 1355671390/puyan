# PHASE-16：B 端项目管理

> Phase：16  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-15 = DONE  
> 下一阶段：PHASE-17  
> 核心目标：建立银行、企业、政府、学校、国企、协会等批量定制项目的独立销售与交付管理能力。

---

# 1. Phase 核心原则

建立银行、企业、政府、学校、国企、协会等批量定制项目的独立销售与交付管理能力。

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

## TASK-1601

建立 b2b_projects、b2b_contacts、b2b_requirements、quotes、quote_items、samples、contracts 基础模型。

## TASK-1602

B2B 项目字段包括客户、用途、数量、预算、交付日期、定制要求、负责人、阶段。

## TASK-1603

定义阶段：LEAD/QUALIFIED/PROPOSAL/SAMPLE/NEGOTIATION/CONTRACTED/PRODUCTION/DELIVERY/COMPLETED/LOST。

## TASK-1604

与 CRM Customer/Lead 建立关系，禁止重复建立孤立客户库。

## TASK-1605

建立报价版本管理，报价修改不得覆盖历史版本。

## TASK-1606

报价项可引用 Artwork、Product Variant 或 Custom Item。

## TASK-1607

支持数量阶梯、单价、定制费、包装费、运费、税费备注等，不做完整税务 ERP。

## TASK-1608

建立样品记录：样品内容、寄送、反馈、状态。

## TASK-1609

建立合同元数据与 DAM 文件关联；不开发电子签章。

## TASK-1610

建立 B2B 项目列表、Kanban/阶段视图、详情、报价和样品 UI。

## TASK-1611

实现企业礼赠模板能力，例如100套/300套场景，但模板仅辅助创建项目。

## TASK-1612

建立交付里程碑：方案、确认、样品、合同、生产、包装、发货。

## TASK-1613

与订单系统关联，合同确认后可创建 B2B Order。

## TASK-1614

建立权限：销售可管理项目，敏感报价/合同按权限查看。

## TASK-1615

建立 300 套非遗礼赠模拟项目作为 E2E 标准案例。

## TASK-1616

执行 E2E：企业线索→B2B项目→需求→报价V1/V2→样品→合同→订单→交付。

# 4. PHASE-16 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-16 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-16 Completion Report

生成：`docs/reports/PHASE-16-COMPLETION.md`。

# 7. PHASE-16 Gate

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
