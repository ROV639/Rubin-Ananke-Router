# Prompt Parameters Reference

This is the modular prompt library for `chatgpt-imagegen`. Use it when a task needs visual planning, portrait continuity, project-specific image direction, materials, lighting, film tone, or multi-image series logic.

Do not copy-paste whole sections into a prompt. Select the relevant modules, rewrite them into natural English, remove duplicates, resolve conflicts, and add task-specific details before generation.

## Quick Index

- 0. Prompt composer rules: line 61
  - 0.1 Current OpenAI prompting guidance: line 63
  - 0.2 Prompt assembly order: line 79
  - 0.3 Reference-image and edit prompt rules: line 119
- 1. Aspect ratio rules: line 137
  - 1.1 Common ratios: line 153
  - 1.2 Less common ratios: line 168
  - 1.3 Project and series consistency rules: line 183
- A. General portrait / character parameters: line 194
  - A1 Identity lock: line 198
  - A2 Character type: line 207
  - A3 Face and temperament: line 220
  - A4 Skin and makeup: line 232
  - A5 Hair: line 242
  - A6 Wardrobe: line 252
  - A7 Pose and action: line 265
  - A8 Scene and place: line 276
  - A9 Camera angle: line 288
  - A10 Composition: line 302
  - A11 Lens and framing: line 313
  - A12 Lighting: line 325
  - A13 Tonality and color: line 338
  - A14 Film simulation: line 349
  - A15 Materials and texture: line 362
  - A16 Negative constraints: line 375
- B. General non-human / object / scene parameters: line 388
  - B1 Subject type: line 392
  - B2 Environment: line 403
  - B3 Materials and texture: line 414
  - B4 Composition: line 436
  - B5 Lens and viewpoint: line 447
  - B6 Lighting: line 457
  - B7 Color and tonality: line 468
  - B8 Negative space and text area: line 478
  - B9 Negative constraints: line 486
- C. Series expansion and lock templates: line 495
  - C1 Mother image rule: line 499
  - C2 Images 2-4 stability variations: line 509
  - C3 Image 5 onward expansion: line 519
  - C4 EVE real-person identity lock: line 528
  - C5 General portrait identity lock: line 563
  - C6 Anime character identity lock: line 588
  - C7 Non-human style / scene lock: line 611
  - C8 Mother-image continuity prompt: line 635
- P. Project-specific modules: line 662
  - P1 EVE real-person series: line 666
  - P2 ROV x EVE anime couple / tutorial series: line 694
  - P3 Arthur Observatory WeChat cover / article image: line 721
  - P4 Paper Moon music cover / music film visual: line 744
  - P5 Web6 media console output: line 767
  - P6 Rubin Motion Lab video cover / background asset: line 788

## 0. Prompt Composer Rules

### 0.1 Current OpenAI Prompting Guidance

Source checked: OpenAI Developers, "GPT Image Generation Models Prompting Guide", published April 21, 2026.
Reference URL: https://developers.openai.com/cookbook/examples/multimodal/image-gen-models-prompting-guide

Use these rules for ChatGPT / GPT image generation:

- State the goal and use case first: `Create a photorealistic editorial cover image...`（先说明用途和目标）
- Use a maintainable structure instead of one vague sentence（复杂图用分段结构，不要一整段糊在一起）
- Be concrete about subject, place, materials, textures, lighting, composition, and constraints（主体、地点、材质、光线、构图、限制都要具体）
- For photorealistic work, use direct photographic language and real texture details（真人摄影要写真实摄影语言和真实质感）
- For people, specify pose, gaze, body framing, and object interaction（人物要写姿态、视线、景别、手和物体关系）
- For reference-image or edit tasks, clearly separate what changes from what must remain fixed（参考图任务要分清改变项和固定项）
- Avoid empty quality stuffing such as `8K`, `masterpiece`, `best quality`, `ultra-sharp 8K detail`（不要堆无意义画质词）
- Camera and lens terms guide the look, but do not guarantee exact physics（镜头词是视觉方向，不是精确物理模拟）

### 0.2 Prompt Assembly Order

Assemble prompts in this order. The final prompt should be English-first, with no Chinese notes unless Robin asks to inspect the prompt.

```text
Goal / intended use
Identity lock or style lock, if a reference image is used
Subject
Scene and story beat
Materials and texture
Composition and placement
Lighting source and direction
Color, tonality, and film simulation
Camera angle, lens, framing, depth of field
Aspect ratio only, never pixel dimensions
Negative constraints
```

Working method:

1. Pick only the modules that matter for the current image.
2. Rewrite the selected terms into smooth English sentences.
3. Remove duplicate style words and conflicting light sources.
4. Add the specific theme, scene, clothing, props, or platform need from Robin's task.
5. Keep the final prompt compact. Strong structure beats long prompt stuffing.

Example shape:

```text
Create a quiet film-photography still life for a social cover.
Subject: a silver vintage camera and an open sketchbook on a wooden desk.
Scene: in front of a misty blue-gray wall with generous empty space.
Materials: natural wood grain, brushed metal camera body, paper fibers, matte ceramic lamp base.
Lighting: warm desk lamp glow from frame right, cool diffused wall reflection from frame left.
Tonality: Fujifilm Classic Chrome, low saturation, soft natural contrast, subtle film grain.
Composition: centered lower third, large negative space above, calm Kinfolk magazine restraint.
Aspect ratio: 3:4 vertical portrait.
Constraints: no text, no logos, no fake writing, no plastic 3D render look.
```

