# Contributing to Spring Boot JWT Security

Thank you for your interest in contributing to this Spring Boot JWT Security template! This guide will help you get started.

## Development Setup

### Prerequisites
- Java 21+
- Maven 3.8+
- Docker & Docker Compose
- PostgreSQL 14+

### Local Development

1. Clone the repository:
```bash
git clone https://github.com/divyapratapdev/spring-boot-jwt-security.git
cd spring-boot-jwt-security
```

2. Start the database:
```bash
docker-compose up -d
```

3. Run the application:
```bash
./mvnw spring-boot:run
```

The API will be available at `http://localhost:8080`.

## Making Changes

### Code Standards
- Follow Google's Java Style Guide
- Use meaningful variable and method names
- Add JavaDoc comments for public methods
- Keep methods focused and concise

### Testing
Before submitting a PR, ensure:
```bash
./mvnw test
./mvnw verify
```

### Commit Messages
Use clear, descriptive commit messages:
- ✅ `feat: add Redis backend option for refresh tokens`
- ✅ `fix: prevent concurrent refresh token exploitation`
- ❌ `update stuff`
- ❌ `fix bug`

## Submitting a Pull Request

1. Create a feature branch: `git checkout -b feature/your-feature`
2. Make your changes and commit with clear messages
3. Push to your fork: `git push origin feature/your-feature`
4. Open a PR with a clear description of changes

### PR Checklist
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] No breaking changes (or clearly documented)
- [ ] Code follows project style guide
- [ ] Commit messages are clear and descriptive

## Areas for Contribution

Check the [README.md](./README.md#todos) for the current TODO list:
- Redis backend for refresh token storage
- External secret manager integration (AWS Secrets Manager)
- Rate-limiting filters
- Additional security enhancements
- Documentation improvements
- Bug fixes and performance optimizations

## Questions?

Open an issue with the `question` label or start a discussion. We're here to help!

---

Happy contributing! 🚀