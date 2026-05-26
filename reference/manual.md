# Полный мануал — AI Media Production Pipeline V3.0

**Версия:** 3.0 (консолидация Universal AI Video Generation Manual v2.9 + Development Concept)
**Назначение:** полный справочник метода. `SKILL.md` — операционный контракт; этот файл —
детали, к которым обращаются по необходимости. Прикладывать целиком к другой модели вместе
с идеей/ТЗ можно, но обычно достаточно `SKILL.md` + нужный раздел отсюда.

## Оглавление

1. Философия и главный принцип
2. Роли моделей
3. Формат ответа на production-задачу
4. Что спросить у пользователя (и дефолты)
5. Разбор ТЗ — обязательная таблица
6. Классификация сложности кадров (4 уровня)
7. Reference Bible — борды
8. Раскадровки и keyframes
9. Промпт по секундам (Timecoded Prompt)
10. Выбор моделей: каталог, матрица, «7 вопросов», decision tree
11. Media Roles (общие)
12. Stage Gates — контрольные точки
13. Upload Map — загрузка референсов и приоритет
14. Complex Action Rule
15. Modular vs Full Scene Generation
16. Density Control и risk-правила
17. Brand / IP Control
18. Дисциплина исполнения и honesty gates
19. QC — чек-листы проверки результата
20. План итераций и экономия генераций
21. Production Log и Post-mortem
22. Concept → Action
23. Глоссарий и терминология для промптов другим моделям
24. Русский слой ответа

---

## 1. Философия и главный принцип

AI-видео делается не как «один промпт», а как маленькое производство:
режиссура → стиль → персонажи → мир → раскадровка → промпты по секундам → видео →
проверка → монтаж → финальная сдача.

**80% успеха лежит в качественной статике, бордах и раскадровке.** Видео-модель должна
получать уже решённый кадр, а не думать вместо нас. Никогда не начинать производство с
одной дорогой генерации.

Типичные ошибки моделей, которые этот метод предотвращает:

```text
1. Сразу генерируют картинку вместо production-плана.
2. Делают один красивый кадр и называют его раскадровкой.
3. Создают борды, но не используют их дальше.
4. Делают keyframes вместо Shot Board.
5. Путают черновик с финалом.
6. Пишут «готово» без файла, звука, монтажа и QC.
7. Выбирают Kling/Hailuo/Krea там, где нужен Seedance/Sora/Veo.
8. Не расписывают промпт по секундам и не ссылаются на panels.
9. Не различают идею, концепт, борды, раскадровку, видео и монтаж.
```

Метод применим к: короткий мультфильм, AI-реклама, UGC / product demo / TV spot,
экшен-сцены, персонажная анимация, продуктовая визуализация, визуальная библия,
production-pack по ТЗ, тестовые задания AI Creator, подготовка промптов под любые
генеративные модели.

---

## 2. Роли моделей

- **Creative Model** — сильна в идеях, мире, стиле, визуальном коде, naming, метафорах,
  культуре, персонажности. Слаба в gates, upload map, остановках, соблюдении этапов,
  status labels. Её результат маркировать «Статус: креативная разработка».
- **Production Orchestrator** — сильна в структуре, этапах, gates, раскадровках, QC,
  статусах, repair loops. Именно она превращает идею в production plan.
- **Execution Agent** — выполняет один утверждённый этап: этап → результат → QC →
  следующий gate. Нельзя давать большую свободу.
- **Image / Video Model** — не получает весь длинный документ. Получает только: конкретный
  prompt, Reference Map, media inputs, negative rules, output requirements.

Рекомендация по reasoning-режиму: для разбора ТЗ, production-pack, treatment, gates,
prompt packs, shot/segment planning, QC и post-mortem — Extended/Deep Thinking,
Orchestrator или Agent mode. Standard Thinking — только с коротким Safe Mode и поэтапным
подтверждением. Instant/Quick/Fast — только для quick idea expansion, naming, короткого
переписывания промпта, one-shot mood; результат маркируется `PROTOTYPE / STYLE EXPLORATION`.

**Compliance Score модели** (после теста на пайплайн): 0 — сразу генерирует, игнорирует
пайплайн; 1 — красивый prompt без gates; 2 — план без shot boards/board usage; 3 — основные
gates, но путает keyframes/storyboard; 4 — корректный workflow, gates, boards, timecoded;
5 — умеет останавливать пайплайн при failed gate и честно маркировать статус.
0–2: только creative exploration; 3: с ручным контролем; 4–5: можно как production assistant.

---

## 3. Формат ответа на production-задачу

```text
1. Что я понял из задачи
2. Уточнения / допущения
3. Сложность задачи
4. Рекомендуемый пайплайн
5. Таблица моделей по этапам
6. Этап 1: модель + зачем + промпт (с Model Block)
7. Что проверяем после генерации (QC)
8. Что делаем дальше (gate)
```

Каждый этап начинается с Model Block (см. `SKILL.md` §2). Промпт без Model Block — ошибка.

---

## 4. Что спросить у пользователя (и дефолты)

Если данных не хватает, уточнить: что показываем; формат (16:9 / 9:16 / 1:1); длительность;
один кадр / набор / полный ролик; есть ли референсы героя/продукта/локации/стиля; нужно ли
сохранять конкретное лицо; нужен ли текст в кадре и кириллица; нужен ли голос/музыка/SFX/
липсинк; уровень качества (черновик / тест / презентация / финал); ограничения по бюджету/
числу генераций; какие модели доступны в интерфейсе.