### 0.3 Reference-Image And Edit Prompt Rules

For `--image` / `--reference` tasks, every prompt should include:

```text
Use the provided reference image as the anchor.
Keep: [identity / object shape / outfit / material / layout / color system / lighting logic].
Change: [scene / action / camera angle / background / styling / mood].
Do not change: [the most important invariant].
```

If the task uses a mother image for expansion, reference both anchors mentally:

- Original identity reference: fixed face, character, object identity（原始身份锚点）
- Mother image: fixed scene, styling, lighting, atmosphere, and color language（母图作为场景风格锚点）

Do not overload the generation with too many reference images. Use the fewest images that express the identity and scene clearly. Default operational limit: no more than 4 reference images, and no more than 25 MB per reference image unless Robin explicitly accepts the higher failure risk.

## 1. Aspect Ratio Rules

Prompts should write aspect ratio only. Do not write pixel dimensions in prompt text. Pixel dimensions belong only to the CLI `--size` flag and are an internal execution mapping.

Correct:

```text
Aspect ratio: 3:4 vertical portrait.
```

Wrong:

```text
1536x2048, 2048 pixels, 4K, 8K.
```

### 1.1 Common Ratios

Use common ratios first:

| Ratio | Use | Prompt wording |
|---|---|---|
| `3:4` | Default when no special need; platform covers; ordinary vertical visuals | `Aspect ratio: 3:4 vertical portrait`（默认竖图 / 平台封面） |
| `16:9` | Cinematic horizontal image, video frame, EVE real-person default | `Aspect ratio: 16:9 cinematic horizontal frame`（电影横屏） |
| `9:16` | Mobile vertical story, short-video visual, anime couple default | `Aspect ratio: 9:16 mobile vertical frame`（手机竖屏） |
| `2.35:1` | WeChat Official Account cover fixed ratio | `Aspect ratio: 2.35:1 wide WeChat cover banner`（公众号封面固定比例） |
| `1:1` | Music cover, avatar, square visual, album art | `Aspect ratio: 1:1 square cover`（方图 / 音乐封面） |
| `4:5` | Social vertical post, portrait crop with more height than square | `Aspect ratio: 4:5 social vertical post`（社媒竖版） |
| `3:2` | Photography landscape, editorial article image, still life | `Aspect ratio: 3:2 photographic landscape`（摄影横图） |
| `2:3` | Magazine portrait, vertical editorial portrait | `Aspect ratio: 2:3 vertical editorial portrait`（杂志竖幅） |

### 1.2 Less Common Ratios

Use these only when the task or platform calls for them:

| Ratio | Use | Prompt wording |
|---|---|---|
| `1.91:1` | X / Twitter link card style | `Aspect ratio: 1.91:1 social card`（X 链接卡片） |
| `2.39:1` | CinemaScope / very cinematic still | `Aspect ratio: 2.39:1 cinematic widescreen`（电影宽银幕） |
| `21:9` | Ultra-wide banner / desktop hero | `Aspect ratio: 21:9 ultra-wide banner`（超宽横幅） |
| `3:1` | Panoramic banner | `Aspect ratio: 3:1 panoramic banner`（全景横幅） |
| `1:3` | Long scroll composition / poster strip | `Aspect ratio: 1:3 vertical scroll composition`（长竖幅） |
| `4:3` | Traditional digital camera, old TV, documentary still | `Aspect ratio: 4:3 classic documentary frame`（传统影像比例） |
| `5:4` | Retro video / medium-format feeling | `Aspect ratio: 5:4 retro medium-format frame`（复古中画幅感） |
| `1.618:1` | Golden ratio composition | `Aspect ratio: 1.618:1 golden-ratio landscape`（黄金比例） |

### 1.3 Project And Series Consistency Rules

- If no special platform or project requirement is present, default to `3:4`.
- WeChat Official Account cover is always `2.35:1`.
- Generic platform cover is usually `3:4`.
- EVE real-person series usually uses `16:9`; rare cases may use `9:16` or `3:4`.
- ROV x EVE anime couple / tutorial series usually uses `9:16`; `3:4` is mainly for separate cover/adaptation work.
- A pure image series must keep one aspect ratio throughout. If a set starts as `9:16`, every image in that set stays `9:16`; if it starts as `16:9`, every image stays `16:9`.
- Do not mix `9:16` and `3:4` inside the same image series just because one image "could be a cover". A cover/adaptation is a separate output version and must be named as such.
- AI real-person model series, anime couple series, and EVE series all follow the same rule: one active series, one ratio.

## A. General Portrait / Character Parameters

Use this section for real people, fictional people, anime characters, humanoid subjects, fashion portraits, headshots, half-body scenes, full-body scenes, and reference-image identity work.

### A1. Identity Lock

Use when identity must stay stable across reference images, outfits, scenes, or angles:

- `Use the provided reference image as the primary identity anchor`（以参考图作为主要身份锚点）
- `Preserve the same face shape, eye spacing, eye size, nose bridge and tip, lip volume, jawline, hair silhouette, age impression, and core temperament`（保留脸型、眼距、眼型、鼻子、嘴唇、下颌、发型轮廓、年龄感、气质）
- `Do not reinterpret the person as a similar-looking model`（禁止变成相似但不是同一个人）
- `Scene, clothing, crop, lighting, and pose may change, identity may not`（场景服装可变，身份不可变）

### A2. Character Type

Choose one or two, then customize:

- `Chinese cinematic heroine`（中国电影女主气质）
- `East Asian high-fashion editorial model`（东亚高定时尚模特）
- `Japanese intellectual elegance`（日系知性克制）
- `Korean cinema actress, natural and understated`（韩影演员感，自然克制）
- `natural authentic woman, not commercial beauty`（真实自然女性，不是商业美颜）
- `semi-realistic anime heroine with grounded proportions`（半写实动漫女主，比例真实）
- `modern urban male protagonist`（现代都市男性主角）
- `quiet documentary subject`（纪实感人物）

### A3. Face And Temperament

Use sparingly and only when it matches the reference:

- `deep-set almond eyes, restrained gaze`（深邃杏仁眼，克制眼神）
- `soft but defined jawline`（柔和但清晰的下颌）
- `high cheekbones with natural asymmetry`（高颧骨，自然不对称）
- `full natural lips with soft sheen`（饱满自然唇，轻光泽）
- `calm, composed, emotionally restrained`（冷静、克制、情绪内收）
- `warm but not overly sweet`（温暖但不甜腻）
- `playful only if the scene requires it`（题材需要时才活泼）

### A4. Skin And Makeup

- `visible pores and fine skin micro-texture`（可见毛孔和细微皮肤纹理）
- `natural luminosity, not plastic smoothing`（自然光泽，非塑料磨皮）
- `warm peach-honey undertone`（暖蜜桃蜂蜜底色）
- `no-makeup makeup, barely visible base`（伪素颜妆）
- `soft natural lip color, not overlined`（自然唇色，不夸张唇线）
- `subtle eye definition, no heavy eyeliner unless required`（轻眼部轮廓，非必要不浓眼线）
- `editorial makeup with one controlled accent`（编辑部妆容，只突出一个重点）

### A5. Hair

- `jet black straight hair past shoulders`（过肩黑直发）
- `soft curtain bangs, wispy fringe`（自然碎刘海 / 八字刘海）
- `loose natural waves`（自然松弛卷发）
- `low ponytail with a few loose strands`（低马尾，少量碎发）
- `short bob with clean silhouette`（短波波头，轮廓干净）
- `slightly messy hair with believable movement`（微乱、有真实动感）
- `anime hair silhouette remains identical to the reference`（动漫角色发型轮廓保持一致）

### A6. Wardrobe

Pick clothing that matches scene logic:

- `black leather jacket, slightly worn texture`（黑色皮夹克，轻微旧化质感）
- `tailored dark blazer, clean shoulder line`（深色西装外套，肩线清晰）
- `soft knit sweater, visible fabric weave`（针织毛衣，可见织纹）
- `cream trench coat with matte cotton texture`（米色风衣，哑光棉质）
- `satin slip dress with controlled sheen`（缎面吊带裙，克制光泽）
- `simple white shirt, crisp cotton folds`（白衬衫，干净棉质褶皱）
- `minimal streetwear, no loud logos`（极简街头穿搭，无大 logo）
- `anime casual couple outfits, coordinated but not identical`（动漫情侣日常穿搭，协调但不完全相同）

### A7. Pose And Action

- `direct camera eye contact`（直视镜头）
- `gaze turned slightly off-camera`（目光略偏离镜头）
- `10-20 degree head turn while keeping identity readable`（10-20 度转头，仍可锁脸）
- `hands naturally holding a camera / book / phone / cup`（手自然拿相机/书/手机/杯子）
- `walking slowly through the scene`（缓慢行走）
- `leaning against a wall with relaxed shoulders`（靠墙，肩膀放松）
- `reading or sketching, eyes lowered`（阅读或素描，垂眼）
- `two characters interacting through a shared object`（双人通过共同物件互动）

### A8. Scene And Place

- `quiet apartment living room`（安静公寓客厅）
- `wooden desk against a blue-gray wall`（蓝灰墙前木质书桌）
- `small cafe table by a window`（窗边咖啡桌）
- `private study library with warm lamp light`（私人书房，暖灯）
- `art gallery with white walls and sparse visitors`（艺术馆，白墙，少量观众）
- `neon night street after rain`（雨后霓虹夜街）
- `city rooftop at blue hour`（蓝调时刻城市天台）
- `subway platform with cinematic depth`（地铁站台，电影景深）
- `anime home workspace with projected UI layer`（动漫居家工作区，透明 UI 信息层）

### A9. Camera Angle

Use the expansion ladder. Do not jump to hard angles before identity stabilizes.

- `front view, directly facing camera`（正面）
- `near-front 10-20 degree head turn`（接近正面，10-20 度）
- `three-quarter view`（45 度三分之四侧）
- `over-the-shoulder view`（过肩视角）
- `eye-level camera`（平视）
- `slightly low angle, restrained power`（轻微低角度）
- `high angle looking down gently`（轻微高角度）
- `mirror reflection shot`（镜面反射）
- `top-down flat lay`（俯拍平铺，非人像或物件常用）

