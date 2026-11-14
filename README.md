# IN3101 - Enterprise Application Development
## Lab 02 & Lab 03: REST API & Docker Containerisation

**Student Name:** Ashen Randira  
**Index Number:** 234206B  
**Institution:** University of Moratuwa - Faculty of Information Technology  
**Degree:** B.Sc. in Information Technology / B.Sc. in Information Technology & Management  
**Module:** IN3101 - Enterprise Application Development  

---

## 📌 Project Overview

This project implements a complete CRUD (Create, Read, Update, Delete) RESTful API for managing books in a library system using **Spring Boot 3**, **Spring Data JPA**, and **MySQL**. It adheres to standard layered architecture, includes interactive API documentation powered by **Swagger / SpringDoc OpenAPI**, and is fully containerised with **Docker & Docker Compose**.

```
Client (Postman / Browser)
         │
         ├─── Port 8080 ────────► [ api container ] (Spring Boot 3 on eclipse-temurin:21-jre)
         │                               │
         │                        db:3306│ (Docker network)
         │                               ▼
         └─── Port 3307 ────────► [ db container ] (MySQL 8.4)
                                         │
                                         ▼
                                 [ library-data ] (Persistent Volume)
```

---

## 🛠️ Tech Stack & Dependencies

- **Java**: JDK 21+
- **Framework**: Spring Boot 3
- **Data Persistence**: Spring Data JPA & Hibernate
- **Database**: MySQL 8.4
- **Containerisation**: Docker, Docker Compose
- **Validation**: Jakarta Bean Validation (`spring-boot-starter-validation`)
- **API Documentation**: SpringDoc OpenAPI UI (`springdoc-openapi-starter-webmvc-ui`)
- **Build Tool**: Maven

---

## 🐳 Docker & Docker Compose Setup (Lab 03)

### 1. Environment Configuration
Create a `.env` file in the project root based on `.env.example`:
```env
DB_ROOT_PASSWORD=root123
```
*(Note: `.env` is ignored by Git to protect sensitive credentials)*

### 2. Build the Application JAR
```powershell
.\mvnw.cmd clean package -DskipTests
```

### 3. Run with Docker Compose
Start both the database and the API services in detached mode:
```powershell
docker compose up --build -d
```
Check running services and health status:
```powershell
docker compose ps
```
View application logs:
```powershell
docker compose logs api
```

### 4. Stopping Containers
Stop containers while preserving database volume data:
```powershell
docker compose down
```
Stop containers and delete volumes (fresh start):
```powershell
docker compose down -v
```

---

## ⚙️ Local Development (Without Docker - Lab 02)

### 1. Database Setup
Create the MySQL database on your local MySQL server (port 3306):
```sql
DROP DATABASE IF EXISTS library_db;
CREATE DATABASE library_db;
```

### 2. Application Properties
Configure your local MySQL credentials in `src/main/resources/application.properties`:
```properties
spring.application.name=library-api

spring.datasource.url=jdbc:mysql://localhost:3306/library_db
spring.datasource.username=root
spring.datasource.password=your_mysql_password

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### 3. Run Locally
```powershell
# Windows
.\mvnw.cmd spring-boot:run

# Linux / macOS
./mvnw spring-boot:run
```

The application starts by default on port `8080`.

---

## 📖 API Documentation (Swagger UI)

Interactive Swagger UI documentation is available when the application is running:
- **Swagger UI URL:** [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)
- **Raw OpenAPI JSON:** [http://localhost:8080/v3/api-docs](http://localhost:8080/v3/api-docs)

---

## 📡 REST API Endpoints

| Method | Endpoint | Description | Request Body | Success Status |
|--------|----------|-------------|--------------|----------------|
| `GET` | `/api/books` | Retrieve all books | None | `200 OK` |
| `GET` | `/api/books/{id}` | Retrieve a book by its ID | None | `200 OK` (or `404 Not Found`) |
| `POST` | `/api/books` | Create a new book | JSON (`title`, `author`, `isbn`, `price`) | `201 Created` (or `400 Bad Request`) |
| `PUT` | `/api/books/{id}` | Update an existing book | JSON (`title`, `author`, `isbn`, `price`) | `200 OK` (or `404 Not Found`) |
| `DELETE` | `/api/books/{id}` | Delete a book by ID | None | `204 No Content` (or `404 Not Found`) |

### Example Request Body (POST / PUT)
```json
{
  "title": "234206B - Clean Code",
  "author": "Robert C. Martin",
  "isbn": "9780132350884",
  "price": 4500.00
}
```

---

## 🧪 Postman Testing

A ready-to-use Postman collection is included in the project:
- File: [`postman_collection.json`](./postman_collection.json)
- To import: Open Postman > Click **Import** > Drag and drop `postman_collection.json`.

---

## 🌿 Git Branching

- `main`: Production-ready release branch containing all deliverables.
- `dev`: Development branch for feature integration and staging.
- `fix`: Hotfix / bugfix branch for patches and adjustments.
