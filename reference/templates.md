# Промпт-шаблоны — AI Media Production Pipeline V3.1

Готовые шаблоны для копирования. Промпты для image/video-моделей — на английском (модели
слушаются точнее). Перед каждым промптом ставить Model Block (см. `SKILL.md` §2).

## Оглавление

1. Борды (Style / Object / Character / Location / Prop / Pose / Shot / Shot Board Production / Object VFX Chroma Key)
2. Keyframe Prompt
3. Universal Video Prompt
4. Cheap Motion Test
5. Premium Video
6. Timecoded Prompt — Seedance 2.0
7. Timecoded Prompt — 10-секундная реклама
8. Timecoded Prompt — multi-clip монтаж
9. JSON Keyframe Pack
10. Audio Map
11. Asset Manifest
12. Production Log
13. Post-mortem
14. Control prompts (Safe Mode / Two-Step / One-Screen)
15. Multi-Character Reference Map Convention

---

## 1. Борды

### Style Board
```text
Create a high-resolution STYLE BOARD for [PROJECT].
Visual language: [описать стиль].
Include: lighting references; color grade; lens language; texture; mood; do/don't examples; production notes.
Critical: No readable text. No random logos. All examples must feel like the same project.
```

### Object Board
```text
Create a high-resolution OBJECT BOARD for [OBJECT].
Use the uploaded references as the single source of truth. The object must stay identical across every panel.
Include: hero shot; 6 angles (front, 3/4 left, side, back, 3/4 right, top); 4 macro details; 4 lighting variants; material notes; color palette; production notes.
Style: photorealistic, commercial, realistic materials, no invented details.
Critical: Do not change shape, color, proportions, logo, surface, material, geometry.
```

### Character Board
```text
Create a high-resolution CHARACTER BOARD for [CHARACTER].
The same person must appear in every panel. Use uploaded references as the source of truth.
Include: 5 full-body views; 5 expressions; 2 face/skin macro shots; outfit details; lighting variants; color palette; production notes.
Character: age, build, hair, eyes, skin texture, facial features, outfit, mood.
Style: photorealistic, natural skin pores, slight imperfections, no plastic skin, no AI smoothing.
Critical: Same identity in every panel. No face drift. No outfit drift.
```

### Location Board
```text
Create a high-resolution LOCATION BOARD for [LOCATION]. Keep the same atmosphere across every panel.
Include: wide shot; medium shot; close details; alternate angle; time-of-day variants; weather variants; props; texture details; color palette.
Style: photorealistic, cinematic, real camera, natural light.
Critical: No random text, no messy signs, no distorted architecture.
```

### Prop Board
```text
Create a PROP BOARD for [PROP]. Show the prop as a real physical object.
Include: front/side/back/top views; hand scale; material details; shadows; reflections; use cases; motion notes.
Critical: The prop must keep the same size, shape and material in all future shots.
```

### Pose Board
```text
Create a POSE BOARD for [CHARACTER]. Same person, same outfit, same identity.
Include: neutral standing; walking; turning; looking left/right; reaching hand; emotional poses; start and end pose for animation.
Critical: Same body proportions. Same face. Same outfit. No identity drift.
```

### Shot Board (базовый)
```text
Create a production SHOT BOARD for [SHOT NAME].
Use: Image 1 = Character Board; Image 2 = Object Board; Image 3 = Location Board; Image 4 = Prop Board; Image 5 = Style Board.
Generate [N] panels. Each panel must show: timestamp; action; camera angle; subject motion; object motion; what stays locked; what appears/disappears.
Add: motion arrows; camera path notes; locked elements notes; post-production notes; negative rules.
Style: photorealistic production storyboard, clear, readable, technical.
```

### Shot Board (production — Seedance 2.0, рекомендуемый)
```text
Create a production SHOT BOARD for [SHOT NAME / SEGMENT NAME].
Use references: Image 1 = Character Board; Image 2 = Object/Prop Board (incl. Chroma Key variant if applicable);
Image 3 = Location Board; Image 4 = Style Board.

Generate exactly 6 panels for 15 seconds (2.5s each). Optimal density — do not exceed.

Panel metadata block per panel:
PANEL [N] | [TIMESTAMP RANGE] | [SHOT TYPE: Wide/Medium/CU/Insert/OTS]
Action: [exact physical action]
Camera: [type, movement, speed]
Locked: [what must not change — identity, outfit, product, background]
VFX NOTE: [ELEMENT — TO BE COMPOSITED] (if any screen or overlay)
Motion arrows: [describe direction]

Add final Coverage Types panel showing icons: Wide / Medium / Close-Up / Insert / OTS — which are used in this segment.

Style: clean black-and-white technical production storyboard, readable annotations, no colour fill.
```

