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
