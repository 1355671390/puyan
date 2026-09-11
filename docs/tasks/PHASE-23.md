# PHASE-23：CEO Agent

> Phase：23  
> 状态：READY  
> 优先级：P1  
> 前置依赖：PHASE-22 = DONE  
> 下一阶段：PHASE-24  
> 核心目标：建立主理人经营助手，把 CRM、订单、B2B、内容、研学、工坊、政策和乡村振兴数据汇总成每日/每周决策简报，但不替代 OWNER 决策。

---

# 1. Phase 目标

建立主理人经营助手，把 CRM、订单、B2B、内容、研学、工坊、政策和乡村振兴数据汇总成每日/每周决策简报，但不替代 OWNER 决策。

继续遵守项目总原则：

```text
业务发生 → 人工跑通 → 工作流标准化 → Codex开发 → Agent接管
```

# 2. 明确禁止事项

- 禁止跨 Phase 开发。
- 禁止为了完成 Roadmap 编造业务数据。
- 禁止覆盖原始资料或历史版本。
- 禁止绕过 OWNER 审核关键动作。
- 禁止为了平台化破坏已经验证的砚台业务链。

# 3. 详细 Task

## TASK-2301

定义 CEO Agent 只读数据权限和数据访问层，禁止模型直接修改核心业务表。

## TASK-2302

定义 Daily Brief Input/Output Schema：经营摘要、客户、订单、B2B、内容、库存、研学、工坊、政策机会、异常、风险、待决策事项。

## TASK-2303

定义 Weekly Review Schema：本周变化、关键指标、漏斗、问题、机会、下周优先级和需 OWNER 决策项。

## TASK-2304

建立 ceo_briefs 或等效结果模型，保存数据快照时间、Prompt Version、输出、审核状态。

## TASK-2305

所有数值必须来自系统聚合接口；模型只能解释，不得自行计算或编造未提供的数据。

## TASK-2306

建立异常规则层：逾期跟进、低库存、B2B交付临近、政策截止、异常退款、研学准备未完成等优先由确定性规则产生。

## TASK-2307

AI 在规则结果和经营数据之上生成摘要与建议，避免完全依赖模型发现异常。

## TASK-2308

建立“待主理人决策”卡片结构：问题、证据、建议选项、影响、截止时间。

## TASK-2309

允许 OWNER 将建议转换为普通系统任务，但不得由 CEO Agent 自动执行高风险操作。

## TASK-2310

建立 Dashboard CEO Brief 页面，支持今日、历史和周报。

## TASK-2311

建立定时任务：每日简报和每周复盘；失败时不影响业务系统。

## TASK-2312

建立通知接口预留；本 Phase 可在后台展示，不强制接微信/短信。

## TASK-2313

建立敏感数据最小化，CEO Prompt 不需要完整客户联系方式。

## TASK-2314

建立固定 Eval：数字引用准确、异常覆盖、建议可执行性、无虚构事实。

## TASK-2315

执行 E2E：准备跨模块真实/测试数据→生成 Daily Brief→核对每个数字→OWNER查看→把一个建议转成待办→Audit。

# 4. PHASE-23 专项验收

Codex 必须针对本 Phase 建立专项 E2E，并验证成功路径、失败路径、权限、审计、数据追溯以及对既有模块的回归影响。

# 5. PHASE-23 真实业务验证

存在真实业务时必须使用真实或脱敏后的真实数据验证；不存在真实业务时必须明确标注 SIMULATION，不得将模拟结果计入对外项目成果。


---

# 通用工程要求

## Migration

所有数据库变化必须使用 Migration，并完成：

```text
upgrade
→ downgrade test
→ upgrade
```

不得手工修改生产表。

## API

新增 API 必须具备：

- Schema 校验
- RBAC
- Request ID
- 统一错误结构
- OpenAPI
- Unit / Integration Test

## UI

新增 UI 必须处理：

- Loading
- Empty
- Error
- Permission
- Responsive
- 高风险操作确认

## AI 安全

适用 AI 的 Phase 必须遵守：

```text
业务数据
→ 受控上下文
→ AI Task
→ Structured Output
→ Validation
→ WAITING_REVIEW
→ 人工确认
→ Application Service
→ 业务数据
```

禁止模型直接操作数据库或未经审核对外提交关键内容。

## 审计

以下操作必须可追溯：

- 关键状态变化
- 人工修改 AI 结果
- Approve / Reject
- 重要数据导出
- 跨项目权限变化

## 回归测试

不得破坏既有核心链：

```text
大师
→ 作品
→ DAM
→ 官网
→ Lead
→ CRM
→ 订单
→ Dashboard
```

同时执行：

```text
Unit Test
Integration Test
E2E
Lint
Type Check
Build
```

## 文档

按实际更新：

```text
ARCHITECTURE.md
DATABASE.md
API.md
AGENTS.md
SECURITY.md
DEPLOYMENT.md
CHANGELOG.md
ROADMAP.md
```

---

# Completion Report

生成：

```text
docs/reports/PHASE-XX-COMPLETION.md
```

必须包含：

```text
Phase目标
已完成
未完成
数据库变更
API
UI
Agent/Workflow
权限
测试
E2E
安全
真实业务验证
已知问题
技术债
迁移/部署说明
OWNER验收步骤
下一步建议
```

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```

---

# Phase Gate

- [ ] P0 Task 全部完成
- [ ] Migration PASS
- [ ] RBAC/Audit PASS
- [ ] Unit/Integration PASS
- [ ] E2E PASS
- [ ] 原核心链回归 PASS
- [ ] 安全检查 PASS
- [ ] 文档完成
- [ ] Completion Report 完成
- [ ] 无未处理 P0/P1 Bug

Codex 完成后必须停止。

只有 OWNER 明确回复：

```text
APPROVED
```

才能将当前 Phase 标记为 DONE。

PHASE-24 完成并获得 OWNER APPROVED 后：

```text
ROADMAP V1 COMPLETE
```

此后不得自行新增 PHASE-25。

新的开发阶段必须由 OWNER 基于真实业务重新制定 ROADMAP V2。
