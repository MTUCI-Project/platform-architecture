# Radio Platform Architecture

## Общая структура

Платформа состоит из следующих сервисов:

* User Backend (Node.js + Express.js)
* Radio Engine (Go)
* AI Backend (Python + TTS)
* Icecast
* Redis
* PostgreSQL
* MinIO
* Nginx

Архитектура построена по принципу слабой связанности сервисов.

---

# Компоненты

## User Backend

Технологии:

* Node.js
* Express.js

Ответственность:

* авторизация пользователей;
* JWT;
* управление станциями;
* плейлистами;
* расписанием;
* загрузка треков;
* административный API;
* WebSocket для клиентов.

Доступ:

* PostgreSQL;
* Redis;
* MinIO;
* AI Backend;
* Radio Engine (HTTP API).

---

## Radio Engine

Технологии:

* Go

Ответственность:

* AutoDJ;
* очередь треков;
* расписания;
* переходы между треками;
* генерация событий;
* вещание в Icecast.

Особенности:

* не имеет прямого доступа к PostgreSQL;
* не имеет доступа к AI Backend;
* имеет доступ к MinIO для получения аудиофайлов;
* взаимодействие с внешним миром происходит через HTTP API и Redis Pub/Sub.

Входящие запросы:

* HTTP API.

Исходящие события:

* Redis Pub/Sub.

---

## Redis

Используется для:

* Pub/Sub событий между сервисами;
* хранения JWT и сессий;
* кэширования.

Примеры событий:

* station.created
* playlist.updated
* schedule.changed
* track.started
* track.finished
* tts.requested
* tts.ready

---

## PostgreSQL

Основное хранилище данных.

Содержит:

* пользователей;
* станции;
* треки;
* плейлисты;
* расписания.

---

## MinIO

Объектное хранилище.

Структура:

tracks/
covers/
tts/
recordings/

Используется:

User Backend:

* загрузка файлов;

Radio Engine:

* чтение треков;

AI Backend:

* сохранение TTS.

---

## AI Backend

Технологии:

* Python

Ответственность:

* генерация TTS;
* сохранение файлов в MinIO.

Взаимодействие:

получает события из Redis;

публикует событие:

tts.ready

---

## Icecast

Streaming сервер.

Получает поток от Radio Engine.

Не взаимодействует с базой данных.

---

## Nginx

Reverse Proxy.

Используется для:

* HTTPS;
* кастомных URL;
* скрытия Icecast;
* проксирования потоков;
* балансировки нагрузки.

Слушатели получают поток по адресу:

https://radio.example.com/live

а не напрямую от Icecast.

---