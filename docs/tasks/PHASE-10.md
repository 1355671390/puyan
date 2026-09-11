# PHASE-10：AI 基础设施

> Phase：10  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-09 = DONE  
> 下一阶段：PHASE-11 Master Agent  
> 核心目标：在真实业务数据和核心流程已经形成后，正式建立可替换模型、可审计、可重试、可人工审核的 AI 基础设施，为后续全部 Agent 提供统一底座。

---

# 1. Phase 目标

建立：

- LLM Provider 抽象
- Prompt Registry
- Prompt Version
- Agent Definition
- Agent Task
- Queue
- Structured Output
- Retry
- Error
- Review
- Token/成本记录（适用时）
- AI 日志
- 人工接管
- AI Evaluation 基础

本阶段：

> 建 AI 底座，不开发具体 Master/Content/Lead Agent。

---

# 2. AI 总原则

系统内部 AI 必须遵守：

```text
AI 生成
→ 结构化结果
→ 置信度/状态
→ 人工审核
→ 业务执行
```

关键业务不得默认：

```text
AI → 直接对外执行
```

---

# 3. TASK-1001：LLM Provider Interface

统一接口：

```text
generate()
vision()
transcribe()
embed()
```

Provider 实现不得侵入业务代码。

---

# 4. TASK-1002：Provider Registry

支持通过配置启用 Provider。

例如未来：

```text
OpenAI
Anthropic
Gemini
国内模型
Local Model
Desktop Automation Adapter
```

本 Phase 至少实现一个可用于开发测试的 Provider 或 Mock Provider。

---

# 5. TASK-1003：模型配置

配置：

```text
provider
model
temperature
max_output
timeout
retry
```

禁止散落在业务代码里。

---

# 6. TASK-1004：Prompt Registry

建立：

```text
prompt_definitions
prompt_versions
```

字段：

```text
id
name
agent_type
version
content
input_schema
output_schema
status
created_by
created_at
```

---

# 7. TASK-1005：Prompt Version

Prompt 修改：

```text
V1
→ V2
```

不得覆盖历史版本。

每次任务记录使用哪个版本。

---

# 8. TASK-1006：Agent Definition

建立：

```text
agent_definitions
```

描述：

```text
name
purpose
trigger
input_schema
output_schema
provider
model
prompt
requires_review
enabled
```

---

# 9. TASK-1007：Agent Task

建立：

```text
agent_tasks
```

字段：

```text
id
agent_definition_id
trigger_type
input
status
provider
model
prompt_version
output
confidence
review_status
reviewer
error
retry_count
created_at
started_at
completed_at
```

---

# 10. TASK-1008：任务状态机

至少：

```text
QUEUED
RUNNING
WAITING_REVIEW
APPROVED
REJECTED
FAILED
CANCELLED
```

---

# 11. TASK-1009：任务队列

基于 PHASE-00 Worker：

```text
API
→ Redis
→ Worker
→ Provider
→ Result
```

长任务不得阻塞 Web Request。

---

# 12. TASK-1010：Structured Output

每个 Agent 必须定义输出 Schema。

AI 输出先 Validate。

失败：

```text
Repair / Retry
```

超过限制：

```text
FAILED
```

禁止未经验证直接写核心业务表。

---

# 13. TASK-1011：Retry

默认有限重试。

例如：

```text
Max Retry = 2
```

禁止无限循环。

记录每次失败原因。

---

# 14. TASK-1012：Timeout

每个模型调用必须有 Timeout。

超时进入可重试错误。

---

# 15. TASK-1013：人工审核中心

后台新增：

```text
AI 中心
 ├─ 任务
 ├─ 待审核
 ├─ Prompt
 └─ Agent
```

待审核可以：

```text
Approve
Reject
Edit Result
Retry
Cancel
```

---

# 16. TASK-1014：任务详情

显示：

- Agent
- 输入
- Prompt Version
- Model
- 输出
- 状态
- 耗时
- 错误
- Review

敏感数据按权限隐藏。

---

# 17. TASK-1015：AI 日志

记录：

