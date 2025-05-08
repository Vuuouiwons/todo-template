# ToDo List Template
This repository provides a structured specification for a ToDo list backend, including all essential API endpoints, request/response formats, and authentication flow. The goal is to serve as a reference blueprint for implementing the same project across multiple backend frameworks.

By standardizing the API design, input/output expectations, and behavior, this repo enables consistent comparisons and learning experiences across different technologies.

# Endpoints
## Simple list of all endpoints
| method | endpoint                              | headers | payload                       | params        | response                           | short description(<=7 words)         |
| ------ | ------------------------------------- | ------- | ----------------------------- | ------------- | ---------------------------------- | ------------------------------------ |
| POST   | `/register`                           | -       | username, password            | -             | OK                                 | register account for authentication  |
| POST   | `/login`                              | -       | username, password            | -             | JWT                                | login to get JWT token               |
| GET    | `/todolists`                          | JWT     | -                             | limit, offset | status, message, data: todolist    | get all todolist                     |
| POST   | `/todolists/:todolistId`              | JWT     | todolistTitle, todolistStatus | -             | status, message, data: NULL        | add todolist to user with: todolist  |
| PUT    | `/todolists/:todolistId`              | JWT     | todolistTitle, todolistStatus | -             | status, message, data: NULL        | update todolist title with: todolist |
| DELETE | `/todolists/:todolistId`              | JWT     | -                             | -             | status, message, data: NULL        | delete todolist with: todolist       |
| GET    | `/todolists/:todolistId`              | JWT     | -                             | limit, offset | status, message, data: todo        | get all todo                         |
| POST   | `/todolists/:todolistId/todo`         | JWT     | todoMessage                   | -             | status, message, data: NULL        | add todo to todolist                 |
| PUT    | `/todolists/:todolistId/todo/:todoId` | JWT     | todoStatus, todoMessage       | -             | status, message, data: todo.status | update todo                          |
| DELETE | `/todolists/:todolistId/todo/:todoId` | JWT     | -                             | -             | status, message, data: NULL        | delete the todo from todolist        |

## Status format
the status format is drived from http status with added information.
```
node-controller-httpCode
x-xx-xxx
```

## Detailed description of the endpoint and its functions
endpoint description contains a header, payload, and reponse table for that endpoint.
#### table example
| key  | value description |
| ---- | ----------------- |
| data | i am data         |

### POST  : `/register`
add new account to database.
#### Payload
| key      | value description                                                       |
| -------- | ----------------------------------------------------------------------- |
| username | username of user, unique, only contains lowercase a-z                   |
| password | password of the user, password is stored in database with sha512 hasing |

### POST  : `/login`
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

### GET   : `/todolists/:todolistId`
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