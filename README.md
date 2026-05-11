# LoginSystem

A Spring Boot application that provides user registration and login functionality with email verification.

## Overview

This application is a secure user authentication system built with Spring Boot that includes:
- User registration with email validation
- Email confirmation for account activation
- Secure password storage
- Role-based access control (USER/ADMIN)
- RESTful API endpoints
- PostgreSQL database integration

## Tech Stack

- **Java 21**
- **Spring Boot 4.0.6**
- **Spring Security** - Authentication and authorization
- **Spring Data JPA** - Database operations
- **PostgreSQL** - Database
- **Thymeleaf** - Template engine
- **Spring Mail** - Email functionality
- **Lombok** - Code generation
- **Maven** - Build tool

## Prerequisites

Before running this application, ensure you have:

1. **Java 21** installed
2. **PostgreSQL** database server running
3. **Maven** installed
4. **Mail server** (for email functionality - see configuration)

## Database Setup

1. Create a PostgreSQL database named `registration`
2. Ensure PostgreSQL is running on `localhost:5432`
3. The application will automatically create/drop tables using Hibernate DDL

```sql
CREATE DATABASE registration;
```

## Configuration

The application configuration is located in `src/main/resources/application.yaml`:

### Database Configuration
```yaml
spring:
  datasource:
    password: password
    url: jdbc:postgresql://localhost:5432/registration
    username: postgres
```

### Email Configuration
```yaml
spring:
  mail:
    host: localhost
    port: 1025
    username: hello
    password: hello
```

**Note**: For development, you can use a local SMTP server like MailHog or MailDev for testing email functionality.

## Running the Application

### Using Maven Wrapper

1. Navigate to the project root directory
2. Run the application:

```bash
./mvnw spring-boot:run
```

### Using Maven

```bash
mvn spring-boot:run
```

The application will start on `http://localhost:8080`

## API Endpoints

### Registration

**POST** `/api/v1/registration`

Register a new user account.

**Request Body:**
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "password": "securePassword123"
}
```

**Response:** 
- Returns a confirmation message
- Sends a confirmation email with a verification token

### Email Confirmation

**GET** `/api/v1/registration/confirm?token={token}`

Confirm user registration using the token sent to the user's email.

**Parameters:**
- `token` - The confirmation token received via email

**Response:**
- Returns success message upon successful confirmation
- Activates the user account

### Web Pages

**GET** `/login-page` - Login page (returns Thymeleaf template)
**GET** `/home` - Home page (returns Thymeleaf template)

## User Roles

The application supports two user roles:

- **USER** - Regular user with basic privileges
- **ADMIN** - Administrative user with elevated privileges

## Security Features

- Password encryption using BCrypt
- Account locking mechanism
- Email verification required for account activation
- Role-based access control
- CSRF protection
- Session management

## Database Schema

### AppUser Entity
- `id` - Primary key (auto-generated)
- `firstName` - User's first name
- `lastName` - User's last name
- `email` - User's email (used as username)
- `password` - Encrypted password
- `appUserRole` - User role (USER/ADMIN)
- `locked` - Account lock status
- `enabled` - Account enabled status

### ConfirmationToken Entity
- `id` - Primary key
- `token` - Confirmation token
- `createdAt` - Token creation timestamp
- `expiresAt` - Token expiration timestamp
- `confirmedAt` - Confirmation timestamp
- `appUser` - Associated user

## Development Notes

### Email Testing

For development purposes, you can use tools like:
https://github.com/maildev/maildev
This is a SMTP server for development

Configure your `application.yaml` to point to these tools:
```yaml
spring:
  mail:
    host: localhost
    port: 1025  # MailHog default port
```

### Database Configuration

The application uses `ddl-auto: create-drop` which means:
- Tables are created on startup
- Tables are dropped on shutdown
- Data is not persisted between restarts

For production, change this to `update` or `validate`.

## Testing

Run the test suite:

```bash
./mvnw test
```

The application includes comprehensive tests for:
- Registration service
- Email validation
- Token confirmation
- Security configuration

## Building for Production

Create a JAR file:

```bash
./mvnw clean package
```

Run the JAR file:

```bash
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

## License

This project is for educational purposes.
