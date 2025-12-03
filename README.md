# Практическая работа № 4
## Подготовила: Сорокина К.С., ЭФМО-01-25.
### Выполнены следующие задачи:
1.	Освоить базовую маршрутизацию HTTP-запросов в Go на примере роутера chi.
2.	Научиться строить REST-маршруты и обрабатывать методы GET/POST/PUT/DELETE.
3.	Реализовать небольшой CRUD-сервис «ToDo» (без БД, хранение в памяти).
4.	Добавить простое middleware (логирование, CORS).
5.	Научиться тестировать API запросами через curl/Postman/HTTPie.
### Структура проекта:
```bash
pz4-todo/
├── go.mod
├── go.sum
├── main.go
├── internal/
│   └── task/
│       ├── model.go       # Модель данных задачи
│       ├── repo.go        # Логика хранения данных в памяти
│       └── handler.go     # Обработчики HTTP-запросов
└── pkg/
    └── middleware/
        ├── logger.go      # Middleware для логирования
        └── cors.go        # Middleware для CORS
```

### Команда для запуска сервера:
```bash
go run .
```
## Запуск
![запуск](https://github.com/krrristina/PR4/blob/main/screenshots/%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA.png)

## Фрагменты кода:
```bash
	r := chi.NewRouter()
	r.Use(chimw.RequestID)
	r.Use(chimw.Recoverer)
	r.Use(myMW.Logger)
	r.Use(myMW.SimpleCORS)

	r.Get("/health", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("OK"))
	})

	r.Route("/api", func(api chi.Router) {
		api.Mount("/tasks", h.Routes())
	})

	addr := ":8080"
	log.Printf("listening on %s", addr)
	log.Fatal(http.ListenAndServe(addr, r))
}
```

### Middleware для логирования (pkg/middleware/logger.go)
```bash
func Logger(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()
        next.ServeHTTP(w, r)
        log.Printf("%s %s %s", r.Method, r.URL.Path, time.Since(start))
    })
}
```

### Обработчик создания задачи (internal/task/handler.go)
```bash
type createReq struct {
	Title string `json:"title"`
}

func (h *Handler) create(w http.ResponseWriter, r *http.Request) {
	var req createReq
	if err := json.NewDecoder(r.Body).Decode(&req); err != nil || req.Title == "" {
		httpError(w, http.StatusBadRequest, "invalid json: require non-empty title")
		return
	}
	t := h.repo.Create(req.Title)
	writeJSON(w, http.StatusCreated, t)
}
```

# Примеры запросов
Запрос GET /health:
![health](https://github.com/krrristina/PR4/blob/main/screenshots/OK.png)
## Примеры запросов через Postman:
### Запрос POST /tasks:
![запуск](https://github.com/krrristina/PR4/blob/main/screenshots/post%20tasks.png)

### Запрос GET /tasks:
![get](https://github.com/krrristina/PR4/blob/main/screenshots/get%20tasks.png)

### Запрос GET /tasks/{id}:
![get](https://github.com/krrristina/PR4/blob/main/screenshots/get2.png)

### Запрос PUT /tasks/{id}:
![put](https://github.com/krrristina/PR4/blob/main/screenshots/put.png)

### Запрос DELETE /tasks/{id}:
![delete](https://github.com/krrristina/PR4/blob/main/screenshots/delete.png)

## Результаты тестирования

| Маршрут       | Метод | Тело запроса                           | Ожидаемый код | Фактический код | Результат  |
|---------------|-------|----------------------------------------|---------------|-----------------|------------|
| /health       | GET   | -                                      | 200           | 200             | Успешно    |
| /api/tasks    | POST  | {"title":"Выучить Go"}               | 201           | 201             | Успешно    |
| /api/tasks    | POST  | []                                     | 400           | 400             | Успешно    |
| /api/tasks    | GET   | -                                      | 200           | 200             | Успешно    |
| /api/tasks/1  | GET   | -                                      | 200           | 200             | Успешно    |
| /api/tasks/99 | GET   | -                                      | 404           | 404             | Успешно    |
| /api/tasks/1  | PUT   | {"title":"Выучить Go глубже","done":true}    | 200           | 200             | Успешно    |
| /api/tasks/1  | DELETE| -                                      | 204           | 204             | Успешно    |
| /api/tasks/1  | DELETE| -                                      | 404           | 404             | Успешно    |

## Выводы
В ходе выполнения практической работы были достигнуты все поставленные цели. Я успешно освоилf основы создания REST API на Go с использованием роутера chi.
#### Что получилось:
- Создан полнофункциональный CRUD-сервис.
- Реализована маршрутизация и обработка всех основных HTTP-методов.
- Подключены и протестированы middleware для логирования и CORS.
#### Что было сложным:
- Наибольшую сложность на начальном этапе вызвало понимание концепции middleware и того, как они встраиваются в конвейер обработки запроса.
- Также потребовалось время, чтобы разобраться с корректной работой с JSON в Go, особенно с декодированием тел запросов.
