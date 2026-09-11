# PHASE-07：主理人驾驶舱 V1

> Phase：07  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-06 = DONE  
> 下一阶段：PHASE-08 官网 + 数字文化馆  
> 核心目标：把作品、产品、库存、客户和 CRM 数据集中到一个主理人首页，让日常开发与运营从“进入多个模块找数据”升级为“打开首页即知道今天该做什么”。

---

# 1. Phase 目标

Dashboard V1 聚合：

- 作品
- 产品
- 库存
- 客户
- 线索
- 跟进
- 基础成交数据
- 系统待办

本阶段不做 AI CEO Agent。

---

# 2. 禁止事项

禁止：

- AI 经营建议
- 自动决策
- 财务完整报表
- 政策数据
- 研学数据
- 工坊数据
- 复杂 BI 系统

---

# 3. TASK-0701：Dashboard 聚合接口

实现：

```http
GET /api/v1/dashboard/overview
```

返回：

```text
artworks
products
inventory
leads
customers
followups
sales_summary
system_tasks
```

---

# 4. TASK-0702：作品指标

显示：

```text
作品总数
在售
已售
待完善
待审核
```

---

# 5. TASK-0703：产品库存指标

显示：

```text
产品数量
SKU 数
库存总量
低库存 SKU
```

---

# 6. TASK-0704：CRM 指标

显示：

```text
今日新增线索
本月新增线索
高意向
待跟进
逾期
WON
LOST
```

---

# 7. TASK-0705：来源概览

显示主要来源 TOP N：

```text
天眼查
电话
搜索
转介绍
其他
```

---

# 8. TASK-0706：待办中心

建立统一待办视图。

初期自动产生：

```text
逾期客户跟进
今天客户跟进
待完善作品
低库存 SKU
```

---

# 9. TASK-0707：Dashboard UI

后台根页面：

```text
/dashboard
```

建议布局：

```text
今日概览
待办
客户
作品
产品/库存
最近活动
```

---

# 10. TASK-0708：指标 Drill Down

点击：

```text
高意向客户
```

直接进入 CRM 对应筛选结果。

点击：

```text
低库存
```

进入库存筛选。

---

# 11. TASK-0709：最近活动

聚合：

- 新作品
- 新线索
- 客户阶段变化
- 库存调整

不得泄露敏感字段。

---

# 12. TASK-0710：时间区间

支持：

```text
今天
7天
30天
本月
```

对于适用指标。

---

# 13. TASK-0711：权限化 Dashboard

不同角色看到不同数据。

例如 MASTER：

- 本人作品相关
- 不默认看到完整 CRM

OWNER：

- 全局

---

# 14. TASK-0712：性能

Dashboard 不允许通过前端连续调用几十个 API 拼接。

优先使用聚合 API。

目标：

在正常数据量下快速打开。

---

# 15. TASK-0713：空状态

新系统无数据时：

- 不报错
- 显示 0
- 给出下一步引导

---

# 16. TASK-0714：异常状态

某一统计失败：

- 页面其他卡片仍正常
- 显示局部错误
- 写入日志

---

# 17. TASK-0715：测试

测试：

- 聚合计算
- 时间区间
- 权限
- 空数据
- 部分失败
- Drill Down

---

# 18. TASK-0716：E2E

创建：

```text
作品
产品
库存
客户
线索
跟进
```

然后确认 Dashboard 数据一致。

---

# 19. TASK-0717：OWNER 真实验收

OWNER 每天打开后台后，应在 30 秒内回答：

```text
今天有没有新客户？
今天要跟进谁？
现在有多少高意向？
哪些作品在售？
哪些产品库存不足？
```

---

# 20. TASK-0718：文档

更新：

```text
API.md
CHANGELOG.md
ROADMAP.md
```

---

# 21. TASK-0719：Completion Report

生成：

```text
docs/reports/PHASE-07-COMPLETION.md
```

---

# 22. Phase Gate

- [ ] Dashboard 正常
- [ ] 作品指标一致
- [ ] 产品库存指标一致
- [ ] CRM 指标一致
- [ ] 待办正确
- [ ] Drill Down 正常
- [ ] 权限正确
- [ ] Test PASS
- [ ] E2E PASS

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```
