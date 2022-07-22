# REST Service – Laravel & MySQL / PostgreSQL & JWT

** – optional task or sub-task

**Create an application, the application should operate with the following resources:**

- `User` (with attributes):
  ```javascript
  { id, name, login, password }
  ```
- `Board` (set of `columns`):
  ```javascript
  { id, title, columns }
  ```
- `Column` (set of tasks):
  ```javascript
   { id, title, order }
  ```
- `Task`:
  ```javascript
  {
    id,
    title,
    order,
    description,
    userId, //assignee
    boardId,
    columnId
  }
  ```

## API schema:

1. For `User`, `Board` and `Task` REST endpoints with separate router paths should be created
    * `User` (`/users` route)
      * `GET /users` - get all users (remove password from response)
      * `GET /users/:userId` - get the user by id (ex. “/users/123”) (remove password from response)
      * `POST /users` - create user
      * `PUT /users/:userId` - update user
      * `DELETE /users/:userId` - delete user
    * `Board` (`/boards` route)
      * `GET /boards` - get all boards
      * `GET /boards/:boardId` - get the board by id
      * `POST /boards` - create board
      * `PUT /boards/:boardId` - update board
      * `DELETE /boards/:boardId` - delete board
    * `Task` (`boards/:boardId/tasks` route)
      * `GET boards/:boardId/tasks` - get all tasks
      * `GET boards/:boardId/tasks/:taskId` - get the task by id
      * `POST boards/:boardId/tasks` - create task
      * `PUT boards/:boardId/tasks/:taskId` - update task
      * `DELETE boards/:boardId/tasks/:taskId` - delete task
2. When somebody `DELETEs` `Board`, all its `Tasks` should be deleted as well.
3. When somebody `DELETEs` `User`, all `Tasks` where `User` is assignee should be updated to put `userId = null`.

## MySQL / PostgreSQL & Eloquent ORM:
1. Use **MySQL / PostgreSQL** database to store **REST** service data (`Users`, `Boards`, `Columns`, `Tasks`)
2. Use [Eloquent ORM](https://laravel.com/docs/9.x/eloquent) to store and update data
3. The information on DB connection should be stored in `.env` file
4. ****MySQL / PostgreSQL** database should run inside of the `docker` container

## Authentication and JWT:
1. `POST /users` should accept `password` field and before save replace it with **hash**.
2. Implement `POST /login` method which accepts **JSON** with `login` and `password` and returns **JWT** token in response body: `{ token: <jwt_token> }`.
3. **JWT** token should contain `userId` and `login` in a **payload**.
4. Secret that used for signing the token should be stored in `.env` file.
5. For all client requests the **JWT** token should be added in HTTP `Authorization` header to all requests that requires authentication. HTTP authentication must follow `Bearer` scheme, e.g.:
  ```
  Authorization: Bearer <jwt_token>
  ```
6. Proxy all the requests (except `/login`) and check that HTTP `Authorization` header has the correct value of **JWT** token.
7. In case of the HTTP `Authorization` header in the request is absent or invalid or doesn’t follow `Bearer` scheme, further router method execution should be stopped and lead to response with HTTP **401** code (Unauthorized error) and the corresponding error message.
8. **Add admin user to DB** on service start with `login = admin` and `password = admin`.

## **Docker:

### Prerequisites

1. Install [Docker](https://docs.docker.com/engine/install/)
2. Create `Docker Hub` account [Docker Hub](https://hub.docker.com/)

**Details:**

1. Create `.dockerignore` file and list all files that should be ignored by `Docker`
2. Create `Dockerfile` that will be used for building image of `MySQL / PostgreSQL` database
3. Create `Dockerfile` that will be used for building image of your aplication
4. Create `docker-compose.yml` file that will be used for running multi-container application (your application and `MySQL / PostgreSQL` database). Specify custom network that will be used for communication between application and database containers.
