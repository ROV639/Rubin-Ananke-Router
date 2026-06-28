# Visual Parameters Reference

Use this reference when a prompt needs visual direction, especially for people, recurring characters, mother-image expansion, image-to-image continuity, or project image sets. These are reference parameters, not mandatory ingredients. Choose only the categories that help the current image; do not stuff every option into one prompt.

## Table of Contents

- Mother image and expansion workflow
- Portrait / character parameter index
- Non-human parameter index
- Lock template 1: EVE real-person portrait
- Lock template 2: general portrait / character
- Lock template 3: non-human style / scene lock

## Mother Image And Expansion Workflow

The mother image is not a throwaway sample. It is the continuity anchor for later images.

Startup plan:
- Before the first generation, briefly tell Robin: aspect ratio, expected image count, mother image goal, environment/scene, clothing/styling, tonality/film style, and the expansion path.
- If Robin does not object, proceed. During generation, do not ask Robin to approve every image unless Robin requested that. Show each image in chat; Robin can interrupt if something is wrong.

Mother image:
- Usually generate **one** mother image. Use two only when the task genuinely has two separate visual anchors.
- The mother image should stay close to the initial reference image angle, usually front-facing or the same angle as the identity reference.
- It should lock environment, scene, clothing, makeup/grooming, accessories, color palette, lighting source, film simulation, and overall visual language.
- For face-lock tasks, use the original identity reference as the identity anchor and the mother image as the scene/style anchor. Do not overload the model with unnecessary reference images.

Expansion ladder:
- Image 1: mother image, front-facing or same angle as the initial reference.
- Images 2-4: stability images. Keep front or near-front angle; vary gaze direction, eye focus, a 10-20 degree head turn, hand gesture, small crop shift, or minor pose. Do not jump straight to a full side profile or extreme pose.
- Image 5 onward: expand composition and shooting method: 45-degree turn, three-quarter body, full body, over-shoulder, foreground layers, frame-within-frame, low angle, high angle, leading lines, negative space, or stronger environment storytelling.
- Each image needs its own visual purpose. Think, generate, inspect, then design the next image. Never mechanically run a continuous batch.

## Portrait / Character Parameter Index

Use for any image involving a person, human-like subject, anime character, recurring IP character, EVE, ROV, fashion portrait, headshot, half-body, full-body, lifestyle scene, or character-consistency task.

Prompt slot order:

```text
IDENTITY LOCK / CHARACTER CONTINUITY
SUBJECT DIRECTION
SCENE + STORY BEAT
WARDROBE / HAIR / MAKEUP
POSE / EXPRESSION
ANGLE / COMPOSITION
LENS / DEPTH
LIGHT / TONALITY / FILM
EXCLUDE
```

Reference categories from the Web6 portrait library:

| Category | Useful options |
|---|---|
| Subject direction | Chinese cinematic heroine; Chinese high-fashion model; Japanese intellectual elegance; Korean cinema actress; natural authentic woman; cinematic male portrait |
| Shooting style | Vogue fashion editorial; cinematic film still; professional studio portrait; street style fashion; retro Hong Kong style; editorial fashion photography; cozy home lifestyle |
| Skin / makeup | realistic skin texture; no-makeup makeup; soft glam; warm cream complexion; sun-kissed freckles; full glam only when the brief calls for it |
| Body / posture | tall model proportions; balanced lean frame; athletic toned physique; ballet neck and shoulders; natural average build; mature composed male build |
| Hair | long straight black hair; soft waves; low ponytail; half updo; bob haircut; curtain bangs; French bangs; wet hair look |
| Clothing | satin slip dress; little black dress; tailored suit; trench coat; streetwear; knit sweater; modern qipao; professional pencil skirt; avant-garde black outfit |
| Pose / expression | direct camera eye contact; thoughtful inward gaze; restrained cool expression; natural gentle smile if the scene asks for warmth; walking motion; adjusting hair; leaning against wall; reading; holding camera |
| Scene | living room; cafe; art gallery; private study library; studio backdrop; yoga/pilates room; city rooftop; urban street; neon alley; beach; flower field; forest |
| Tonality / film | high-key bright; low-key dark; high contrast; low saturation; warm palette; cool palette; Fujifilm Classic Chrome; Classic Neg; Eterna Cinema; Kodak Portra 400; Kodak Vision3 500T |
| Light | overcast diffused daylight; golden hour; blue hour; soft window light; butterfly light; loop light; Rembrandt light; split light; rim backlight; clamshell light |
| Lens / depth | 24mm environmental portrait; 35mm documentary; 50mm natural perspective; 85mm flattering portrait; 135mm compressed portrait; f/1.8 shallow depth; f/5.6 readable environment; foreground bokeh |
| Angle / composition | front view; near-front 10-20 degree head turn; three-quarter view; half-body; full-body; eye-level; low angle; high angle; over-shoulder; rule of thirds; centered symmetry; frame within frame; leading lines; negative space; foreground layers |

## Non-Human Parameter Index