### A10. Composition

- `centered symmetrical composition`（居中对称）
- `rule of thirds, subject off-center`（三分法，主体偏置）
- `negative space in the upper third`（上三分之一留白）
- `foreground layer partially framing the subject`（前景遮挡形成层次）
- `frame within frame through doorway or window`（门窗内框构图）
- `leading lines drawing the eye inward`（引导线）
- `environment 60 percent, character 40 percent`（环境 60%，人物 40%）
- `close crop but keep hands visible if they matter`（近景但关键手部保留）

### A11. Lens And Framing

- `24mm environmental portrait`（24mm 环境人像）
- `35mm documentary photography look`（35mm 纪实感）
- `50mm natural perspective`（50mm 自然视角）
- `85mm portrait lens look, shallow depth of field`（85mm 人像镜头感，浅景深）
- `135mm compressed background layers`（135mm 压缩空间）
- `head and shoulders portrait`（头肩像）
- `waist-up portrait`（半身上半）
- `full body visible, feet included`（全身，脚入画）
- `medium shot with readable environment`（中景，环境可读）

### A12. Lighting

Always name the visible or believable source:

- `soft diffused daylight from a large window on frame left`（左侧窗户柔和自然光）
- `warm desk lamp glow from frame right`（右侧暖黄台灯）
- `red and blue neon signs visible behind the subject`（可见红蓝霓虹招牌）
- `overcast daylight, shadowless and quiet`（阴天漫射光）
- `blue hour ambient city light with warm shop windows`（蓝调城市环境光 + 暖窗）
- `single Rembrandt-style key light from upper left`（左上单主光，伦勃朗感）
- `rim backlight separating hair from the background`（轮廓逆光分离头发）
- `screen glow from an open laptop, subtle and realistic`（笔电屏幕光，微弱真实）

### A13. Tonality And Color

- `low saturation, restrained color palette`（低饱和克制色彩）
- `muted blue-gray and warm amber contrast`（雾蓝灰与暖琥珀对比）
- `warm golden interior with cool exterior spill`（暖室内与冷外部光）
- `teal and red neon contrast, controlled not garish`（青红霓虹对比，克制不艳俗）
- `matte earth tones with one dark accent`（哑光土色，一处深色重点）
- `soft high-key natural color`（柔和高调自然色）
- `low-key cinematic shadow, readable face`（低调电影阴影，但脸可读）
- `monochrome black and white with rich midtones`（黑白，中间调丰富）

### A14. Film Simulation

Use one film simulation, not many at once:

- `Fujifilm Classic Chrome, muted documentary tones`（富士 Classic Chrome，低饱和纪实色调）
- `Fujifilm Classic Neg, nostalgic contrast and lifted shadows`（富士 Classic Neg，怀旧对比和抬阴影）
- `Fujifilm Eterna Cinema, soft cinematic low contrast`（富士 Eterna，柔和电影低对比）
- `Kodak Portra 400, warm natural skin and soft contrast`（柯达 Portra 400，暖肤色柔对比）
- `Kodak Gold 200, warm consumer-film nostalgia`（柯达 Gold 200，暖调日常胶片）
- `Kodak Vision3 500T, tungsten night cinema color`（柯达 Vision3 500T，钨丝灯夜景电影色）
- `Cinestill 800T, halation around practical lights`（Cinestill 800T，实用灯周围光晕）
- `Ilford HP5 black and white grain`（Ilford HP5 黑白颗粒）

### A15. Materials And Texture

For portraits, materials make clothing and environment believable:

- `visible fabric weave in cotton and wool`（棉/羊毛织纹）
- `worn leather grain with subtle creases`（旧皮革纹理和褶皱）
- `brushed metal camera body with soft edge highlights`（拉丝金属相机，柔和边缘高光）
- `paper fibers on the sketchbook pages`（素描本纸纤维）
- `matte ceramic lamp base`（哑光陶瓷灯座）
- `slightly scratched glass reflections`（轻微划痕玻璃反射）
- `natural wood grain, not fake laminate`（天然木纹，非假贴皮）
- `rain-wet pavement reflections`（雨后路面反射）

### A16. Negative Constraints

Use targeted negatives, not a giant universal tail:

- `No text, no letters, no numbers, no watermark`（无文字、字母、数字、水印）
- `No AI-smoothed plastic skin`（无 AI 塑料磨皮）
- `Do not enlarge eyes or change face proportions`（不放大眼睛、不改脸部比例）
- `No generic influencer beauty face`（不变网红脸）
- `No fake brand logos`（无虚假品牌 logo）
- `No overdone neon glow without a visible light source`（没有可见光源就不要霓虹发光）
- `No stock-photo smile or staged commercial pose`（不要图库式笑容和摆拍）
- `No extra fingers, distorted hands, or missing hands when hands are visible`（手入画时避免畸形/缺失）

## B. General Non-Human / Object / Scene Parameters

Use this section for still life, products, architecture, interiors, food, landscapes, abstract systems, posters without characters, object-led covers, and visual metaphors.

### B1. Subject Type

