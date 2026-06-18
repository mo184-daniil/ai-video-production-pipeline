# Lookup Tables — AI Media Production Pipeline V3.2

> Только таблицы. Правила — в `SKILL.md`. Шаблоны — в `templates.md`.

---

## L1. Каталог видео-моделей

| Модель | Когда | Не когда | HF |
|---|---|---|---|
| Seedance 2.0 | Финал: timecoded, камера, физика, multi-ref, 7–15c | Не для черновиков | ✓ |
| Seedance 2.0 Fast | Черновой прогон Seedance-логики, тест движения Level 2–3 | Не ждать макс. стабильности | ✓ |
| Kling 3.0 | i2v, движение персонажа, быстрые коммерч. клипы | Сложная логика нескольких объектов | ✓ |
| Kling Motion Control | Есть video reference, нужно перенести motion | Без хорошего motion ref | ✓ |
| Minimax Hailuo | Быстрые affordable clips, черновики | Не для строгой product-консистентности | ✓ |
| Krea Video / HappyHorse | Быстрые creative тесты, дешёвые проверки | Не финал сложной рекламы | ✓ |
| Wan 2.7 | 1080p, cinematic tests | Проверять на типе сцены | ✓ |
| Google Veo 3.1 | Кинематографичность, first/last frame, audio | Не тратить на черновики | ✗ |
| Google Veo | Киношный реализм, image-to-video | Без keyframes не начинать | ✗ |
| OpenAI Sora 2 | Сложные сцены, 3D, аудио, сторителлинг | Дорого для поиска стиля | ✗ |
| Runway Gen-4 | 5–10c из image + prompt, VFX/live-action | Нужен сильный input image | ✗ |

✗ = Non-Higgsfield only (только в external workflow)

## L2. Каталог image-моделей

| Модель | Когда | HF |
|---|---|---|
| GPT Image 2 | Главная: keyframes, boards, multi-reference, сложная композиция | ✓ |
| GPT Image | Универсальные изображения, быстрые варианты | ✓ |
| Nano Banana Pro / 2 | Быстрый качественный static, итерации | ✓ |
| Seedream 5.0 Lite / 4.5 | Визуальное рассуждение / 4K детали | ✓ |
| FLUX.2 Pro / Max | Фотореализм / макс. точность для финальных static | ✗ |
| Higgsfield Soul Cinema / 2.0 | Cinema-grade stills, ультрареалистичная fashion | ✓ |

---

## L3. Матрица выбора видео-модели

| Задача | Модель |
|---|---|
| Один keyframe + простое движение (Level 1) | Kling 3.0 / Hailuo / Krea |
| Сложное действие по секундам (Level 2) | Seedance 2.0 Fast → 2.0 |
| 5–15c сцена с развитием | Seedance 2.0 |
| Несколько объектов взаимодействуют | Seedance 2.0 / Sora 2 / Veo |
| Сложная камера (orbit/fly-through) | Seedance 2.0 / Veo / Sora 2 |
| Есть reference video с движением | Kling Motion Control |
| Строго first frame → last frame | Veo 3.1 / Runway / Sora |
| Киношная сцена с аудио | Seedance 2.0 / Sora 2 / Veo 3.1 |
| Тест движения (дёшево) | Kling / Hailuo / Krea / Seedance Fast |
| Финал сложной сцены | Seedance 2.0 / Veo / Sora |

### «7 вопросов» перед выбором

```text
1. Простое оживление или сцена?         оживление → Kling/Hailuo; сцена → Seedance/Sora/Veo
2. Есть действия по секундам?           нет → Kling/Hailuo; да → Seedance/Sora/Veo
3. Есть reference video движения?       да → Kling Motion Control; нет → по сложности
4. Есть first frame + last frame?       да → Veo/Runway/Sora/Seedance; нет → обычный i2v
5. Нужен звук?                          нет → Kling/Seedance/Runway; да → Seedance/Sora/Veo 3.1
6. Сколько объектов взаимодействуют?    1 → Kling/Hailuo; 2+ → Seedance/Sora/Veo
7. Тест или финал?                      тест → дешёвая/Fast; финал → Seedance 2.0/Sora/Veo
```

