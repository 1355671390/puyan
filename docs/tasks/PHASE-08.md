# PHASE-08：官网 + 数字文化馆

> Phase：08  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-07 = DONE  
> 下一阶段：PHASE-09 线索来源追踪  
> 核心目标：建立官方数字入口，解决“客户有购买需求却只能通过天眼查寻找法人电话”的问题。

---

# 1. Phase 目标

建立：

> 官方砚文化数字馆 + 官方作品展示 + 官方咨询入口

第一版必须让客户 30 秒内知道：

```text
你们是谁
是什么非遗
两位大师是谁
有哪些作品/产品
如何购买/咨询
```

---

# 2. 网站定位

禁止做成传统：

```text
首页
公司简介
新闻动态
企业荣誉
联系我们
```

而应突出：

```text
非遗
大师
作品
工艺
文化
购买
企业定制
```

---

# 3. TASK-0801：前台信息架构

至少：

```text
/
 /heritage
 /masters
 /masters/[slug]
 /artworks
 /artworks/[code]
 /products
 /craft
 /stories
 /business
 /contact
```

---

# 4. TASK-0802：首页

建议核心：

```text
省级非遗 · XX砚
两位大师坚守传统制砚技艺
从一方石，到一方砚
```

随后：

- 认识非遗
- 认识大师
- 在售作品
- 文创产品
- 工艺
- 企业定制
- 联系方式

---

# 5. TASK-0803：非遗页面

来源必须使用后台 PHASE-02 的真实数据。

显示：

- 历史
- 地域
- 石料
- 工具
- 工序
- 技法

---

# 6. TASK-0804：大师列表与详情

每位大师独立页面：

- 简介
- 身份
- 履历
- 荣誉
- 技艺
- 作品
- 影像

---

# 7. TASK-0805：作品列表

数据来源：

```text
artworks
```

过滤：

```text
is_public = true
```

可以展示：

- 编号
- 名称
- 大师
- 石料
- 状态
- 价格策略

---

# 8. TASK-0806：一砚一页

路径：

```text
/artworks/YAN-2026-00001
```

展示：

- 编号
- 大师
- 石料
- 尺寸
- 工艺
- 故事
- 图片
- 视频
- 收藏状态
- 咨询入口

二维码必须指向该稳定页面。

---

# 9. TASK-0807：售出作品策略

作品售出后页面不删除。

显示：

```text
已收藏 / 已售出
```

仍保留文化档案。

不得公开客户信息。

---

# 10. TASK-0808：产品页

展示 PHASE-05 中：

```text
is_public = true
```

的产品。

本阶段可以询价/咨询，不强制在线支付。

---

# 11. TASK-0809：企业定制页

内容：

- 适用场景
- 非遗礼赠
- 企业周年
- 银行 VIP
- 会议礼品
- 城市文化礼
- 批量定制

提供企业咨询表单。

---

# 12. TASK-0810：联系页

至少：

- 官方电话
- 微信联系方式
- 地址
- 咨询表单

敏感个人号码是否公开由 OWNER 配置。

---

# 13. TASK-0811：咨询表单

字段：

```text
姓名
联系方式
客户类型
需求
预算（可选）
感兴趣作品
用途
留言
```

提交后进入 CRM。

来源：

```text
WEBSITE
```

---

# 14. TASK-0812：CMS 发布机制

后台数据必须一处维护，多端复用。

禁止在网站代码中手工写两位大师或作品数据。

---

# 15. TASK-0813：SEO 基础

实现：

- SSR/SSG
- Meta Title
- Description
- Canonical
- Sitemap
- robots.txt
- OpenGraph

---

# 16. TASK-0814：Structured Data

适当加入：

- Organization
- Person
- Product/CreativeWork（按实际语义）
- Breadcrumb

禁止伪造评价和价格。

---

# 17. TASK-0815：性能

图片：

- 响应式
- WebP/AVIF（适用时）
- Lazy Load
- Thumbnail

关注移动端。

---

# 18. TASK-0816：设计原则

风格：

```text
克制
东方
真实
高级
以石材和手工痕迹为主
```

禁止：

- 廉价电商风
- 过度金色
- 过度“古风特效”
- AI 感强烈的虚假工艺图片

---

# 19. TASK-0817：权限与发布

只有具备：

```text
artwork.publish
product.publish
heritage.update
```

等权限者可控制公开状态。

---

# 20. TASK-0818：404 / 失效内容

不存在作品：

```text
404
```

Archived 内容按业务决定是否展示历史页。

---

# 21. TASK-0819：测试

测试：

- 首页
- 大师
- 作品
- 产品
- 表单
- SEO Meta
- Sitemap
- 移动端
- 404

---

# 22. TASK-0820：E2E

流程：

```text
后台创建作品
→ is_public = false
→ 官网不可见
→ OWNER 审核
→ is_public = true
→ 官网出现
→ 客户打开作品页
→ 提交咨询
→ CRM 自动出现 Lead
```

---

# 23. TASK-0821：真实内容验收

不得用占位数据上线。

至少：

```text
真实非遗资料
两位真实大师
≥20件真实作品中的可公开部分
真实联系方式
```

---

# 24. TASK-0822：文档

更新：

```text
ARCHITECTURE.md
API.md
DEPLOYMENT.md
CHANGELOG.md
ROADMAP.md
```

---

# 25. TASK-0823：Completion Report

生成：

```text
docs/reports/PHASE-08-COMPLETION.md
```

报告列出：

- 已上线页面
- 公开作品数量
- 大师页面
- 表单测试
- SEO 状态
- 性能测试

---

# 26. Phase Gate

- [ ] 官网可访问
- [ ] 非遗页真实
- [ ] 两位大师页面完整
- [ ] 作品列表可用
- [ ] 一砚一页可用
- [ ] QR 链接稳定
- [ ] 产品可展示
- [ ] 企业定制页完成
- [ ] 联系入口完成
- [ ] 表单进入 CRM
- [ ] Sitemap 正常
- [ ] 移动端正常
- [ ] E2E PASS

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```
