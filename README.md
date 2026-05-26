# README для backend-разработчика

## 1) Что это за проект
Фронтенд многошагового конструктора мозаики на чистом HTML/CSS/JS без сборки.

- Формат: статические страницы `index.html`, `generate_step2.html` ... `generate_step10.html`.
- Логика: inline-скрипты в каждом HTML-файле.
- Текущее состояние: backend API не подключен, данные держатся в `localStorage` и `IndexedDB`.

## 2) Структура проекта
```text
codex_generator1/
  index.html
  generate_step2.html
  generate_step3.html
  generate_step4.html
  generate_step5.html
  generate_step6.html
  generate_step7.html
  generate_step8.html
  generate_step9.html
  generate_step10.html
  style/
    style_generate.css
    style_generate_1000.css
    style_generate_1000_step2.css
    style_generate_1000_step3.css
    style_generate_1000_step4.css
    style_generate_1000_step5.css
    style_generate_1000_step6.css
    style_generate_1000_step7.css
    style_generate_1000_step8.css
    style_generate_1000_step9.css
    style_generate_1000_step10.css
    style_generate_buttons_unified.css
  img/
    ... статические изображения
```

## 3) Навигация по шагам
- `index.html` -> `generate_step2.html`
- `generate_step2.html` -> `generate_step3.html` или `generate_step4.html`
- `generate_step3.html` -> `generate_step4.html`
- `generate_step4.html` -> `generate_step5.html`
- `generate_step5.html` -> `generate_step6.html`
- `generate_step6.html` -> `generate_step7.html`
- `generate_step7.html` -> `generate_step8.html`
- `generate_step8.html` -> `generate_step9.html`
- `generate_step9.html` -> `generate_step10.html`

Особое условие:
- на шаге 2, если выбрано `S + 1 набор`, шаг 3 пропускается, а `mozle_canvas_size` принудительно ставится в `2x2`.

## 4) Файлы экранов и основные классы

### `index.html`
- Назначение: вход + проверка секретного кода.
- Основные классы:
  - `code-step`, `code-step__input`, `code-step__actions`
  - `step-btn`, `step-btn__main`, `step-btn__icon`
- Ключевые JS-функции:
  - `formatAndValidateCode`, `setNextStepState`, `setCodeStatus`

### `generate_step2.html` (выбор набора/количества)
- Корневой блок: `sets-step`
- Основные классы:
  - `sets-step__track`, `set-card`, `sets-count`, `sets-count__btn`
- Ключевые JS-функции:
  - `selectCard`, `selectCount`, `updateAvailableCounts`, `updateNextAvailability`
- Бизнес-правило: лимиты количества наборов зависят от размера (`S/M/L/XL`).

### `generate_step3.html` (выбор формата полотна)
- Корневой блок: `canvas-step`
- Основные классы:
  - `canvas-preview-card`, `canvas-step__schemes`, `canvas-size`, `canvas-size__btn`
- Ключевые JS-объекты/функции:
  - `FORMAT_RULES`, `DEFAULT_CANVAS_BY_SET`
  - `applyAvailableFormats`, `selectCanvas`, `updateSchemePreview`

### `generate_step4.html` (загрузка фото)
- Корневой блок: `upload-step`
- Основные классы:
  - `upload-box`, `upload-action`, `upload-step__actions`
- Ключевые JS-функции:
  - `saveImageToStorage`, `saveImageFallback`, `prepareStoredImage`
- Здесь сохраняется исходное фото (`step4_original_image`).

### `generate_step5.html` (подсказки)
- Основные классы:
  - `tips_step`, `kolobka1`, `img_step5_1`, `img_step5_2`
- Экран в основном информационный.

### `generate_step6.html` (редактор кадрирования)
- Корневой блок: `editor-step`
- Основные классы:
  - `editor-stage`, `editor-stage__viewport`, `editor-stage__image`, `editor-stage__grid`
  - `editor-tool`, `editor-hint`, `editor-slider`
- Ключевые JS-функции:
  - геометрия/ориентация: `updateStageRatio`, `renderGrid`, `applyOrientationState`
  - трансформации: `updateImageTransform`, `getPanBounds`, `handleImageDrag*`
  - сохранение снапшота: `buildEditedSnapshotBlob`, `persistEditedSnapshotNow`
- Здесь формируется отредактированное фото (`step6_edited_image`).

### `generate_step7.html` (превью + подсказки улучшений)
- Корневой блок: `enhance-step`
- Основные классы:
  - `enhance-step__image-wrap`, `enhance-step__markers`, `enhance-step__cards`
- Ключевые JS-функции:
  - `applyCanvasAspectRatio`, `getStoredImage`, `setPreviewSource`

