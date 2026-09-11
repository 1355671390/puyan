# PHASE-21：Policy Agent

> Phase：21  
> 状态：READY  
> 优先级：P1  
> 前置依赖：PHASE-20 = DONE  
> 下一阶段：PHASE-22  
> 核心目标：建立政策发现、归档、匹配和提醒能力，让系统持续识别与非遗、传统工艺、文旅、研学、乡村振兴、就业、工坊、数字化相关的政策机会。

---

# 1. Phase 目标

建立政策发现、归档、匹配和提醒能力，让系统持续识别与非遗、传统工艺、文旅、研学、乡村振兴、就业、工坊、数字化相关的政策机会。

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

## TASK-2101

建立 policies、policy_sources、policy_matches 等模型，字段包含发布机关、层级、地区、发布时间、申报期限、原文来源、适用主体、支持方向和状态。

## TASK-2102

政策来源必须优先使用政府/主管部门官方公开来源；保存原始 URL、抓取时间、正文快照/摘要和来源可信度。

## TASK-2103

建立关键词与主题体系：非遗、传统工艺、文旅融合、研学、乡村振兴、就业创业、非遗工坊、数字化保护、文化产业、人才培训等。

## TASK-2104

定义 Policy Agent Input/Output Schema，输出匹配分、匹配理由、适用主体、主管部门、截止时间、所需材料、建议动作和风险提示。

## TASK-2105

匹配时读取 PHASE-20 的真实项目指标和基础资料，禁止用 AI 编造项目成果。

## TASK-2106

建立政策去重、更新版本和失效/过期状态；同一政策不同转载不得形成重复机会。

## TASK-2107

建立定时任务接口，可按日/周执行；抓取器与 Agent 解耦，单一来源失败不得影响其他来源。

## TASK-2108

建立政策列表、详情、来源原文、匹配结果和待处理机会 UI。

## TASK-2109

建立高匹配机会视图，允许 OWNER 标记关注、忽略、准备申报、已申报。

## TASK-2110

政策截止时间进入系统待办；本 Phase 只提醒，不自动向政府提交任何材料。

## TASK-2111

建立匹配解释：必须明确引用哪些项目事实导致匹配，不只给一个黑盒分数。

## TASK-2112

建立人工复核机制，OWNER 可调整匹配结果并记录原因。

## TASK-2113

建立 Policy Agent 固定 Eval 集，覆盖国家/省/市/县不同政策样例。

## TASK-2114

建立安全与合规检查：政策摘要不得替代官方原文，关键申报条件必须提示人工核对。

## TASK-2115

执行 E2E：导入官方政策→解析→Agent 匹配→WAITING_REVIEW→OWNER确认→进入机会列表→生成截止待办。

# 4. PHASE-21 专项验收

Codex 必须针对本 Phase 建立专项 E2E，并验证成功路径、失败路径、权限、审计、数据追溯以及对既有模块的回归影响。

# 5. PHASE-21 真实业务验证

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
