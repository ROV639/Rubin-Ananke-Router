---
name: "chatgpt-imagegen"
version: "0.3.6"
description: "Generate or transform raster images (PNG/JPEG/WebP) using the user's ChatGPT subscription via a local one-file Python CLI — no OPENAI_API_KEY, no gateway, no daemon. Use when an agent needs to create a bitmap asset for the current project (photos, illustrations, icons, hero banners, mockups, sprites, concept art), or create a new variant from one or more reference images, and the output should be a bitmap file saved into the workspace. Do not use when the task is better solved by editing existing SVG/vector assets, writing code-native graphics (HTML/CSS/canvas), or extending an established repo icon system."
---

# chatgpt-imagegen — agent skill

A two-route image workflow for Robin's ChatGPT subscription:

1. **Preferred route:** use Codex's in-app browser to open ChatGPT Web, enter the task's designated ChatGPT project, create/name a project conversation, and generate images there one by one.
2. **Fallback route:** use the bundled Python CLI / Codex image skill when the web route is unavailable, stuck, logged out, quota-blocked, or the user explicitly asks for the CLI route.

The Python CLI is a thin wrapper around ChatGPT's internal `image_generation` Responses-API tool, the same one the Codex CLI invokes. No API key, no network service, no extra config.

## Mandatory Quick Check

Before any production image generation for Robin, run this check in order:

```text
1. Which ChatGPT project? If unknown, ask Robin.
2. Open that ChatGPT project in Codex's in-app browser.
3. New task: send one startup message with the task theme. Same task: reuse the existing named conversation.
4. Refresh once if the new conversation/title/menu is not visible yet.
5. Rename the conversation to YYYY-MM-DD_<theme>.
6. Generate images only after the conversation name is visible.
```

For ChatGPT Web, the stable tested place for renaming is the conversation row inside the project page list: three-dot menu -> Rename. The top conversation menu may not show Rename.

If image generation appears stuck for about 2 minutes, refresh once and inspect the real state. Do not treat it as failed unless the UI explicitly shows failure or refresh confirms there is no usable result and no active generation.

## Preferred Route A — ChatGPT Web Project

Use this route by default for Robin's production image workflows when a ChatGPT project is available or the user specifies one. The project name is task-specific and must not be hard-coded here. If no project is known, ask Robin which ChatGPT project to use before producing images.

### Required conversation setup

1. Open ChatGPT Web in Codex's in-app browser.
2. Enter the designated ChatGPT project: `<ChatGPT project name from the active task>`.
3. Start a new project conversation only when this is a new image task. If continuing the same task, reuse the existing task conversation.
4. Send one startup message containing the task theme/name. This creates the conversation and lets the project load its instructions and knowledge.
5. **Refresh after the startup response if the new conversation does not appear in the sidebar, the title is stale, or the rename controls are missing.** ChatGPT Web often lags; refresh is part of the normal workflow, not an error recovery afterthought.
6. Rename the conversation to `YYYY-MM-DD_<theme>` before formal image generation. If the title auto-generates first, still rename it to the project naming pattern.
7. Only after the conversation is named, begin formal image generation.

Do not leave unnamed image-generation conversations in ChatGPT projects. Robin uses conversation names to map images back to tasks.

### Web generation loop

- Generate sequentially: one prompt, one image, one save/QC step. Do not run parallel browser image generations.
- If an image has not completed after about **2 minutes**, refresh the page and inspect the conversation state before judging. The web UI can stall visually while the backend result exists.
- Only mark a web generation as failed when the ChatGPT UI explicitly shows a generation failure/error, or when refresh confirms there is still no usable result and the UI is not actively generating.
- If refresh recovers the image or conversation state, continue in the same conversation.
- If the web route is logged out, quota-blocked, repeatedly stuck, or cannot access the designated project, report that route state and then use Route B only if the active project permits fallback.

### Naming and reuse rules

- New task = new project conversation + startup message + rename.
- Same task = same project conversation, including prompt revisions and regeneration.
- Task change = new conversation.
- If the user supplies a different ChatGPT project for the new task, switch projects before the startup message.

## Fallback Route B — CLI / Codex Image Skill

Use the CLI route only when Route A is unavailable, blocked, or explicitly requested. All save-path, preview, prompt-composition, quota, and result-delivery rules below still apply.

## Prerequisites

