# PHASE-24：多非遗架构验证

> Phase：24  
> 状态：READY  
> 优先级：P1  
> 前置依赖：PHASE-23 = DONE  
> 下一阶段：ROADMAP V1 COMPLETE  
> 核心目标：验证系统是否真正从“砚台专用软件”升级为可复用的非遗数字化运营平台，在不破坏现有砚台业务的前提下加入第二个模拟/真实非遗项目。

---

# 1. Phase 目标

验证系统是否真正从“砚台专用软件”升级为可复用的非遗数字化运营平台，在不破坏现有砚台业务的前提下加入第二个模拟/真实非遗项目。

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

## TASK-2401

全面扫描代码、数据库、Prompt、URL、枚举、组件和配置中的砚台硬编码，形成 HARD_CODE_AUDIT.md。

## TASK-2402

确认所有核心业务实体具备 heritage_project_id 或明确的跨项目归属策略。

## TASK-2403

建立 Project Context/Scope，OWNER 可切换当前非遗项目，查询默认按授权项目隔离。

## TASK-2404

验证 Master、Heritage、Artwork、Product、DAM、CRM、Order、Content、Agent、Study、Workshop、Policy 等模块的项目隔离。

## TASK-2405

建立第二非遗测试项目，建议使用竹编/刺绣/陶瓷之一；仅使用模拟或明确可用数据，不伪装成真实合作项目。

## TASK-2406

为第二项目建立大师、技法、作品、产品、素材等最小数据，验证无需修改核心代码即可运行。

## TASK-2407

检查编号规则是否支持项目级前缀，避免 YAN 等砚台专属前缀成为系统硬依赖。

## TASK-2408

检查 UI 文案，把必须通用的“砚台/砚”替换为项目配置或通用术语；砚台品牌前台允许继续保留业务文案。

## TASK-2409

检查 Agent Prompt：通用 Agent 不得默认用户一定经营砚台；项目专属 Prompt 通过 heritage context 注入。

## TASK-2410

检查搜索、SEO 和公开 URL 的多项目策略，保证现有砚台 URL 不因重构失效。

## TASK-2411

检查权限：用户可被授权一个或多个 heritage_project，跨项目数据不得越权。

## TASK-2412

建立数据导出/统计时的项目维度，避免两个非遗项目指标混算。

## TASK-2413

完成数据库 Migration 与回填，任何重构不得丢失现有砚台数据、编号、QR、订单或 CRM 关联。

## TASK-2414

执行完整回归测试：原砚台项目核心链全部 PASS。

## TASK-2415

执行第二项目 E2E：创建非遗→大师→作品→素材→产品→内容/官网数据→CRM/订单最小链路。

## TASK-2416

生成 MULTI_HERITAGE_VALIDATION.md，列出可复用模块、仍专属模块、技术债和下一版平台化建议。

# 4. PHASE-24 专项验收

Codex 必须针对本 Phase 建立专项 E2E，并验证成功路径、失败路径、权限、审计、数据追溯以及对既有模块的回归影响。

# 5. PHASE-24 真实业务验证

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
