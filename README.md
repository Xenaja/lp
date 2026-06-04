# Лендинг Ксении Телегиной

Личный одностраничный лендинг: маркетинг + разработка в одних руках. Тёмная техно-тема.

**Живой сайт:** https://xenaja.github.io/lp/

---

## Стек

- **Один статический HTML-файл** (`index.html`) — CSS и JS внутри, без фреймворков.
- **Mobile-first**, брейкпоинты 640 / 860 / 1024px.
- **Шрифты:** Familjen Grotesk (заголовки), Hanken Grotesk (текст), IBM Plex Mono (mono-акценты) — Google Fonts.
- **Хостинг:** GitHub Pages, ветка `main`, репозиторий `Xenaja/lp`.

## Структура

```
index.html              — сайт (главная страница)
privacy.html            — политика конфиденциальности (152-ФЗ)
cases/                  — страницы-кейсы
  telo-pomnit.html      — кейс приложения «Тело помнит» (опубликован)
  fuelmonitor.html      — кейс приложения FuelMonitor (опубликован)
  _template.html        — шаблон кейса (не публикуется)
  kochevnik/kutkevich/  — заготовки кейсов лендингов (не публикуются, в .gitignore)
  polina/anastasia.html
visual/
  me-loop.mp4           — видео в блоке «обо мне» + me-poster.jpg
  cases/                — скриншоты и обложки работ (.jpg / .webp)
tools/capture.js        — скрипт скриншотов (puppeteer-core + Edge); НЕ часть сайта
FORM-SETUP.md           — инструкция по форме (Cloudflare Worker → Telegram)
telegram-form-worker.js — код воркера для формы
TODO.md                 — список задач (не публикуется)
```

`.gitignore` исключает из публикации `tools/`, черновики, внутренние доки и исходники
скриншотов. Кейс-страницы лендингов/ботов пока скрыты — публикуются по мере готовности
(см. строки-исключения `!/cases/...` в `.gitignore`).

## Как обновлять сайт

```bash
git add -A
git commit -m "что поменяли"
git push
```
Через ~1 минуту GitHub Pages автоматически пересоберётся.

## Скриншоты для портфолио

```bash
node tools/capture.js
```
Снимает полностраничные скриншоты сайтов из списка внутри скрипта (Edge в headless,
с догрузкой ленивых картинок и прокруткой). Дальше оптимизация в WebP/JPG через ImageMagick.
`tools/` и исходные PNG в репозиторий не попадают.

## Форма заявки

Подключена и работает: форма → **Cloudflare Worker** `lead-form` (`lead-form.xenonline77.workers.dev`)
→ Telegram-бот. Токен бота и `chat_id` лежат в секретах воркера (в коде сайта их нет).
CORS ограничен origin `https://xenaja.github.io`. На стороне формы — honeypot и чекбокс согласия.

Конфиг деплоя — в `worker/` (в `.gitignore`): `wrangler.toml` + установленный Wrangler.
- Обновить воркер: `cd worker && npx wrangler deploy`
- Поменять секреты: `npx wrangler secret bulk secrets.json` (файл `{"BOT_TOKEN":"...","CHAT_ID":"..."}`,
  после — удалить). **Важно:** не задавать секреты через `echo "x" | wrangler secret put` —
  PowerShell добавляет перевод строки и значение ломается; используйте `secret bulk`.

Исходник воркера — `telegram-form-worker.js`. Подробности — `FORM-SETUP.md`.

## 152-ФЗ

Форма собирает имя + контакт, поэтому:
- `privacy.html` — политика конфиденциальности (оператор: самозанятая Телегина К. В.; email kwag@yandex.ru);
- обязательный чекбокс согласия в форме;
- ссылка на политику в футере.

## Гайдлайны

Проект ведётся по внутреннему `LANDING-GUIDE.md` и скиллу `frontend-design` (`SKILL.md`):
один HTML-файл, mobile-first, один акцентный цвет, анимации только scroll-reveal + с уважением
к `prefers-reduced-motion`, доступность (skip-link, `:focus-visible`, кнопки 48px), без каруселей/таймеров.

Текущие задачи и план — в [TODO.md](TODO.md).
