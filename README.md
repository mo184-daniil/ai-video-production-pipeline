# AI Video Production Pipeline — Claude Code Skill

> **Версия:** 3.0  
> **Тип:** [Claude Code](https://claude.ai/claude-code) skill (`.skill`)  
> **Язык ответов:** русский + английские промпты для моделей

Универсальный production-пайплайн для AI-видео, AI-анимации, рекламных роликов и мультимедийных проектов. Превращает идею или ТЗ в **управляемый процесс** вместо одной случайной генерации.

---

## Зачем этот скилл

Большинство AI-моделей при запросе "сделай видео" сразу генерируют картинку или пишут один промпт. Этот скилл заставляет модель вести себя как **production supervisor**: режиссёр + prompt engineer + технический продюсер + контролёр качества.

**Главный принцип:** 80% успеха в AI-видео — в качественной статике, бордах и раскадровке. Видео-модель должна получать уже *решённый* кадр.

---

## Когда срабатывает

Скилл активируется **автоматически**, когда ты просишь:

- Сделать AI-видео, мультфильм, рекламный ролик, UGC, product demo, TV spot
- Экшен-сцену, персонажную анимацию, продуктовую визуализацию
- Написать промпты для **Seedance, Sora, Veo, Kling, Runway, Higgsfield, Flux, Midjourney** и других генеративных моделей
- Раскадровку (storyboard), keyframe, визуальную библию, production-pack по ТЗ

Ключевые триггеры: `AI video`, `AI ad`, `storyboard`, `shot board`, `keyframe`, `video prompt`, `image-to-video`, `реклама`, `ролик`, `мультфильм`, `раскадровка`, `генерация видео`.

---

## Канонический пайплайн

```
Идея / ТЗ
→ Разбор задачи и рисков
→ Три креативных варианта (A — безопасный / B — сбалансированный / C — амбициозный)
→ Reference Bible (борды: стиль / персонаж / локация / объект / движение)
→ Главная раскадровка (Master Storyboard)
→ Раскадровки сегментов (Segment Storyboards)
→ [опционально] Дополнительные визуальные якоря (keyframes)
→ Дешёвые motion-тесты
→ Промпты по секундам (Timecoded Prompts)
→ Финальные видео-генерации по сегментам
→ Монтаж / звук / текст / графика (post-production)
→ Проверка качества (QC)
→ Финальная сдача (Delivery Evidence)
```

**Первый ответ всегда — план**, никогда не генерация. Модель останавливается после плана и ждёт подтверждения.

---

## Структура файлов

```
ai-video-production-pipeline/
├── SKILL.md                        # Главный операционный контракт (читает модель)
└── reference/
    ├── manual.md                   # Полный мануал: каталог моделей, stage gates, QC-чек-листы
    ├── templates.md                # Готовые промпт-шаблоны для копирования
    ├── lessons-and-example.md      # Карта «реальный провал → правило», эталонный пример
    └── higgsfield.md               # Специфика платформы Higgsfield (опционально)
```

### `SKILL.md` — операционный контракт
Содержит 12 разделов: контракт первого ответа, пайплайн, выбор моделей (Model Block), Reference Bible, раскадровки vs keyframes, промпты по секундам, Stage Gates, Density Control, Honesty Gates, правила текста/брендов, концепция ролей моделей.

### `reference/manual.md` — полный мануал
Философия, классификация сложности кадров (4 уровня), полный каталог моделей + матрица выбора + «7 вопросов», полный список Stage Gates, Upload Map, Complex Action Rule, Modular Generation, QC-чек-листы, план итераций, Production Log, глоссарий.

### `reference/templates.md` — шаблоны промптов
Style Board, Object Board, Character Board, Location Board, Keyframe, Universal Video, Cheap Motion Test, Premium Video, Timecoded (Seedance / 10-сек реклама / multi-clip), JSON Keyframe Pack, Audio Map, Asset Manifest, Production Log, control prompts.

### `reference/lessons-and-example.md` — уроки разработки
Карта «что реально случилось на тесте → правило-противоядие» и один эталонный первый ответ. Полезен чтобы понять *цену* нарушения каждого правила.

### `reference/higgsfield.md` — слой Higgsfield
Router по workflow, default-модели, Marketing Studio Video, Brand Kits, DTC Ads Engine, Product Photoshoot, Marketplace Cards, Soul Character / Identity, Virality Predictor. Нужен **только при работе внутри Higgsfield**.

---

## Поддерживаемые модели

| Модель | Лучше всего для |
|--------|----------------|
| **Seedance 2.0** | Сложная сцена: timecoded beats, камера, физика, multi-reference, 7–15 с |
| **Sora 2 / Veo 3.1** | Сторителлинг, character refs, sound, first/last frame, cinematic realism |
| **Kling / Hailuo / Krea** | Быстро/дёшево оживить один утверждённый кадр, простое движение |
| **Runway** | Controlled image-to-video / VFX при сильном input image |
| **Kling Motion Control** | Перенести конкретное движение из видео-референса |
| **GPT Image 2 / Flux / Seedream** | Борды и keyframes (статика) |

---

## Ключевые концепции

### Model Block
Перед **каждым** промптом обязательный блок с указанием этапа, цели, модели, формата, входных данных, критериев приёмки и следующего шага. Без него промпт считается ошибкой.

### Reference Bible (борды)
Визуальные референсные листы, фиксирующие один аспект проекта. Созданный борд **обязан** использоваться дальше через Reference Map.

- **Style Board** — визуальный язык, палитра, свет, mood
- **Character Board** — внешность, силуэт, лицо (каждый персонаж — отдельный борд)
- **Location Board** — пространство, архитектура, атмосфера
- **Object/Prop Board** — продукт/предмет как single source of truth
- **Action Board** — конкретное движение: трюк, бой, танец, траектория

### Stage Gates
Контрольные точки: если gate не пройден — дальше не идём. Не делать видео без утверждённой статики.

### Honesty Gates
Генерация файла ≠ выполнение этапа. Статус «готово» только при наличии **Delivery Evidence**: файл, путь, duration, fps, resolution, audio, QC-статус.

### Density Control
Главная причина провалов — слишком плотная сцена. Разбивать на клипы если beats > 4 на 5 с, или локаций > 1 в одном непрерывном видео.

---

## Установка

### Способ 1 — через архив `.skill.zip`

Если у тебя есть файл `ai-video-production-pipeline.skill.zip`:

```bash
# Распаковать в папку skills Claude Code
# Windows
Expand-Archive ai-video-production-pipeline.skill.zip -DestinationPath "$env:USERPROFILE\.claude\skills\"

# macOS / Linux
unzip ai-video-production-pipeline.skill.zip -d ~/.claude/skills/
```

### Способ 2 — клонировать репозиторий

```bash
# Windows
git clone https://github.com/mo184-daniil/ai-video-production-pipeline.git "$env:USERPROFILE\.claude\skills\ai-video-production-pipeline"

# macOS / Linux
git clone https://github.com/mo184-daniil/ai-video-production-pipeline.git ~/.claude/skills/ai-video-production-pipeline
```

После установки перезапусти Claude Code — скилл появится автоматически.

---

## Использование

После установки просто описывай задачу естественным языком:

```
Сделай рекламный ролик 15 секунд для кроссовок. Молодёжная аудитория, городской стиль.
```

```
Нужен промпт для Seedance: персонаж прыгает с крыши на крышу, ночной город, неон.
```

```
Помоги сделать storyboard для мультфильма: кот учится летать.
```

Модель автоматически применит пайплайн: план → борды → раскадровка → промпты по секундам → видео.

---

## Лицензия

MIT