### Decision tree

```text
Нет Style/Character/Keyframe        → не делать видео, вернуться к boards.
Один keyframe + простое движение    → Kling / Hailuo / Krea.
Есть video reference движения       → Kling Motion Control.
Сложная логика / Action / Shot Board → Seedance 2.0 Fast.
Seedance Fast прошёл                 → Seedance 2.0 final.
Seedance не справился               → разбить / больше keyframes / Sora/Veo/Runway.
Нужен текст/логотип                 → не генерировать в видео, добавить в post.
```

---

## L4. Классификация сложности кадра

| Уровень | Тип | Motion Test | Статика | Видео |
|---|---|---|---|---|
| 1 — простой | held shot, лёгкий push-in, parallax | **опционально** | GPT Image / Nano Banana 2 | Kling 3.0 / Hailuo / Krea |
| 2 — средний | герой идёт, поворот, объект берут рукой, лёгкая камера | **обязателен** | GPT Image 2 / Nano Banana Pro / FLUX.2 Pro | Kling 3.0 / Runway / Seedance Fast |
| 3 — сложный | орбитальная камера, несколько синхронных движений, удержание лица | **обязателен** | GPT Image 2 Multi Ref / FLUX.2 Max | Seedance 2.0 / Kling Motion / Veo / Sora |
| 4 — очень сложный | lip-sync, известное лицо, сложные руки, несколько локаций, переход через экран | **обязателен** | GPT Image 2 / Face Swap / FLUX.2 Max | Sora 2 / Veo / Seedance 2.0 / Runway |

---

## L5. Stage Gates (полный список)

| Gate | Что | Условие перехода |
|---|---|---|
| 1 — Brief | Задача, формат, стиль, ограничения | Есть план + выбраны модели |
| 2 — Style | Визуальный стиль | Единый стиль, нет мусора, нет случайного текста |
| 3 — Character | Individual character boards | Персонажи стабильны, не смешиваются |
| 4 — Object/Prop | Продукты, предметы, реквизит | Стабильны; Chroma Key variant создан если нужен |
| 5 — Location | Локации | Узнаваема, стабильна |
| 6 — Cast/Scale | Герои вместе | Масштаб и стиль совместимы |
| 7 — Keyframe | Ключевые кадры (если нужны) | Композиция, эмоция, объект, стиль совпадают |
| 8 — Shot/Action Board | Раскадровка (≤6 panels/15c) | Понятны beats, камера, тайминг; VFX NOTE проставлены |
| 9 — Motion Test | Тест движения (Level 2+ обязателен) | Нет морфинга, реверса, сломанной камеры |
| 10 — Final Render | Финальный клип | Pre-flight passed; END FRAME usable |
| 11 — Post | Текст, звук, графика, монтаж | Можно экспортировать |
| 12 — Delivery | Финальный файл | Delivery Evidence заполнен |

---

## L6. Upload Map — что загружать

| Этап | Обязательно | Дополнительно |
|---|---|---|
| Style Board | style/mood refs (или реальные фото) | treatment, palette |
| Character Board | Style Board + character refs | outfit/expression refs |
| Location Board | Style Board + location refs | lighting, architectural refs |
| Object Board | Style Board + object refs | material refs; Chroma Key refs если экран |
| Keyframe | Style + Character + Location + Object | Shot Board, Action Board |
| Shot Board | Style + Keyframes + Character/Location/Object | camera refs |
| Motion Test | approved Keyframe | Character/Style Board |
| Seedance Fast | Keyframe + Shot + Action + Char/Loc/Obj | video/audio reference |
| Seedance Final | всё approved | audio, motion refs, start/end frames, END FRAME prev. сегмента |
| Post | final render + clean text/logo assets | brand guide, sound refs |

---

## L7. QC-чеклист Draft 1