- `object-led hero image`（物件主视觉）
- `tabletop still life`（桌面静物）
- `product scene with real use context`（真实使用场景产品图）
- `architectural interior`（建筑室内）
- `editorial article cover image`（文章封面图）
- `music cover visual system`（音乐封面视觉）
- `quiet landscape mood image`（安静风景氛围图）
- `abstract visual metaphor built from real materials`（真实材料构成的抽象隐喻）

### B2. Environment

- `wooden desk against a muted blue-gray limewash wall`（蓝灰石灰洗墙前木桌）
- `private study with books, desk lamp, and deep shadows`（私人书房，书、台灯、深阴影）
- `minimal gallery space with white walls and concrete floor`（白墙水泥地极简画廊）
- `rain-wet city street with reflected shop signs`（雨后城市街道和店招反射）
- `quiet hotel lobby with stone floor and brass details`（安静酒店大堂，石材地面，黄铜细节）
- `cafe tabletop by a window`（窗边咖啡桌）
- `paper archive table with folders, notes, and soft dust`（纸质档案桌，文件夹、笔记、轻微灰尘）
- `empty cinematic room with a single practical light`（空房间，单个实用灯源）

### B3. Materials And Texture

This module fills the gap for material-specific prompts. Pick 2-5 materials that are visible in the scene.

- `natural walnut wood grain with satin finish`（胡桃木自然纹理，缎面清漆）
- `raw oak tabletop with subtle scratches`（原木橡木桌面，轻微划痕）
- `brushed aluminum, cool and softly reflective`（拉丝铝，冷调柔反射）
- `aged brass with oxidized edges`（旧黄铜，边缘氧化）
- `weathered copper with green patina`（风化铜，绿色铜锈）
- `matte ceramic glaze, imperfect handmade surface`（哑光陶瓷釉，手作不完美表面）
- `translucent frosted glass, soft internal glow`（磨砂半透明玻璃，柔和内发光）
- `clear glass with realistic refraction and edge highlights`（透明玻璃，真实折射和边缘高光）
- `Japanese washi paper fibers`（和纸纤维）
- `rough cotton rag paper, deckled edges`（棉浆纸，毛边）
- `linen fabric weave, dry matte texture`（亚麻织纹，干燥哑光）
- `felt surface, soft compressed fibers`（毛毡表面，压缩纤维）
- `worn leather patina, creases and rubbed edges`（旧皮革包浆，褶皱和磨边）
- `rough concrete wall with mineral variation`（粗水泥墙，矿物色差）
- `limewash plaster wall, cloudy blue-gray variation`（石灰洗墙，云雾蓝灰变化）
- `black lacquered wood with controlled reflections`（黑色漆木，克制反光）
- `matte plastic only if the product truly requires it`（只有产品需要时才写哑光塑料）

### B4. Composition

- `centered hero object with generous padding`（主体居中，四周留白）
- `lower-third object placement with empty upper field`（主体在下三分之一，上方留白）
- `diagonal tabletop arrangement`（桌面斜线构图）
- `top-down flat lay with aligned edges`（俯拍平铺，边缘对齐）
- `45-degree product view`（45 度产品视角）
- `foreground object partially out of focus`（前景物件虚化）
- `wide establishing scene with small subject`（广角建立场，主体较小）
- `negative space reserved for future title overlay`（为后续标题留白）

### B5. Lens And Viewpoint

- `35mm documentary still-life view`（35mm 纪实静物）
- `50mm natural product perspective`（50mm 自然产品视角）
- `85mm compressed object isolation`（85mm 压缩物件分离）
- `macro close-up showing material detail`（微距展示材质）
- `wide architectural view, vertical lines kept straight`（建筑广角，垂直线保持直）
- `top-down orthographic flat-lay feeling`（正俯拍平铺感）
- `eye-level tabletop view`（桌面平视）

### B6. Lighting

- `soft window light from frame left`（左侧窗光）
- `warm desk lamp visible in frame`（画面内可见暖台灯）
- `large diffused softbox reflection on glass`（玻璃上的大柔光箱反射）
- `blue hour ambient light through the window`（窗外蓝调环境光）
- `single candle or small practical light, warm falloff`（蜡烛/小实用灯，暖衰减）
- `neon sign visible as the colored light source`（霓虹招牌作为可见彩色光源）
- `overcast daylight, soft shadow edges`（阴天光，柔边阴影）
- `low-key pool of light, surrounding darkness remains readable`（低调光池，周围暗部仍可读）

### B7. Color And Tonality

- `low saturation and restrained contrast`（低饱和低调对比）
- `cool blue-gray wall against warm amber lamp light`（冷蓝灰墙面与暖琥珀灯光）
- `warm cream, black, and aged brass palette`（暖奶油、黑、旧黄铜）
- `moss green, paper white, and ink black`（苔藓绿、纸白、墨黑）
- `charcoal, muted red, and sodium-vapor yellow`（炭黑、暗红、钠灯黄）
- `soft editorial neutrals with one accent color`（柔和编辑部中性色和一个强调色）
- `monochrome graphite and paper texture`（石墨黑白与纸张质感）

### B8. Negative Space And Text Area

- `large clean negative space in the top third`（上三分之一大留白）
- `clear left side reserved for later text overlay`（左侧为后续文字预留）
- `no generated text inside the image`（图内不生成文字）
- `composition should support future title placement without feeling empty`（留白要可放标题，但画面本身不空）
- `avoid busy background behind intended text area`（文字区背后不复杂）