The user must have run, **once, ever**:

```bash
npm i -g @openai/codex
codex login    # opens browser to sign in to ChatGPT
```

That writes `~/.codex/auth.json`, which this skill reads. If the file is missing, tell the user to run the two commands above and stop. Do not try to authenticate any other way.

No `OPENAI_API_KEY` is required — and setting one will not help. This is the subscription path, not the API path.

## Version fields: mandatory mismatch policy

`chatgpt-imagegen 0.2.4`, `chatgpt-imagegen 0.2.3`, or `chatgpt-imagegen 0.2.2` is **not** the Skill version. It is only the executable Python CLI version printed by `chatgpt-imagegen --version`.

There are two independent version numbers:

- `SKILL.md` frontmatter `version` is the workflow/documentation version. It changes when Robin's save-path rules, preview protocol, mother-image expansion logic, prompt libraries, lock templates, or reference-file structure are updated.
- `chatgpt-imagegen --version` (currently often `chatgpt-imagegen 0.2.4`) is the Python CLI script version. It changes only when the executable script itself is updated.

These versions do **not** need to match. A newer Skill document can intentionally wrap an older stable CLI script.

Mandatory agent behavior:
- If the CLI prints `chatgpt-imagegen 0.2.4` or another script version, label it as `CLI version: chatgpt-imagegen X.Y.Z`.
- If the Skill frontmatter says `version: "X.Y.Z"`, label it as `SKILL.md workflow version: X.Y.Z`.
- Do **not** compare these two fields as if they should be equal.
- Do **not** stop, reinstall, ask Robin for clarification, or report a failure just because the CLI version and Skill document version differ.
- Treat a mismatch as normal unless the CLI is missing, cannot run, or truly lacks a flag required by this Skill, such as `--image` / `--reference`.
- When reporting diagnostics, write both labels explicitly: `CLI version: ...` and `Skill workflow version: ...`.

## When to use

- The user asks for a new photo, illustration, icon, hero banner, sprite, cover image, infographic, product mockup, concept art, or any other bitmap deliverable for the current project.
- The user provides one or more reference images and asks for a new version, style transfer, character/subject-preserving variant, or image-to-image generation.
- Reference-image / image-to-image work is supported via `--image` or `--reference`. Do not describe this skill as text-to-image only. The boundary is precision: it can make new variants from reference images, but it is not a deterministic mask/inpainting editor.
- The user is happy with subscription-tier quality (`medium` quality, no native transparent backgrounds — see *Limits* below).
- The deliverable is intended to be saved into the repo or build inputs.

## When not to use

- The user wants an SVG icon that matches an in-repo vector set — edit those instead.
- The task is better solved with code (HTML/CSS, canvas, Mermaid, PlantUML).
- The user needs precise pixel-level editing, masking, local object removal, or exact inpainting. This CLI supports reference-image generation via `--image`, not deterministic masked edits.
- The user explicitly needs **true `quality=high`** or **`background=transparent`** — the subscription path caps quality at `medium` and rejects transparent. Tell the user to use the official `/v1/images/generations` API with their `OPENAI_API_KEY` for those cases.
- The deliverable will be served to end users (e.g. a public service generating images for visitors) — that violates OpenAI's ToS for personal subscriptions. Refuse and explain.

## How to invoke Route B

```bash
"<skill-dir>/chatgpt-imagegen" "<prompt>" [options]
```

When this skill is installed via `npx skills add leeguooooo/chatgpt-imagegen -g`, the bundled `chatgpt-imagegen` script sits next to this `SKILL.md`. Call it by its absolute path — that is the most reliable way and never depends on `$PATH`. If your agent harness exposes a variable that points to the skill's install directory, use it; otherwise expand the path you read this file from.

If the user has separately put `chatgpt-imagegen` on `$PATH` (Option B in the README), you can also just run `chatgpt-imagegen "<prompt>"` directly.

Useful flags:

