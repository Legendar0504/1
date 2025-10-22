# 🐘 Настройка PostgreSQL в Docker для сайта

## 📋 Что создано

Я настроил полную систему для работы с PostgreSQL в Docker контейнере:

### 🐳 Docker файлы:
- `docker-compose.yml` - Конфигурация всех сервисов
- `Dockerfile.php` - Образ PHP с расширениями PostgreSQL
- `nginx.conf` - Конфигурация Nginx с поддержкой PHP

### 🗄️ База данных:
- `database_postgresql.sql` - SQL скрипт для создания БД
- `api/config.php` - Конфигурация подключения к PostgreSQL

### 🔌 API файлы:
- `api/register.php` - Регистрация пользователей
- `api/login.php` - Вход в систему
- `api/check-auth.php` - Проверка аутентификации
- `api/logout.php` - Выход из системы
- `api/test-db.php` - Тестирование подключения

### 🎯 JavaScript:
- `js/api-auth.js` - Клиентская часть для работы с API

## 🚀 Запуск системы

### 1. Запуск Docker контейнеров

```bash
# Остановить существующие контейнеры (если есть)
docker-compose down

# Собрать и запустить все сервисы
docker-compose up --build -d
```

### 2. Проверка работы контейнеров

```bash
# Посмотреть статус контейнеров
docker-compose ps

# Посмотреть логи
docker-compose logs
```

### 3. Тестирование подключения к БД

Откройте в браузере: `http://localhost:8080/api/test-db.php`

Должен вернуться JSON ответ:
```json
{
  "success": true,
  "message": "Подключение к PostgreSQL успешно",
  "version": "PostgreSQL 15.x..."
}
```

## 🔧 Настройка базы данных

### Автоматическая инициализация

База данных создается автоматически при первом запуске контейнера PostgreSQL. SQL скрипт `database_postgresql.sql` выполняется автоматически.

### Ручная настройка (если нужно)

```bash
# Подключиться к контейнеру PostgreSQL
docker exec -it my_postgres psql -U admin -d kdmsenter_db

# Или выполнить SQL файл
docker exec -i my_postgres psql -U admin -d kdmsenter_db < database_postgresql.sql
```

## 🧪 Тестирование системы

### Тестовые данные:
- **Email:** admin@kdmsenter.ru
- **Пароль:** admin2024

### Проверьте:
1. Откройте `http://localhost:8080`
2. Перейдите на страницу регистрации
3. Зарегистрируйтесь как новый пользователь
4. Войдите в систему
5. Проверьте личный кабинет

## 🛠️ Устранение проблем

### Ошибка "Connection refused"
```bash
# Проверьте, что контейнеры запущены
docker-compose ps

# Перезапустите контейнеры
docker-compose restart
```

### Ошибка "Database does not exist"
```bash
# Подключитесь к PostgreSQL и создайте БД
docker exec -it my_postgres psql -U admin
CREATE DATABASE kdmsenter_db;
\q
```

### Ошибка PHP расширений
```bash
# Пересоберите PHP контейнер
docker-compose build php
docker-compose up -d
```

### Проблемы с правами доступа
```bash
# Исправьте права на файлы
chmod -R 755 .
chmod -R 777 api/
```

## 📊 Мониторинг

### Просмотр логов
```bash
# Логи всех сервисов
docker-compose logs

# Логи конкретного сервиса
docker-compose logs db
docker-compose logs php
docker-compose logs web
```

### Подключение к базе данных
```bash
# Через psql
docker exec -it my_postgres psql -U admin -d kdmsenter_db

# Через pgAdmin (если установлен)
# Host: localhost
# Port: 5432
# Database: kdmsenter_db
# Username: admin
# Password: daniil05042012
```

## 🔒 Безопасность

### Рекомендации для продакшена:
1. Измените пароли в `docker-compose.yml`
2. Используйте переменные окружения для паролей
3. Настройте SSL/TLS для PostgreSQL
4. Ограничьте доступ к порту 5432
5. Регулярно обновляйте образы Docker

### Переменные окружения (опционально)
Создайте файл `.env`:
```env
POSTGRES_USER=admin
POSTGRES_PASSWORD=your_secure_password
POSTGRES_DB=kdmsenter_db
```

## 📁 Структура проекта

```
project/
├── docker-compose.yml          # Конфигурация Docker
├── Dockerfile.php             # Образ PHP
├── nginx.conf                 # Конфигурация Nginx
├── database_postgresql.sql    # SQL скрипт БД
├── api/
│   ├── config.php            # Конфигурация БД
│   ├── register.php          # API регистрации
│   ├── login.php             # API входа
│   ├── check-auth.php        # API проверки аутентификации
│   ├── logout.php            # API выхода
│   └── test-db.php           # Тест подключения
├── js/
│   └── api-auth.js           # Клиентская часть
└── [остальные файлы сайта]
```

## 🎯 Результат

После настройки у вас будет:
- ✅ PostgreSQL 15 в Docker контейнере
- ✅ PHP 8.2 с расширениями PostgreSQL
- ✅ Nginx веб-сервер
- ✅ Полнофункциональная система регистрации
- ✅ Безопасное хранение данных
- ✅ Современная архитектура

## 📞 Поддержка

Если возникли проблемы:
1. Проверьте логи Docker: `docker-compose logs`
2. Убедитесь, что все порты свободны
3. Проверьте подключение к БД: `http://localhost:8080/api/test-db.php`
4. Проверьте консоль браузера на ошибки JavaScript




