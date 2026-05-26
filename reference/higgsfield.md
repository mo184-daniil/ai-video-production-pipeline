# Higgsfield — специфичный слой (опционально)

Читать **только если работаем внутри Higgsfield**. Если платформа другая — этот файл не
нужен, метод полностью работает на модель-агностичном ядре (`SKILL.md` + `manual.md`).

Higgsfield не отменяет пайплайн: борды → раскадровка → keyframes (если нужны) → Shot/Action
Board → Seedance Fast → Seedance 2.0. Здесь — какой специализированный workflow выбрать и
как с ним работать.

## Оглавление

1. Router — какой workflow выбирать
2. Default-модели
3. Media Roles
4. Marketing Studio Video (modes / hooks / ad reference)
5. Brand Kits
6. Products и Product URL
7. DTC Ads Engine
8. Product Photoshoot
9. Marketplace Cards
10. Soul Character / Identity
11. Virality Predictor
12. Prompt Engineering Rules
13. Troubleshooting / Validation

---

## 1. Router — какой workflow выбирать

| Задача | Workflow | Почему |
|---|---|---|
| Сложное custom видео: cinematic, action, story, image-to-video | Seedance 2.0 pipeline | Основной all-purpose video для production |
| Реклама: UGC, TV spot, product demo, unboxing, presenter | Marketing Studio Video | Спец. рекламный workflow (mode/avatar/product/hooks) |
| Брендовая продуктовая фото, lifestyle, hero banner, carousel | Product Photoshoot | Enhancer под продуктовые visuals |
| Marketplace карточки, listing, A+, инфографика | Marketplace Cards | Workflow под карточки и compliance |
| DTC ad image с brand kit / avatar / product | DTC Ads Engine | Форматная реклама, ad format обязателен |
| Анализ hook / attention / virality готового видео | Virality Predictor | Анализ, не генерация |
| Стабильное лицо человека в серии | Soul Character / Soul ID | Тренировка identity |
| Простое изображение без продукта/рекламы | GPT Image 2 / Nano Banana / Soul / Flux | По типу картинки |
| Простая анимация одного кадра | Kling / Hailuo / Krea | Быстро и дешевле |
| Сложная логика / action / story | Seedance 2.0 Fast → 2.0 | Основной production-путь |

Если реклама/UGC/unboxing/product demo/TV spot/presenter — сначала рассмотреть Marketing
Studio Video. Если кастомная кинематографичная сцена/сложный action/художественный ролик —
основной путь: boards → keyframes → Shot/Action Board → Seedance Fast → Seedance 2.0.

---

## 2. Default-модели

```text
Images / design / text:            GPT Image 2
Character / cartoon / reference:   Nano Banana 2 / Nano Banana Pro
Serious / production video:        Seedance 2.0
Ads / UGC / product / branded:     Marketing Studio Video
Product photoshoot:                Product Photoshoot workflow (не прямой GPT Image prompt)
Marketplace listing:               Marketplace Cards workflow
Video analysis:                    Virality Predictor
```

---

## 3. Media Roles

| Модель | Роли |
|---|---|
| Seedance 2.0 | `image`, `start_image`, `end_image`, `video`, `audio` |
| Kling 3.0 / 2.6 | `start_image`, `end_image` / `start_image` |
| Veo 3.1 / Veo 3 | `start_image` (≤1 ref) / `image` (≤1 ref) |
| Marketing Studio Video | `image`, `start_image`, `end_image` + avatars/product_ids отдельно |
| Virality Predictor | `video` |
| Z-Image / Soul Cast / Soul Location | media не принимает, prompt-only |

Для Seedance аудио передавать как media role `audio` (не отдельным параметром). Upload Map v2:
Image 1 = Style Board; 2 = Character Board; 3 = Location Board; 4 = Object/Prop Board;
5 = Keyframe; 6 = Shot Board; 7 = Action Board (если есть); Video 1 = motion ref; Audio 1 = rhythm/voice/ambience.

---

## 4. Marketing Studio Video

Для рекламных видео, где важна структура объявления, продукт, аватар, UGC, TV spot.

| Mode | Когда | Hook/setting |
|---|---|---|
| `ugc` | обычный UGC «как будто снял человек» | да |
| `ugc_how_to` | tutorial / explainer | да |
| `ugc_unboxing` | unboxing reveal | да |
| `product_showcase` | чистый продуктовый highlight | нет |
| `product_review` | presenter даёт мнение | да |
| `tv_spot` | polished broadcast commercial | нет |
| `wild_card` | экспериментальный vibe | нет |
| `ugc_virtual_try_on` | человек примеряет вещь, UGC | да |
| `virtual_try_on` | polished virtual try-on | нет |

Hook и Setting — только в modes `ugc`, `ugc_how_to`, `ugc_unboxing`, `product_review`,
`ugc_virtual_try_on`. Нельзя — в `product_showcase`, `tv_spot`, `wild_card`, `virtual_try_on`.

**Ad Reference vs Hook/Setting — нельзя смешивать.** Выбрать один путь: reference-driven
(`ad_reference`) ИЛИ composed-from-blocks (`hook + setting`).

