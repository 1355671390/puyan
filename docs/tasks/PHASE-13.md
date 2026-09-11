# PHASE-13：Content Agent

> Phase：13  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-12 = DONE  
> 下一阶段：PHASE-14  
> 核心目标：把大师知识、作品、工艺素材和客户高频问题转化为可审核的多渠道内容候选，提高内容生产效率。

---

# 1. Phase 核心原则

把大师知识、作品、工艺素材和客户高频问题转化为可审核的多渠道内容候选，提高内容生产效率。

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

## TASK-1301

建立 content_items、content_versions、content_publications 等内容模型。

## TASK-1302

定义内容状态：MATERIAL/CANDIDATE/DRAFT/WAITING_REVIEW/APPROVED/PUBLISHED/ARCHIVED。

## TASK-1303

建立内容类型：短视频选题、脚本、标题、字幕建议、公众号/长文、SEO文章、FAQ、社交短文案。

## TASK-1304

建立渠道：官网、抖音、视频号、小红书、公众号等，仅作为内容适配，不在本 Phase 自动发布。

## TASK-1305

定义 Content Agent Input：knowledge_item、artwork、assets、customer_question、channel、goal。

## TASK-1306

输出严格 Schema：主题、目标受众、Hook、结构、正文/脚本、素材引用、CTA、风险提示。

## TASK-1307

实现“一条知识→至少3种内容形式”的生成能力。

## TASK-1308

所有事实型内容必须保留 source_refs，可回到大师知识或作品档案。

## TASK-1309

建立内容候选池 UI，支持筛选来源、渠道、状态、主题。

## TASK-1310

建立编辑器与版本历史；人工修改不得覆盖 AI 原始输出。

## TASK-1311

建立 OWNER Review：Approve/Reject/Edit。

## TASK-1312

实现重复主题检测基础逻辑，避免连续生成高度重复内容。

## TASK-1313

建立客户 FAQ 输入入口，把 CRM 中高频问题转为内容候选，但不得暴露客户个人信息。

## TASK-1314

建立内容效果字段接口，为未来录入播放、互动、咨询等表现预留。

## TASK-1315

建立固定 Eval：文化准确性、事实引用完整度、格式合格率、重复度。

## TASK-1316

执行 E2E：大师知识/作品→Content Agent→3种候选→审核→APPROVED→形成待发布内容。

# 4. PHASE-13 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-13 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-13 Completion Report

生成：`docs/reports/PHASE-13-COMPLETION.md`。

# 7. PHASE-13 Gate

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
