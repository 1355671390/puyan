# PHASE-06：CRM MVP

> Phase：06  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-05 = DONE  
> 下一阶段：PHASE-07 主理人驾驶舱 V1  
> 核心目标：建立统一客户、线索、来源、需求、预算、跟进与成交记录体系，把当前通过天眼查、电话、熟人介绍等方式出现的真实需求全部沉淀为可分析的数据资产。

---

# 1. Phase 目标

本阶段完成：

- 客户管理
- 线索管理
- 来源管理
- 跟进记录
- 意向等级
- 预算
- 用途
- 感兴趣作品/产品
- 下一次跟进时间
- 成交/未成交结果
- 客户搜索与筛选
- 基础 CRM 统计

本阶段成功后，系统必须能够回答：

```text
本月来了多少客户？
客户从哪里找到我们？
客户主要想买什么？
预算集中在哪些区间？
哪些客户需要今天跟进？
哪些客户最后成交？
为什么没有成交？
```

---

# 2. 本阶段明确禁止

禁止开发：

- AI Lead Agent
- 自动外呼
- 自动发微信
- 自动报价
- 复杂营销自动化
- 完整订单系统
- B2B 项目管理
- 广告投放系统
- 自动抓取天眼查数据

当前重点是：

> 先把真实客户记录完整，再谈 AI 自动判断。

---

# 3. TASK-0601：客户模型

建立：

```text
customers
```

建议字段：

```text
id
heritage_project_id
customer_type
name
company_name
phone
email
wechat
region
address
notes
status
created_at
updated_at
```

客户类型：

```text
INDIVIDUAL
COLLECTOR
ENTERPRISE
BANK
STATE_OWNED_ENTERPRISE
GOVERNMENT
SCHOOL
ASSOCIATION
SCENIC_AREA
CULTURAL_TOURISM
DEALER
MEDIA
OTHER
```

---

# 4. TASK-0602：线索模型

建立：

```text
leads
```

字段：

```text
id
customer_id
source_id
source_detail
search_keyword
landing_page
demand_type
demand_description
budget_min
budget_max
purpose
intent_level
stage
next_follow_up_at
owner_user_id
won_amount
lost_reason
created_at
updated_at
```

---

# 5. TASK-0603：客户来源

建立：

```text
lead_sources
```

初始来源：

```text
TIANYANCHA
PHONE
BAIDU_SEARCH
WECHAT_SEARCH
DOUYIN
XIAOHONGSHU
WEBSITE
REFERRAL
EXHIBITION
GOVERNMENT_REFERRAL
SCHOOL
OTHER
```

允许后台配置。

---

# 6. TASK-0604：线索阶段

建议：

```text
NEW
CONTACTED
QUALIFIED
PROPOSAL
NEGOTIATION
WON
LOST
DORMANT
```

所有阶段变化写入历史。

---

# 7. TASK-0605：跟进记录

建立：

```text
lead_activities
```

字段：

```text
id
lead_id
activity_type
content
operator_id
occurred_at
next_action
next_follow_up_at
created_at
```

类型：

```text
PHONE
WECHAT
EMAIL
MEETING
QUOTE
NOTE
OTHER
```

---

# 8. TASK-0606：线索阶段历史

建立：

```text
lead_stage_history
```

记录：

- 原阶段
- 新阶段
- 操作人
- 时间
- 原因

---

# 9. TASK-0607：兴趣关联

线索允许关联：

```text
artworks
products
```

建立：

```text
lead_artworks
lead_products
```

用于记录客户对哪些真实作品/产品感兴趣。

---

# 10. TASK-0608：CRM API

实现：

```http
GET    /api/v1/customers
POST   /api/v1/customers
GET    /api/v1/customers/{id}
PATCH  /api/v1/customers/{id}

GET    /api/v1/leads
POST   /api/v1/leads
GET    /api/v1/leads/{id}
PATCH  /api/v1/leads/{id}

POST   /api/v1/leads/{id}/activities
POST   /api/v1/leads/{id}/transition
```

---

