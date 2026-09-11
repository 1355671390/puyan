# CODEX RULES

## 项目开发最高规则

1. 开始任何开发工作前，必须首先阅读：
   - ARCHITECTURE.md
   - DEVELOPMENT_WORKFLOW.md
   - ROADMAP.md

2. DEVELOPMENT_WORKFLOW.md 是项目 Phase 顺序的唯一依据。

3. 禁止跨 Phase 开发。

4. 每次只执行当前 Phase。

5. 开始 Phase 前：
   - 分析当前 Phase
   - 检查前置依赖
   - 生成任务清单
   - 向 OWNER 汇报执行计划

6. 开发过程中：
   - 所有数据库修改必须使用 Migration
   - 所有关键业务必须测试
   - 不得删除原始业务数据
   - 不得擅自改变系统总体架构
   - 不得提前开发未来 Phase 功能
   - 不得因为“顺便”而扩大 Scope

7. 遇到架构冲突：
   停止开发并向 OWNER 汇报。

8. 遇到需求不明确：
   不自行假设关键业务规则，向 OWNER 汇报。

9. 每完成一个 Task：
   - 执行相关测试
   - 更新任务状态
   - 记录重要变更

10. 每完成一个 Phase：
    - 完成全部测试
    - 检查数据库 Migration
    - 检查安全问题
    - 更新 CHANGELOG.md
    - 更新 ROADMAP.md
    - 生成 Phase Completion Report

11. Phase Completion Report 保存到：

    docs/reports/

12. 报告最后必须输出：

    STATUS: WAITING_FOR_OWNER_APPROVAL

13. 输出报告后立即停止。

14. 未收到 OWNER 明确回复：

    APPROVED

    禁止进入下一 Phase。

15. OWNER 拥有项目最终决策权。

---

## 开发原则

优先级：

业务正确性
> 数据安全
> 可维护性
> 测试
> 性能
> 开发速度

原则：

先业务，后自动化。
先人工跑通，再让 Agent 接管。
原始数据永远保留。
AI 输出必须可审核。
所有业务数据必须可追溯。