### B9. Negative Constraints

- `No text, no letters, no numbers, no fake handwriting`（无文字、字母、数字、假手写）
- `No watermark, logo, QR code, UI screenshot, or brand mark`（无水印、logo、二维码、UI 截图、品牌标）
- `No generic stock photo look`（无图库感）
- `No glossy plastic render unless explicitly requested`（非明确要求不做塑料 3D 感）
- `No random props unrelated to the subject`（无无关道具）
- `No impossible reflections or floating shadows`（无错误反射或漂浮阴影）

## C. Series Expansion And Lock Templates

Use this section when the task asks for a set, series, candidate images, EVE portrait sequence, ROV x EVE episode, recurring product style, or any workflow where one image becomes the anchor for later images.

### C1. Mother Image Rule

The first generated image in a production task is not a test. It is usually the mother image.

- The mother image must be a real deliverable for the task.
- It should stay close to the initial reference angle, usually front-facing or near-front for face-lock work.
- It locks environment, scene, clothing, makeup/grooming, accessories, color palette, lighting source, film simulation, and overall visual language.
- Agent self-check counts as "mother image accepted enough to proceed" unless Robin interrupts. Robin will correct in chat if something is wrong.
- Usually generate one mother image. Use two only for genuinely separate visual anchors.

### C2. Images 2-4 Stability Variations

After the mother image, stabilize identity and style before changing too much:

- Keep front or near-front angle.
- Vary gaze direction, eye focus, hand gesture, crop, slight body posture, or a 10-20 degree head turn.
- Keep the same aspect ratio as the mother image.
- Keep clothing, scene, lighting, and color language close to the mother image unless the task requires a controlled change.
- Do not jump directly into side profile, back view, extreme low angle, or complex motion.

### C3. Image 5 Onward Expansion

Only after three stable images should the sequence expand more strongly:

- Move to 45-degree view, three-quarter body, full body, over-shoulder, foreground layers, doorway framing, low angle, high angle, leading lines, or stronger environment storytelling.
- Each next image needs a reason: new scene beat, new composition, new action, new camera distance, new emotional beat, or correction from the previous image.
- Think, compose, generate, inspect, then design the next image. Do not run a mechanical continuous batch.
- Parallel generation is not part of this workflow unless Robin explicitly confirms it, and even then the maximum is two simultaneous images.

### C4. EVE Real-Person Identity Lock

Use only for EVE real-person portrait / creative portrait work. Keep this as English prompt text. The Chinese notes are for Robin and agent understanding.

For `Eve_Frames` project work, load project identity facts, reference-image source, and QC rejection rules from `/Users/robin/AltmanCodex/Eve_Frames/_workspace/refs/EVE_IDENTITY_LOCK.md`. This C4 section is the executable prompt template; the project file is the identity/QC fact source. Do not maintain a second executable EVE lock template inside project notes.

```text
[CHARACTER LOCK - EVE PRIMARY DIRECTIVE]
Use the provided EVE reference image as the primary identity source, but do not copy-paste the reference face unchanged. Preserve Eve's identity and facial feature relationships while adapting naturally to this camera angle, expression, lighting, lens perspective, and body pose. The face, head, neck, shoulders, and body must read as one coherent real person in the current scene. Do not reinterpret her as a similar-looking model, idol, influencer, or generic commercial beauty.

[FIXED IDENTITY]
East Asian woman with Chinese-Japanese mixed-heritage feeling. Age impression: 20-23. Tall model presence without exaggerating body metrics. Raw mixed-heritage bone structure: high cheekbones, defined jawline, natural facial asymmetry allowed.

[EYES]
Deep-set, heavy-lidded almond eyes. Slight downward tilt at the outer corners. Dark brown iris, nearly black. Narrow eye opening, not wide or round. Gaze carries quiet intensity.

[LIPS]
Very full natural lips, upper and lower both visibly voluminous. Soft rounded upper-lip arch, not a sharp cupid's bow. Heavier lower lip with slight natural protrusion. Warm brick-rose color, natural soft sheen.

[NOSE]
Moderate bridge height. Naturally rounded, slightly fleshy nose tip. Medium nostril width. Do not sharpen, slim, or over-refine.

[SKIN]
Warm peach-honey undertone in lit areas. Retain pores and fine skin micro-texture. Natural luminosity only, no artificial glow, no smoothing, no plastic finish.

[HAIR]
Jet black straight hair past shoulders. Wispy curtain fringe or bangs falling naturally across the forehead. Medium density, natural movement, not overly styled.

[TEMPERAMENT]
Default temperament is restrained, composed, cool, and quietly intense. Adapt warmth, softness, playfulness, or friendliness only when the story theme calls for it, while preserving the same bone structure, eyes, lips, skin texture, and identity.

[NEGATIVE CONSTRAINTS]
Do not over-Westernize or over-Easternize facial features. Do not enlarge the eyes, sharpen the nose, reduce lip volume, whiten skin, over-smooth skin, or add heavy makeup unless the specific scene explicitly requires that makeup style. Do not paste the reference face unchanged; do not keep the passport-photo angle, expression, or lighting when the current camera angle, expression, lighting, or body pose is different.
```

