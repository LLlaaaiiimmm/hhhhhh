# 🚀 Бесплатный деплой сайта ФК "Александрія"

## Шаг 1: MongoDB Atlas (5 минут) - БЕСПЛАТНО

1. Зайди на https://www.mongodb.com/cloud/atlas/register
2. Создай аккаунт (можно через Google)
3. Выбери **FREE** план (M0 Sandbox)
4. Выбери регион (например, Frankfurt или Amsterdam - ближе к Украине)
5. Назови кластер: `alexandria-cluster`
6. Нажми **Create Cluster** (подожди 3-5 минут)

### Настройка доступа:

7. Слева выбери **Database Access** → **Add New Database User**
   - Username: `alexandria_admin`
   - Password: придумай и **СОХРАНИ** (например: `Alex2024Secure!`)
   - Database User Privileges: **Read and write to any database**
   - Нажми **Add User**

8. Слева выбери **Network Access** → **Add IP Address**
   - Нажми **Allow Access from Anywhere** (добавит `0.0.0.0/0`)
   - Нажми **Confirm**

9. Вернись в **Database** → нажми **Connect** на своем кластере
   - Выбери **Connect your application**
   - Driver: **Python**, Version: **3.12 or later**
   - Скопируй connection string (будет вида):
   ```
   mongodb+srv://alexandria_admin:<password>@alexandria-cluster.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
   - **ВАЖНО**: Замени `<password>` на свой пароль!

**Сохрани этот connection string - он понадобится!**

---

## Шаг 2: Подготовка GitHub репозитория (2 минуты)

Если у тебя еще нет репозитория на GitHub:

```bash
# Инициализируй git (если еще не сделано)
git init

# Добавь все файлы
git add .

# Сделай коммит
git commit -m "Initial commit for deployment"

# Создай репозиторий на GitHub (зайди на github.com → New repository)
# Назови его: alexandria-fc

# Подключи удаленный репозиторий (замени YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/alexandria-fc.git

# Запуш код
git branch -M main
git push -u origin main
```

---

## Шаг 3: Деплой на Render.com (10 минут) - БЕСПЛАТНО

### 3.1 Регистрация

1. Зайди на https://render.com
2. Нажми **Get Started** → **Sign up with GitHub**
3. Авторизуй Render доступ к своим репозиториям

### 3.2 Деплой Backend

1. На дашборде Render нажми **New +** → **Web Service**
2. Найди и выбери свой репозиторий `alexandria-fc`
3. Настройки:
   - **Name**: `alexandria-fc-backend`
   - **Region**: Frankfurt (EU Central)
   - **Branch**: `main`
   - **Root Directory**: `backend`
   - **Runtime**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn server:app --host 0.0.0.0 --port $PORT`
   - **Instance Type**: **Free**

4. **Environment Variables** (нажми Add Environment Variable):
   - `MONGO_URL` = (твой connection string из Шага 1)
   - `DB_NAME` = `alexandria_fc_db`
   - `CORS_ORIGINS` = `*`
   - `JWT_SECRET_KEY` = `alexandria-fc-secret-key-2024-change-in-production`

5. Нажми **Create Web Service**
6. Подожди 5-10 минут (следи за логами)
7. **СОХРАНИ URL** (будет вида: `https://alexandria-fc-backend.onrender.com`)

### 3.3 Деплой Frontend

1. Снова нажми **New +** → **Static Site**
2. Выбери тот же репозиторий `alexandria-fc`
3. Настройки:
   - **Name**: `alexandria-fc-frontend`
   - **Branch**: `main`
   - **Root Directory**: `frontend`
   - **Build Command**: `yarn install && yarn build`
   - **Publish Directory**: `build`

4. **Environment Variables**:
   - `REACT_APP_BACKEND_URL` = (URL backend из шага 3.2)
   - `REACT_APP_ENABLE_VISUAL_EDITS` = `false`
   - `ENABLE_HEALTH_CHECK` = `false`

5. Нажми **Create Static Site**
6. Подожди 5-10 минут

---

## Шаг 4: Обновление CORS (2 минуты)

После деплоя frontend:

1. Скопируй URL frontend (например: `https://alexandria-fc-frontend.onrender.com`)
2. Зайди в настройки **Backend** сервиса на Render
3. Найди переменную `CORS_ORIGINS`
4. Измени значение с `*` на URL frontend:
   ```
   https://alexandria-fc-frontend.onrender.com
   ```
5. Сохрани (сервис автоматически перезапустится)

---

## 🎉 Готово!

Твой сайт работает:
- **Frontend**: https://alexandria-fc-frontend.onrender.com
- **Backend API**: https://alexandria-fc-backend.onrender.com
- **Админка**: https://alexandria-fc-frontend.onrender.com/admin/login

### Данные для входа:
- Email: `fcoleksandria2133@fc.com`
- Пароль: `Jingle2018!!!`

---

## 📝 Важно знать:

### Бесплатный tier Render.com:
- ✅ Полностью бесплатно
- ⚠️ Сервисы "засыпают" после 15 минут неактивности
- ⚠️ Первый запрос после сна занимает ~30 секунд
- ✅ 750 часов работы в месяц (достаточно для одного сервиса 24/7)
- ✅ Автоматический деплой при push в GitHub

### MongoDB Atlas Free:
- ✅ 512 MB хранилища (достаточно для небольшого сайта)
- ✅ Shared RAM и CPU
- ✅ Без ограничений по времени

---

## 🔄 Как обновлять сайт:

Просто делай push в GitHub:

```bash
git add .
git commit -m "Update website"
git push
```

Render автоматически задеплоит изменения!

---

## ❓ Проблемы?

### Backend не запускается:
- Проверь логи в Render Dashboard
- Убедись, что `MONGO_URL` правильный (с замененным паролем)
- Проверь, что в MongoDB Atlas разрешен доступ с `0.0.0.0/0`

### Frontend не видит данные:
- Проверь `REACT_APP_BACKEND_URL` в настройках frontend
- Проверь `CORS_ORIGINS` в настройках backend
- Открой DevTools (F12) → Console для ошибок

### Сайт медленно загружается:
- Это нормально для бесплатного tier после "сна"
- Первый запрос занимает ~30 секунд
- Последующие запросы быстрые

### Ошибка авторизации:
- Проверь `JWT_SECRET_KEY` в backend
- Попробуй очистить cookies браузера

---

## 💡 Альтернативы (тоже бесплатно):

Если Render не подходит:

1. **Railway.app** - $5 бесплатных кредитов в месяц
2. **Fly.io** - бесплатный tier для небольших приложений
3. **Netlify** - для frontend (аналог Render Static Sites)
4. **Heroku** - больше не бесплатный ❌

---

## 🚀 Upgrade в будущем:

Когда сайт станет популярным:

- **Render Starter** ($7/мес) - сервис не засыпает
- **MongoDB M10** ($9/мес) - больше места и производительности
- **Cloudflare** - бесплатный CDN для ускорения

Удачи! ⚽