# 11. TASK-0609：客户去重

创建客户时至少检查：

```text
phone
email
wechat
```

发现疑似重复时：

- 提示
- 允许合并或选择已有客户

本阶段不自动合并。

---

# 12. TASK-0610：CRM 列表 UI

菜单：

```text
客户管理
 ├─ 线索
 ├─ 客户
 └─ 来源
```

线索列表显示：

- 客户
- 来源
- 需求
- 预算
- 意向
- 阶段
- 负责人
- 下次跟进
- 创建时间

---

# 13. TASK-0611：线索详情 UI

布局建议：

```text
客户资料
需求
预算
来源
感兴趣作品/产品
跟进时间轴
阶段历史
成交结果
```

支持快速新增：

```text
电话记录
微信记录
备注
下一次跟进
```

---

# 14. TASK-0612：今日待跟进

提供：

```text
今天
已逾期
未来7天
```

三个视图。

---

# 15. TASK-0613：搜索与筛选

支持：

- 姓名
- 手机
- 企业
- 来源
- 阶段
- 预算
- 客户类型
- 负责人
- 日期
- 意向

---

# 16. TASK-0614：权限

至少：

```text
customer.read
customer.create
customer.update
lead.read
lead.create
lead.update
lead.assign
lead.transition
lead.export
```

---

# 17. TASK-0615：隐私保护

客户电话、微信、邮箱属于敏感业务数据。

要求：

- API 权限控制
- 日志不得完整输出
- 普通 VIEWER 不默认可看完整联系方式
- 导出必须单独权限

---

# 18. TASK-0616：审计

必须记录：

- 客户联系方式修改
- 负责人变更
- 阶段变更
- WON/LOST
- 导出

---

# 19. TASK-0617：基础统计 API

提供：

```text
新增线索数
来源分布
阶段分布
成交数量
成交金额
待跟进
逾期跟进
```

PHASE-07 用于 Dashboard。

---

# 20. TASK-0618：真实业务录入规则

从本 Phase 上线当天开始：

> 所有真实来电、天眼查、熟人介绍、官网咨询均必须录入 CRM。

至少询问并记录：

```text
您从哪里了解到我们？
想购买什么？
主要用途是什么？
预算大概多少？
```

---

# 21. TASK-0619：历史客户补录

将现有可以确认的历史客户/咨询线索补录。

不要为了数量编造数据。

---

# 22. TASK-0620：测试

测试：

- Customer CRUD
- Lead CRUD
- 手机号重复
- 来源
- 阶段机
- 跟进
- 权限
- 隐私
- 搜索
- 统计

---

# 23. TASK-0621：E2E

流程：

```text
天眼查来电
→ 新建客户
→ 来源选择“天眼查”
→ 创建线索
→ 记录需求与预算
→ 关联某件作品
→ 添加电话跟进
→ 设置明天下次联系
→ 进入 QUALIFIED
→ 成交/未成交
→ 查看完整时间轴
```

---

# 24. TASK-0622：业务验收

系统上线后 OWNER 必须能够在 1 分钟内查出：

```text
今天新增客户
今天待跟进
当前高意向客户
天眼查来源客户
某预算区间客户
最近成交客户
```

---

# 25. TASK-0623：文档

更新：

```text
DATABASE.md
API.md
SECURITY.md
CHANGELOG.md
ROADMAP.md
```

---

# 26. TASK-0624：Completion Report

生成：

```text
docs/reports/PHASE-06-COMPLETION.md
```

必须包含：

```text
客户总数
线索总数
真实线索数
来源分布
待跟进数
WON
LOST
已知问题
```

---

# 27. Phase Gate

- [ ] Customer 正常
- [ ] Lead 正常
- [ ] 来源正常
- [ ] 跟进正常
- [ ] 阶段历史正常
- [ ] 作品/产品关联正常
- [ ] 今日待跟进正常
- [ ] 隐私权限正常
- [ ] 真实客户已开始进入 CRM
- [ ] Test PASS
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
PHASE-06 → DONE
PHASE-07 → READY
```
