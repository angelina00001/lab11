Авдошина Ангелина Евгеньевна, группа 221331, Лабораторная работа №11, Вариант 8

Задания средней сложности:

## Выполненные задачи

### Средняя сложность

M1.  **Dockerfile для Go-приложения:**
Реализована **многоэтапная сборка** (`builder` → `scratch`).
`CGO_ENABLED=0` для получения статически скомпилированного бинарного файла.
**Финальный размер образа: 7.76 МБ** (менее 15 МБ по заданию).

M2.  **Healthcheck для каждого сервиса:**
Во все Dockerfile (`go`, `python`, `rust`) добавлена инструкция `HEALTHCHECK`.
Для проверки используется встроенный HTTP-эндпоинт `/health`, который возвращает `{"status": "healthy"}`.
**Параметры:** `interval=30s`, `timeout=3s`, `retries=3`.

M3.  **Переменные окружения:**
Все сервисы считывают порт из переменной `PORT` (значение по умолчанию: 8080 для Go/Rust, 8000 для Python).
Добавлена поддержка режима `DEBUG`.

### Повышенная сложность

B1.  **Rust-приложение с поддержкой `musl`:**
Выполнена **полностью статическая сборка** с целевой платформой `x86_64-unknown-linux-musl`.
Финальный образ собран на основе `alpine:latest` (содержит `curl` для `HEALTHCHECK`).
**Финальный размер образа: 3.76 МБ.**

B2.  **Автоматическое обновление контейнеров (Watchtower):**
В `docker-compose.yml` добавлен сервис `watchtower`.
Настроен интервал проверки обновлений: `60` секунд.
Включена автоматическая очистка старых образов (`CLEANUP=true`).


## Старт

1.  **Клонируйте репозиторий:**
    ```bash
    git clone https://github.com/MunyTa/Lab-11.git
    cd LR-11
Запустите все сервисы:

bash
docker-compose up --build -d
Проверьте работоспособность:

bash
# Статус контейнеров (все должны быть healthy)
docker-compose ps

# Проверка эндпоинтов
curl http://localhost:8080/health  # Go
curl http://localhost:8000/health  # Python
curl http://localhost:8081/health  # Rust
Остановите сервисы:

bash
docker-compose down
Результаты оптимизации образов
Сервис	Размер образа
Go	7.76 МБ
Rust	3.76 МБ
Python	207 МБ
Используемые технологии
Go: Dockerfile (multi-stage, scratch), net/http

Python: Dockerfile (multi-stage, slim), FastAPI / uvicorn

Rust: Dockerfile (multi-stage, musl, alpine), actix-web

Оркестрация: Docker Compose

Автообновление: Watchtower

Методология: Agentic Engineering (TDD, атомарные коммиты)