| Flag | When to use |
| --- | --- |
| `-o PATH` | Always use when you know where the file should go in the repo. |
| `--image PATH`, `--reference PATH` | Use for image-to-image generation from a local reference image. Repeat for multiple references. Supports PNG/JPEG/WebP. |
| `--size 1024x1024` | Square icons / logos (verified CLI size) |
| `--size 1536x1024` | Landscape hero banners, social cards (verified CLI size) |
| `--size 1024x1536` | Portrait covers, mobile splashes (verified CLI size) |
| `--size WIDTHxHEIGHT` | Internal execution size derived from the chosen aspect ratio. Do not put pixel dimensions in the prompt text. |
| `--format webp` | Smaller files for web assets |
| `--quiet` | Use in agent contexts so stdout is *only* the saved path. Progress still streams to stderr (use `--no-progress` to silence it). |
| `--no-progress` | Fully silence the stderr progress timeline (errors still print). |
| `--timeout SECONDS` | Total wall-clock budget (default 300). Large/detailed images can take 2–3 min — raise it if you see a `timed out` error. |
| `--stall-timeout SECONDS` | Max silence (no data from backend) before declaring a stall (default 120, clamped to `--timeout`). Lower it to fail faster on a hung backend. |
| `--log-file PATH` | Append a JSONL run record. Default: `~/Library/Logs/chatgpt-imagegen/runs.jsonl`. Use `none` to disable. |
| `--max-log-bytes N` | Rotate the JSONL log after it grows beyond this size. Default: `2097152` bytes (2 MB). |
| `--max-log-files N` | Number of rotated logs to keep. Default: `3`; use `0` to keep only the active log. |
| `--max-prompt-chars N` | Preflight safety limit for prompt length. Default: `12000`. Raise only when Robin explicitly accepts the risk. |
| `--max-reference-images N` | Preflight safety limit for reference image count. Default: `4`. Raise only when Robin explicitly accepts the risk. |
| `--max-reference-mb N` | Preflight safety limit per reference image. Default: `25` MB. Raise only when Robin explicitly accepts the risk. |
| `-V`, `--version` | Print the executable CLI version and exit, e.g. `chatgpt-imagegen 0.2.2`. This is not the `SKILL.md` workflow version and must not be compared with it. |

The script prints **just the saved path on stdout** in every mode; the readable progress timeline and any errors go to **stderr**, so `OUT=$(chatgpt-imagegen "..." --quiet)` captures only the path while you still see the timeline. Each timeline line is stamped with elapsed seconds (`[ 12.3s] generating`), so a slow run is legible and a stall is obvious.

The script also appends a compact JSONL run record to `~/Library/Logs/chatgpt-imagegen/runs.jsonl` by default. The log records status, CLI version, model, size, format, prompt character count, prompt hash, reference count, reference file names/sizes, output path, elapsed time, HTTP status when available, and a short error string. It intentionally does **not** store OAuth tokens or the full prompt. The active log rotates at about 2 MB by default and keeps 3 older files (`runs.jsonl.1`, `.2`, `.3`), so the log is useful for recent diagnosis without becoming a permanent bulky archive.

## Production guardrail: no unrelated test images

Do not run a capability test, smoke test, random sample prompt, or unrelated "first image" in a real user task. Robin already knows whether the tool works; spending a generation on a generic test image wastes quota and pollutes project folders.

Only generate a test image when Robin explicitly asks to test the tool itself, for example "测试一下生图功能" or "跑一个 smoke test". In every production workflow, the first `chatgpt-imagegen` call must be the task's first real deliverable: usually the mother image / reference image / first scene image for that project. If the task needs validation, validate with that mother image rather than a throwaway prompt.

Do not create filenames containing `test`, `smoke`, `sample`, `demo`, or `generated` for production image tasks. Name the first image by its actual subject and role, such as `mother`, `cover`, `scene-01`, or a concrete scene slug.

## Prompt Engineering — Modular Composer

Use `references/prompt-parameters.md` as the primary prompt library when the task needs visual planning, portrait continuity, project-specific image direction, material/texture language, lighting, film tone, aspect-ratio choice, or a multi-image series.

Do not copy-paste a whole parameter section into the prompt. Select the relevant modules, rewrite them into natural English, remove duplicates/conflicts, and add the task's specific subject, scene, clothing, props, platform, or project context.

Core prompt order:

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