### Object Board (VFX Placeholder / Chroma Key Screen)
```text
Create a high-resolution OBJECT BOARD for [OBJECT WITH SCREEN].
Use uploaded references. The object shape, material, and dimensions must stay identical across every panel.

Include:
— 6 standard angle views (front, 3/4 left, side, back, 3/4 right, top)
— 4 material/detail close-ups
— 2 lighting variants
— CHROMA KEY VARIANT: 2–3 panels where the screen/display surface is replaced with solid #00FF00.
   Label these panels: "VFX PLACEHOLDER — SCREEN = #00FF00 — TO BE COMPOSITED"
— Material notes, color palette, production notes.

Style: photorealistic commercial product shot.
Critical: Do not put any app UI, video, or content on the screen. Screen = #00FF00 only in Chroma Key panels.
```

---

## 2. Keyframe Prompt

```text
Create a photorealistic keyframe for [SHOT NAME].
References: Image 1 = character; Image 2 = object/product; Image 3 = location; Image 4 = style.
Frame state: [Start / Middle / Peak / End]
Composition: [wide / medium / close-up / OTS / top shot]
Action: [exact physical state]
Camera: [lens, angle, height, distance]
Lighting: [time of day, softness, contrast]
Critical locked elements: same identity; same outfit; same object; same location; same scale; no text; no unwanted logos; no distorted hands/faces.
Output: single clean production still, suitable as image-to-video input.
```

---

## 3. Universal Video Prompt

```text
Reference map:
Image 1 = approved shot board; Image 2 = character reference; Image 3 = object reference; Image 4 = location reference; Image 5 = style reference.

Create cinematic photorealistic live-action footage.
Goal: [what the shot must communicate]
Scene: [who/what/where]
Action: [physical action in the frame]
Timing:
0.0–1.0s: ...
1.0–2.5s: ...
2.5–4.0s: ...
4.0–end: ...
Camera: [movement, lens feel, speed, path]. The camera must [move / stay locked] exactly as described.
Subject motion: [actor/product/prop motion]
Locked elements: identity consistent; product geometry consistent; background consistent; scale consistent; no unexpected edits.
Style: real camera, natural film grain, realistic light, realistic skin/materials, no CGI look.
Post-production: Final text, typography, logos, UI and Cyrillic labels added later if needed. Leave clean space / blank cards / graphic placeholders.
Negative rules: No unreadable text. No random logos. No subtitles. No duplicate subject. No morphing product. No broken hands. No frame scaling unless specified. No sudden cuts unless specified. No reverse motion unless specified. No plastic skin.
```

---

## 4. Cheap Motion Test

```text
Этап: Cheap Motion Test
Цель: проверить движение до дорогой финальной генерации.
Модель: Kling 3.0 / Minimax Hailuo / Krea Video / HappyHorse
Тип: image-to-video    Режим: draft motion    Длительность: 3–5 c    Качество: draft / 720p / 1080p
Зачем: проверить, понимает ли модель движение камеры и объекта.
Почему эта модель: дешевле и быстрее, чем Seedance / Sora / Veo.
Почему не Seedance сейчас: ещё не доказана механика движения.
Что подаём: утверждённый keyframe или shot board.
Критерии приёмки: движение правильно читается, нет реверса, нет морфинга.
Если плохо: уточнить motion prompt, сделать доп. keyframes.
Следующий шаг: Seedance / Veo / Sora для финала.
Промпт: [video prompt]
```

---

## 5. Premium Video

```text
Этап: Final Video Generation
Цель: финальный клип после утверждения статики и движения.
Модель: Seedance 2.0 / OpenAI Sora 2 / Google Veo / Kling 3.0 Motion Control
Тип: image-to-video / multi-reference    Режим: final    Формат: 16:9 или 9:16    Качество: максимальное
Зачем: финальная сложная анимация с камерой, персонажем, объектом.
Почему не дешевле: черновые тесты пройдены, нужен финальный уровень.
Что подаём: approved keyframes, shot board, references.
Критерии приёмки: соответствует ТЗ, без морфинга, движение правильное.
Если плохо: менять не только промпт, но и стратегию (разбить клип, сменить модель, вынести элемент в post).
Следующий шаг: финальная графика / звук / монтаж.
Промпт: [final video prompt]
```