```text
request_id
task_id
provider
model
latency
status
error_type
usage
estimated_cost
```

如果调用路径不产生 Token/费用，则允许为空。

---

# 18. TASK-1016：隐私过滤

客户敏感信息默认不得直接进入外部模型 Prompt。

建立可复用：

```text
PII Redaction Layer
```

至少支持：

- 电话
- 邮箱
- 地址等必要脱敏策略

具体业务若必须使用，应显式授权和记录。

---

# 19. TASK-1017：Prompt Injection 基础防护

未来处理外部网页/文件时：

- 外部内容视为数据
- 不允许外部文本修改系统指令
- Tool 权限与模型输出隔离
- 关键动作人工确认

---

# 20. TASK-1018：AI 写入隔离

Agent 输出先写：

```text
agent_tasks.output
```

审核后再由 Application Service 写入业务表。

禁止模型直接连接数据库执行任意 SQL。

---

# 21. TASK-1019：Mock Agent

实现一个无业务风险的测试 Agent，例如：

```text
Text Summary Demo
```

用于验证完整基础链。

不对外发布。

---

# 22. TASK-1020：API

至少：

```http
GET  /api/v1/agents
GET  /api/v1/agent-tasks
GET  /api/v1/agent-tasks/{id}
POST /api/v1/agent-tasks/{id}/retry
POST /api/v1/agent-tasks/{id}/approve
POST /api/v1/agent-tasks/{id}/reject
POST /api/v1/agent-tasks/{id}/cancel
```

---

# 23. TASK-1021：权限

至少：

```text
ai.agent.read
ai.agent.manage
ai.task.read
ai.task.run
ai.task.review
ai.prompt.read
ai.prompt.manage
```

Prompt 修改属于高权限操作。

---

# 24. TASK-1022：审计

记录：

- Agent Enable/Disable
- Prompt 新版本
- Task Retry
- Approve
- Reject
- 人工编辑结果

---

# 25. TASK-1023：Evaluation 基础

建立：

```text
tests/ai/evals/
```

支持固定输入输出测试集。

目标不是评测“模型聪不聪明”，而是验证：

- Schema 合格率
- Prompt 稳定性
- 输出完整性

---

# 26. TASK-1024：Provider 故障测试

模拟：

```text
Timeout
Rate Limit
Invalid JSON
Provider Down
```

系统不得拖垮业务 API。

---

# 27. TASK-1025：E2E

流程：

```text
后台创建测试 AI Task
→ QUEUED
→ Worker 获取
→ Mock/测试 Provider
→ Structured Output
→ WAITING_REVIEW
→ OWNER 查看
→ Edit/Approve
→ APPROVED
→ Audit Log
```

---

# 28. TASK-1026：成本保护

即使当前用户可能通过桌面自动化等方式调用模型，也要预留：

```text
daily task limit
per-agent concurrency
timeout
provider quota
```

避免失控。

---

# 29. TASK-1027：文档

建立/更新：

```text
AGENTS.md
AI_SECURITY.md
API.md
DATABASE.md
ARCHITECTURE.md
CHANGELOG.md
ROADMAP.md
```

---

# 30. TASK-1028：Completion Report

生成：

```text
docs/reports/PHASE-10-COMPLETION.md
```

报告至少：

```text
Provider
Prompt Versions
Agent Definitions
Task Queue
Review Flow
Failure Tests
Security
E2E
```

---

# 31. Phase Gate

- [ ] Provider 抽象完成
- [ ] Prompt 版本化
- [ ] Agent Definition 完成
- [ ] Agent Task 完成
- [ ] Queue 正常
- [ ] Structured Output 正常
- [ ] Retry 正常
- [ ] Timeout 正常
- [ ] Review 正常
- [ ] Audit 正常
- [ ] 隐私保护基础完成
- [ ] Provider 故障不影响业务系统
- [ ] E2E PASS
- [ ] Completion Report 完成

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```

OWNER 回复：

```text
APPROVED
```

后：

```text
PHASE-10 → DONE
PHASE-11 → READY
```
