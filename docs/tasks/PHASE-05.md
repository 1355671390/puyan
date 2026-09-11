# PHASE-05：产品与库存管理

> Phase：05  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-04 = DONE  
> 下一阶段：PHASE-06 CRM MVP  
> 核心目标：建立可规模化销售的标准产品、SKU 与库存体系。

---

# 1. Phase 目标

把：

> “大师孤品作品”

与：

> “可以重复生产销售的产品”

正式分开。

产品包括未来可能的：

- 文创砚
- 标准砚
- 镇纸
- 笔搁
- 香插
- 印章
- 礼盒
- 研学套件

---

# 2. 明确原则

```text
Artwork = 独立文化作品
Product = 商业产品
Variant = 产品规格
SKU = 可库存销售单位
```

不得混为一张表。

---

# 3. TASK-0501：产品分类

建立：

```text
product_categories
```

支持层级：

```text
文创
 ├─ 镇纸
 ├─ 笔搁
 └─ 香插

砚台
 ├─ 入门
 └─ 精品

礼赠
 └─ 企业礼盒
```

---

# 4. TASK-0502：Product

建立：

```text
products
```

字段：

```text
id
heritage_project_id
category_id
name
slug
description
status
is_public
created_at
updated_at
```

---

# 5. TASK-0503：Variant

建立：

```text
product_variants
```

字段：

```text
id
product_id
sku
name
specifications
cost_price
retail_price
b2b_price
currency
status
```

---

# 6. TASK-0504：SKU 规则

SKU 必须唯一。

建议格式：

```text
CATEGORY-PRODUCT-VARIANT
```

具体规则放系统配置。

禁止依赖产品名称作为唯一标识。

---

# 7. TASK-0505：库存模型

建立：

```text
inventory
inventory_transactions
```

库存：

```text
sku_id
quantity
reserved_quantity
available_quantity
safety_stock
```

---

# 8. TASK-0506：库存流水

类型：

```text
IN
OUT
ADJUST
RESERVE
RELEASE
```

每次变化记录：

```text
before
change
after
reason
operator
reference
time
```

禁止直接 UPDATE 数量而不产生流水。

---

# 9. TASK-0507：产品 API

实现：

```http
GET    /api/v1/products
POST   /api/v1/products
GET    /api/v1/products/{id}
PATCH  /api/v1/products/{id}
```

---

# 10. TASK-0508：Variant API

实现：

```text
Create
Read
Update
Archive
```

---

# 11. TASK-0509：库存 API

实现：

```http
GET  /api/v1/inventory
POST /api/v1/inventory/in
POST /api/v1/inventory/out
POST /api/v1/inventory/adjust
```

所有操作要求权限。

---

# 12. TASK-0510：产品媒体

直接使用 PHASE-04 DAM。

禁止建立：

```text
product_images
```

这种重复文件系统。

通过 Asset Relations 关联。

---

# 13. TASK-0511：产品列表 UI

新增：

```text
商品
 ├─ 产品
 ├─ 分类
 └─ 库存
```

列表：

- 产品
- 分类
- SKU 数
- 价格
- 库存
- 状态

---

# 14. TASK-0512：产品编辑 UI

Tabs：

```text
基础
规格
价格
库存
媒体
```

---

# 15. TASK-0513：库存 UI

显示：

```text
SKU
实际库存
预留
可售
安全库存
状态
```

低于安全库存：

```text
LOW STOCK
```

---

# 16. TASK-0514：库存流水 UI

可以按：

- SKU
- 时间
- 操作人
- 类型

查询。

---

# 17. TASK-0515：价格历史

建立：

```text
product_price_history
```

记录：

- Retail
- B2B
- Cost

变化。

---

# 18. TASK-0516：成本

当前只维护：

```text
标准成本
```

不要提前开发复杂 BOM/MRP。

后续工坊 Phase 再扩展生产成本。

---

# 19. TASK-0517：产品与作品关系

允许某个 Product 的设计来源关联大师作品。

例如：

```text
某大师作品
→ 衍生文创产品
```

但不能把 Artwork 变成 SKU。

---

# 20. TASK-0518：权限

建立：

```text
product.read
product.create
product.update
product.price
inventory.read
inventory.in
inventory.out
inventory.adjust
```

库存调整属于高权限。

---

# 21. TASK-0519：审计

必须记录：

- 成本修改
- 售价修改
- B2B 价格修改
- 库存调整
- 产品状态修改

---

# 22. TASK-0520：真实产品测试

不要一次开发几十款产品。

先建立真实或计划首发：

> 3—10 款产品。

例如：

```text
文创砚
镇纸
笔搁
企业礼盒
```

根据当前实际情况录入。

---

# 23. TASK-0521：库存测试

至少模拟：

```text
SKU 初始库存 100

入库 +50
→ 150

预留 20
→ Available 130

出库 10
→ 正确变化

释放预留
→ 正确变化
```

确保不会出现负库存异常。

---

# 24. TASK-0522：并发测试

两个请求同时扣减库存。

系统不得：

- 超卖
- 产生负数
- 丢失流水

需要事务/锁策略。

---

# 25. TASK-0523：E2E

完整流程：

```text
创建分类
→ 创建产品
→ 创建 Variant
→ 自动/手工生成 SKU
→ 关联 DAM 图片
→ 设置价格
→ 入库
→ 查看库存
→ 出库
→ 查看流水
→ 修改价格
→ 查看价格历史
```

---

# 26. TASK-0524：Dashboard 数据接口预留

提供：

```text
产品数量
SKU 数
库存总量
低库存数量
```

PHASE-07 再正式展示。

---

# 27. TASK-0525：文档

更新：

```text
DATABASE.md
API.md
CHANGELOG.md
ROADMAP.md
```

---

# 28. TASK-0526：Completion Report

生成：

```text
docs/reports/PHASE-05-COMPLETION.md
```

报告包含：

```text
产品数量
SKU 数量
库存
低库存
真实产品数量
测试结果
```

---

# 29. Phase Gate

必须：

- [ ] Product 与 Artwork 分离
- [ ] 分类正常
- [ ] SKU 唯一
- [ ] Variant 正常
- [ ] 入库正常
- [ ] 出库正常
- [ ] 调整正常
- [ ] 流水完整
- [ ] 价格历史正常
- [ ] DAM 关联正常
- [ ] 权限正常
- [ ] 并发测试 PASS
- [ ] E2E PASS
- [ ] Completion Report 完成

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```

OWNER：

```text
APPROVED
```

后才能：

```text
PHASE-05 → DONE
PHASE-06 → READY
```