Use for still life, product, architecture, food, landscape, abstract visual systems, scene mockups, object-led covers, and non-human visual metaphors. These options can also support human scenes when the environment needs stronger art direction.

Prompt slot order:

```text
STYLE / VISUAL SYSTEM
SUBJECT TYPE
SCENE / ENVIRONMENT
MATERIAL / TEXTURE
COMPOSITION / CAMERA
LIGHT / TONALITY / FILM
NEGATIVE SPACE / TEXT PLACEMENT NEEDS
EXCLUDE
```

Reference categories:

| Category | Useful options |
|---|---|
| Subject type | object-only hero image; product scene; architectural space; food/drink still life; document/tabletop still life; abstract visual system; realistic scene mockup |
| Scene | clean studio backdrop; wood desk; private study; cafe table; hotel lobby; gallery space; street at night; rain-wet pavement; natural landscape; designed interior |
| Material / texture | natural wood grain; stone texture; brushed metal; glass reflection; paper grain; fabric weave; ceramic glaze; aged patina; matte plastic only when needed |
| Tonality / film | high-key airy; low-key dark; high contrast; low saturation; warm golden; cool blue-green; Fujifilm Classic Chrome; Kodak Portra 400; Vision3 500T |
| Light | soft window light; large softbox; warm desk lamp; neon sign with visible source; colored gel; candlelight; blue hour; after-rain reflections |
| Lens / camera | 24mm wide environmental view; 35mm documentary still life; 50mm natural product view; 85mm compressed subject isolation; macro close-up; 45-degree product view; top-down flat lay |
| Composition | centered hero composition; rule of thirds; negative space; frame within frame; leading lines; repeating pattern; foreground layers; wide establishing view |

## Lock Template 1: EVE Real-Person Portrait

Compatibility note: the executable EVE real-person lock template now lives in `references/prompt-parameters.md` section `C4. EVE Real-Person Identity Lock`. Use that C4 section for new prompts. This section is retained only as a legacy pointer so agents do not copy a second EVE template from this compatibility file.

```text
Load and use: references/prompt-parameters.md -> C4. EVE Real-Person Identity Lock
For Eve_Frames project facts and QC: /Users/robin/AltmanCodex/Eve_Frames/_workspace/refs/EVE_IDENTITY_LOCK.md
```

中文理解要点：EVE 的核心身份事实和 QC 红线在 `Eve_Frames/_workspace/refs/EVE_IDENTITY_LOCK.md`；可执行英文锁脸模板只维护在 `prompt-parameters.md` 的 C4。不要把本文件当成第二套 EVE prompt 模板。

## Lock Template 2: General Portrait / Character

Use for real people, fictional characters, anime characters, semi-real subjects, or any recurring humanoid identity. Fill only what you know; do not invent exact ethnicity, age, or body metrics unless Robin provided them.

```text
[CHARACTER LOCK — GENERAL]
The provided reference image is the identity anchor. Preserve the same person/character across scene, outfit, lighting, pose, camera angle, and art style changes.

[FIXED IDENTITY FEATURES]
Face shape: [describe]
Eyes: [shape, size, gaze, iris color if known]
Nose: [bridge, tip, width]
Lips / mouth: [volume, shape, expression baseline]
Skin / surface: [tone, texture, stylization level]
Hair: [color, length, silhouette, bangs/fringe]
Body / posture: [height impression, build, posture]
Temperament: [calm, playful, serious, gentle, intense, etc.]

[ALLOWED VARIATION]
Scene, clothing, lighting, crop, pose, expression intensity, and camera angle may change according to the brief. Keep identity stable.

[NEGATIVE CONSTRAINTS]
Do not change age impression, facial proportions, eye size, nose shape, lip volume, hairstyle silhouette, body build, or core temperament unless Robin explicitly asks.
```

## Lock Template 3: Non-Human Style / Scene Lock

Use for products, still life, architecture, places, visual metaphors, object-led covers, or any non-human series where style continuity matters.

```text
[STYLE LOCK — NON-HUMAN]
Use the provided reference image or mother image as the style and environment anchor. Preserve the same visual system while allowing subject arrangement and scene beat to change.

[LOCKED VISUAL SYSTEM]
Tonality / film simulation: [e.g. low saturation, Fujifilm Classic Chrome, Kodak Portra 400]
Lighting source: [visible object or natural source; direction and color]
Location / environment: [room, street, desk, architecture, landscape]
Material language: [wood, stone, paper, metal, glass, fabric, ceramic]
Composition language: [negative space, centered hero, rule of thirds, frame within frame, leading lines]
Texture / grain: [film grain, paper grain, matte finish, reflective highlights]
Color palette: [2-4 dominant colors or hex codes]

[ALLOWED VARIATION]
Object placement, crop, foreground layers, camera distance, scene detail, and minor prop changes may vary. Keep the tonality, light logic, materials, and place identity consistent.

[NEGATIVE CONSTRAINTS]
Do not drift into a different aesthetic system, different film look, different lighting logic, stock-photo look, generic 3D render, fake text, watermarks, logos, or unrelated props.
```
