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
## Simple list of all endpoints

| Method | Endpoint                               | Auth | Payload                  | Response Example                   | Description                     |
| ------ | -------------------------------------- | ---- | ------------------------ | ---------------------------------- | ------------------------------- |
| POST   | `/auth/register`                       | -    | `{ username, password }` | OK                                 | Register a new user             |
| POST   | `/auth/login`                          | -    | `{ username, password }` | JWT                                | Login and receive JWT           |
| GET    | `/todolists`                           | JWT  | –                        | status, message, data: todolist    | Fetch all to-do lists           |
| POST   | `/todolists`                           | JWT  | `{ title }`              | status, message, data: NULL        | Create a new to-do list         |
| GET    | `/todolists/:todolistId`               | JWT  | `{ title, status }`      | status, message, data: todolist    | Fetch one to-do lists           |
| PUT    | `/todolists/:todolistId`               | JWT  | `{ title, status }`      | status, message, data: NULL        | Update the title of a list      |
| DELETE | `/todolists/:todolistId`               | JWT  | –                        | status, message, data: todo        | Delete a to-do list             |
| GET    | `/todolists/:todolistId/todos`         | JWT  | –                        | status, message, data: NULL        | Fet all todo in a todolists     |
| POST   | `/todolists/:todolistId/todos`         | JWT  | `{ message }`            | status, message, data: NULL        | Add a new todo to a list        |
| GET    | `/todolists/:todolistId/todos/:todoId` | JWT  | –                        | status, message, data: NULL        | Get a single todo in a todolist |
| PUT    | `/todolists/:todolistId/todos/:todoId` | JWT  | `{ message, status }`    | status, message, data: todo.status | Update a specific todo          |
| DELETE | `/todolists/:todolistId/todos/:todoId` | JWT  | –                        | status, message, data: NULL        | Delete a specific todo          |

## Detailed description of the endpoint and its functions
endpoint description contains a header, payload, and reponse table for that endpoint.
#### table example
| key  | value description |
| ---- | ----------------- |
| data | i am data         |

### POST  : `/auth/register`
add new account to database.
#### Payload
| key      | value description                                                       |
| -------- | ----------------------------------------------------------------------- |
| username | username of user, unique, only contains lowercase a-z                   |
| password | password of the user, password is stored in database with sha512 hasing |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 201       | RE-201 | success, user created                            |
| 422       | RE-422 | request body invalid                             |
| 400       | RE-400 | username not available                           |
| 500       | RE-500 | internal server error, new unknown error occured |

### POST  : `/auth/login`
login to the account, and retrives a json web token (JWT).

#### Payload
| key      | value description |
| -------- | ----------------- |
| username | self explainatory |
| password | self explainatory |

#### Response payload
| key     | value description            |
| ------- | ---------------------------- |
| status  | self explainatory            |
| message | self explainatory            |
| data    | JSON with keys: jwt (string) |

#### Status
| HTTP Code | Code   | Description            |
| --------- | ------ | ---------------------- |
| 201       | LO-201 | success, token created |
| 422       | LO-422 | request body invalid   |
| 401       | LO-401 | user unauthenticated   |

### GET   : `/todolists`
get all todolists for the user. **Note: User credential is taken from jwt**.

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Params
| key    | value desciption       |
| ------ | ---------------------- |
| limit  | amount of todolists    |
| offset | todolists index offset |

#### Response payload
| key     | value description                                 |
| ------- | ------------------------------------------------- |
| status  | self explainatory                                 |
| message | self explainatory                                 |
| data    | JSON with keys: todolists [title, status] (array) |

| HTTP Code | Code   | Description           |
| --------- | ------ | --------------------- |
| 200       | TL-200 | success               |
| 500       | LO-500 | unknown error occured |

### POST  : `/todolists/:todolistId`
add a todo to todolist to todolistId.

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Payload
| key     | value description                    |
| ------- | ------------------------------------ |
| message | message that the todo should contain |

#### Response payload
| key     | value description |
| ------- | ----------------- |
| status  | self explainatory |
| message | self explainatory |
| data    | NULL              |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 201       | TL-201 | success, todolist created                        |
| 422       | TL-422 | request body invalid                             |
| 500       | TL-500 | internal server error, new unknown error occured |

### GET   : `/todolists`
get a single todolists for the user. **Note: User credential is taken from jwt**.

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Params
| key    | value desciption       |
| ------ | ---------------------- |
| limit  | amount of todolists    |
| offset | todolists index offset |

