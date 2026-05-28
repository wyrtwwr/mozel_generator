# MozLe Generator

## Что это
Веб-приложение с пошаговым генератором мозаики (конструктор от выбора набора до финальной инструкции).
Проект полностью фронтендовый, без сборщика и без подключенного backend API.

## На каком языке написан генератор
- `HTML5` — структура страниц каждого шага
- `CSS3` — стили, адаптив, единые кнопки
- `Vanilla JavaScript (ES6+)` — вся логика шагов, валидации, переходы, работа с хранилищем

То есть генератор мозаики написан на чистом `HTML/CSS/JavaScript`.

## Структура файлов
```text
mozel_generator/
  index.html                    # ввод кода доступа
  generate_step2.html           # выбор размера набора и количества
  generate_step3.html           # выбор формата полотна
  generate_step4.html           # загрузка фото
  generate_step5.html           # подсказки перед редактором
  generate_step6.html           # редактор (кадрирование/масштаб/поворот)
  generate_step7.html           # предпросмотр и советы по улучшению
  generate_step8.html           # выбор опций/режимов
  generate_step9.html           # финальная проверка
  generate_step10.html          # ввод e-mail и переход к инструкции
  instructions_step1.html       # экран инструкции
  instructions_step2.html       # экран инструкции
  README.md

  style/
    style_generate.css
    style_generate_1000.css
    style_generate_buttons_unified.css
    style_generate_1000_step2.css
    style_generate_1000_step3.css
    style_generate_1000_step4.css
    style_generate_1000_step5.css
    style_generate_1000_step6.css
    style_generate_1000_step7.css
    style_generate_1000_step8.css
    style_generate_1000_step9.css
    style_generate_1000_step10.css

  img/
    ...                          # статические изображения интерфейса
    mosaic/                      # изображения мозаики
    mosaicactive/                # активные варианты
    mosaicafter/                 # варианты после обработки
```

## Как строится логика (по шагам)
1. `index.html` — пользователь вводит 9-значный код, после валидации открывается шаг 2.
2. `generate_step2.html` — выбор размера набора (`S/M/L/XL`) и количества наборов; данные сохраняются в `localStorage`.
3. `generate_step3.html` — выбор формата полотна (`2x2`, `3x4` и т.д.) по правилам доступности для выбранного набора.
4. `generate_step4.html` — загрузка фотографии; файл сохраняется в `IndexedDB` (fallback в `localStorage`).
5. `generate_step5.html` — информационный промежуточный шаг.
6. `generate_step6.html` — редактор фото: кадрирование, зум, поворот, ориентация; сохраняется отредактированный снимок.
7. `generate_step7.html` — предпросмотр текущего изображения и подсказки.
8. `generate_step8.html` — выбор визуальных режимов/вариантов предпросмотра.
9. `generate_step9.html` — финальный предпросмотр перед инструкцией.
10. `generate_step10.html` — ввод e-mail, затем переход в `instructions_step1.html`.

## Где хранится состояние
- `localStorage`:
  - выбранные параметры (`mozle_set_size`, `mozle_set_count`, `mozle_canvas_size` и др.)
  - служебные и fallback-данные
  - e-mail (`mozle_instruction_email`)
- `IndexedDB` (`mozle_step_storage`, store `files`):
  - `step4_original_image` — исходная загрузка
  - `step6_edited_image` — результат после редактора

## Примечание
Сейчас отправка email и серверная генерация результата работают как заглушки на фронтенде; backend-интеграция не подключена.
