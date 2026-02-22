# Тестовое Задание: Junior DevOps - Effective Mobile
Репозиторий содержит решение тестового задания по развертыванию простого веб-приложения с использованием Docker, Docker Compose и Nginx в качестве reverse-proxy.

## Использованные технологии
* **Docker & Docker Compose** — контейнеризация и оркестрация
* **Nginx (Alpine)** — веб-сервер / reverse-proxy
* **Python 3.11 (Alpine)** — минималистичный backend-сервер (http.server)

## Как запустить проект
Для запуска вам потребуются установленные `git`, `docker` и `docker-compose`.

1. Склонируйте репозиторий на вашу машину:
```bash
git clone https://github.com/ahmadjon-rajabov/em_devops_task.git
cd em_devops_task
```
2. Запустите контейнеры в фоновом режиме:
```bash
docker-compose up -d --build
```
3. Проверте результат:
```bash
docker ps -a
"Увидите  em-backend, em-nginx и их статусы"

curl http://localhost
Hello from Effective Mobile!!!
```

## Архитектура и схема взаимодействия
Взаимодействие между сервисами происходит исключительно внутри изолированной docker-сети `em_network`. Backend-сервис не имеет прямого доступа извне (порты не проброшены на хост), что обеспечивает безопасность. 
Nginx принимает внешние HTTP-запросы на порт 80 и проксирует их на backend-сервис (порт 8080), попутно обогащая запрос стандартными заголовками (`Host`, `X-Real-IP`, `X-Forwarded-For`).

### ASCII Схема:
```text
[ Пользователь / Browser ]
           |
           | HTTP (port 80)
           v
+-----------------------+       proxy_pass       +------------------------+
|        Nginx          | ---------------------> |       Backend          |
|  (container: em_nginx)|  (docker network)      | (container: em_backend)|
|    [ 0.0.0.0:80 ]     |  http://backend:8080   |       [ :8080 ]        |
+-----------------------+                        +------------------------+