#### Response payload
| key     | value description                                 |
| ------- | ------------------------------------------------- |
| status  | self explainatory                                 |
| message | self explainatory                                 |
| data    | JSON with keys: todolists [title, status] (array) |

| HTTP Code | Code   | Description           |
| --------- | ------ | --------------------- |
| 200       | TL-200 | success               |
| 500       | LO-500 | unknown error occured |


### PUT   : `/todolists/:todolistId`
update the todolist property in this case title.

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Payload
| key                | value description                    |
| ------------------ | ------------------------------------ |
| message (optional) | message that the todo should contain |
| status (optional)  | status complete and incomplete       |

#### Response payload
| key     | value description |
| ------- | ----------------- |
| status  | self explainatory |
| message | self explainatory |
| data    | NULL              |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 201       | TL-201 | success, todolist updated                        |
| 422       | TL-422 | request body invalid                             |
| 400       | TL-400 | todolist doesn't exist                           |
| 500       | TL-500 | internal server error, new unknown error occured |

### DELETE: `/todolists/:todolistId`
delete the todolist from the user.

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Response payload
| key     | value description |
| ------- | ----------------- |
| status  | self explainatory |
| message | self explainatory |
| data    | NULL              |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 204       | TL-204 | deleted                                          |
| 400       | TL-400 | delete failed                                    |
| 500       | TL-500 | internal server error, new unknown error occured |

### GET   : `/todolists/:todolistId/todos`
get all the todo from todolistId.

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Params
| key    | value desciption       |
| ------ | ---------------------- |
| limit  | amount of todolists    |
| offset | todolists index offset |

#### Response payload
| key     | value description                             |
| ------- | --------------------------------------------- |
| status  | self explainatory                             |
| message | self explainatory                             |
| data    | JSON with keys: todo[status, message] (array) |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 200       | TO-200 | success                                          |
| 400       | TO-400 | todolist doesn't exist                           |
| 500       | TO-500 | internal server error, new unknown error occured |

### POST  : `/todolists/:todolistId/todo`
add todo to todolist

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Payload
| key     | value description                    |
| ------- | ------------------------------------ |
| message | message that the todo should contain |

#### Response payload
| key     | value description |
| ------- | ----------------- |
| status  | self explainatory |
| message | self explainatory |
| data    | NULL              |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 201       | TO-201 | success, todo added                              |
| 422       | TO-422 | request body invalid                             |
| 400       | TO-400 | todolist doesn't exist                           |
| 500       | TO-500 | internal server error, new unknown error occured |

### GET   : `/todolists/:todolistId/todos/:todoId`
get a single todo from todolistId.

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Params
| key    | value desciption       |
| ------ | ---------------------- |
| limit  | amount of todolists    |
| offset | todolists index offset |

#### Response payload
| key     | value description                             |
| ------- | --------------------------------------------- |
| status  | self explainatory                             |
| message | self explainatory                             |
| data    | JSON with keys: todo[status, message] (array) |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 200       | TO-200 | success                                          |
| 400       | TO-400 | todolist doesn't exist                           |
| 500       | TO-500 | internal server error, new unknown error occured |

### PUT   : `/todolists/:todolistId/todo/:todoId`
update the todo status (completed, incomplete), and todo message.

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Payload
| key                | value description                    |
| ------------------ | ------------------------------------ |
| message (optional) | message that the todo should contain |
| status (optional)  | status complete and incomplete       |

#### Response payload
| key     | value description |
| ------- | ----------------- |
| status  | self explainatory |
| message | self explainatory |
| data    | NULL              |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 201       | TO-201 | success, todo updated                            |
| 422       | TO-422 | request body invalid                             |
| 400       | TO-400 | todolist or todo doesn't exist                   |
| 500       | TO-500 | internal server error, new unknown error occured |

### DELETE: `/todolists/:todolistId/todo/:todoId`
delete the todo from todolist

#### Header
| key           | value description       |
| ------------- | ----------------------- |
| Authorization | jwt token from `/login` |

#### Response payload
| key     | value description |
| ------- | ----------------- |
| status  | self explainatory |
| message | self explainatory |
| data    | NULL              |

#### Status
| HTTP Code | Code   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| 204       | TO-204 | success, todo deleted                            |
| 400       | TO-400 | todolist or todo doesn't exist                   |
| 500       | TO-500 | internal server error, new unknown error occured |