Rules:
- Prompt text must write **aspect ratio only**, for example `Aspect ratio: 3:4 vertical portrait`. Do not put `1536x2048`, `2048px`, `4K`, or `8K` in the prompt.
- Pixel size belongs only to the CLI `--size` flag. Treat `--size` as an internal execution mapping, not a prompt ingredient.
- Avoid empty quality stuffing: no `8K`, `masterpiece`, `best quality`, `ultra-sharp 8K detail`, or similar filler.
- Name a visible or believable lighting source. Do not write only "cinematic lighting" or "dramatic lighting" without the actual source.
- For reference-image work, explicitly say what must stay fixed and what may change.
- For simple 2-3 element icons or abstract marks, use a short prompt: style + subject + aspect ratio + constraints.

Load `references/prompt-parameters.md` when any of these apply:
- portrait / character image generation
- recurring identity, EVE / ROV, face lock, character lock, or reference-image continuity
- mother-image expansion, candidate image sets, or any multi-image sequence
- still life, product, architecture, object-led cover, music cover, article cover, or non-human scene where style/material continuity matters
- project-specific work for EVE, ROV x EVE, Arthur Observatory, Paper Moon, Web6, or Rubin Motion Lab

Do not load the reference file for a very simple one-off icon or when Robin supplied a complete prompt and no style/identity planning is needed.

---

### CANON / aesthetic library updates

Do **not** update CANON or any aesthetic style library automatically.

User approval of an individual generated image — e.g. "通过", "可以", "满意", "就这张", or similar — only means the current image is accepted for the current task. It does **not** mean the image, prompt, style recipe, color palette, composition, or filename should be added to CANON.

Before writing anything into CANON or an aesthetic library, ask Robin explicitly whether this specific result should be canonized. Proceed only after Robin clearly confirms the CANON update. If the confirmation is ambiguous, do not update CANON.

---

### Quota guardrails

Do **not** generate multiple images from the same or substantially similar prompt. One prompt gets one image. If Robin wants another attempt, require a meaningful prompt change first: different subject, composition, style, lighting, color palette, or concrete correction based on the previous output.

Planned expansion after an accepted/self-checked mother image is allowed only when each next image has its own visual role, composition, angle, pose, lighting, scene beat, or correction target. Treat expansion as sequential visual direction, not as bulk prompt repetition.

Treat these as duplicate generation requests and refuse or ask for a prompt change before spending quota:
- "再来一张", "多出几张", "生成几版", "用同一段提示词再跑"
- same prompt with only punctuation, whitespace, seed, filename, or minor wording changes
- parallel or batch requests where two jobs use substantially similar prompts

Parallel generation is allowed only when Robin explicitly confirms it and each prompt is clearly different. Cap concurrency at **2 simultaneous images**. Never launch larger batches.

---

For multi-image portrait or character tasks, start by telling Robin a compact production plan before the first generation: aspect ratio, expected image count, mother image goal, scene/environment, clothing/styling, tonality, and the planned expansion ladder. After that, do **not** stop for confirmation between images unless Robin requested per-image approval. Show each generated image in the conversation; Robin can interrupt with corrections if needed. The agent should self-check and continue sequentially.

## Size and aspect-ratio policy

Choose the visual aspect ratio first, then map it to the CLI `--size` flag. The prompt itself should name only the aspect ratio, never pixel dimensions. If the user writes a Midjourney-style aspect flag such as `--ar 3:4`, preserve `Aspect ratio: 3:4` in the prompt and convert it to `--size WIDTHxHEIGHT` for the CLI; the CLI does not parse `--ar` itself.

For series or batch-style work, keep one aspect ratio for the whole image series. A separate cover/adaptation can use another ratio only when it is explicitly a separate output.

Use these internal CLI size mappings first. Prefer dimensions that are multiples of 16; they align with the current GPT image size constraints better than arbitrary rounded values.

| Aspect ratio | Preferred `--size` | Fallback |
| --- | --- | --- |
| `1:1` | `2048x2048` | `1024x1024` |
| `3:4` | `1536x2048` | `1024x1536` |
| `4:5` | `1632x2048` | `1216x1536` |
| `4:3` | `2048x1536` | `1536x1024` |
| `2:3` | `1360x2048` | `1024x1536` |
| `3:2` | `2048x1360` | `1536x1024` |
| `16:9` | `2048x1152` | `1536x1024` |
| `9:16` | `1152x2048` | `1024x1536` |
| `2.35:1` | `2048x864` | `1536x656` |
| `1.91:1` | `2048x1072` | `1536x800` |
| `2.39:1` | `2032x848` | `1536x640` |
| `21:9` | `2560x1088` | `1536x656` |
| `3:1` | `2048x688` | `1536x512` |
| `1:3` | `688x2048` | `512x1536` |
| `5:4` | `2048x1632` | `1536x1216` |
| `1.618:1` | `2048x1264` | `1536x944` |

