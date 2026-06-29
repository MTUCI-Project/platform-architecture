# Событийная модель

Взаимодействие между сервисами построено вокруг Redis Pub/Sub.

Сервисы не должны зависеть друг от друга напрямую.

## Основные события

station.created

track.uploaded

playlist.updated

schedule.changed

track.started

track.finished

tts.requested

tts.ready

---

Radio Engine публикует события.

User Backend подписывается на них и обновляет состояние клиентов через WebSocket.

AI Backend подписывается на tts.requested и публикует tts.ready после завершения генерации.

В будущем Redis Pub/Sub может быть заменен на Redis Streams, NATS или Kafka без изменения внутренней логики сервисов.