Дефолты, если пользователь не знает: формат 16:9; реалистичный или заданный стиль; сначала
keyframes/борды; текст и кириллицу добавлять после генерации; дорогую видео-модель — только
после утверждения статики; нет референсов → сначала создаём референс-библию.

Задавать вопросы только если без них нельзя двигаться; иначе — предложить рабочие допущения.

---

## 5. Разбор ТЗ — обязательная таблица

| Кадр | Хрон | Действие | Герой/объект | Камера | Свет | Текст | Звук | Сложность | Риски | Модель статики | Модель видео |
|---|---:|---|---|---|---|---|---|---|---|---|---|
| 01 | 0–2с | … | … | … | … | … | … | низк/сред/выс | … | GPT Image 2 | Kling / Seedance |
| 02 | 2–5с | … | … | … | … | … | … | … | … | … | … |

Дополнительно зафиксировать: цель ролика; ключевое сообщение; ЦА; формат сдачи; визуальный
стиль; что нельзя менять; что должно быть читаемо; что лучше вынести после генерации; какие
референсы нужны; какие модели использовать.

---

## 6. Классификация сложности кадров

**Уровень 1 — простой кадр** (продукт/человек стоит, лёгкий push-in/parallax, оживление
фото, простое движение волос/ткани/света). Статика: GPT Image / GPT Image 2 / Nano Banana 2 /
Seedream 5.0 Lite / Auto. Видео: Kling 3.0 / Minimax Hailuo / Krea Video / Midjourney Video /
HappyHorse. Seedance / Sora / Veo обычно не нужны.

**Уровень 2 — средний кадр** (герой идёт, поворот головы, лёгкое движение камеры, дверь
открывается, продукт вращается, предмет берут рукой, простой интерьерный проход). Статика:
GPT Image 2 / Nano Banana Pro / FLUX.2 Pro / Seedream 4.5. Видео: Kling 3.0 / Kling Motion
Control / Runway Gen-4 / Seedance 2.0 Fast / Veo 3.1 Lite / Wan 2.7.

**Уровень 3 — сложный кадр** (орбитальная камера, несколько синхронных движений,
взаимодействие с графикой, неизменный продукт, смена эмоции, пролёт камеры через
пространство, физический переход между сценами, удержание лица и одежды). Статика: GPT
Image 2 / Multi Reference / FLUX.2 Max / Nano Banana Pro / Higgsfield Soul Cinema. Видео:
Seedance 2.0 / Kling 3.0 Motion Control / Veo / Sora 2 / Wan 2.7 / Runway Gen-4.

**Уровень 4 — очень сложный кадр** (липсинк, известное лицо, сложные руки, толпа, сложный
текст в кадре, брендовые логотипы, длинный continuous shot, несколько локаций в одном кадре,
переход через зеркало/телефон/дверь/экран, точная мимика). Статика: GPT Image 2 / Multi
Reference / Higgsfield Face Swap / Character Swap / FLUX.2 Max / Nano Banana Pro. Видео:
Sora 2 / Veo / Seedance 2.0 / Kling 3.0 Motion Control / Runway Gen-4. Дополнительно:
изолированный lip-sync workflow, dedicated identity tools, разбивка на сегменты.

---

## 7. Reference Bible — борды

Борд — визуальный референсный лист, фиксирующий один аспект проекта (промпты — в
`templates.md`). Если борд создан — он обязан использоваться дальше (Board Utilization).

- **Style Board** — визуальный язык, палитра, свет, фактуры, mood, lens/camera language,
  motion language, do/don't, production notes. Делать всегда, если нет готового стиля.
- **Character Board** — внешность, силуэт, лицо, одежда, пропорции, эмоции, позы, материалы.
  **Каждого важного персонажа — отдельный борд, не объединять в один общий.**
- **Location Board** — пространство, архитектура, свет, атмосфера, props, масштаб, правила окружения.
- **Object / Prop Board** — продукт/транспорт/телефон/реквизит: форма, материал, цвет, масштаб
  (single source of truth, объект идентичен во всех панелях).
- **Pose Board** — позы и эмоции героя перед видео (тот же человек/одежда/идентичность),
  включая start/end pose для анимации.
- **Culture / World Rules Board** — правила мира, символы, одежда, граффити, соц. структура,
  музыкальный вайб, что принадлежит миру и что нет. Для миростроения.
- **Motion Philosophy Board** — общий язык движения: smear frames, frame skipping, движение
  хвостов/одежды/дыма, ритм, camera distortion, stop-motion pauses, 2D/3D hybrid. Не замена Action Board.
- **Action Board / Choreography Sheet** — конкретное движение: трюк, прыжок, бой, танец, жест,
  работа рук, траектория объекта, позы по шагам.
- **Shot Board** — техническая раскадровка перед видео (см. §8).

---

## 8. Раскадровки и keyframes

**Главный управляющий документ для видео — раскадровка, а не keyframes.**

**Master Storyboard** (главная раскадровка) — верхнеуровневая карта всего ролика: из каких
частей состоит, где начало/середина/финал/переход/CTA, какие сегменты генерировать отдельно,
сколько будет video generations, как монтировать.