---

## 6. Timecoded Prompt — Seedance 2.0

### Одиночный персонаж
```text
Media roles: start_image = approved keyframe/first frame; image = Style/Character/Location/Object/Shot/Action Boards; video = motion reference (если есть); audio = rhythm/ambience/voice (если есть).

Reference map:
Image 1 = approved Style Board.
Image 2 = approved Individual Character Board for [CHARACTER].
Image 3 = approved Location Board for [LOCATION].
Image 4 = approved Object/Prop Board for [OBJECT]. (Chroma Key variant if screen compositing needed)
Image 5 = approved Start Keyframe.
Image 6 = approved Shot Board.
Image 7 = approved Action Board, if available.

Create a [duration]-second [aspect ratio] video.
Main instruction: Follow Image 6 (Shot Board) as the main shot logic. Use Image 7 (Action Board) for motion phases if available. Follow the planned panel order and timing. Do not invent new actions. Do not skip beats. Do not jump to the ending early.

Timeline:
0.0–2.5s: Start from Image 5 and Shot Board panel 1. [действие]  Camera: [...]  Locked: [...]
2.5–5.0s: Transition toward Shot Board panel 2. [действие]  Camera: [...]  Locked: [...]
5.0–7.5s: Follow Shot Board panel 3 / Action Board panels [x-y]. [действие]  Camera: [...]  Locked: [...]
7.5–10.0s: Follow Shot Board panel 4. [действие]
10.0–12.5s: Follow Shot Board panel 5. [действие]
12.5–15.0s: Reach final planned state from panel 6. Hold ending.

END FRAME: [точное визуальное состояние: поза, выражение лица, положение камеры, расположение объектов, свет]. Используется как start_image следующего сегмента.

Style: [из Style Board]    Environment: [из Location Board]
Post-production: Final text, Cyrillic, logos, UI, legal, CTA, typography added after generation. Leave clean blank areas or placeholders. Chroma key screens composited in post.
Negative rules: No random text. No generated logos. No character mixing. No identity drift. No object morphing. No reverse motion. No skipped beats. No sudden camera jump. No broken hands. No extra limbs. No face melting. No unreadable letters.
```

### Два персонажа (Multi-Character)
```text
Media roles: start_image = approved keyframe/first frame; image = boards; video = motion reference (если есть).

Reference map — FIXED NUMBERING (не менять между сегментами):
@image 1 = Character 1 Board — [ИМЯ/ОПИСАНИЕ]
@image 2 = Character 2 Board — [ИМЯ/ОПИСАНИЕ]
@image 3 = Location Board for [LOCATION].
@image 4 = Object/Prop Board for [OBJECT]. (Chroma Key variant if needed)
@image 5 = Shot Board for this segment.
@image 6 = Style Board. (если нужен)

Create a [duration]-second [aspect ratio] video.
Main instruction: Follow @image 5 (Shot Board). Character 1 = always @image 1. Character 2 = always @image 2. Do not mix identities.

Timeline:
0.0–2.5s: Shot Board panel 1. [действие]  Camera: [...]
2.5–5.0s: Shot Board panel 2. [действие]  Camera: [...]
5.0–7.5s: Shot Board panel 3. [действие]
7.5–10.0s: Shot Board panel 4. [действие]
10.0–12.5s: Shot Board panel 5. [действие]
12.5–15.0s: Shot Board panel 6. Hold ending.

END FRAME: [точное состояние обоих персонажей, камеры и фона].

Negative rules: No identity swap between characters. No character mixing. [standard rules]
```

---

## 7. Timecoded Prompt — 10-секундная реклама

