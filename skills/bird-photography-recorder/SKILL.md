---
name: bird-photography-recorder
description: Record and archive bird photography sightings for users. Use when a user sends a bird photo, asks to identify a bird, or wants to log a bird sighting. Handles photo saving, species identification logging, daily journal creation, and species profile management. Triggers on bird photos, "这是什么鸟", "认鸟", "记录", "存档".
---

# Bird Photography Recorder Skill

## Workflow

### 1. Receive Photo
When user sends a bird photo:
- Save to `bird-records/raw-photos/`
- Filename: `YYYY-MM-DD_HH-MM-SS_<species>.jpg` (update after ID)

### 2. Identify Species
- Analyze visual features from photo
- Query iNaturalist API for confirmation:
  ```bash
  curl -s "https://api.inaturalist.org/v1/taxa?q=<keyword>&rank=species&per_page=5"
  ```
- Return: 中文名、科学名、英文名、family、保护等级

### 3. Create Daily Log Entry
Append to `bird-records/sightings-log/YYYY-MM-DD.md`:
```markdown
## HH:MM - [中文名]
**发现者**: @[user]  
**设备**: {{ user.camera }}  
**照片**: [raw-photos/YYYY-MM-DD_HH-MM-SS_鸟名.jpg]

### 识别结果
- **中文名**: 
- **科学名**: 
- **英文名**: 
- **科属**: 
- **保护等级**: 

### 这是什么鸟？
[2-3 句介绍：体型、特征、习性、分布]

### 我们的对话
> [user]: [用户原话]  
> Bird: [我的回复]  
> [user]: [后续互动]

### 小知识
- [趣味知识点 1]
- [趣味知识点 2]

---
```

### 4. Update Species Profile
Create/update `bird-records/by-species/<鸟名>/profile.md`:
```markdown
# [中文名] ([科学名])

## 基本信息
- **英文名**: 
- **科属**: 
- **保护等级**: 
- **常见度**: ⭐⭐⭐☆☆

## 目击记录
| 日期 | 时间 | 发现者 | 照片 |
|------|------|--------|------|
| YYYY-MM-DD | HH:MM | @user | [link] |

## 累计统计
- **总目击次数**: N
- **首次发现**: YYYY-MM-DD
- **最近发现**: YYYY-MM-DD

## 小知识
[物种趣味知识合集]

## IP 素材（未来）
- **卡牌文案草稿**: 
- **稀有度**: 
- **栖息地**: 
- **趣味标签**: 
```

### 5. Move Photo to Species Folder
```bash
mv bird-records/raw-photos/YYYY-MM-DD_HH-MM-SS_鸟名.jpg \
   bird-records/by-species/鸟名/photos/
```

### 6. Offer Bird Diary Publication
After archiving, ask the user once:

> "要把这张发布到 Bird Diary 图鉴吗？会生成缩略图，不上传原图。"

- If **yes**: invoke the `bird-diary-publisher` skill.
- If **no**: skip silently, archive stays local only.
- Never publish without explicit confirmation.

## Stats Tracking

Maintain a running count in `bird-records/stats.json`:
```json
{
  "totalSpecies": 0,
  "totalSightings": 0,
  "todaySightings": 0,
  "todayDate": "YYYY-MM-DD",
  "rarest": { "species": "", "level": "" },
  "mostPhotographed": "",
  "firstSightingDate": "YYYY-MM-DD",
  "publishedSpecies": 0,
  "lastPublished": ""
}
```

`publishedSpecies` = species count in bird-diary sightings.json. `lastPublished` = ISO date of most recent bird-diary commit.

## IP Card Draft Template

For future card generation, store in `bird-records/ip-assets/cards/draft/鸟名.md`:
```markdown
# 卡牌草案：[鸟名]

## 正面
- **主图**: [精选照片]
- **边框风格**: [建议]

## 背面
- **鸟名**: [中文名]
- **科学名**: [学名]
- **稀有度**: [⭐评级]
- **栖息地**: 
- **首次发现者**: @[user]
- **首次发现日期**: YYYY-MM-DD
- **趣味知识**: [一句话]
- ** Bird 评语**: [互动金句]
```

## Example Interaction

**User**: [sends photo of a magpie-robin]

**Bird**:
> 这是鹊鸲（Copsychus saularis），也叫四喜！雄性黑白分明，非常帅气 🎩
> 
> 它们喜欢在水边活动，尾巴经常翘起来，很有个性。你家附近有水源吗？
> 
> 📸 已保存到档案！今天已记录 3 只鸟~ 累计解锁 12 个物种！

---

_🐦 每只鸟都有故事 📸_
