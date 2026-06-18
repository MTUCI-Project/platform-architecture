# Радиодвижок

Radio Engine является центральной частью платформы.

Он должен быть максимально независимым от остальных сервисов.

## Входящие взаимодействия

Единственный способ обратиться к движку — HTTP API.

Примеры:

POST /stations/{id}/play

POST /stations/{id}/stop

POST /stations/{id}/queue

POST /stations/{id}/next

---

## Исходящие взаимодействия

Движок никогда не обращается к другим сервисам напрямую.

Все события публикуются через Redis Pub/Sub.

Примеры:

track.started

track.finished

station.started

station.stopped

tts.requested

---

## Доступ к данным

Radio Engine имеет доступ только к MinIO.

Движок не знает о существовании:

* PostgreSQL;
* User Backend;
* AI Backend.

Вся бизнес-логика находится вне движка.

Это позволяет сохранить его простым и независимым.