```text
Главная раскадровка 10-секундного ролика:
Panel 1 — 0–2с — завязка
Panel 2 — 2–4с — триггер / решение
Panel 3 — 4–7с — переход / действие
Panel 4 — 7–10с — результат / CTA
```

**Segment Storyboard** (раскадровка сегмента) — полная раскадровка одной части ролика;
именно она чаще всего идёт в video generation. Сложный ролик = несколько раскадровок
сегментов + несколько генераций. Не делать всё одним клипом.

**Что является раскадровкой:** несколько panels, номера panels, порядок действий, timing,
camera notes, motion arrows, transition logic, post placeholders, locked elements.
**Что НЕ является:** один красивый кадр, poster, moodboard, single still, герой в одной позе,
character board, key visual.

**Keyframe (визуальный якорь)** — статичный кадр. Нужен НЕ всегда. Добавлять, только если
раскадровки недостаточно для точного элемента: стартовый/финальный кадр, packshot, экран
телефона, лицо крупным планом, форма продукта, точная одежда, CTA-карточка, сложная рука.
Keyframe не идёт автоматически после раскадровки. В ответах пользователю — «дополнительный
визуальный якорь» вместо «keyframe».

**Shot Board vs Keyframes — оба нужны, но для разного:**

```text
Shot Board     = режиссура: порядок шотов, timing, camera movement, action arrows,
                 transition logic, SFX points, где в post добавится текст/лого.
Keyframes      = визуальный anchor: лицо, свет, wardrobe, локация, props, композиция
                 конкретного кадра, start/end frames для video model.
Video prompt   = использует оба: Shot Board (sequence map) + Keyframes (anchors) +
                 Action Board (motion phases).
```

Если платформа ограничивает число референсов: motion/story сложная → Shot Board как primary
+ ключевые start/end frames; visual consistency сложная → keyframes как primary refs +
Shot Board в логике prompt; рефов слишком много → разбить сцену на клипы.

---

## 9. Промпт по секундам (Timecoded Prompt)

Если раскадровка есть — video prompt обязан ссылаться на её panels по времени (полные
шаблоны — `templates.md`):

```text
0.0–1.0s: Follow Segment Storyboard panel 1. Hero pushes forward on the skateboard.
1.0–2.0s: Transition from panel 1 to panel 2. The board begins to flip.
2.0–3.0s: Follow panel 3. Hero reaches the peak of the trick.
3.0–4.0s: Transition to panel 4. Hero lands and holds final pose.
```

Правило: `Follow storyboard panel order. Do not skip panels. Do not invent new actions.
Do not jump to the final panel early.`

**Mapping panels → секунды:** для каждого segment-клипа явно сопоставить panel ↔ временной
отрезок, заложить hold в конце для монтажа, генерировать source clip длиннее нужного
(например 5 c) и в монтаже использовать только нужный отрезок (например 0.0–1.2 c).

---

## 10. Выбор моделей

### 10.1. Видео-модели

| Модель | Тип | Когда использовать | Когда не использовать |
|---|---|---|---|
| Seedance 2.0 | premium video | Финальные сложные кадры, орбит. камеры, мульти-шот, сложная синхронизация, cinematic realism | Не как черновик для поиска композиции |
| Seedance 2.0 Fast | fast video | Быстрый тест Seedance-логики, черновой прогон сложного движения, экономия перед финалом | Не ждать макс. стабильности |
| Kling 3.0 | video | Качественный image-to-video, движение персонажа/объекта, быстрые коммерч. клипы, тесты | Не всегда держит логику нескольких объектов |
| Kling Motion Control | video reference control | Есть видео-референс движения, нужно перенести motion | Без хорошего motion reference |
| Kling 3.0 Motion Control | video motion transfer | Перенос движения из видео в изображение, управляемая камера/персонаж | Не для свободного текстового сторителлинга |
| Kling O1 Video Edit / Omni Edit | video edit | Редактирование/текстовое редактирование готового видео | Не для первичной генерации с нуля |
| Google Veo 3.1 Lite | video | Короткие реалист. кадры, кинематографичность, audio/video | Длина ограничена; не тратить на черновики |
| Google Veo | video family | Киношный реализм, image-to-video, first/last frame, видео с аудио | Не начинать без keyframes |
| OpenAI Sora 2 | high-end video | Сложные сцены, 3D-пространство, динамика, аудио, сторителлинг | Дорого/рискованно для поиска стиля |
| Wan 2.7 / 2.2 | video | 1080p, cinematic tests, альтернатива если Seedance/Kling не дают результат | Нужна проверка на типе сцены |
| Minimax Hailuo | video | Быстрые affordable clips, human/social motion, черновики | Не для сверхточной продуктовой консистентности |
| HappyHorse | video | Быстрые креатив. тесты, social-first | Новая/менее проверенная, тестировать на малом |
| Grok Imagine / Edit | image/video | Быстрые стилизованные social visuals / текстовое исправление видео | Не для строгого бренд-реализма / типографики |
| Runway Gen-4 | external video | 5–10 c из image + prompt, VFX/live-action | Нужен сильный input image |
| Midjourney Video | external video | Mood, visual exploration, loops, aesthetic movement | Не для рекламы с юр. текстом |
| Krea Video | external video | Short clips, image-to-video, extend, быстрые тесты | Не финал сложной рекламы без композита |