### `generate_step8.html` (выбор варианта/режимов)
- Корневой блок: `select-step`
- Основные классы:
  - `select-step__preview`, `palette-row`, `option-row`, `thumbs-track`, `thumb-card`, `thumb-card__image`
- Ключевые JS-функции:
  - `applyCanvasAspectRatio`, `applyPreviewWindowSize`, `applyThumbAspectRatio`
  - `bindToggleGroups`, `bindThumbArrows`, `openHint/closeHint`

### `generate_step9.html` (финальная проверка)
- Корневой блок: `final-step`
- Основные классы:
  - `final-step__preview`, `final-step__notice`, `final-step__actions`
- Ключевые JS-функции:
  - `applyCanvasAspectRatio`, `getStoredImage`, `setPreviewSource`

### `generate_step10.html` (email)
- Корневой блок: `email-step`
- Основные классы:
  - `email-step__input`, `email-step__actions-row`, `email-step__skip`
- Ключевые JS-функции:
  - `isValidEmail`, `updateEmailState`, `showEmailValidationError`
- Сейчас отправка не подключена, стоят `alert`-заглушки.

## 5) Общие CSS-классы (shared)
Базовые классы лежат в `style/style_generate.css`:
- `code-step*` и `step-btn*` (переиспользуемые кнопки/контейнеры).
- `site-footer*`, `footer-*`, `logo_*` (общая шапка/подвал).

Дополнительно:
- `style/style_generate_buttons_unified.css` — унификация кнопок между шагами.
- `style/style_generate_1000_stepX.css` — адаптив и специфика конкретного шага.

## 6) JS-классы (ES6 class)
В проекте нет `class Foo { ... }`.

- Логика реализована через функции, объекты-константы и DOM-классы CSS.
- Если нужен переход на OOP/модульную архитектуру, нужно выделять отдельные JS-файлы.

## 7) Хранилище состояния (`localStorage`)
Основные ключи:

| Ключ | Кто пишет | Кто читает | Назначение |
|---|---|---|---|
| `mozle_set_size` | step2 | step2, step3, step4 | Размер набора (`S/M/L/XL`) |
| `mozle_set_index` | step2 | step2 | Индекс карточки набора |
| `mozle_set_count` | step2 | step2, step3, step4 | Количество наборов |
| `mozle_canvas_size` | step2/step3 | step3, step6, step7, step8, step9 | Формат полотна (`2x3`, `3x4`...) |
| `mozle_uploaded_image_name` | step4/step6 | step4, step6, step7, step8, step9 | Имя исходного файла |
| `mozle_uploaded_image_data` | step4/step6 fallback | step4, step6, step7, step8, step9 | Base64 fallback исходного изображения |
| `mozle_edited_image_data` | step6 fallback | step7, step8, step9 | Base64 fallback отредактированного изображения |
| `mozle_editor_orientation` | step6 | step6, step7, step8, step9 | `landscape/portrait` |
| `mozle_editor_zoom` | step6 | step6 | Уровень зума редактора |
| `mozle_editor_rotate` | step6 | step6 | Поворот редактора |
| `mozle_instruction_email` | step10 | step10 | Email пользователя |

Примечание:
- В `step6` удаляются ключи `mozle_editor_hint_orientation_closed` и `mozle_editor_hint_replace_closed` (legacy-очистка).

## 8) Хранилище изображений (`IndexedDB`)
DB-конфигурация единая:
- DB name: `mozle_step_storage`
- version: `1`
- object store: `files` (`keyPath: id`)

Используемые записи:
- `step4_original_image` — исходное фото после загрузки.
- `step6_edited_image` — результат после кадрирования/трансформаций на шаге 6.

Формат записи:
```json
{
  "id": "step4_original_image | step6_edited_image",
  "blob": "<Blob>",
  "name": "filename.jpg",
  "type": "image/jpeg",
  "savedAt": 1710000000000
}
```

## 9) Точки подключения backend API
Сейчас сеть не используется. Рекомендуемые точки интеграции:

1. `index.html`:
   - заменить `validCode = "111111111"` на API-валидацию кода доступа.

2. `generate_step4.html`:
   - опционально загружать исходное изображение на сервер и получать `image_id`.

3. `generate_step6.html`:
   - отправлять параметры кадрирования/поворота/ориентации или готовый blob.
   - сохранять серверный `edited_image_id`.

4. `generate_step8.html`:
   - передавать выбранные режимы (`turbo/tone/frame`), палитру и выбранную миниатюру.

5. `generate_step9.html`:
   - запуск генерации итоговой инструкции/мозаики.

6. `generate_step10.html`:
   - заменить `alert` на вызов API отправки email.

## 10) Быстрый старт для разработки
1. Открыть `index.html` в браузере.
2. Пройти флоу до нужного шага.
3. Проверять состояние в DevTools:
   - `Application -> Local Storage`
   - `Application -> IndexedDB -> mozle_step_storage`

