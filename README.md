# Практическая работа № 4
## Подготовила: Сорокина К.С., ЭФМО-01-25.
### Выполнены следующие задачи:
1.	Освоить базовую маршрутизацию HTTP-запросов в Go на примере роутера chi.
2.	Научиться строить REST-маршруты и обрабатывать методы GET/POST/PUT/DELETE.
3.	Реализовать небольшой CRUD-сервис «ToDo» (без БД, хранение в памяти).
4.	Добавить простое middleware (логирование, CORS).
5.	Научиться тестировать API запросами через curl/Postman/HTTPie.
### Структура проекта:
![Структура](https://github.com/krrristina/PR4/blob/main/screenshots/%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D0%B0.png)
### Команда для запуска сервера:
```bash
go run .
```
### Запуск
![запуск](https://github.com/krrristina/PR4/blob/main/screenshots/%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA.png)
### Фрагменты кода:
# Примеры запросов
Запрос GET /health:
![health](https://github.com/krrristina/PR4/blob/main/screenshots/OK.png)
### Примеры запросов через Postman:
Запрос POST /tasks:
![запуск](https://github.com/krrristina/PR4/blob/main/screenshots/post%20tasks.png)
Запрос GET /tasks:
![get](https://github.com/krrristina/PR4/blob/main/screenshots/get%20tasks.png)
Запрос GET /tasks/{id}
![get](https://github.com/krrristina/PR4/blob/main/screenshots/get2.png)
Запрос PUT /tasks/{id}
![put](https://github.com/krrristina/PR4/blob/main/screenshots/put.png)
Запрос DELETE /tasks/{id}
![delete](https://github.com/krrristina/PR4/blob/main/screenshots/delete.png)

