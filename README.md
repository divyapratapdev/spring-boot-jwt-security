# spring-boot-jwt-security

![Java](https://img.shields.io/badge/Java-21-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_3-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security_6-6DB33F?style=for-the-badge&logo=spring-security&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![CI](https://img.shields.io/github/actions/workflow/status/divyapratapdev/spring-boot-jwt-security/ci.yml?style=for-the-badge&label=CI)
![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)

Production-grade **Spring Boot 3** REST API with **JWT Authentication**, **Spring Security 6**, and **Role-Based Access Control (RBAC)**. Built with clean layered architecture following industry standards.

---

## Architecture

```
┌────────────────────────────────────────────┐
│             CLIENT REQUEST                │
└─────────────────────┬─────────────────────┘
                     │
            ▼
┌────────────────────────────────────────────┐
│       JwtAuthFilter (OncePerRequest)       │
│   Extracts + validates JWT from header     │
└─────────────────────┬─────────────────────┘
                     │
            ▼
┌────────────────────────────────────────────┐
│    SecurityFilterChain (Spring Security)    │
│    Authorizes based on roles / endpoints   │
└─────────────────────┬─────────────────────┘
                     │
            ▼
┌────────────────────────────────────────────┐
│              REST Controllers                │
│   AuthController  |  UserController         │
└─────────────────────┬─────────────────────┘
                     │
            ▼
┌────────────────────────────────────────────┐
│         Service Layer (Business Logic)       │
│   AuthService  |  JwtService  |  UserService│
└─────────────────────┬─────────────────────┘
                     │
            ▼
┌────────────────────────────────────────────┐
│    Repository Layer (Spring Data JPA)        │
│          UserRepository                      │
└─────────────────────┬─────────────────────┘
                     │
            ▼
┌────────────────────────────────────────────┐
│              MySQL Database                  │
└────────────────────────────────────────────┘
```

---

## Features

- **JWT Authentication** — stateless token-based auth with configurable expiry
- **Spring Security 6** — filter chain with custom `OncePerRequestFilter`
- **Role-Based Access Control** — `ROLE_USER` and `ROLE_ADMIN` with method-level security
- **Refresh Token** support — secure token rotation
- **BCrypt Password Hashing** — industry-standard password security
- **Spring Data JPA** — clean repository pattern with MySQL
- **Input Validation** — Bean Validation on all DTOs
- **Global Exception Handling** — structured error responses via `@ControllerAdvice`
- **Docker + Docker Compose** — single-command local setup
- **GitHub Actions CI** — automated build and test on every push

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 21 |
| Framework | Spring Boot 3.2 |
| Security | Spring Security 6, JWT (JJWT 0.12) |
| Database | MySQL 8.0 |
| ORM | Spring Data JPA / Hibernate |
| Build Tool | Maven |
| Containerization | Docker, Docker Compose |
| CI/CD | GitHub Actions |

---

## Project Structure

```
src/
└── main/
    ├── java/com/divyapratap/security/
    │   ├── config/
    │   │   ├── SecurityConfig.java       # Filter chain, CORS, CSRF config
    │   │   └── ApplicationConfig.java    # Beans: UserDetailsService, PasswordEncoder
    │   ├── controller/
    │   │   ├── AuthController.java       # /api/v1/auth endpoints
    │   │   └── UserController.java       # /api/v1/users (protected)
    │   ├── dto/
    │   │   ├── AuthRequest.java
    │   │   ├── AuthResponse.java
    │   │   ├── RegisterRequest.java
    │   │   └── UserResponse.java
    │   ├── exception/
    │   │   ├── GlobalExceptionHandler.java
    │   │   └── ApiError.java
    │   ├── filter/
    │   │   └── JwtAuthFilter.java        # JWT validation per request
    │   ├── model/
    │   │   ├── User.java                 # JPA Entity + UserDetails impl
    │   │   └── Role.java                 # ENUM: USER, ADMIN
    │   ├── repository/
    │   │   └── UserRepository.java
    │   ├── service/
    │   │   ├── AuthService.java
    │   │   ├── JwtService.java           # Token generation, validation, extraction
    │   │   └── UserService.java
    │   └── SecurityApplication.java
    └── resources/
        └── application.yml
```

---

## API Endpoints

### Auth (Public)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/v1/auth/register` | Register new user |
| `POST` | `/api/v1/auth/login` | Login, returns JWT |
| `POST` | `/api/v1/auth/refresh` | Refresh access token |

### Users (Protected)

| Method | Endpoint | Role Required | Description |
|--------|----------|---------------|-------------|
| `GET` | `/api/v1/users/me` | `USER` / `ADMIN` | Get current user |
| `GET` | `/api/v1/users` | `ADMIN` | List all users |
| `DELETE` | `/api/v1/users/{id}` | `ADMIN` | Delete user |

---

## Getting Started

### Prerequisites

- Java 21+
- Maven 3.9+
- Docker & Docker Compose
- MySQL 8.0 (or use Docker Compose)

### Run with Docker Compose (Recommended)

```bash
git clone https://github.com/divyapratapdev/spring-boot-jwt-security.git
cd spring-boot-jwt-security
docker-compose up --build
```

Application starts at `http://localhost:8080`

### Run Locally

```bash
# 1. Configure your database in application.yml
# 2. Build and run
mvn spring-boot:run
```

---

## Usage Examples

### Register

```bash
curl -X POST http://localhost:8080/api/v1/auth/register \
  -H 'Content-Type: application/json' \
  -d '{
    "firstName": "Divya",
    "lastName": "Pratap",
    "email": "divya@example.com",
    "password": "securePassword123"
  }'
```

### Login

```bash
curl -X POST http://localhost:8080/api/v1/auth/login \
  -H 'Content-Type: application/json' \
  -d '{"email": "divya@example.com", "password": "securePassword123"}'
```

Response:
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiJ9...",
  "tokenType": "Bearer"
}
```

### Access Protected Route

```bash
curl -X GET http://localhost:8080/api/v1/users/me \
  -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...'
```

---

## Security Design

- Tokens signed with **HMAC-SHA256** using a 256-bit secret key
- Access token expiry: **15 minutes** (configurable)
- Refresh token expiry: **7 days** (configurable)
- Passwords hashed with **BCrypt** (strength 10)
- Stateless session — `SessionCreationPolicy.STATELESS`
- CSRF disabled (JWT-based, stateless API)
- Public endpoints whitelisted via `requestMatchers`

---

## Running Tests

```bash
mvn test
```

---

## Environment Variables

| Variable | Description | Default |
|----------|-------------|----------|
| `DB_URL` | MySQL JDBC URL | `jdbc:mysql://localhost:3306/securitydb` |
| `DB_USERNAME` | Database username | `root` |
| `DB_PASSWORD` | Database password | `` |
| `JWT_SECRET` | 256-bit secret key | (set in application.yml) |
| `JWT_ACCESS_EXPIRY` | Access token expiry (ms) | `900000` (15 min) |
| `JWT_REFRESH_EXPIRY` | Refresh token expiry (ms) | `604800000` (7 days) |

---

## Contributing

Pull requests are welcome. For major changes, open an issue first to discuss.

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

<div align="center">
Built by <a href="https://github.com/divyapratapdev">Divya Pratap Singh</a>
</div>
