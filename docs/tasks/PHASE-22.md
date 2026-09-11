# PHASE-22：政策申报辅助

> Phase：22  
> 状态：READY  
> 优先级：P1  
> 前置依赖：PHASE-21 = DONE  
> 下一阶段：PHASE-23  
> 核心目标：在 Policy Agent 找到真实机会后，利用系统内可追溯数据辅助生成申报材料草稿、证据清单和缺失材料列表，但最终申报始终由人工审核。

---

# 1. Phase 目标

在 Policy Agent 找到真实机会后，利用系统内可追溯数据辅助生成申报材料草稿、证据清单和缺失材料列表，但最终申报始终由人工审核。

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

## TASK-2201

建立 applications、application_versions、application_evidence、application_tasks 等模型，并与 policy_match 关联。

## TASK-2202

定义申报状态：DRAFT/PREPARING/WAITING_REVIEW/READY/SUBMITTED/NEEDS_SUPPLEMENT/APPROVED/REJECTED/CLOSED。

## TASK-2203

建立申报项目模板：项目概况、非遗基础、大师情况、数字化保护、产品与经营、研学、工坊、培训就业、乡村振兴成果、实施计划、预算说明、预期成果。

## TASK-2204

定义 Application Assistant Input/Output Schema；只能引用系统真实数据、官方政策要求和人工补充资料。

## TASK-2205

建立 Evidence 引用机制：报告中的人数、金额、场次、作品、培训等数字必须能回到具体系统记录。

## TASK-2206

建立缺失材料检测：营业执照、非遗证明、大师证明、场地资料、照片、合同、财务/经营证明等按政策实际要求生成清单。

## TASK-2207

利用 DAM 选择证明材料，禁止重复上传形成孤立文件。

## TASK-2208

建立申报草稿生成任务，AI 输出先进入版本化 Draft，不直接覆盖人工稿。

## TASK-2209

建立逐章节编辑和来源引用 UI，显示“系统数据/政策原文/人工输入/AI表述”来源类型。

## TASK-2210

建立事实锁定：关键数字和身份信息必须由数据库或人工确认，模型不可自由改写数值。

## TASK-2211

支持导出结构化 Markdown/DOCX 数据接口预留；本 Phase 重点是内容管理，不实现复杂排版系统。

## TASK-2212

建立申报任务清单、负责人、截止时间和完成状态。

## TASK-2213

建立 OWNER Review 与最终 READY Gate；系统不得自动提交政府网站。

## TASK-2214

建立至少一个真实或历史政策案例做完整演练。

## TASK-2215

执行 E2E：政策机会→创建申报→自动拉取真实项目数据→生成草稿→发现缺失材料→补充→人工审核→READY。

# 4. PHASE-22 专项验收

Codex 必须针对本 Phase 建立专项 E2E，并验证成功路径、失败路径、权限、审计、数据追溯以及对既有模块的回归影响。

# 5. PHASE-22 真实业务验证

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