**Ad Reference** — reusable inspiration video. Использовать, если нужно повторить стиль/
структуру/pacing/монтаж/angle/hook удачного клипа или сделать серию по шаблону. Создаётся
только из local video file или previous generation job (не URL / не внешний TikTok/YouTube link
без локального файла). Usable только при `status = completed`.

---

## 5. Brand Kits

Brand Kit фиксирует brand name, logo, hero images, colours, fonts, tone, products, business
overview. Использовать для брендовой рекламы, DTC ad images, consistent brand visuals, серии
материалов, единого visual identity для сайта/соцсетей. Создаётся из website URL; если kit
failed — попросить другой URL или генерировать без него.

```text
Brand Kit ≠ Style Board. Brand Kit = брендовые данные. Style Board = визуальная режиссура проекта.
```

---

## 6. Products и Product URL

Продукт регистрируется: (1) URL fetch — если есть product/shop page; (2) Manual create — если
есть фото + title + description. Если URL fetch failed: проверить, не заблокирован ли scrape;
попросить фото и описание; создать вручную.

---

## 7. DTC Ads Engine

Для рекламных **изображений** (не видео). Ad format обязателен — нельзя запускать без format_id.
Порядок: (1) показать список доступных ad formats; (2) дать выбрать by name; (3) только потом
генерировать. Optional inputs (только если выбраны/предоставлены): brand kit, avatar, product,
reference media.

---

## 8. Product Photoshoot

Для брендовых продуктовых изображений. Сцены: `product_shot`, `lifestyle_scene`,
`closeup_product_with_person`, `moodboard_pin`, `hero_banner`, `social_carousel`,
`ad_creative_pack`, `virtual_model_tryout`, `conceptual_product`, `restyle`.

Использовать вместо обычного GPT Image prompt для: product photo, lifestyle, hero/banner,
carousel, paid social, Meta/TikTok/Pinterest ads, virtual try-on, model wearing product, person
holding product, closeup with hands, levitating/splash product, seasonal variation.

```text
Не писать финальный GPT Image 2 prompt вручную при Product Photoshoot — backend enhancer собирает prompt сам.
```

---

## 9. Marketplace Cards

Для marketplace listing images, compliant main image, secondary images, product infographics,
lifestyle listing shots, A+ modules, image sets. Scopes: `main` (1 main); `product-images`
(main + 5 secondary); `aplus` (main + 7 A+ modules); `full-set` (main + 5 secondary + 7 A+).
Не использовать для обычной брендовой фотосессии, видео или UGC ads.

---

## 10. Soul Character / Identity

Для стабильного лица конкретного человека в серии фото/видео (founder в рекламе, employee
avatar, digital twin, «моё лицо», reusable identity).

Фото: минимум 5, максимум 20, оптимально 8–12. Нужно: clear face, eyes visible, single person,
разные углы/свет/выражения, sharp, ≥1024×1024. Избегать: group photos, sunglasses, heavy
filters, повтор одной позы, шляпы, костюмы.

```text
Soul training → Soul reference id → Soul 2.0 / Soul Cinema stills → Character Board → Keyframes → Seedance / Marketing Studio.
```

Honesty: не заявлять, что Soul ID готов, если training failed/не завершился/подменён (см.
`manual.md` §18). Не тренировать на AI-портретах, если цель — реальный человек.

---

## 11. Virality Predictor

Анализ готового видео (prompt не нужен, подаётся один video file). Когда: оценить hook,
virality potential, attention, retention, distraction risk, сравнить creative versions.
Возвращать пользователю: overall score, peak hook second, sustain score, strongest region,
weakest/risk areas, business interpretation, ссылку на отчёт (если есть).

```text
Final render → Virality Predictor → creative diagnosis → итерация hook / montage / first 3 sec / CTA.
```

---

## 12. Prompt Engineering Rules

Базовая структура: subject + setting + style → camera → lighting → motion → mood → quality.

- **image-to-image:** описывать, что меняется, не пересказывать всё. Плохо: «a man with brown
  hair in leather jacket holding coffee, made into anime». Лучше: «transform into anime style,
  vibrant colors, soft cel shading».
- **image-to-video:** start image задаёт статику, prompt описывает движение («Camera slowly
  dollies in. The subject turns toward camera. Scarf moves in the wind»). Не описывать заново
  детали кадра.
- **Длина:** ~200 tokens для простых задач; длиннее — для production bible / Seedance timecoded
  scenes, но со ясной структурой.
- **Negative phrasing:** если нет отдельного negative_prompt — формулировать позитивно («tack
  sharp, clean background, stable camera, consistent character»). В production bible можно
  хранить negative rules как чеклист.

---

## 13. Troubleshooting / Validation

| Ошибка | Что делать |
|---|---|
| Missing required params: prompt / medias | запросить prompt / передать нужное media |
| Invalid values | выбрать из allowed enum |
| Unknown params | проверить schema через model get |
| Session expired / Not authenticated | нужна авторизация |
| nsfw / ip_detected | переформулировать / убрать рискованный контент |
| timeout / HTTP 429 / captcha | увеличить timeout, backoff, подождать и повторить |

Если важна стоимость — сначала cost estimate, не запускать без согласования. Schema rule:
не уверены, что модель принимает параметр — сначала `model get`, потом запуск.