```text
Reference map: Image 1 = Style Board; Image 2 = Character Board; Image 3 = Location Board A; Image 4 = Location Board B; Image 5 = Object/Prop Board; Image 6 = Shot Board; Image 7 = Start Keyframe; Image 8 = End Keyframe (если есть).

Create a 10-second 16:9 commercial video.
Main instruction: Follow the approved Shot Board (Image 6) as the main logic reference. Build from planned beats, not improvised.

0.0–1.0s — Beat 1 / panel 1: [действие]  Camera: [...]  Locked: [identity, outfit, product, location]
1.0–2.0s — Beat 2 / panel 2: [действие]  Camera: [...]
2.0–3.0s — Beat 3 / panel 3: [действие]  Camera: [...]
3.0–4.0s — Beat 4 / panel 4: [действие]
4.0–5.0s — Beat 5: [действие]
5.0–6.0s — Beat 6: [действие]
6.0–7.0s — Beat 7: [действие]
7.0–8.0s — Beat 8: [действие]
8.0–9.0s — Beat 9: [действие]
9.0–10.0s — Final hold: [финальное состояние, usable for edit]

Camera: [общие правила]    Style: [общие правила]    Sound/Audio reference: [ритм/атмосфера/голос, если есть]
Post-production: Readable text, Cyrillic, logo, legal, CTA added after generation.
Negative rules: [список запретов]
```

---

## 8. Timecoded Prompt — multi-clip монтаж

Каждый клип — свой промпт по секундам. Генерировать source длиннее нужного, в монтаже брать
только нужный отрезок.

```text
Clip 1 — 5s source, final edit uses 0.0–1.2s
0.0–1.0s: The cashier is already in position from Keyframe 1. She holds the receipt, tired but polite. Camera locked medium shot.
1.0–2.5s: Her eyes briefly drop toward the counter. Small hand movement only. No change in store layout.
2.5–5.0s: She returns to neutral position. Hold usable frame for edit. No new action.
```

Плохо: `Clip 1: cashier routine. Clip 2: phone lights up.` (без таймингов и locked-правил).

---

## 9. JSON Keyframe Pack

Когда использовать: keyframes, scene-correct stills, сложная identity, brand/IP placeholders,
много negative constraints, нужно отделить identity от scene, нужен QC-mapping.

```json
{
  "project": "[project name]",
  "asset_type": "keyframe",
  "keyframe_id": "KF01",
  "usage_context": "image-to-video start frame",
  "reference_map": {
    "image_1": "approved Style Board — color, lighting, mood",
    "image_2": "approved Character Board — identity/proportions/outfit",
    "image_3": "approved Location Board — layout/atmosphere",
    "image_4": "approved Prop Board — object shape/material",
    "image_5": "approved Shot Board panel — composition/beat"
  },
  "identity_direction": {
    "strategy": "approved likeness / fictional lookalike / generic heroine",
    "face": "[facial anchors]",
    "wardrobe": "[exact wardrobe lock]"
  },
  "scene": {
    "location": "[location]",
    "composition": "[shot size / camera angle]",
    "action_beat": "[what happens in this exact frame]",
    "emotion": "[emotion]"
  },
  "visual_style": { "lighting": "[lighting]", "color_grade": "[grade]", "texture": "[texture]" },
  "post_production_fields": { "text": "blank placeholder only", "logos": "placeholder only", "cta": "added in post" },
  "negative_constraints": ["no random text", "no generated logo", "no character mixing", "no style drift", "no distorted hands", "no extra limbs"],
  "output_requirements": { "aspect_ratio": "16:9", "resolution": "2K or higher", "result": "single clean production still" }
}
```

---

## 10. Audio Map

```text
0.0–1.2s: small store ambience, low mood, no music or very soft bed.
1.2–2.0s: SMS notification ping.
2.0–3.0s: short silence / decision beat.
3.0–5.0s: door whoosh, music starts.
5.0–8.0s: bright store ambience, scanner beep.
8.0–10.0s: music resolves, CTA hold.
```

Даже если звук генерируется ненадёжно, audio map нужен монтажёру и post-production.

---

## 11. Asset Manifest

```text
# Asset Manifest

## Boards
- style_board.png — status PASS — source model — notes
- character_board.png — status PASS — source model — notes

## Keyframes
- KF01.png — status PASS — scene — model — refs
- KF02.png — status PASS — scene — model — refs

## Videos
- V01.mp4 — status PASS/PARTIAL/FAILED — duration — resolution — model — source keyframe

## Post
- logo.svg / sms_overlay.psd / music.wav / voiceover.wav

## Missing
- lipsync_final.mp4 / final_master.mp4
```

---

## 12. Production Log