```text
[ ] Структура следует Shot Board (panels по порядку)
[ ] Тайминг читаем (beats различимы)
[ ] Identity stable (герой тот же на протяжении)
[ ] Wardrobe stable (одежда не меняется)
[ ] Hands acceptable (нет сломанных пальцев)
[ ] Props stable (объекты не морфируют)
[ ] Camera acceptable (движение/статика по заданию)
[ ] Нет нежелательного текста/логотипов
[ ] Chroma key placeholders intact (если есть VFX NOTE)
[ ] END FRAME usable (не mid-action, нет blur, camera stable)
[ ] Можно монтировать (clip usable)
```

## L8. Типовые ошибки → фиксы

```text
Промпт без модели       → добавить Model Block.
Меняется не тот объект  → "Only [X] changes. Everything else 100% unchanged."
Zoom вместо действия    → "No camera zoom. Camera locked. The object moves."
Объект плывёт           → "Object remains grounded. Never floats/detaches."
Движение назад          → "One direction only. Never reverses. Exits frame naturally."
Текст появляется сам    → "No text, subtitles, labels, readable words, random letters."
Продукт меняется        → "Exact same geometry/color/logo/proportions. No morphing."
Лицо пластиковое        → "Natural skin texture, pores, imperfections. No wax/filter."
Руки ломаются           → "Hands relaxed, partially visible. Avoid complex gestures."
Chroma key залит        → "Screen = solid #00FF00. No UI/app/text generated on screen."
END FRAME mid-action    → "Last frame: subject at rest, camera stable, no motion blur."
```

---

## L9. Разбор ТЗ — таблица

| Кадр | Хрон | Действие | Герой/объект | Камера | Свет | Текст | Звук | Уровень | Риски | Статика | Видео |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 01 | 0–2c | … | … | … | … | post | … | 1–4 | … | GPT Image 2 | Kling/Seedance |
| 02 | 2–5c | … | … | … | … | post | … | … | … | … | … |

---

## L10. Форматы платформ

| Платформа | Ratio | Resolution | FPS |
|---|---|---|---|
| TikTok | 9:16 | 1080×1920 | 30 |
| Instagram Reels | 9:16 | 1080×1920 | 30 |
| Instagram Story | 9:16 | 1080×1920 | 30 |
| YouTube Shorts | 9:16 | 1080×1920 | 30 |
| Instagram Feed | 1:1 | 1080×1080 | 30 |
| YouTube (стандарт) | 16:9 | 1920×1080 | 24/30 |
| Facebook Feed | 16:9 или 1:1 | 1280×720+ | 30 |
| LinkedIn | 16:9 | 1920×1080 | 30 |
| Cinematic / TV | 16:9 | 3840×2160 | 24 |

---

## L11. Что спросить у пользователя

Если данных не хватает:
- что показываем / цель ролика / ЦА
- формат и длительность
- один кадр / набор кадров / полный ролик
- есть ли референсы героя / продукта / локации / стиля (или реальные фото)
- нужно ли сохранять конкретное лицо
- нужен ли текст / кириллица / логотип в кадре
- нужен ли голос / музыка / SFX / lip-sync
- уровень качества: черновик / тест / финал
- какие модели доступны в интерфейсе

Дефолты: 16:9; реалистичный стиль; текст в post; дорогую модель — только после статики.

---

## L12. Краткий глоссарий

| Термин | Что это |
|---|---|
| Борд (board) | Визуальный референсный лист одного аспекта проекта |
| Shot Board | Техническая раскадровка (≤6 panels/15c для Seedance) |
| Keyframe / anchor | Статичный кадр-якорь (опциональный, не обязательный) |
| Beat | Событие в timecoded prompt (≤4/5c) |
| Panel | Стадия в Shot Board (≤6/15c) |
| END FRAME | Описание последнего кадра сегмента; становится start_image следующего |
| Reference Map | Нумерованная привязка бордов к @image слотам модели |
| Gate | Контрольная точка; если не пройдена — дальше не идём |
| Model Block | Обязательный блок перед промптом с метаданными этапа |
| Delivery Evidence | Файл + путь + duration + fps + resolution + QC-статус |
| VFX NOTE | Аннотация в Shot Board: элемент будет composited в post |
| Chroma Key | #00FF00 placeholder на экране устройства для post-compositing |
| HF | Higgsfield — платформа (✓ = доступно, ✗ = только external workflow) |