### 10.2. Image / edit / reference-модели

| Модель | Тип | Когда использовать |
|---|---|---|
| GPT Image 2 | image | Главная модель для keyframes, boards, сложной композиции, production stills, точного промптинга |
| GPT Image | image | Универсальные изображения, быстрые варианты, базовые boards |
| Multi Reference | image/edit | Объединить несколько референсов: герой + продукт + локация + стиль |
| Nano Banana Pro / 2 / (base) | image | Быстрый качественный static, база для keyframes / быстрые итерации и черновики |
| Seedream 5.0 Lite / 4.5 / 4.0 | image / image edit | Визуальное рассуждение и итерации / 4K детальные изображения / продвинутое редактирование |
| FLUX.2 Pro / Flex / Max | image | Фотореализм и скорость / creative variants / макс. точность для финальных static |
| Flux Kontext Max / Reve | image edit | Точные edits по контексту, исправление деталей |
| Higgsfield Soul 2.0 / Soul Cinema / Soul | image | Ультрареалистичная fashion/beauty / cinema-grade stills / реалистичные fashion visuals |
| Higgsfield Face Swap / Character Swap | identity | Замена лица / персонажа, consistency |
| Z-Image / Kling O1 (image) | image | Lifelike портреты / альтернативные реалистичные кадры |
| Auto | router | Нет времени выбирать — система выбирает под prompt |

### 10.3. Выбор по проблеме

- Быстро нащупать стиль → GPT Image / Nano Banana 2 / Seedream 5.0 Lite / Auto.
- Красивые keyframes → GPT Image 2 / Nano Banana Pro / FLUX.2 Max / Seedream 4.5 / Soul Cinema.
- Сохранить лицо героя → Character Board + Face/Character Swap + Multi Reference + LoRA/identity workflow.
- Неизменный продукт → Object Board + Multi Reference + GPT Image 2 / FLUX.2 Max / Nano Banana Pro; видео только после approved object keyframe.
- Точное движение из референса → Kling Motion Control / Kling 3.0 Motion Control / Runway / Krea.
- Орбитальная/сложная камера → Seedance 2.0 / Veo / Sora 2 / Kling Motion Control (+ Shot Board с motion arrows, camera path diagram, start/mid/end keyframes).
- Видео с текстом → чистый кадр + место под текст + текст в post (не генерировать кириллицу).
- Липсинк → Sora / Veo / dedicated lipsync tool, изолированно (см. §16).

### 10.4. Матрица «фактор задачи → модель»

| Фактор задачи | Лучше использовать | Почему |
|---|---|---|
| Один keyframe, лёгкое движение | Kling 3.0 / Hailuo / Krea | Быстро, дёшево, хватает для simple i2v |
| Простая продуктовая анимация | Kling 3.0 / Runway / Krea | Хорошо оживляют approved still |
| Много быстрых вариантов движения | Hailuo / Krea / HappyHorse | Быстро тестировать гипотезы |
| Сложное действие по секундам | Seedance 2.0 Fast → 2.0 | Лучше для timecoded prompts |
| 5–10 c сцена с развитием | Seedance 2.0 | Держит последовательность beats |
| Несколько объектов взаимодействуют | Seedance 2.0 / Sora 2 / Veo | Нужна логика сцены |
| Сложная камера (orbit/fly-through) | Seedance 2.0 / Veo / Sora | Осмысленная камера |
| Есть reference video с движением | Kling (3.0) Motion Control | Специализация на переносе |
| Строго first frame → last frame | Veo 3.1 / Runway / Sora | first/last frame workflow |
| Киношная сцена с аудио | Seedance 2.0 / Sora 2 / Veo 3.1 | Качество audio-visual |
| Редактировать готовое видео | Kling O1/Omni Edit / Grok Edit | Это edit-модели |
| Точный текст/логотип | Не генерировать в видео | Добавлять в post |
| Дешевле | Kling/Hailuo/Krea/Seedance Fast | Сначала тест |
| Финал сложной сцены | Seedance 2.0 / Veo / Sora | После approved boards/keyframes |

### 10.5. «7 вопросов» перед выбором видео-модели

```text
1. Простое оживление или сцена?         оживление → Kling/Hailuo/Krea; сцена → Seedance/Sora/Veo
2. Есть действия по секундам?           нет → Kling/Hailuo/Krea; да → Seedance Fast/2.0/Sora/Veo
3. Есть reference video движения?       да → Kling (3.0) Motion Control; нет → по сложности
4. Есть first frame и last frame?       да → Veo/Runway/Sora/Seedance; нет → обычный i2v / multi-ref
5. Нужен звук?                          нет → Kling/Seedance/Runway/Krea; да → Seedance 2.0/Sora 2/Veo 3.1
6. Сколько объектов взаимодействуют?    1 → Kling/Hailuo/Krea; 2+ с логикой → Seedance/Sora/Veo
7. Тест или финал?                      тест → дешёвая/Fast; финал → Seedance 2.0/Sora/Veo/Runway
```

### 10.6. Decision tree