For unlisted ratios, keep the long edge around `2048` pixels and compute the other edge from the ratio. If the backend rejects the size, retry once with the fallback nearest to the requested composition. Do not use `512x512`; the current backend rejects it as below the minimum pixel budget.

## Preflight and failure logging

Default safety limits exist to prevent high-risk calls from wasting time or quota:

| Input | Default safety limit | Action before raising |
| --- | --- | --- |
| Prompt length | `12000` characters | Shorten and structure the prompt; remove duplicated parameter blocks and quality filler. |
| Reference images | `4` images | Use the fewest decisive references: identity anchor, mother/style anchor, and only essential supporting references. |
| Reference image size | `25` MB per image | Compress or resize the reference image before calling the CLI. |

These are operational safety limits, not guaranteed backend hard limits. Do not raise them casually. Raise `--max-prompt-chars`, `--max-reference-images`, or `--max-reference-mb` only when Robin explicitly accepts the higher failure risk.

Default log retention is intentionally small:

- Active log: `~/Library/Logs/chatgpt-imagegen/runs.jsonl`
- Rotation: about `2 MB` per file
- Retention: active file + `3` rotated files (`runs.jsonl.1`, `runs.jsonl.2`, `runs.jsonl.3`)

Check the log when a generation fails, stalls, returns no image, or behaves differently from the prompt/reference setup. Do not inspect or paste logs after every successful run; that creates noise. Do not paste a large log into chat. Summarize only the latest relevant record and, if needed, list file sizes with:

```bash
ls -lh ~/Library/Logs/chatgpt-imagegen/
```

On any failure, do not blindly retry. First inspect the latest JSONL record:

```bash
tail -n 1 ~/Library/Logs/chatgpt-imagegen/runs.jsonl
```

Report the actionable fields to Robin:

- `status`
- `error_type`
- `error`
- `prompt_chars`
- `reference_count`
- reference image file names and sizes
- `http_status`, if present
- `elapsed_seconds`

Then choose one targeted correction:

- `preflight_failed` with long prompt: shorten the prompt and remove redundant modules.
- `preflight_failed` with too many references: reduce references to the strongest identity/style anchors.
- `preflight_failed` with oversized reference: compress or resize the image.
- `HTTP 429 usage_limit_reached`: stop and wait for reset; do not retry in a loop.
- `stalled` or `timed out`: retry once with a higher timeout only if the prompt/reference set is otherwise reasonable.
- `no image returned`: rephrase to explicitly request `Use the image_generation tool to render...`.

## Save-path policy

1. Choose a final/archive path first. If the user named a destination, pass it via `-o`. If the active project rules imply a project folder, use that folder. If no destination is implied, use Robin's default save path below.
2. Also keep a chat-preview copy under the active workspace with an ASCII-only path, so Codex desktop and mobile can render it in-thread. If the final path is already an absolute ASCII path under the workspace, it can serve as both final and preview.
3. Never use `/tmp`, `$HOME`, `~/.codex/...`, or a Chinese/emoji path as the Markdown preview path. Those may exist on disk but are unreliable for mobile thread preview.
4. Don't overwrite existing files unless the user asked. With `-o` the script overwrites silently; without `-o` it auto-numbers (`name.png`, `name-2.png`).
5. After saving, **show the image preview in chat and echo the final path back to the user**. Follow the result delivery protocol below; do not return only a path unless Robin explicitly asks for path-only output.

### Robin default save path

This is the **catch-all fallback for any image task where no output path was specified** and no project rule or context implies a destination. The folder syncs to Robin's phone, so images are immediately retrievable without searching deep directory trees.

Use this path when: no `-o` argument was given, no project folder is implied by context, and no active project rule specifies a save location.

Do **not** use this path when: a project-specific rule or reference module implies another project directory, or the user has named a destination.

Save to:

```text
/Users/robin/Rubin-Studio/🧵X碎碎念
```

Name files according to the existing pattern in that folder:

```text
YYYY-MM-DD_X平台_<image-topic>.png
```

