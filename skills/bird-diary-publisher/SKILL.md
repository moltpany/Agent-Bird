---
name: bird-diary-publisher
description: Publish a confirmed bird sighting to the Bird Diary field guide. Invoked by bird-photography-recorder after user confirms publication. Handles thumbnail generation, sightings.json update, and git commit to the bird-diary repository.
---

# Bird Diary Publisher Skill

## Prerequisites

- The sighting must already be archived locally by `bird-photography-recorder`.
- User has explicitly confirmed publication.
- `bird-diary` repository is cloned or accessible at `{{ user.bird_diary_path }}`.

## Workflow

### 1. Generate Thumbnail

Resize and strip EXIF from the source photo. Max 400×400px, JPEG quality 85.

```bash
# Option A — ImageMagick
convert "bird-records/by-species/鸟名/photos/YYYY-MM-DD_HH-MM-SS_鸟名.jpg" \
  -resize 400x400> -strip \
  "{{ user.bird_diary_path }}/thumbs/YYYY-MM-DD_鸟名_1.jpg"

# Option B — Python (Pillow)
python3 - <<'EOF'
from PIL import Image
src = "bird-records/by-species/鸟名/photos/YYYY-MM-DD_HH-MM-SS_鸟名.jpg"
dst = "{{ user.bird_diary_path }}/thumbs/YYYY-MM-DD_鸟名_1.jpg"
img = Image.open(src)
img.thumbnail((400, 400))
img.save(dst, "JPEG", quality=85)
print(f"Thumbnail saved: {dst}")
EOF
```

Naming: `YYYY-MM-DD_<species_zh>_<n>.jpg` where `n` is a per-species sequence (1, 2, 3…).

### 2. Build Sighting Entry

Construct the JSON object from the archived sighting data:

```json
{
  "id": "YYYY-MM-DD_<species_zh>_<NNN>",
  "date": "YYYY-MM-DD",
  "time": "HH:MM",
  "species_zh": "<中文名>",
  "species_en": "<English name>",
  "species_sci": "<Scientific name>",
  "family": "<科名>",
  "area": "<区级地名>",
  "city": "<城市>",
  "rarity": "<common|uncommon|rare|very_rare>",
  "thumb": "thumbs/YYYY-MM-DD_<species_zh>_<n>.jpg",
  "lifer": "<true|false>"
}
```

**Privacy rules:**
- `area` must be district-level (区/县) or coarser — never street address or GPS coordinates.
- `thumb` path is relative to the bird-diary repo root.

### 3. Append to sightings.json

```python
import json, pathlib

p = pathlib.Path("{{ user.bird_diary_path }}/data/sightings.json")
sightings = json.loads(p.read_text())
sightings.append(new_entry)
p.write_text(json.dumps(sightings, ensure_ascii=False, indent=2))
```

Validate JSON is still valid before writing.

### 4. Commit and Push

```bash
cd "{{ user.bird_diary_path }}"
git add data/sightings.json thumbs/YYYY-MM-DD_鸟名_*.jpg
git commit -m "sighting: YYYY-MM-DD 鸟名 (family · area)"
git push origin main
```

### 5. Update Local stats.json

After a successful push, update `bird-records/stats.json`:
- Increment `publishedSpecies` if this is the first published sighting for this species.
- Set `lastPublished` to today's date.

### 6. Confirm to User

> "✅ 已发布到 Bird Diary！[鸟名] 现在在图鉴里亮起来了。"
>
> 附上链接：`https://moltpany.github.io/bird-diary/`

---

## Error Handling

| Situation | Action |
|-----------|--------|
| Thumbnail tool not found | Tell user, skip publication, keep archive |
| sightings.json invalid JSON | Stop, do not overwrite, report error |
| git push fails | Retry once; if still fails, tell user to push manually |
| Duplicate id | Append `_b`, `_c` suffix to id field |

---

_🐦 Published birds glow. Unpublished birds wait._