```text
Нет Style/Character/Keyframe        → не делать видео, вернуться к boards.
Один keyframe + простое движение    → Kling / Hailuo / Krea.
Есть video reference движения       → Kling Motion Control.
Сложная логика / Action / Shot Board → Seedance 2.0 Fast.
Seedance Fast прошёл                 → Seedance 2.0 final.
Seedance не справился               → разбить на клипы / усилить keyframes / добавить Action Board / Sora/Veo/Runway.
Нужен точный текст/логотип          → не генерировать в видео, добавить после.
```

Главное: **выбирай модель по задаче кадра, а не по названию.** Seedance 2.0 — не «ниже»
Sora/Veo; это один из основных вариантов для сложных сцен. Sora/Veo — альтернативы и иногда
лучший выбор ради их специфики: character references, first/last frame, audiovisual realism,
longer storytelling.

---

## 11. Media Roles (общие)

Перед генерацией проверять, какие роли входных медиа принимает модель.

| Модель | Принимаемые роли |
|---|---|
| Seedance 2.0 | `image`, `start_image`, `end_image`, `video`, `audio` |
| Kling 3.0 / 2.6 | `start_image`, `end_image` / `start_image` |
| Veo 3.1 / Veo 3 | `start_image` (≤1 ref) / `image` (≤1 ref) |
| Большинство image models | `image`, обычно 1+ references |
| Z-Image / Soul Cast / Soul Location | media не принимает, prompt-only |

Для Seedance аудио передавать как media role `audio`, а не отдельным параметром.

---

## 12. Stage Gates — контрольные точки

Если gate не пройден — дальше не идём. Не делать видео без утверждённой статики; не делать
финальный Seedance без пройденного Fast/motion-теста; не делать full scene без проверенных сегментов.

| Gate | Что проверяем | Идём дальше, если |
|---|---|---|
| 1 — Brief Approved | Задача, формат, стиль, ограничения | Есть понятный план и модели |
| 2 — Style Approved | Визуальный стиль | Стиль единый, без мусора и случайного текста |
| 3 — Character Approved | Individual character boards | Персонажи стабильны, не смешиваются |
| 4 — Object/Prop Approved | Продукты, предметы, реквизит | Стабильны по форме, цвету, масштабу |
| 5 — Location Approved | Локации | Локация узнаваема и стабильна |
| 6 — Cast/Scale Approved | Герои вместе | Масштаб и стиль совместимы |
| 7 — Keyframe Approved | Ключевые кадры (если нужны) | Композиция, эмоция, объект, стиль совпадают |
| 8 — Shot/Action Board Approved | Раскадровка движения | Понятны beats, камера, тайминг, траектории |
| 9 — Motion Test Approved | Тест движения | Нет морфинга, реверса, сломанной камеры |
| 10 — Final Render Approved | Финальный клип по QC | Можно в монтаж/звук/графику |
| 11 — Post Approved | Текст, звук, графика, монтаж | Можно экспортировать |
| 12 — Delivery Approved | Финальный файл | Готово к сдаче |

**Gate checklist:** что утверждаем; какие входные материалы использованы; что получилось;
что не получилось; остались ли риски; можно ли дальше; если нет — что исправляем.

---

## 13. Upload Map — загрузка референсов и приоритет

Перед каждым промптом указывать не только модель, но и Input assets (что и зачем).

| Этап | Обязательно загрузить | Дополнительно |
|---|---|---|
| Style Board | style/mood/brand refs (если можно) | treatment, palette |
| Individual Character Board | Style Board + character refs | outfit/expression refs |
| Cast Lineup | Style Board + все approved Character Boards | scale notes |
| Location Board | Style Board + location refs | lighting/architectural refs |
| Object/Prop Board | Style Board + object/product refs | material/macro refs |
| Keyframe | Style + Character + Location + Object/Prop | Shot Board, Action Board, Cast Lineup |
| Shot Board | Style + Keyframes + Character/Location/Object | camera refs |
| Action Board | Character + Prop/Object + Location/Style | motion/pose refs |
| Cheap Motion Test | approved Keyframe | Character/Style Board |
| Seedance Fast | Keyframe + Shot + Action + Character/Location/Object | video/audio reference |
| Seedance Final | всё approved | audio, motion refs, first/last frames |
| Motion Control | approved keyframe + video reference | motion library refs |
| Post | final render + clean text/logo assets | brand guide, sound refs |

**Приоритет при конфликте референсов:** Individual Character Board → Style Board →
Object/Prop Board → Location Board → Cast Lineup → Shot Board → Action Board → Video ref → Audio ref.

---

## 14. Complex Action Rule

Сцена — complex action, если есть ≥2–3 из: герой делает >1 действия; прыжок/бег/удар/танец/
спорт/падение; камера двигается с героем; объект в руках; несколько объектов вокруг; нужен
точный timing; есть начало/середина/финал; нужна физика; взрыв/жидкость/искры/debris;
удержание персонажа при активном движении; сцена 7–15 c.

Пайплайн complex action: Style Board → Individual Character Board → Outfit/Weapon/Prop Board →
Enemy/Object Board → Location Board → Keyframes (start / mid-action / impact / end pose) →
Action Board → Shot Board → Seedance 2.0 Fast test → Seedance 2.0 final → fallback (разбивка
на клипы / Kling Motion Control при наличии reference / Sora/Veo / Runway).

