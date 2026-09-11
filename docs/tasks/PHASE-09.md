# PHASE-09：线索来源追踪与搜索承接

> Phase：09  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-08 = DONE  
> 下一阶段：PHASE-10 AI 基础设施  
> 核心目标：知道每一个线上线索尽可能是“从哪里来的、通过什么页面来的、对什么内容感兴趣”，建立从搜索曝光到 CRM 的可追踪链路。

---

# 1. Phase 目标

形成：

```text
渠道
→ Landing Page
→ 浏览
→ 咨询
→ Lead
→ 跟进
→ 成交
```

重点不是复杂广告归因，而是先让自然搜索和官网客户可追踪。

---

# 2. 禁止事项

禁止：

- 指纹追踪
- 跨站隐私追踪
- 黑帽 SEO
- 搜索引擎作弊
- 批量垃圾文章
- AI 自动生成数千页面
- 未经同意收集不必要个人数据

---

# 3. TASK-0901：Tracking Session

建立匿名访问信息模型或等效统计机制。

尽量最小化数据。

记录：

```text
session_id
first_source
first_referrer
first_landing_page
utm_source
utm_medium
utm_campaign
utm_content
utm_term
created_at
```

---

# 4. TASK-0902：UTM

支持标准：

```text
utm_source
utm_medium
utm_campaign
utm_term
utm_content
```

---

# 5. TASK-0903：Referrer

记录首次来源域名。

避免保存不必要完整 URL 查询参数中的敏感数据。

---

# 6. TASK-0904：Landing Page

记录客户第一次进入：

```text
/artworks/...
/masters/...
/business
```

---

# 7. TASK-0905：Lead Attribution

咨询表单提交时，将：

```text
source
referrer
landing_page
utm
```

写入 Lead Attribution。

---

# 8. TASK-0906：归因模型

初期采用：

```text
First Touch
+
Lead Conversion Touch
```

不要开发复杂多触点模型。

---

# 9. TASK-0907：线索来源详情

CRM Lead 页面显示：

```text
来源
首次页面
咨询页面
UTM
Referrer
```

---

# 10. TASK-0908：搜索关键词字段

支持：

```text
search_keyword
```

但明确：

现代搜索引擎未必能提供真实自然搜索关键词。

无法获取时不要伪造。

可以通过：

- Search Console 类数据
- 客户人工询问

补充。

---

# 11. TASK-0909：客户人工来源补充

CRM 提供：

```text
客户口述来源
```

例如：

```text
在百度搜“XX砚哪里买”找到
天眼查找到电话
朋友推荐
```

和技术归因分开保存。

---

# 12. TASK-0910：SEO Landing Page 模型

建立 SEO 内容页的基础元数据：

```text
target_keyword
search_intent
title
description
slug
related_master
related_artwork
status
```

不必本阶段建立完整 Content Center。

---

# 13. TASK-0911：核心关键词页面

围绕真实需求规划首批页面：

```text
XX砚
XX砚价格
XX砚哪里买
XX砚官网
XX砚大师
XX砚非遗
XX砚收藏
XX砚定制
大师姓名
大师姓名 + 作品
```

内容必须来自真实资料。

---

# 14. TASK-0912：内部链接

建立：

```text
非遗 → 大师
大师 → 作品
作品 → 工艺
工艺 → 非遗
产品 → 企业定制
```

避免孤岛页面。

---

# 15. TASK-0913：404 与 Redirect

Slug 变更时支持 301。

禁止形成大量死链。

---

# 16. TASK-0914：基础统计

Dashboard/CRM 可查看：

```text
网站 Lead 数
主要 Landing Page
主要 Referrer
主要 UTM Source
各来源 WON 数
```

---

# 17. TASK-0915：来源字典统一

避免：

```text
Baidu
百度
baidu.com
百度搜索
```

变成四个来源。

建立归一化规则。

---

# 18. TASK-0916：隐私与 Cookie

根据实际部署地区和受众实现必要的隐私说明。

原则：

> 只收集完成业务分析真正需要的数据。

---

# 19. TASK-0917：Search Console 接口预留

为以后接入搜索平台数据预留服务边界。

本阶段不强制自动同步。

---

# 20. TASK-0918：测试

测试：

- Direct
- Referrer
- UTM
- Landing
- 表单
- CRM Attribution
- 301
- Source Normalization

---

# 21. TASK-0919：E2E

模拟：

```text
用户通过带 UTM 链接进入某作品
→ 浏览
→ 打开企业定制
→ 提交咨询
→ CRM Lead
→ 可看到 First Touch 和 Conversion Touch
```

---

# 22. TASK-0920：业务验证

上线后 OWNER 每周查看：

```text
哪些页面带来咨询？
哪些来源带来咨询？
哪些来源最终成交？
客户自己说是怎么找到我们的？
```

---

# 23. TASK-0921：文档

更新：

```text
DATABASE.md
API.md
PRIVACY.md
CHANGELOG.md
ROADMAP.md
```

---

# 24. TASK-0922：Completion Report

生成：

```text
docs/reports/PHASE-09-COMPLETION.md
```

---

# 25. Phase Gate

- [ ] UTM 正常
- [ ] Referrer 正常
- [ ] Landing 正常
- [ ] CRM Attribution 正常
- [ ] 人工来源补充正常
- [ ] 来源归一化正常
- [ ] 核心 SEO 页面已规划/落地
- [ ] 统计正常
- [ ] 隐私检查完成
- [ ] E2E PASS

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```
