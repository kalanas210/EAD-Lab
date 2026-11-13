# EAD-Lab

IN3101 - Enterprise Application Development lab work.

**Name:** T.K.K. Sandakelum
**Index No:** 235097V

## Lab 02 - Building a REST API with Spring Boot

Project: [`library-api`](library-api)

A Spring Boot REST API that performs CRUD operations on books stored in MySQL.

| Layer | Class |
|---|---|
| Entity | `model/Book.java` |
| Repository | `repository/BookRepository.java` |
| Service | `service/BookService.java` |
| Controller | `controller/BookController.java` |

### Endpoints

| Method | URL | Action | Status |
|---|---|---|---|
| GET | `/api/books` | List all books | 200 |
| GET | `/api/books/{id}` | Get one book | 200 / 404 |
| POST | `/api/books` | Create a book | 201 / 400 |
| PUT | `/api/books/{id}` | Update a book | 200 / 404 |
| DELETE | `/api/books/{id}` | Delete a book | 204 / 404 |

### Running

1. Create the database in MySQL:
   ```sql
   CREATE DATABASE library_db;
   ```
2. Set the MySQL root password and start the app (PowerShell):
   ```powershell
   cd library-api
   $env:DB_PASSWORD="your_root_password"
   .\mvnw.cmd spring-boot:run
   ```
3. The API runs on port **8081** (port 8080 is used by another service on my machine).
   - API: http://localhost:8081/api/books
   - Swagger UI: http://localhost:8081/swagger-ui.html

A Postman collection with all test requests is in `library-api/postman/`.

### Tech

Java 21, Spring Boot 4.1, Spring Data JPA, MySQL 8, Bean Validation, springdoc-openapi 3.1.1

## Lab 03 - Containerising the Library API with Docker

The same `library-api` now runs in Docker together with a MySQL 8.4 container.

| File | Purpose |
|---|---|
| `library-api/Dockerfile` | Image based on `eclipse-temurin:21-jre` that runs the built JAR |
| `library-api/.dockerignore` | Keeps `.git`, `.mvn`, `.vscode` and `src` out of the build context |
| `library-api/compose.yaml` | `db` (MySQL 8.4, volume, healthcheck) + `api` (built from the Dockerfile) |
| `library-api/.env.example` | Variables to copy into `.env` (`.env` itself is git-ignored) |

The API connects to `db:3306` (the Compose service name) instead of `localhost:3306`. The datasource URL, username and password are set through `SPRING_DATASOURCE_*` environment variables, so `application.properties` and the Java code stay unchanged. `SERVER_PORT=8080` is set in the Dockerfile, so the container listens on 8080 (local runs still use 8081).

### Running with Docker (PowerShell)

```powershell
cd library-api
copy .env.example .env          # then set DB_ROOT_PASSWORD in .env
.\mvnw.cmd clean package -DskipTests
docker compose up --build -d
docker compose ps               # wait for db (healthy) and api Up
```

- API: http://localhost:8080/api/books
- Swagger UI: http://localhost:8080/swagger-ui.html
- MySQL (Workbench): `127.0.0.1`, port **3307**, user `root`

`docker compose down` stops the system and keeps the `library-api_library-data` volume, so the books are still there after `docker compose up -d`. `docker compose down -v` also deletes the volume.
