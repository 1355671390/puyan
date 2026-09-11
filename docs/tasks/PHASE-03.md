# PHASE-03：作品管理 MVP

> Phase：03  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-02 = DONE  
> 下一阶段：PHASE-04 素材中心 DAM  
> 核心目标：实现“一砚一档”。

---

# 1. Phase 目标

让每一方真实砚台成为独立数字资产。

完成：

```text
作品
→ 编号
→ 大师
→ 石料
→ 工艺
→ 尺寸
→ 价格
→ 状态
→ 图片
→ 证书
→ QR Code
```

---

# 2. 核心原则

作品 ≠ 产品 SKU。

作品主要指：

- 大师作品
- 孤品
- 收藏作品
- 手工作品

标准化商品在 PHASE-05 处理。

---

# 3. TASK-0301：Artwork 数据模型

建立：

```text
artworks
```

字段：

```text
id
heritage_project_id
code
name
master_id
material_id
created_year
length
width
height
weight
pattern
craft_description
creation_story
production_days
cost
suggested_price
sale_price
currency
status
is_public
is_unique
is_masterpiece
created_at
updated_at
```

---

# 4. TASK-0302：作品编号

默认：

```text
YAN-YYYY-XXXXX
```

例如：

```text
YAN-2026-00001
```

编号：

- 唯一
- 不重复
- 创建后不可随意修改

前缀从 System Settings 获取。

---

# 5. TASK-0303：作品状态机

实现：

```text
DRAFT
ARCHIVING
PENDING_REVIEW
COMPLETED
FOR_SALE
RESERVED
SOLD
COLLECTION
ARCHIVED
```

禁止任意跨状态。

建立 Transition Rules。

---

# 6. TASK-0304：作品关系

支持关联：

```text
大师
非遗
材料
技法
工序
```

未来再关联知识和内容。

---

# 7. TASK-0305：作品媒体临时能力

由于 DAM 在 PHASE-04，本阶段只实现最小作品图片附件能力。

禁止重复建设完整 DAM。

PHASE-04 后迁移到统一 Asset 模型。

---

# 8. TASK-0306：证书模型

建立：

```text
artwork_certificates
```

记录：

```text
certificate_no
artwork_id
issued_at
version
status
```

本阶段只生成基础数字证书信息。

不做区块链。

---

# 9. TASK-0307：QR Code

每件作品生成 QR。

二维码内容应指向稳定的作品公开 URL 标识。

即使当前官网尚未开发，也要预留。

---

# 10. TASK-0308：Migration

建立完整 Migration。

编号建立 Unique Index。

---

# 11. TASK-0309：Artwork API

实现：

```http
GET    /api/v1/artworks
POST   /api/v1/artworks
GET    /api/v1/artworks/{id}
PATCH  /api/v1/artworks/{id}
```

---

# 12. TASK-0310：状态 API

例如：

```http
POST /api/v1/artworks/{id}/transition
```

Body：

```json
{
  "target_status": "FOR_SALE"
}
```

后端验证是否合法。

---

# 13. TASK-0311：作品列表 UI

菜单：

```text
作品管理
```

显示：

- 编号
- 图片
- 名称
- 大师
- 石料
- 价格
- 状态
- 是否公开

---

# 14. TASK-0312：筛选

支持：

- 大师
- 状态
- 石料
- 年份
- 价格区间
- 是否公开

---

# 15. TASK-0313：作品创建 UI

分区：

```text
基础
作者
材料
尺寸
工艺
故事
价格
媒体
状态
```

支持保存草稿。

---

# 16. TASK-0314：作品详情 UI

顶部：

```text
编号
名称
状态
大师
价格
```

下方：

```text
基础信息
创作故事
工艺
媒体
证书
历史记录
```

---

# 17. TASK-0315：状态历史

建立：

```text
artwork_status_history
```

记录：

- 原状态
- 新状态
- 操作人
- 时间
- 原因

---

# 18. TASK-0316：价格历史

建立：

```text
artwork_price_history
```

任何售价变化必须可追踪。

---

# 19. TASK-0317：权限

至少：

```text
artwork.read
artwork.create
artwork.update
artwork.price
artwork.publish
artwork.transition
```

大师可根据权限查看本人作品。

---

# 20. TASK-0318：真实作品录入

必须录入：

> 至少 20 件当前真实作品。

推荐最终逐步做到 30—50 件。

这是业务验收，不是测试数据。

---

# 21. TASK-0319：作品数据完整度

内部显示：

```text
完整度 82%
```

提醒缺：

- 图片
- 故事
- 尺寸
- 石料
- 工艺

---

# 22. TASK-0320：搜索

支持：

```text
作品编号
作品名称
大师
石料
```

---

# 23. TASK-0321：测试

测试：

- 编号并发唯一
- CRUD
- 权限
- 状态机
- 价格历史
- 状态历史
- QR
- Search

---

# 24. TASK-0322：E2E

完整测试：

```text
创建作品
→ 自动编号
→ 选择大师
→ 填石料
→ 上传图片
→ 保存草稿
→ 完善
→ 审核
→ 设置价格
→ FOR_SALE
→ 生成证书
→ QR
→ 搜索
```

---

# 25. TASK-0323：业务验收

随机选择一件真实砚台。

OWNER 必须能：

> 30 秒内从后台找到其完整资料。

---

# 26. TASK-0324：文档

更新：

```text
DATABASE.md
API.md
CHANGELOG.md
ROADMAP.md
```

---

# 27. TASK-0325：Completion Report

生成：

```text
docs/reports/PHASE-03-COMPLETION.md
```

报告必须注明：

```text
真实作品录入数量：
完整档案数量：
在售数量：
待补资料数量：
```

---

# 28. Phase Gate

- [ ] 编号正常
- [ ] 状态机正常
- [ ] 作品 CRUD 正常
- [ ] 两位大师均有关联作品
- [ ] ≥20件真实作品
- [ ] QR 正常
- [ ] 证书基础数据正常
- [ ] 历史可追踪
- [ ] 搜索正常
- [ ] 测试 PASS
- [ ] E2E PASS

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```