中文理解要点：EVE 的核心是身份锚定、眼睛、唇部、鼻子、暖蜜桃皮肤、黑直发和克制气质；但不是永远不能温柔、亲切或可爱。题材需要时可以调整表情温度，不能换脸或变通用甜妹。

### C5. General Portrait Identity Lock

Use for non-EVE real people, fictional people, anime characters, semi-real characters, or any recurring humanoid identity.

```text
[CHARACTER LOCK - GENERAL]
Use the provided reference image as the identity anchor. Preserve the same person or character across scene, outfit, lighting, pose, camera angle, and art style changes.

[FIXED FEATURES]
Face shape: [describe only what is visible]
Eyes: [shape, size, gaze, iris color if known]
Nose: [bridge, tip, width]
Lips / mouth: [volume, shape, expression baseline]
Skin or surface: [tone, texture, stylization level]
Hair: [color, length, silhouette, bangs/fringe]
Body / posture: [only if relevant and known]
Temperament: [calm, playful, serious, gentle, intense, etc.]

[ALLOWED VARIATION]
Scene, clothing, lighting, crop, pose, expression intensity, and camera angle may change according to the brief. Keep identity stable.

[NEGATIVE CONSTRAINTS]
Do not change age impression, facial proportions, eye size, nose shape, lip volume, hairstyle silhouette, body build, or core temperament unless Robin explicitly asks.
```

### C6. Anime Character Identity Lock

Use for ROV x EVE, anime couple, manga panels, semi-real anime, and recurring illustrated characters.

```text
[ANIME CHARACTER LOCK]
Use the provided character reference sheet as the identity anchor. Preserve the same character design, hairstyle silhouette, eye shape, face proportions, height relationship, outfit logic, and relationship dynamic across scenes.

[FIXED DESIGN]
Character design remains consistent: face shape, eye shape, hair color, hair silhouette, body proportion, and recognizable outfit cues. Keep the couple relationship readable through distance, gaze, and shared action.

[STYLE]
Semi-realistic anime with grounded proportions, clean line art, soft cel shading, natural lifestyle lighting, and readable everyday environments.

[ALLOWED VARIATION]
Daily scene, activity, clothing coordination, camera angle, transparent UI layer, expression, and tutorial action may change. Do not redesign the characters.

[NEGATIVE CONSTRAINTS]
No generic anime faces, no chibi deformation unless requested, no random hair color changes, no inconsistent height relationship, no PPT-style UI, no empty couple pose without a useful scene action.
```

中文理解要点：动漫情侣系列不需要真人皮肤、真人骨相和身材库噪音。重点是角色设计、关系感、服饰协调、生活动作、教程信息层和场景连续。

### C7. Non-Human Style / Scene Lock

Use for product, still life, architecture, article covers, music covers, scene systems, and visual metaphors.

```text
[STYLE LOCK - NON-HUMAN]
Use the provided reference image or mother image as the style and environment anchor. Preserve the same visual system while allowing object arrangement and scene beat to change.

[LOCKED VISUAL SYSTEM]
Tonality / film simulation: [low saturation, Fujifilm Classic Chrome, Kodak Portra 400, etc.]
Lighting source: [visible object or natural source, direction, color temperature]
Location / environment: [room, street, desk, architecture, landscape]
Material language: [wood, stone, paper, metal, glass, fabric, ceramic]
Composition language: [negative space, centered hero, rule of thirds, frame within frame, leading lines]
Texture / grain: [film grain, paper grain, matte finish, reflective highlights]
Color palette: [2-4 dominant colors]

[ALLOWED VARIATION]
Object placement, crop, foreground layers, camera distance, scene detail, and minor prop changes may vary. Keep tonality, light logic, materials, and place identity consistent.

[NEGATIVE CONSTRAINTS]
Do not drift into a different aesthetic system, different film look, different lighting logic, stock-photo look, generic 3D render, fake text, watermarks, logos, or unrelated props.
```

### C8. Mother-Image Continuity Prompt

Use this structure for every image after the mother image:

```text
Use the original identity reference as the fixed identity anchor.
Use the mother image as the scene, styling, lighting, tonality, and material anchor.

Keep from the mother image:
- same environment family
- same wardrobe / styling logic
- same lighting source and color temperature
- same film simulation and saturation level
- same aspect ratio

Change in this image:
- [specific next scene beat]
- [specific camera angle or crop]
- [specific pose, gaze, hand action, or object interaction]

Do not change:
- identity
- core face features
- aspect ratio
- established film tone
```

## P. Project-Specific Modules

Use project modules only when the current task belongs to that project. Do not mix project-specific modules into generic prompts.

### P1. EVE Real-Person Series

Default:

- Aspect ratio: usually `16:9`; if Robin or the director plan chooses `9:16` or `3:4`, keep the whole series in that chosen ratio.
- Must use the EVE identity reference image through `--image` / `--reference`.
- Use C4 EVE lock, with identity facts and QC rules loaded from `/Users/robin/AltmanCodex/Eve_Frames/_workspace/refs/EVE_IDENTITY_LOCK.md` when the task belongs to `Eve_Frames`.
- Mother image: close to the identity reference angle, usually front or near-front, with readable environment and outfit.
- Expansion: images 2-4 are near-front stabilizers; image 5 onward may use stronger angle/composition.

Prompt ingredients:

