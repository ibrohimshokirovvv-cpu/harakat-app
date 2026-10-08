# Harakat by Ibrokhim — запуск сайта

Всё уже готово и настроено. От вас нужно всего несколько кликов — ниже самый короткий путь.

## Что нужно от вас (неизбежные шаги — их нельзя сделать за вас)

Эти шаги требуют именно ваш аккаунт, потому что к ним привязывается оплата и владение:

1. **Аккаунт на console.anthropic.com** — создать API-ключ (2 минуты, нужен только email)
2. **Аккаунт на GitHub** (github.com) — бесплатно, нужен email
3. **Аккаунт на Render** (render.com) — можно войти через тот же GitHub-аккаунт, одним кликом
4. **Аккаунт на Netlify** (netlify.com) — тоже можно войти через GitHub

Дальше — всё по шагам, без необходимости писать код.

## Шаг 1. Получить API-ключ

1. Зайдите на https://console.anthropic.com → API Keys → Create Key
2. Скопируйте ключ (он показывается один раз, сохраните его куда-нибудь)
3. Привяжите карту для оплаты (тарификация по факту использования)

## Шаг 2. Загрузить код на GitHub (без терминала)

1. Зайдите на https://github.com → New repository → назовите, например, `harakat-app` → Create repository
2. На странице нового репозитория нажмите "uploading an existing file"
3. Перетащите туда ВСЕ файлы и папки из этого архива (сохраняя структуру: `backend/`, `frontend/`, `render.yaml`)
4. Внизу нажмите "Commit changes"

## Шаг 3. Задеплоить backend на Render — один клик

1. Зайдите на https://dashboard.render.com/blueprints
2. New Blueprint Instance → выберите ваш репозиторий `harakat-app`
3. Render сам найдёт файл `render.yaml` и настроит всё автоматически (команды сборки и запуска уже прописаны)
4. Когда попросит — вставьте ваш `ANTHROPIC_API_KEY` в поле переменной окружения
5. Нажмите Apply/Deploy — через пару минут Render даст вам адрес вида `https://harakat-backend.onrender.com`

## Шаг 4. Указать backend-адрес во frontend

1. На GitHub откройте файл `frontend/index.html` в вашем репозитории
2. Нажмите на иконку карандаша (Edit)
3. Найдите строку:
   ```js
   const API_BASE = "http://localhost:3000";
   ```
4. Замените на адрес из Render (без `/api/...` в конце, просто базовый адрес), например:
   ```js
   const API_BASE = "https://harakat-backend.onrender.com";
   ```
5. Commit changes

## Шаг 5. Задеплоить frontend на Netlify

1. Зайдите на https://app.netlify.com
2. Add new site → Import an existing project → GitHub → выберите репозиторий `harakat-app`
3. В настройках сборки укажите:
   - Base directory: `frontend`
   - Build command: оставьте пустым
   - Publish directory: `frontend`
4. Deploy site — через минуту Netlify даст вам ссылку вида `https://harakat-by-ibrokhim.netlify.app`

**Готово — сайт работает по этой ссылке, доступен всем.**

## Шаг 6. Свой домен (по желанию)

Купите домен (Namecheap, Reg.ru) и привяжите в настройках Netlify: Site settings → Domain management → Add custom domain. Netlify покажет, какие записи добавить у регистратора домена.

## Если что-то не заработало

- Backend не отвечает → в Render откройте вкладку Logs у вашего сервиса, посмотрите на ошибку
- Сайт грузится, но при вводе текста ошибка → проверьте, что в `frontend/index.html` правильно указан `API_BASE` (шаг 4), и что переменная `ANTHROPIC_API_KEY` действительно задана в Render (шаг 3)
- Пришлите мне текст ошибки из логов — помогу разобраться

## На будущее (ещё не делали, но держим в уме)

- Скачивание мобильного приложения через сайт
- Платная подписка
