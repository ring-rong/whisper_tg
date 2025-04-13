# Whisper Telegram Bot  

Телеграм-бот для расшифровки аудиосообщений, голосовых сообщений и видеосообщений с использованием модели OpenAI Whisper, размещённой на Hugging Face Inference API.  

## Возможности  

* Расшифровывает аудиофайлы (`.ogg`, `.mp3` и др.) и голосовые сообщения.  
* Извлекает аудио из видеосообщений (`.mp4` и др.) и расшифровывает его.  
* Использует модель `openai/whisper-large-v3-turbo` через API.  
* Обрабатывает длинные расшифровки, разбивая их на несколько сообщений.  
* Поддерживает базовые команды: `/start`, `/id`, `/help`.  
* Запускается в Docker-контейнере для удобного развёртывания.  

## Установка и настройка  

### Требования  

* [Docker](https://docs.docker.com/get-docker/)  
* [Docker Compose](https://docs.docker.com/compose/install/)  

### Шаги  

1. **Клонируйте репозиторий:**  
    ```bash  
    git clone <ваш-URL-репозитория>  
    cd <директория-репозитория>  
    ```  

2. **Настройте переменные окружения:**  
    Боту требуется токен Telegram Bot. Его нужно указать в файле `docker-compose.yml`:  
    ```yaml  
    # docker-compose.yml  
    services:  
      whispergram:  
        # ... другие настройки  
        environment:  
          - WHISPER_MIBOT_TOKEN=ВАШ_ТОКЕН_ТЕЛЕГРАМ_БОТА # Замените на реальный токен  
        # ... другие настройки  
    ```  
    *Замените `ВАШ_ТОКЕН_ТЕЛЕГРАМ_БОТА` на токен, полученный от BotFather в Telegram.*  

    **Важно:** Токен Hugging Face API в текущей версии жёстко прописан в `app.py` (`API_URL` и `headers`). Для безопасности рекомендуется вынести токен Hugging Face (`hf_...`) в переменную окружения, аналогично Telegram-токену. Для этого нужно изменить `app.py`, чтобы он читал токен через `os.getenv()`, и добавить его в `docker-compose.yml` в раздел `environment`.  

3. **Соберите и запустите через Docker Compose:**  
    ```bash  
    docker-compose up --build -d  
    ```  
    Эта команда соберёт Docker-образ (если он ещё не собран) и запустит контейнер с ботом в фоновом режиме. Параметр `restart: always` в `docker-compose.yml` гарантирует, что бот автоматически перезапустится при остановке.  

## Использование  

1. Начните чат с ботом в Telegram.  
2. Отправьте аудиофайл, голосовое сообщение или видеофайл.  
3. Бот ответит «Обработка аудио...» и отправит расшифрованный текст.  
4. Если текст превышает лимит Telegram (~4000 символов), он будет разбит на несколько сообщений.  

### Доступные команды  

* `/start` — приветственное сообщение.  
* `/id` — показывает ID чата и пользователя.  
* `/help` — краткая справка (на русском).  

## Конфигурация  

* `WHISPER_MIBOT_TOKEN` — токен Telegram-бота (указывается в `docker-compose.yml`).  
* `API_URL` — эндпоинт Hugging Face Inference API для модели Whisper (прописан в `app.py`).  
* `headers` — содержит токен авторизации Hugging Face API (прописан в `app.py` — **рекомендуется вынести в переменные окружения**).  

## Зависимости  

Основные зависимости Python перечислены в `requirements.txt` и устанавливаются через `Dockerfile`:  

* `aiogram` — асинхронный фреймворк для Telegram Bot API.  
* `moviepy` — для работы с видео (используется для извлечения аудио).  
* `requests` — для HTTP-запросов к API Whisper.  

*(Убедитесь, что ваш файл `requirements.txt` включает как минимум эти пакеты.)*

---------

# Whisper Telegram Bot

A Telegram bot that transcribes audio messages, voice messages, and video messages using the OpenAI Whisper model hosted on Hugging Face Inference API.

## Features

*   Transcribes audio (`.ogg`, `.mp3`, etc.) and voice messages.
*   Extracts audio from video messages (`.mp4`, etc.) and transcribes it.
*   Uses the `openai/whisper-large-v3-turbo` model via API.
*   Handles long transcriptions by splitting them into multiple messages.
*   Provides basic commands: `/start`, `/id`, `/help`.
*   Runs in a Docker container for easy deployment.

## Setup and Installation

### Prerequisites

*   [Docker](https://docs.docker.com/get-docker/)
*   [Docker Compose](https://docs.docker.com/compose/install/)

### Steps

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd <repository-directory>
    ```

2.  **Configure Environment Variables:**
    The bot requires a Telegram Bot Token. You need to set this in the `docker-compose.yml` file:
    ```yaml
    # docker-compose.yml
    services:
      whispergram:
        # ... other settings
        environment:
          - WHISPER_MIBOT_TOKEN=YOUR_TELEGRAM_BOT_TOKEN # Replace with your actual token
        # ... other settings
    ```
    *Replace `YOUR_TELEGRAM_BOT_TOKEN` with the token obtained from BotFather on Telegram.*

    **Security Note:** The Hugging Face API token is currently hardcoded in `app.py` (`API_URL` and `headers`). For better security, it is strongly recommended to move the Hugging Face token (`hf_...`) to an environment variable, similar to the Telegram token. You would need to modify `app.py` to read it using `os.getenv()` and update the `docker-compose.yml` to include it under the `environment` section.

3.  **Build and Run with Docker Compose:**
    ```bash
    docker-compose up --build -d
    ```
    This command will build the Docker image (if not already built) and start the bot container in detached mode. The `restart: always` policy in `docker-compose.yml` ensures the bot restarts automatically if it stops.

## Usage

1.  Start a chat with your bot on Telegram.
2.  Send an audio file, voice message, or video file to the chat.
3.  The bot will reply with "Обработка аудио..." (Processing audio...) and then send the transcribed text.
4.  If the text is longer than Telegram's message limit (approx. 4000 characters), it will be split into multiple messages.

### Available Commands

*   `/start`: Displays a start message.
*   `/id`: Shows your chat ID and user ID.
*   `/help`: Provides a brief description (in Russian).

## Configuration

*   `WHISPER_MIBOT_TOKEN`: Your Telegram Bot token (set in `docker-compose.yml`).
*   `API_URL`: The Hugging Face Inference API endpoint for the Whisper model (currently hardcoded in `app.py`).
*   `headers`: Contains the Hugging Face API authorization token (currently hardcoded in `app.py` - **move to environment variable for security**).

## Dependencies

The main Python dependencies are listed in `requirements.txt` and installed via the `Dockerfile`:

*   `aiogram`: Asynchronous framework for Telegram Bot API.
*   `moviepy`: For video editing (used here to extract audio).
*   `requests`: For making HTTP requests to the Whisper API.

*(Ensure your `requirements.txt` file includes at least these packages)*
