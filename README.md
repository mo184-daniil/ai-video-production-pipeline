# AI Video Production Pipeline — Claude Code Skill

> **Версия:** 3.2 | **Тип:** [Claude Code](https://claude.ai/claude-code) skill | **Язык ответов:** русский + английские промпты

Превращает языковую модель из «генератора одного промпта» в **production supervisor**: режиссёр + prompt engineer + технический продюсер + контролёр качества.

---

## Зачем

Большинство моделей при запросе «сделай видео» сразу генерируют картинку или пишут один промпт. Этот скилл устанавливает другой режим:

- Первый ответ = только план. Никогда генерация.
- Борды → раскадровка → промпты по секундам → видео по сегментам → монтаж.
- Каждый этап с Model Block, Stage Gate и QC.

**Главный принцип:** 80% успеха в AI-видео — в качественной статике, бордах и раскадровке. Видео-модель должна получать уже *решённый* кадр, а не думать вместо нас.

---

## Канонический пайплайн

```
Идея / ТЗ
→ разбор задачи и рисков
→ варианты (treatment)
→ Reference Bible (борды: стиль / персонаж / локация / объект / движение)
→ Главная раскадровка (Master Storyboard)
→ Раскадровки сегментов (Segment Storyboards)
→ [опционально] дополнительные визуальные якоря (keyframes)
→ дешёвые motion-тесты (Level 2+ обязательны)
→ промпты по секундам (Timecoded Prompts)
→ финальные видео-генерации по сегментам
→ монтаж / звук / текст / графика (post-production)
→ QC → Delivery Evidence
```

---

## Структура скилла

```
ai-video-production-pipeline/
├── SKILL.md              # Полный операционный контракт — всегда загружается
└── reference/
    ├── lookup.md         # Таблицы: модели, гейты, QC, форматы платформ, глоссарий
    ├── templates.md      # Готовые промпт-шаблоны для копирования
    ├── lessons.md        # Карта «провал → правило» + эталонный первый ответ
    └── higgsfield.md     # Специфика платформы Higgsfield (опционально)
```

### Как это работает для токенов

`SKILL.md` загружается при каждом вызове скилла (~7 500 токенов).  
`reference/` — только по требованию, когда модели нужна конкретная таблица или шаблон.  
Это позволяет SKILL.md быть самодостаточным, а детали держать отдельно.

---

## Ключевые концепции

### First Response Contract
Первый ответ всегда план с 10 пунктами. Если модель сразу генерирует — она нарушила контракт.

### NANO Command (быстрая остановка)
```
СТОП. Не генерируй ничего. Сначала: 1) скажи что понял, 2) скажи какие борды нужны,
3) скажи первый шаг. Жди подтверждения. Нарушение = результат помечается PROTOTYPE.
```

### Model Block
Обязателен перед каждым промптом: этап, цель, модель, тип, формат, входные данные, критерии приёмки, следующий шаг. Промпт без Model Block — ошибка.

### Panels ≠ Beats
- **Panels** = стадии в Shot Board (≤ 6/15c, один panel ≈ 2.5c)
- **Beats** = события в timecoded prompt (≤ 4/5c)
Разные метрики, не путать.

### Три правила чистого кадра
1. Текст, кириллица, логотипы → **post-production**, не в генерацию.
2. Экраны устройств с composited контентом → **#00FF00** (Chroma Key Green).
3. Перед каждым этапом — Post-production Disclosure (что генерирует AI, что идёт в post).

### END FRAME
Для multi-segment проектов: описание последнего кадра (not mid-action, camera stable, no motion blur). Становится `start_image` следующего сегмента. Без END FRAME сшить сегменты невозможно.

### Stage Gates
12 контрольных точек от Brief до Delivery. Если gate не пройден — дальше не идём.  
Motion Test: Level 1 = опционально, Level 2+ = обязателен.

### Board Utilization Lock
Созданный борд **обязан** использоваться downstream через Reference Map. Board без downstream usage = waste.

---

## Поддерживаемые модели

| Класс | Модели |
|---|---|
| Видео финал (сложный) | Seedance 2.0, Sora 2*, Veo 3.1*, Runway* |
| Видео тест (быстрый) | Seedance 2.0 Fast, Kling 3.0, Hailuo, Krea |
| Видео reference motion | Kling Motion Control |
| Image (борды, keyframes) | GPT Image 2, Nano Banana Pro, FLUX.2*, Seedream |

\* Non-Higgsfield only (только external workflow)

Полная матрица, «7 вопросов» и decision tree → `reference/lookup.md`.

---

## Когда срабатывает

Скилл активируется автоматически при запросах:
- AI-видео, мультфильм, рекламный ролик, UGC, product demo, TV spot
- Экшен-сцена, персонажная анимация, продуктовая визуализация
- Промпты для Seedance, Sora, Veo, Kling, Runway, Higgsfield, Flux, Midjourney
- Раскадровка (storyboard), shot board, keyframe, визуальная библия, production-pack

---

## Установка

### Клонировать репозиторий

```bash
# Windows
git clone https://github.com/mo184-daniil/ai-video-production-pipeline.git "$env:USERPROFILE\.claude\skills\ai-video-production-pipeline"

# macOS / Linux
git clone https://github.com/mo184-daniil/ai-video-production-pipeline.git ~/.claude/skills/ai-video-production-pipeline
```

Перезапустить Claude Code — скилл появится автоматически.

### Обновить существующий скилл

```bash
# Windows
cd "$env:USERPROFILE\.claude\skills\ai-video-production-pipeline"; git pull

# macOS / Linux
cd ~/.claude/skills/ai-video-production-pipeline && git pull
```

---

## Использование

```
Сделай рекламный ролик 15 секунд для кроссовок. Молодёжная аудитория, городской стиль.
```

```
Нужен промпт для Seedance: персонаж прыгает с крыши на крышу, ночной город, неон.
```

```
Помоги сделать storyboard для мультфильма: кот учится летать.
```

Модель выдаст план с вариантами (A/B/C), списком бордов, stage gates и первым шагом.  
Генерация — только после подтверждения.

---

## Что нового в V3.2

- **Panels ≠ Beats** — явное разграничение двух метрик
- **NANO Command** — 3-строчная команда для упрямых быстрых моделей
- **Pre-flight чеклист** — 5 вопросов перед отправкой video generation промпта
- **END FRAME quality criteria** — 4 критерия пригодности кадра как start_image
- **Segment Cut Guide** — где резать между сегментами, где нельзя
- **Skip-plan provision** — протокол когда пользователь просит пропустить план
- **Pre-made references path** — когда у пользователя уже есть реальные фото
- **Motion Test condition** — Level 1 = опционально, Level 2+ = обязателен
- **§9 объединён** — Три правила чистого кадра (текст, экраны, disclosure)
- **Platform formats table** — 9 платформ с resolution и FPS
- **Higgsfield equivalents** — маппинг этапов пайплайна на HF инструменты
- **Архитектура**: `manual.md` (57KB, V3.0) заменён на `lookup.md` (~12KB, V3.2)

---

## Лицензия

MIT — см. [LICENSE](LICENSE)
