# converter-code
# Claude Telegram Bot

Telegram-бот на базе `aiogram 3` и Anthropic Claude API. Принимает сообщения и файлы от пользователей, обрабатывает их через Claude и возвращает результат. Поддерживает whitelist пользователей, ограничение параллельных задач и автоматическую очистку временных файлов.

A Telegram bot built with `aiogram 3` and the Anthropic Claude API. It accepts messages and files from users, processes them via Claude, and returns the result. Features user whitelisting, concurrency limits, and automatic cleanup of temporary files.

---

## 🇷🇺 Русская версия

✨ Возможности

- 🤖 Интеграция с **Anthropic Claude API**
- 📎 Обработка файлов с временным хранилищем и авто-очисткой по TTL
- 🔐 **Whitelist** — доступ только для разрешённых `user_id`
- ⚡ Ограничение количества одновременных задач (`max_concurrent_conversions`)
- 🧹 Фоновый сборщик мусора: удаление просроченных файлов каждые 5 минут
- 🛡 Корректная обработка ошибок Telegram API и недействительного `BOT_TOKEN`
- 📝 Структурированное логирование с записью в файл
- 🔄 FSM на `MemoryStorage` (состояние сбрасывается при перезапуске — временные файлы тоже)

📋 Требования

- Python **3.10+**
- Токен Telegram-бота (получить у [@BotFather](https://t.me/BotFather))
- API-ключ [Anthropic](https://console.anthropic.com/)

🚀 Установка

1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/your-username/claude-telegram-bot.git
   cd claude-telegram-bot
   ```

2. Создайте и активируйте виртуальное окружение:
   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Linux/macOS
   .venv\Scripts\activate         # Windows
   ```

3. Установите зависимости:
   ```bash
   pip install -r requirements.txt
   ```

4. Создайте файл `.env` (см. раздел «Конфигурация»).

⚙️ Конфигурация

Создайте файл `.env` в корне проекта:

```env
BOT_TOKEN=123456:ABC-DEF...
ANTHROPIC_API_KEY=sk-ant-...

# Модель Claude (например, claude-sonnet-4-5)
CLAUDE_MODEL=claude-sonnet-4-5

# Максимум токенов в ответе
MAX_OUTPUT_TOKENS=4096

# Таймаут запроса к Claude, сек
REQUEST_TIMEOUT_SECONDS=120

# Сколько задач может обрабатываться одновременно
MAX_CONCURRENT_CONVERSIONS=3

# Время жизни временных файлов, минуты
TEMP_TTL_MINUTES=30

# Директория для временных файлов
TEMP_DIR=./tmp

# Директория для логов
LOG_DIR=./logs

# Уровень логирования: DEBUG / INFO / WARNING / ERROR
LOG_LEVEL=INFO

# Whitelist user_id через запятую. Пусто — доступ для всех.
ALLOWED_USER_IDS=123456789,987654321
```

▶️ Запуск

```bash
python main.py
```

После запуска в логах появится сообщение `Бот запущен: @your_bot_username`.

🗂 Структура проекта

```
.
├── main.py                 # Точка входа
├── bot/
│   ├── config.py           # Загрузка настроек из .env
│   ├── handlers.py         # Роутеры и хэндлеры
│   └── middlewares.py      # AccessMiddleware (whitelist)
├── services/
│   └── claude.py           # Обёртка над Anthropic API
├── utils/
│   ├── files.py            # TTL-очистка временных файлов
│   └── logging_setup.py    # Настройка логирования
└── requirements.txt
```

🔒 Безопасность

- Токены и ключи хранятся **только** в `.env` — не коммитьте его.
- Используйте `ALLOWED_USER_IDS`, чтобы ограничить доступ к боту.
- Временные файлы удаляются автоматически и при завершении работы.

📄 Лицензия

MIT — используйте свободно.

---

## 🇬🇧 English version

✨ Features

- 🤖 **Anthropic Claude API** integration
- 📎 File handling with TTL-based temporary storage
- 🔐 **Whitelist** — access restricted to allowed `user_id`s
- ⚡ Concurrency limit (`max_concurrent_conversions`)
- 🧹 Background garbage collector: purges expired files every 5 minutes
- 🛡 Graceful handling of Telegram API errors and invalid `BOT_TOKEN`
- 📝 Structured logging to file
- 🔄 FSM backed by `MemoryStorage` (state resets on restart — temp files are purged too)

📋 Requirements

- Python **3.10+**
- Telegram bot token (get one from [@BotFather](https://t.me/BotFather))
- [Anthropic](https://console.anthropic.com/) API key

🚀 Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/claude-telegram-bot.git
   cd claude-telegram-bot
   ```

2. Create and activate a virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Linux/macOS
   .venv\Scripts\activate         # Windows
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Create a `.env` file (see “Configuration”).

⚙️ Configuration

Create a `.env` file in the project root:

```env
BOT_TOKEN=123456:ABC-DEF...
ANTHROPIC_API_KEY=sk-ant-...

# Claude model (e.g. claude-sonnet-4-5)
CLAUDE_MODEL=claude-sonnet-4-5

# Max tokens in response
MAX_OUTPUT_TOKENS=4096

# Request timeout to Claude, seconds
REQUEST_TIMEOUT_SECONDS=120

# Max tasks processed concurrently
MAX_CONCURRENT_CONVERSIONS=3

# Temp file time-to-live, minutes
TEMP_TTL_MINUTES=30

# Directory for temporary files
TEMP_DIR=./tmp

# Directory for logs
LOG_DIR=./logs

# Log level: DEBUG / INFO / WARNING / ERROR
LOG_LEVEL=INFO

# Comma-separated user_id whitelist. Empty — everyone allowed.
ALLOWED_USER_IDS=123456789,987654321
```

▶️ Running

```bash
python main.py
```

Once started, the log will show `Бот запущен: @your_bot_username`.

🗂 Project Structure

```
.
├── main.py                 # Entry point
├── bot/
│   ├── config.py           # Loads settings from .env
│   ├── handlers.py         # Routers and handlers
│   └── middlewares.py      # AccessMiddleware (whitelist)
├── services/
│   └── claude.py           # Anthropic API wrapper
├── utils/
│   ├── files.py            # TTL cleanup of temp files
│   └── logging_setup.py    # Logging configuration
└── requirements.txt
```

🔒 Security

- Tokens and keys live **only** in `.env` — don’t commit it.
- Use `ALLOWED_USER_IDS` to restrict bot access.
- Temporary files are purged automatically and on shutdown.

📄 License

MIT — free to use.