Для complex action не выбирать Kling/Hailuo/Krea как основной production-инструмент (только
простой сегмент или дешёвый mood-test). Основной путь: Action Board → Seedance 2.0 Fast → 2.0.

---

## 15. Modular vs Full Scene Generation

Сложную сцену сначала дробим на сегменты; full scene generation — только после успешных сегментов.

Дробить, если: >2 локаций; >2 персонажей; сложная камера; переход через дверь/экран/зеркало;
бой/спорт/трюк; диалог; руки крупным планом; объект появляется/исчезает; точная драматургия;
сцена длиннее 6–8 c.

```text
Стратегия: Clip A — setup → Clip B — action/transition → Clip C — result/reaction → Clip D — packshot/final hold.
Реклама:   store routine → SMS/trigger → entrance/doors → interior fly-through → final cashier smile → packshot.
Action:    hero pose/enemies → jump/chase → attack/impact → explosion/debris → landing/final pose.
```

---

## 16. Density Control и risk-правила

**Density Control.** Перегруженная сцена — главная причина провалов. Считать: beats_count,
locations_count, characters_count, hands_or_props, text_logo, camera_complexity, audio_lipsync.
Разбивать на клипы, если: beats > 4 на 5 c (или > 5–6 на 10 c); локаций > 1 в одном
непрерывном видео; lip-sync + action; экран телефона + читаемый текст; scan/hand + CTA;
переход между несвязанными локациями.

**Minimal Motion Rule** (для рекламы). Часто лучше held shot, лёгкое движение глаз/руки,
лёгкий push-in, свечение телефона, открывание двери, плавный interior glide, финальный
smile hold — а не активная физика в каждом кадре. Чем меньше сложной физики, тем выше шанс,
что монтаж будет usable.

**Hands / Props Risk.** Лучше: предмет уже на столе, минимальное движение руки, телефон
статичен, текст в post, prop частично вне фокуса. Хуже: рука берёт предмет и сканирует
штрихкод; телефон движется, показывая текст; close-up пальцев на экране; несколько товаров
едут по ленте; кассир обрабатывает несколько предметов за секунду.

**Wardrobe Lock.** При смене роли/локации drift почти гарантирован. Фиксировать: тип одежды,
цвет, вырез, длину рукава, форму жилета, позицию бейджа; «no logo text generated; same outfit
as approved keyframe». Пример: «wears the exact same blue retail vest over a plain grey
crew-neck shirt as in Keyframe 5; shape/neckline/sleeve/color must not change».

**Lip-sync isolated.** Senior lip-sync — отдельный controlled pass, не часть перегруженного
монтажа. Путь: approved keyframe → отдельный direct-to-camera head/medium shot → voice/TTS →
dedicated lip-sync tool → mouth/face QC → вставить как alternate shot. In-prompt lip-sync —
только если весь клип построен вокруг говорящего лица, камера стабильная, фраза короткая,
нет сложного action и плотного монтажа. При rejection: не повторять тот же prompt, проверить
причину, убрать рискованные элементы, разделить video/voice/lipsync/mix.

**Audio Map.** Даже если звук будет в post, timecoded prompt должен иметь audio map (ambience,
ping, silence beat, music start, scanner beep, music resolves + CTA) — для монтажёра.

**Draft 1 ≠ финал.** QC по чек-листу (§19); ≥2 критичных проблем → `DRAFT 1 FAILED / NEEDS
REPAIR`. Repair Options: reduce motion; held shots вместо transitions; split на 2 клипа;
reduce hand action; phone static; текст/лого в post; stronger wardrobe lock; start/end frames;
Seedance 2.0 вместо Fast; dedicated lipsync. **Draft 2** — только после root cause analysis,
revised prompt, revised assets/density, и user approval если меняются cost/quality. Если
ломается 3 раза подряд — менять подход (разбить действие, больше keyframes, сменить модель,
вынести элемент в post, упростить камеру, заменить тех. описание визуальной метафорой, убрать текст).

---

## 17. Brand / IP Control

Brand/IP риск: логотипы, названия брендов, форма упаковки, фирменные цвета, реальные товары,
знаменитости, likeness реального человека, форма сотрудника, брендовые магазины, товарные
знаки, фирменные шрифты, рекламные claim'ы.

```text
Если бренд разрешён:  чистые approved assets после генерации; в видео — clean placeholder;
                      логотип/текст — отдельным слоем; не доверять видео-модели точный логотип.
Если не разрешён:     вымышленный аналог; не копировать логотип/упаковку 1:1; не использовать
                      реальное имя знаменитости как прямой likeness.
В генерации:          blank package / blank badge / clean storefront / no readable logo.
После генерации:      official logo asset, official text, legal, CTA, typography.
Запреты:              no fake logos, no distorted brand marks, no unreadable brand text,
                      no accidental trademark imitation if not allowed.
```

---

## 18. Дисциплина исполнения и honesty gates

Генерация файла ≠ выполнение этапа. Этап выполнен, только если результат прошёл QC по смыслу.
Пример: задача «keyframe героиня за кассой», а получился портрет без кассы → это не keyframe,
максимум Character Reference / failed keyframe.

