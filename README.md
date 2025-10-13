# ToDo List Template
This repository provides a structured specification for a ToDo list backend, including all essential API endpoints, request/response formats, and authentication flow. The goal is to serve as a reference blueprint for implementing the same project across multiple backend frameworks.

By standardizing the API design, input/output expectations, and behavior, this repo enables consistent comparisons and learning experiences across different technologies.

# Tech stack
Describe the tech stack used to make this todo app.
1. Backend Framework
2. ORM
3. Logging
4. Parameter Checker

## etc Tech stack
Supporting microservices
1. Database
2. Cache

## Status code format
the status format is drived from http status with added information.
```
controller_id-http_code
xx-xxx
```


# Data flow
Data flow follows SOLID code principle
```text
app -> global middlewares-> routes -> local middlewares -> controllers -> services -> repository -> database -> models
```
# Endpoints
## List of all endpoints

| Method | Endpoint                               | Controller ID | Auth | Payload                  | Response Example                  | Description                           |
| ------ | -------------------------------------- | ------------- | ---- | ------------------------ | --------------------------------- | ------------------------------------- |
| POST   | `/register`                            | RE            | -    | `{ username, password }` | status                            | Register a new user                   |
| POST   | `/login`                               | RE            | -    | `{ username, password }` | status, data: token               | Login and receive JWT                 |
| GET    | `/todolists`                           | TL            | JWT  | –                        | status, message, data: todolist[] | Fetch all to-do lists                 |
| GET    | `/todolists/:todolistId`               | TL            | JWT  | –                        | status, message, data: todolist   | Get a single to-do list and its todos |
| POST   | `/todolists`                           | TL            | JWT  | `{ title }`              | status, message, data: todolist   | Create a new to-do list               |
| PUT    | `/todolists/:todolistId`               | TL            | JWT  | `{ title, status }`      | status, message, data: todolist   | Update a to-do list (title or status) |
| DELETE | `/todolists/:todolistId`               | TL            | JWT  | –                        | -                                 | Delete a to-do list                   |
| GET    | `/todolists/:todolistId/todos`         | TO            | JWT  | –                        | status, message, data: todo[]     | Fetch all todos in a list             |
| GET    | `/todolists/:todolistId/todos/:todoId` | TO            | JWT  | –                        | status, message, data: todo       | Get a single todo                     |
| POST   | `/todolists/:todolistId/todos`         | TO            | JWT  | `{ message }`            | status, message, data: todo       | Add a new todo to a list              |
| PUT    | `/todolists/:todolistId/todos/:todoId` | TO            | JWT  | `{ message, status }`    | status, message, data: todo       | Update a specific todo                |
| DELETE | `/todolists/:todolistId/todos/:todoId` | TO            | JWT  | –                        | status, message, data: NULL       | Delete a specific todo                |

