# PHASE-04：素材中心 DAM

> Phase：04  
> 状态：READY  
> 优先级：P0  
> 前置依赖：PHASE-03 = DONE  
> 下一阶段：PHASE-05 产品与库存  
> 核心目标：建立项目统一数字媒体资产库。

---

# 1. Phase 目标

解决：

> 图片、视频、录音、文件散落在电脑、手机和聊天软件的问题。

所有项目素材统一进入：

# Digital Asset Management

---

# 2. 支持类型

至少：

```text
JPG
JPEG
PNG
WEBP
MP4
MOV
MP3
WAV
PDF
DOCX
TXT
MD
```

---

# 3. TASK-0401：Asset 数据模型

建立：

```text
assets
```

字段：

```text
id
heritage_project_id
filename
original_filename
mime_type
file_type
size
storage_provider
storage_key
checksum
width
height
duration
uploaded_by
status
is_public
created_at
updated_at
```

---

# 4. TASK-0402：Asset Tags

建立：

```text
asset_tags
asset_tag_relations
```

标签例如：

```text
大师
作品
制作
研学
历史照片
采访
工艺
```

---

# 5. TASK-0403：Asset Relations

建立通用关联。

建议：

```text
asset_relations
```

支持：

```text
MASTER
ARTWORK
HERITAGE
```

未来：

```text
PRODUCT
CONTENT
STUDY_EVENT
```

---

# 6. TASK-0404：对象存储正式接入

使用 PHASE-00 建立的 S3 Compatible Storage。

开发：

```text
upload
download
delete
presigned URL
```

生产环境禁止依赖本地磁盘。

---

# 7. TASK-0405：Checksum

上传时计算：

```text
SHA-256
```

用于：

- 文件完整性
- 重复文件提示

不要强制拒绝所有重复文件。

---

# 8. TASK-0406：文件校验

检查：

- MIME
- 扩展名
- 大小
- 文件头

禁止仅依赖扩展名。

---

# 9. TASK-0407：上传 API

实现：

```http
POST /api/v1/assets
```

支持单文件。

---

# 10. TASK-0408：批量上传

实现：

```text
Multiple Upload
Progress
Partial Failure
Retry
```

单个文件失败不能导致全部丢失。

---

# 11. TASK-0409：Asset API

实现：

```http
GET    /api/v1/assets
GET    /api/v1/assets/{id}
PATCH  /api/v1/assets/{id}
DELETE /api/v1/assets/{id}
```

Delete 默认采用安全策略。

如果被业务引用，禁止直接物理删除。

---

# 12. TASK-0410：素材中心 UI

新增：

```text
素材中心
```

支持：

```text
网格
列表
```

---

# 13. TASK-0411：素材详情

显示：

- 预览
- 文件信息
- 标签
- 上传时间
- 上传人
- 关联对象
- 使用位置

---

# 14. TASK-0412：搜索与过滤

支持：

- 文件名
- 类型
- 标签
- 大师
- 作品
- 时间
- 是否公开

---

# 15. TASK-0413：图片预览

支持：

- Thumbnail
- 原图
- 尺寸
- 下载

缩略图异步生成。

---

# 16. TASK-0414：视频预览

支持：

- HTML5 播放
- Duration
- Thumbnail

本阶段不做 AI 视频理解。

---

# 17. TASK-0415：音频

支持播放。

不做转录。

转录在 AI Phase。

---

# 18. TASK-0416：作品媒体迁移

将 PHASE-03 临时作品图片机制迁移到：

```text
assets
+
asset_relations
```

迁移后：

- 旧数据不得丢失
- 作品页面正常
- QR 不受影响

完成后移除重复逻辑。

---

# 19. TASK-0417：大师素材关联

大师详情页：

```text
素材
```

自动显示关联：

- 图片
- 视频
- 录音
- 文档

---

# 20. TASK-0418：非遗素材关联

非遗档案可直接选择 DAM 中已有文件。

禁止重复上传。

---

# 21. TASK-0419：权限

建立：

```text
asset.read
asset.upload
asset.update
asset.delete
asset.download
asset.publish
```

---

# 22. TASK-0420：删除保护

如果素材被：

- 大师
- 作品
- 非遗

引用，删除时必须：

```text
阻止
或
明确解除关联后删除
```

禁止产生 Broken Reference。

---

# 23. TASK-0421：审计

记录：

- Upload
- Delete
- Publish
- Relation Change

---

# 24. TASK-0422：测试

测试：

- 上传
- 批量上传
- MIME
- 大文件
- 重复
- 关联
- 删除保护
- 权限
- S3 Failure

---

# 25. TASK-0423：E2E

流程：

```text
上传大师照片
→ 打标签
→ 关联大师
→ 上传作品视频
→ 关联作品
→ 作品详情查看
→ 素材中心搜索
→ 尝试删除被引用素材
→ 系统阻止
```

---

# 26. TASK-0424：真实素材迁移

至少整理真实：

```text
大师图片
作品图片
作品视频
历史资料
```

推荐首批：

> ≥100 个真实文件。

数量不是最终 KPI，但必须验证 DAM 确实能管理现实素材。

---

# 27. TASK-0425：文档

更新：

```text
DATABASE.md
API.md
ARCHITECTURE.md
CHANGELOG.md
ROADMAP.md
```

---

# 28. TASK-0426：Completion Report

生成：

```text
docs/reports/PHASE-04-COMPLETION.md
```

报告：

```text
素材总数
图片
视频
音频
文档
关联作品数
关联大师数
存储使用量
```

---

# 29. Phase Gate

- [ ] S3 正常
- [ ] 单文件上传
- [ ] 批量上传
- [ ] 图片预览
- [ ] 视频预览
- [ ] 音频播放
- [ ] 标签
- [ ] 搜索
- [ ] 作品关联
- [ ] 大师关联
- [ ] 删除保护
- [ ] PHASE-03 媒体迁移成功
- [ ] Test PASS
- [ ] E2E PASS

最后：

```text
STATUS: WAITING_FOR_OWNER_APPROVAL
```