**Status Labels:** `DONE` (соответствует задаче, идёт дальше) / `PARTIAL` (часть выполнена,
есть недостатки) / `PROTOTYPE` (черновик, не финал) / `READY FOR EDIT` (исходник для монтажа,
не финал) / `FAILED` (переделывать) / `BLOCKED` (нельзя без входных данных/прав/референсов/
доступа). Для пользователя дублировать русскими статусами (см. §24).

**Failure Gate** (при generation failed/rejected, rate limit, model ignored prompt, changed
identity, wrong scene, auto-downgrade, tool unavailable, upload failed): остановиться и
описать — что сломалось, на каком этапе, почему критично, можно ли продолжать без исправления
(если да — что теряем и какой статус), новая стратегия (A/B/C), спросить ли пользователя.
Нельзя молча продолжать иначе, если это меняет качество/методологию/смысл.

**Failed keyframe:** нельзя анимировать; нельзя называть ошибку «правильным workflow». Если
получился Character Board вместо сцены: `QC: результат не keyframe, только character reference;
Gate 7 FAILED; перегенерировать scene keyframe`.

**Identity / Soul / LoRA honesty:** нельзя писать «Soul ID создан / LoRA обучена / персонаж
обучен», если training failed/rejected/не завершился/подменён одним reference или AI-портретами/
не прошёл identity QC. Корректные статусы: «Soul ID trained and verified» (training завершён,
проверен на 2–4 prompts) / «training failed» / «single anchor reference used» / «character
reference generated» / «lookalike/fictional substitute». Не тренировать identity на
AI-portraits, если цель — реальный человек (drift закрепится). Для реального лица — реальные
разрешённые фото; если их нет — fictional lookalike или запрос материалов.

**Rights / Likeness Gate** (реальный человек/знаменитость): используется ли реальный человек;
есть ли разрешение; стратегия (approved likeness / fictional lookalike / generic / remove).
Нельзя заявлять права, которых нет; не обучать identity на случайных публичных фото; не
выдавать generated likeness за legal-ready commercial asset. Для тестов lookalike/inspired-by
можно, но явно помечать.

**Tool Availability Check:** не утверждать доступность инструмента без проверки окружения.
Если заявлен в pipeline, но не найден: проверить список tools; если недоступен — назвать;
предложить внешний путь и внутреннюю альтернативу; **этап не пропускать**.

**Boardless Generation Ban.** Без boards/keyframes можно только если пользователь явно просит
быстрый тест/mood/prototype, нет требования к consistency, результат маркируется PROTOTYPE.
Запрещено без boards: тестовое задание, коммерческий ролик, repeated character, product/brand
consistency, несколько сцен, lipsync, legal text/logo, уровень Middle/Senior, ожидание «готового результата».

**Post-production Disclosure** (до генерации): что создаёт AI-генерация (source clips,
keyframes, движение, атмосфера) и что делает post (читаемый текст, кириллица, логотипы, legal,
монтаж, timing, музыка, SFX, voice-over, lipsync cleanup, color, экспорт) + текущий статус
deliverable. Если модель не может сама смонтировать в текущей среде — сказать это до начала.

**«Готово» без Delivery Evidence запрещено.** Нужны: финальный файл, путь/ссылка, формат,
duration, fps/resolution, QC-статус, список того что осталось на post. Иначе писать «Исходники
готовы для монтажа; финальный ролик ещё не готов». На вопрос «ты продолжаешь или у меня
постпродакшн?» — не отвечать автоматически «продолжаю»; сначала проверить доступ к скачиванию/
монтажу/аудио/lipsync/экспорту; если нет — отдать монтажный план и список файлов.

---

## 19. QC — чек-листы проверки результата

**Общий QC:** соответствует ТЗ? правильная модель? был ли Model Block? сохранился герой?
сохранился продукт? правильная камера? правильное движение? нет лишнего текста? нет морфинга?
нет дублирования? не сломаны руки/лицо? читается эмоция? совпадает свет? не поменялась локация?
есть место под финальную графику?

**Персонаж:** то же лицо? та же одежда? естественная кожа? нет пластика? нет лишних пальцев?
эмоция соответствует? движение не слишком резкое?

**Продукт:** та же модель/форма/геометрия/палитра? логотип не испорчен? материал реалистичный?
отражения не ломают форму?

**Камера:** движется/не движется как надо? нет случайного zoom? нет случайной смены ракурса?
нет рывков? нет обратного движения? движение читается за весь хрон?

**Draft 1 video QC:** structure follows Shot Board; timing readable; identity stable;
wardrobe stable; hands acceptable; props stable; camera acceptable; no unwanted text/logos;
placeholders usable; can be edited.

**Типовые ошибки и фиксы:**

```text
Промпт без модели         → добавить Model Block.
Меняется не тот объект     → "Only [X] changes. Everything else 100% unchanged."
Zoom вместо действия       → "No camera zoom / frame scaling. Camera locked. The object moves."
Объект плывёт/отрывается   → "Object remains physically attached / grounded. Never floats/detaches."
Камера статична, нужна динамика → "Camera visibly changes angle over time: front-left → side → rear."
Камера двигается, нужна статика → "Locked tripod. No movement/zoom/pan/tilt. Only subject acts."
Движение назад             → "One continuous direction only. Never reverses/loops. Exits naturally."
Текст появляется сам       → "No text, subtitles, labels, signage, readable words, random letters."
Продукт меняется           → "Exact same geometry/color/material/logo/proportions. No morphing/redesign."
Лицо пластиковое           → "Natural skin texture, pores, imperfections. No wax/plastic/beauty filter."
Руки ломаются              → "Hands relaxed, partially visible. Avoid complex gestures. Dedicated hand keyframe if needed."
```

