# PHASE-12：作品 AI Agent

> Phase：12  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-11 = DONE  
> 下一阶段：PHASE-13  
> 核心目标：利用真实作品图片、基础档案、大师口述和知识库辅助生成作品档案内容，但始终由人工审核后写入作品数据。

---

# 1. Phase 核心原则

利用真实作品图片、基础档案、大师口述和知识库辅助生成作品档案内容，但始终由人工审核后写入作品数据。

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

## TASK-1201

定义 Artwork AI Agent 的 Input/Output Schema，输入包括 artwork_id、图片、已有字段、大师、材料和可选口述。

## TASK-1202

定义输出：候选名称、创作故事、工艺说明、石料描述、文化说明、SEO摘要、社交媒体短文案、缺失字段提示。

## TASK-1203

建立 artwork_ai_drafts 或等效审核草稿模型，禁止直接覆盖 artworks 原始字段。

## TASK-1204

接入 PHASE-04 DAM 图片与 PHASE-11 大师知识库作为受控上下文。

## TASK-1205

实现图片选择与 Vision 调用流程；不允许 AI 凭图片臆断无法确认的石种、年代、作者或价格。

## TASK-1206

实现来源标记：哪些内容来自数据库、哪些来自大师知识、哪些是 AI 表述建议。

## TASK-1207

建立事实字段保护规则，作者、石种、尺寸、编号、价格、状态等不得由模型未经确认自动修改。

## TASK-1208

后台作品详情增加“AI辅助整理”入口。

## TASK-1209

建立 AI Draft 对比审核 UI：现有值 / AI建议 / OWNER最终值。

## TASK-1210

支持字段级接受、全部接受、拒绝、编辑后接受。

## TASK-1211

实现 Prompt 版本化和固定 Eval 数据集，至少选择 10 件真实作品。

## TASK-1212

建立幻觉检查规则：无法从输入确认的信息必须输出 unknown/needs_review。

## TASK-1213

实现失败、超时、重试、取消和人工补录。

## TASK-1214

审核通过后由 Application Service 写入作品档案，并产生 Audit Log。

## TASK-1215

执行 E2E：真实作品→选素材→AI任务→WAITING_REVIEW→人工修改→批准→作品档案更新。

# 4. PHASE-12 专项 E2E 验收

Codex 必须根据以上 Task 建立覆盖本 Phase 主业务链的 E2E 测试；不仅验证页面可打开，还必须验证数据库状态、权限、历史记录、关联数据和失败路径正确。

# 5. PHASE-12 真实业务验收

不得仅使用 Mock/Lorem Ipsum 宣布完成。凡本 Phase 已存在现实业务数据，应使用真实或脱敏后的真实数据完成至少一次 OWNER 可复核的业务流程。

# 6. PHASE-12 Completion Report

生成：`docs/reports/PHASE-12-COMPLETION.md`。

# 7. PHASE-12 Gate

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