Use the current local date. Derive `<image-topic>` from the actual image subject/style, keep it short and descriptive, and avoid generic names such as `image`, `test`, `smoke`, `sample`, `demo`, or `generated`. Examples:

```text
2026-06-11_X平台_雾蓝书桌胶片静物.png
2026-06-11_X平台_霓虹夜街皮衣人像.png
```

### Preview copy naming

For the in-chat preview copy, use an ASCII-only filename under the active workspace:

```text
<active-workspace>/_workspace/preview_images/YYYY-MM-DD-chatgpt-imagegen-<ascii-topic-slug>.<ext>
```

Examples:

```text
/Users/robin/AltmanCodex/_workspace/preview_images/2026-06-12-chatgpt-imagegen-neo-noir-library.png
/Users/robin/AltmanCodex/Rov_Eve/_workspace/preview_images/2026-06-12-chatgpt-imagegen-world-cup-night-mother-a.png
```

Use lowercase ASCII words joined by hyphens for `<ascii-topic-slug>`. Keep it short but descriptive. If a Robin/default archive copy is also needed, copy the same generated file to the archive path; do **not** regenerate the same prompt just to create another copy.

## Result delivery / preview protocol

Do not return only a path after successful image generation unless Robin explicitly asks for path-only output. The image should be easy to view from the conversation thread on desktop and mobile, and the absolute path should still be included for file access.

Verified cross-device preview format for Codex desktop + mobile:

```markdown
![效果图](/Users/robin/AltmanCodex/_workspace/preview_tests/example-image.png)

文件：/Users/robin/AltmanCodex/_workspace/preview_tests/example-image.png
```

Rules:
- Use Markdown image syntax in the final reply: `![效果图](/absolute/ascii/workspace/path.png)`.
- The path used inside the Markdown image should be an absolute path under the active workspace, with ASCII-only directory and filename. Verified working example: `/Users/robin/AltmanCodex/_workspace/preview_tests/chatgpt-imagegen-preview-test.png`.
- Do not use `/tmp` for the preview path; mobile clients may not render it reliably.
- Do not rely on raw `<image name=[Image #1] path="...">` blocks. They are client-internal attachment rendering, not a reliable assistant-authored preview format.
- If the final asset must live in Robin's default phone-synced folder (`/Users/robin/Rubin-Studio/🧵X碎碎念`) or another project archive folder, also create or keep an ASCII-named preview copy under the active workspace for chat rendering, then list both paths.
- Put the Markdown image line in the actual final reply, not in a code block.
- Do not only say "saved to ..." or paste a plain path as the whole answer.

## Route B Workflow

1. **Compose a structured prompt** using `## Prompt Engineering — Modular Composer`. Load `references/prompt-parameters.md` when the task needs visual planning, identity/style continuity, project-specific modules, materials, lighting, or series logic. For simple 2-3 element icons, use a short prompt: style + subject + aspect ratio + constraints.
2. **Pick the aspect ratio, then size and format** based on the aspect-ratio policy above. The prompt says only the ratio; the CLI gets `--size`.
3. **Pick the output path** inside the workspace.
4. **Run the task image directly**: `chatgpt-imagegen "<prompt>" -o <path> --size <wxh> --quiet`. The first call in a production task must be the mother image / first reference image / first real deliverable, not an unrelated smoke test. For image-to-image work, add `--image <reference-path>` for each required reference image.
5. **Inspect the result** if you can (e.g. with a `view_image` tool or by reading the file). If clearly wrong, iterate with a single targeted prompt change — do not loop blindly (each call costs subscription quota).
6. **Report with the verified Markdown preview format first**, then the saved path(s) and final prompt used. Do not return only a path unless Robin explicitly asked for path-only output.

## Reference files

- `references/prompt-parameters.md` — primary modular prompt library: current prompting rules, aspect-ratio rules, portrait parameters, non-human material/scene parameters, series expansion logic, lock templates, and project-specific modules for EVE, ROV x EVE, Arthur Observatory, Paper Moon, Web6, and Rubin Motion Lab.
- `references/visual-parameters.md` — legacy compact visual reference kept for compatibility with older agent notes. Prefer `prompt-parameters.md` for new work unless a task explicitly points to this older file.

## Limits