```text
# Журнал продакшена — [название проекта]

## Запись 001 — старт
Статус: [только план / креативная разработка / производственный план]

## Проект
Название:    Жанр:    Формат:    Длительность:    Платформа:

## Суть идеи
[...]

## Текущее решение
Выбранный вариант:    Workflow:    Если Higgsfield:    Если не Higgsfield:

## Текущий этап
Контрольная точка:    Статус контрольной точки:

## Готовые материалы
- пока нет / список

## Открытые риски
- ...

## Следующее действие
[...]
## Следующий результат
[...]
## Когда остановиться
[условие]
```

Расширенный вариант для отчёта может дополнительно включать: Models used (по этапам),
Iterations (images/storyboards/videos/edits), Problems (problem/cause/solution/model/next model),
Final result (что получилось / что не идеально / что вынесено в post / что сработало / что улучшить).

---

## 13. Post-mortem

```text
# Post-Mortem — [Project]
Что было сделано:
Что не соответствует пайплайну:
Какие gates провалены:
Что можно сохранить:
Что нужно переделать:
Корневая причина:
План исправления (Repair Loop):
```

---

## 14. Control prompts

### Safe Mode — только план
```text
SAFE MODE — ТОЛЬКО ПЛАН.
Не генерируй изображения. Не генерируй видео. Не вызывай инструменты. Не создавай media.
Сначала прочитай production pipeline и преврати мою идею в:
1) понимание проекта; 2) workflow; 3) варианты treatment; 4) список нужных boards;
5) stage gates; 6) upload map; 7) только первый prompt.
Остановись после плана. Жди подтверждения перед генерацией.
```

### Two-Step Prompting
```text
Step 1 — PLAN ONLY: что понял; сложность; риски; treatment options; workflow (Higgsfield / не Higgsfield);
required boards; Stage Gates; Upload Map; первый deliverable. Остановись.
Step 2 — First Board Only: сделай только Stage 1. Перед prompt укажи workflow, model, input assets, gate, QC. Не переходи дальше.
Step 3 — Continue by Gate: gate passed → только следующий этап. Не перескакивай.
```

### One-Screen Control Prompt (вставлять перед идеей)
```text
ВАЖНЫЕ ПРАВИЛА ИСПОЛНЕНИЯ:
1. Не генерируй media сразу.  2. Сначала production plan.
3. Для видео/сюжета главным является Shot Board / Storyboard.
4. Keyframes — опциональные anchors, не обязательны после storyboard.
5. Для сложных видео — Segment Storyboards + несколько генераций.
6. Каждый этап генерации: Workflow → Stage Gate → Upload Map → Reference Map → Prompt → QC.
7. Созданный board обязан использоваться дальше.
8. Если результат только concept image — маркируй STYLE EXPLORATION, не DONE.
9. Остановись после первого плана и жди подтверждения.
```

---

## 15. Multi-Character Reference Map Convention

Когда в проекте 2+ персонажа, нумерация @image фиксируется **один раз** и не меняется
на протяжении всего проекта (все сегменты, все промпты).

```text
PROJECT REFERENCE MAP — FIXED

@image 1 = Character 1 Board — [ИМЯ]   (главный персонаж)
@image 2 = Character 2 Board — [ИМЯ]   (второй персонаж)
@image 3 = Location Board     — [ЛОКАЦИЯ]
@image 4 = Object / Prop Board — [ОБЪЕКТ] (Chroma Key variant если нужен экран)
@image 5 = Shot Board          — (текущий сегмент — меняется каждый сегмент)
@image 6 = Style Board         — (добавлять если нужен явный control)
```

**Правила:**
- Номера 1 и 2 всегда Character Boards — нельзя вставить между ними Location или Style.
- Shot Board (image 5) — единственный, который меняется от сегмента к сегменту.
- Перед каждым промптом в multi-character проекте всегда писать FULL Reference Map block.
- Если персонажей 3+, продолжать нумерацию: @image 3 = Character 3 Board, остальные сдвигаются.

**Пример для 2 персонажей:**
```text
Reference map:
@image 1 = Protagonist Character Board (Lisa, 28y, red jacket, dark bob)
@image 2 = Antagonist Character Board (Marcus, 35y, grey suit, short hair)
@image 3 = Office Location Board
@image 4 = Laptop Object Board (Chroma Key variant — screen = #00FF00)
@image 5 = Segment 3 Shot Board
```
