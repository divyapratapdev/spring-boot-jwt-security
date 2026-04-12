# Spring Boot JWT Security Template

A boilerplate implementation of stateless JWT authentication in Spring Boot 3.x + Spring Security 6.

I got tired of re-writing the same security configurations for every microservice, so I extracted my standard JWT implementation into this template. It handles access tokens, refresh tokens, and role-based access control (RBAC).

## What's Inside

- **Spring Security 6 Configuration:** Using `SecurityFilterChain` (no deprecated `WebSecurityConfigurerAdapter` nonsense).
- **Stateless Sessions:** Completely stateless architecture (`SessionCreationPolicy.STATELESS`). The server doesn't store session data, meaning you can load balance requests across multiple instances without sticky sessions.
- **Refresh Token Rotation:** Access tokens expire quickly (e.g., 15 mins). Refresh tokens are tracked in the database and handle long-term localized persistent sessions.
- **Filters:** Custom `OncePerRequestFilter` to intercept and validate the `Authorization: Bearer <token>` header payload.

## Core Flow (Why I built it this way)

1. Client sends `POST /api/auth/login`.
2. Server validates credentials and returns an `access_token` and `refresh_token`.
3. Client attaches `access_token` to subsequent requests.
4. When `access_token` expires, client hits `POST /api/auth/refresh` using the `refresh_token` to get a new pair.

Storing the refresh token in the DB allows us to instantly revoke sessions (e.g. if an account is compromised) without waiting for the JWT expiry. It's the only safe way to do JWTs in a real production environment.

## Running Locally

```bash
docker-compose up -d
./mvnw spring-boot:run
```

You'll need a standard PostgreSQL instance running on `localhost:5432`. See `application.yml` for the standard DB configs.

## TODOs
- [ ] Add Redis backend option for refresh token storage (DB is fine for small apps, but Redis is faster for high throughput revocation checks).
- [ ] Make the JWT secret key fetch from an external secret manager like AWS Secrets Manager instead of env variables.
- [ ] Add generic rate-limiting filter for auth endpoints.
