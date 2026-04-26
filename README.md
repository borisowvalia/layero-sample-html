# layero-sample-html

Чистый HTML без сборщика и `package.json`. Минимальный multi-page сайт для тестирования раздачи статики на [Layero](https://layero.ru).

## Quick facts

- **Framework**: нет (vanilla HTML/CSS/JS)
- **Build command**: нет — деплоится как есть
- **Output directory**: `.` (корень репо)
- **Node**: не требуется

## Структура

```
/index.html       главная
/about.html       вторая страница
/contact.html     третья страница
/404.html         кастомная 404
/styles.css
/main.js
/logo.svg
```

## Что демонстрирует

1. **Простой случай раздачи**: ассеты, относительные/абсолютные ссылки, корректные MIME-типы (`text/html`, `text/css`, `application/javascript`, `image/svg+xml`).
2. **Multi-page без JS-роутера**: `/about.html` и `/contact.html` — отдельные HTML-файлы. Для них SPA-fallback на `/index.html` не нужен и **не должен** срабатывать (мы хотим именно эти страницы).
3. **Кастомная 404**: `/404.html` — пользователь ожидает, что несуществующий путь покажет её. На Layero сегодня edge nginx переписывает любой 4xx → корневой `/index.html`, поэтому `/404.html` **не показывается**. Это ограничение нужно снимать через `project_type: static` в плане работ.

## Deploy on Layero

1. Создать проект, указать этот репо.
2. Build: оставить пустым (или `echo "no build"`).
3. `output_dir`: `.` (корень).

Сейчас сборщик ожидает Node-проекта — может ругнуться, что нет `package.json`. Это **отдельный пробел детектора**: должен быть «Static / no-build» режим, который просто заливает содержимое репо в S3.

## Local verify

```bash
npx serve .
# или
python3 -m http.server 8000
```

Открыть `http://localhost:8000/`, проверить переходы по ссылкам и заход на несуществующий путь (должен показать `/404.html`).
