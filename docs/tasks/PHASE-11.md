# PHASE-11：Master Agent

> Phase：11  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-10 = DONE  
> 下一阶段：PHASE-12  
> 核心目标：把两位大师的采访、口述、视频和既有资料转化为可审核、可追溯的结构化知识库。

---

# 1. Phase 核心原则

把两位大师的采访、口述、视频和既有资料转化为可审核、可追溯的结构化知识库。

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

## TASK-1101

定义大师知识条目 knowledge_items 及来源引用模型，保留原视频、原音频、原始转录、AI整理版和人工审核版。

## TASK-1102

建立采访素材导入流程：DAM素材→创建转录任务→分段→知识提取→待审核。

## TASK-1103

实现 Transcription Adapter，复用 PHASE-10 Provider，不把具体模型写死在业务代码。

## TASK-1104

建立 transcript_segments，保存时间码、说话人、原文、来源 asset_id。

## TASK-1105

建立知识分类：石料、工具、工序、技法、历史、审美、作品故事、师承、人物经历、行业知识、FAQ。

## TASK-1106

实现知识条目与 master、heritage_project、technique、process、artwork 的关联。

## TASK-1107

实现 Master Agent Definition、Prompt Version 与严格 Structured Output Schema。

## TASK-1108

建立知识提取任务：标题、摘要、正文、标签、分类、来源片段、置信度。

## TASK-1109

后台建立“大师知识库”列表、详情、来源回看和待审核页面。

## TASK-1110

审核支持 Approve、Reject、Edit、Merge、Split；任何 AI 结果不得直接成为已发布知识。

## TASK-1111

实现知识检索 API，优先 PostgreSQL FTS；为后续向量检索保留接口。

## TASK-1112

实现来源追溯：每条知识必须能回到具体素材和时间码。

## TASK-1113

录入/采访两位真实大师，建立首批真实知识条目。

## TASK-1114

建立固定 AI Eval 集，至少覆盖两位大师、工艺、石料、历史、作品故事等。

## TASK-1115

完成权限、审计、失败重试、人工接管和隐私检查。

## TASK-1116

执行 E2E：上传大师采访→转录→分段→提取→审核→知识库检索→回看原始来源。

# 4. PHASE-11 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-11 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-11 Completion Report

生成：`docs/reports/PHASE-11-COMPLETION.md`。

# 7. PHASE-11 Gate

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