- Reference-image / image-to-image generation is a supported mode through `--image` / `--reference`. Do not remove or downplay it because of a quota or 429 error; those errors are access-gate issues, not capability proof.
- **Image quality** is chosen by the backend; this skill has no `--quality` flag, and the subscription path does not honour explicit quality requests reliably. Don't promise a specific quality level to the user. If they need explicit `quality=high`, route them to the official `/v1/images/generations` API with their own `OPENAI_API_KEY`.
- `background: transparent` is **not supported** on the subscription path.
- A single image typically takes **15–60 s**, but large or detailed ones occasionally run **2–3 min**. The default `--timeout` is 300 s to cover this; a genuine hang is caught sooner by the `--stall-timeout` idle window (default 120 s).
- **Avoid parallel image generation for Robin's workflows.** Default to sequential execution: start one `chatgpt-imagegen` process, wait until it exits and the output file is verified, then start the next one. Only run in parallel when Robin explicitly asks or confirms it, and cap concurrency at **2 simultaneous images**. Do not launch larger batches; high concurrency may make quota use and rate limiting harder to reason about.
- Requests go through ChatGPT's internal `backend-api/codex/responses` path. The script uses the user's ChatGPT subscription image capability and does not consume the Codex text budget like a normal Codex conversation, but the backend still requires the Codex/GPT-5.5 quota gate to be open before the call can run. `HTTP 429 usage_limit_reached` means the Codex backend gate is closed until its reset time; it is **not** evidence that ChatGPT image generation, reference-image generation, or image-to-image is unsupported.

## Error handling

| Symptom | Cause | Fix |
| --- | --- | --- |
| `~/.codex/auth.json not found` | Codex CLI never signed in | Tell user to run `npm i -g @openai/codex && codex login` |
| `no ChatGPT OAuth access_token in ~/.codex/auth.json` | Only an API key is present, not a subscription OAuth token | Tell user to run `codex login`; an `OPENAI_API_KEY` value in that file is not a substitute |
| `HTTP 400 requires a newer version of Codex` | local codex CLI is outdated | Tell user to run `npm i -g @openai/codex@latest`; the script reads version from `~/.codex/version.json` which `codex` updates on launch |
| CLI prints `chatgpt-imagegen 0.2.2` while `SKILL.md` says another version | Normal version split: executable version vs Skill workflow version | Continue. Report both labels if needed; do not reinstall, stop, or treat this as an error. |
| `HTTP 401` / `HTTP 403` then refresh works | Token expired and refresh succeeded | No action needed — script auto-retried |
| `refresh_token is no longer valid — run codex login again` | Refresh token revoked or rotated | Tell user to run `codex login` again |
| `stalled: the image backend sent no data for ~Ns (last phase: …)` | No data for the whole `--stall-timeout` idle window — backend hung or overloaded | Retry; if it recurs, raise `--stall-timeout` (and `--timeout`). The message names the phase it stalled in. |
| `timed out: no image within the Ns total budget (last phase: …)` | The whole `--timeout` budget elapsed — usually a genuinely large image | Raise `--timeout` (e.g. `--timeout 420`) and retry |
| `no image returned. events seen: ...` | Model decided not to call the tool | Rephrase prompt to explicitly say "Use the image_generation tool to render…" |
| `HTTP 429 usage_limit_reached` | Codex/GPT-5.5 backend quota gate is closed; this blocks the call even though the script itself is using ChatGPT subscription image generation | Wait until the `resets_at` time; do not retry in a loop; do not mark image-to-image unsupported because of this error |
| `warning: --format=X but FILE.Y has .Y extension` | `-o` extension disagrees with `--format` | Fix the path or the format flag; the file IS written with the format you specified |

## Internals (for maintainers / debugging)

- Reads `~/.codex/auth.json` for `access_token`, `account_id`, `refresh_token`.
- Reads `~/.codex/version.json` for the `version` header.
- POSTs to `https://chatgpt.com/backend-api/codex/responses` with `tools: [{"type": "image_generation"}]`.
- Streams the SSE response, extracts the `image_generation_call` output item, base64-decodes `item.result`, writes it to disk.
- Auto-refreshes the OAuth token on 401/403 via `https://auth.openai.com/oauth/token` using `client_id=app_EMoamEEZ73f0CkXaXp7hrann`. The refreshed token is persisted back to `auth.json`.

## Related

- HTTP gateway sibling (for multi-app / SDK-compatible usage): https://github.com/leeguooooo/agent-cli-to-api