---

## 20. План итераций и экономия генераций

Минимальный план: 1–2 генерации на стиль; 2–4 на Object/Character/Location Boards; 3–6
keyframes; 1–2 shot boards; 1 дешёвый motion test; 1–3 финальных video generations.

Экономия через boards: каждый approved board переиспользуется как референс на всех следующих
этапах. Дорогие видео-модели — только после утверждённой статики и пройденного motion-теста.
Если генерация ломается 3 раза — менять подход, а не жечь генерации тем же промптом.

---

## 21. Production Log и Post-mortem

**Production Log (журнал продакшена)** — фактический журнал решений, не замена пайплайна.
Стартовая запись: статус (только план / креативная разработка / производственный план),
готовые материалы (пока нет), контрольная точка (не пройдена), следующее действие. Полный
шаблон — в `templates.md`.

**Post-mortem** (если уже нагенерирован хаос): остановиться; что было сделано; что не
соответствует пайплайну; какие gates провалены; что можно сохранить; что переделать.

---

## 22. Concept → Action

Если ответ звучит концептом — переводить в действие с конкретным deliverable, статусом и
следующим шагом (пример — `SKILL.md` §10). Не писать концепт без действия; не писать
«создаём культуру» абстрактно — сразу давать deliverable (например «борд стиля на 8–12 panels
→ style_board_v01.png»).

---

## 23. Глоссарий и терминология для промптов другим моделям

| Термин | Международный аналог | Что значит |
|---|---|---|
| Master Shot Board | Master Storyboard / Full Video Storyboard | Карта всего ролика |
| Segment Shot Board | Segment Storyboard / Shot Board for one clip | Раскадровка одной части |
| Shot Board | Storyboard / shot-by-shot board | Визуальная последовательность действий |
| Action Board | Choreography Sheet / Motion Breakdown | Раскадровка движения, поз, траекторий |
| Panel | Storyboard Panel / Frame | Ячейка раскадровки |
| Beat | Story/Action Beat | Смысловой момент сцены |
| Timecoded Prompt | Second-by-second / Timeline Prompt | Промпт по секундам |
| Reference Map | Input Reference Map / Media Map | Список референсов и зачем они |
| Upload Map | Input Asset Plan | Что загружать на каждом этапе |
| Supplementary Anchor | Optional Visual Anchor / Extra Still | Доп. референс для точного элемента |
| Keyframe | Start/End Still / Visual Anchor / Hero Still | Статичный кадр-якорь (если нужен) |
| Stage Gate | Approval Gate / QC Gate | Контрольная точка |
| Delivery Evidence | Final Output Evidence | Файл, duration, fps, QC |
| Repair Loop | Revision Loop / Fix Pass | Цикл исправления после failed draft |
| READY FOR EDIT | Source Clips Ready / Edit Package | Исходники готовы, финал — нет |

Для промптов в другие модели (Claude/Gemini/ChatGPT/Sora/Veo/Runway/Kling/Seedance)
использовать универсальные формулировки: вместо «Shot Board» → «Segment Storyboard / a
multi-panel storyboard for one video segment»; вместо «Micro Shot Board» → «Segment Storyboard»;
вместо «Supplementary Anchor» → «optional visual anchor / additional still reference for one
risky detail»; вместо «keyframe» без пояснения → «optional still image anchor, only if the
storyboard does not provide enough visual control» (чтобы модель не считала keyframes обязательными).

---

## 24. Русский слой ответа

Для пользователя — русский язык и понятные названия; английский термин — в скобках при первом
упоминании. Промпты для генеративных моделей — на английском.

Русские статусы (вместо голых английских): **креативная разработка** (обсуждаем идею/стиль/мир) /
**только план — без генерации** / **производственный план** (есть workflow, gates, boards) /
**готово к монтажу** (исходники есть, финала нет) / **черновик требует правок** (draft есть, QC
не пройден) / **финальная сдача** (есть файл + параметры + QC + known issues).

Слово «борд» допустимо (привычно в AI-production), но в важных местах давать русское пояснение:
Style Board = борд стиля; Character Board = борд персонажа; Location Board = борд локации;
Object Board = борд объекта; Action Board = схема движения/трюка; Master Storyboard = главная
раскадровка; Segment Storyboard = раскадровка сегмента.

Про keyframes пользователю: если не обязателен — «отдельные ключевые кадры не делаем
автоматически; главным документом остаётся раскадровка сегмента»; если нужен точный still —
«нужен дополнительный визуальный якорь (например, точный экран телефона / packshot / крупный
план лица)»; если спрашивают что это — «ключевой кадр — статичный визуальный якорь, нужен не
всегда; для видео главным остаётся раскадровка».

Правила слоя: писать русские заголовки; не перегружать английскими терминами; объяснять зачем
этап нужен; писать «что делаем сейчас», а не только «что это значит»; чётко маркировать статус;
не писать концепт без действия; не писать английский статус без русского пояснения; не писать
«keyframes» как обязательный этап.