- `cinematic environmental portrait`（电影环境人像）
- `EVE identity lock from reference image`（EVE 参考图锁脸）
- `same person, not a similar model`（同一个人，不是相似模特）
- `city slice / private interior / quiet cinematic location`（城市切片 / 私密室内 / 克制电影场景）
- `Fujifilm Classic Chrome or Kodak Vision3 depending on scene`（按场景选富士 CC 或柯达 Vision3）
- `visible practical light source`（可见实用光源）
- `low saturation, restrained premium mood`（低饱和克制高级感）

Avoid:

- broad body-shape library noise
- pure text-to-image EVE face
- generic idol beauty
- over-smoothed skin
- changing aspect ratio inside the series

### P2. ROV x EVE Anime Couple / Tutorial Series

Default:

- Aspect ratio: usually `9:16`.
- If the project chooses `3:4`, the whole series must stay `3:4`.
- Use `3:4` only as a separate cover/adaptation when explicitly needed.
- Must use character-lock reference images when generating official scenes.
- Use C6 anime character lock.

Prompt ingredients:

- `semi-realistic anime couple with consistent character designs`（半写实动漫情侣，角色设计一致）
- `warm everyday interior, tutorial scene embedded in real life`（温暖日常室内，教程藏在生活场景中）
- `transparent UI information layer in the environment`（环境中的透明信息层）
- `shared object interaction, not empty posing`（通过共同物件互动，不空摆）
- `coordinated outfits, not identical costumes`（服饰协调但不完全一样）
- `soft cel shading, clean manga composition`（柔和赛璐璐，干净漫画构图）

Avoid:

- realistic skin/face/body prompt modules
- PPT page, app screenshot, giant dashboard
- generic anime faces
- repeated same camera angle across the set
- mixed aspect ratios in the same pure image series

### P3. Arthur Observatory WeChat Cover / Article Image

Default:

- WeChat Official Account cover: fixed `2.35:1`.
- Other platform cover: usually `3:4`.
- In-article editorial images: choose `3:2`, `16:9`, or `3:4` based on article layout, but do not mix a production series unless they are separate use cases.

Prompt ingredients:

- `editorial magazine cover image`（编辑部杂志封面）
- `observer perspective, restrained and intelligent`（观察者视角，克制有智性）
- `symbolic object-led composition`（物件象征主视觉）
- `paper archive, desk, map, glass, screen reflection, or architectural metaphor`（纸质档案、桌面、地图、玻璃、屏幕反射、建筑隐喻）
- `large negative space for later title overlay`（为后续标题留白）
- `no generated text inside image`（图内不生成文字）

Avoid:

- celebrity likeness unless the task explicitly permits editorial concept treatment
- clickbait visual exaggeration
- fake text, fake charts, random logos

### P4. Paper Moon Music Cover / Music Film Visual

Default:

- Music cover / album visual: usually `1:1`.
- Music film frame: `16:9` for horizontal film, `9:16` for vertical social cut.
- Lyric poster / platform cover: choose one ratio for the series and keep it consistent.

Prompt ingredients:

- `album-cover visual system`（唱片封面视觉系统）
- `quiet cinematic atmosphere, low-interference image`（安静电影氛围，低干扰）
- `scene as an emotional room, not a marketing poster`（画面像情绪空间，不像营销海报）
- `film still, muted tones, restrained texture`（电影截图感，低饱和克制质感）
- `negative space that can hold lyrics later, but no generated text`（为后续歌词留白，但图内无字）
- `paper, moonlight, room, window, dust, tape, record, instrument, or sea as motif`（纸、月光、房间、窗、灰尘、磁带、唱片、乐器、海等母题）

Avoid:

- self-media hook visual
- loud typography inside generated image
- over-decorated fantasy glow

### P5. Web6 Media Console Output

Default:

- Use the app's selected ratio if the local UI provides one.
- If no ratio is selected, follow the general default `3:4`.
- For screenshots or UI proofs, keep the actual viewport ratio instead of inventing an image ratio.

Prompt ingredients:

- `private media generation console output`（私人媒体生成控制台产物）
- `respect uploaded reference images via --image`（尊重上传参考图）
- `clean prompt parameters, no pixel dimensions in prompt`（提示词写比例，不写像素）
- `output path and preview copy follow chatgpt-imagegen rules`（输出路径和预览副本遵循 skill 规则）

Avoid:

- unrelated smoke tests
- generic generated filenames
- converting app ratio choices into prompt pixel dimensions

### P6. Rubin Motion Lab Video Cover / Background Asset

Default:

- Video background / frame: match the video project ratio, usually `16:9` or `9:16`.
- Cover adaptation: `3:4` only when requested as a separate cover output.
- Do not mix ratios inside one visual asset set.

Prompt ingredients:

- `motion video background asset`（视频背景资产）
- `low-noise composition that leaves room for motion graphics`（低噪声构图，为动态图形留空间）
- `editorial illustration or cinematic still depending on template`（按模板选编辑插画或电影静帧）
- `clear subject hierarchy, one main visual idea`（层级清楚，一个主视觉）
- `no generated text unless the video template explicitly requires text baked into the image`（除非模板要求，否则图内不生成字）

Avoid:

- busy background that fights captions
- inconsistent aspect ratios across a scene set
- style that conflicts with the chosen Remotion template
