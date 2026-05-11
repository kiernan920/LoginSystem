# LoginSystem

A Spring Boot application that provides user registration and login functionality with email verification.

## Overview

This application is a secure user authentication system built with Spring Boot that includes:
- User registration with email validation
- Email confirmation for account activation
- Secure password storage
- Role-based access control (USER/ADMIN)
- RESTful API endpoints
- Web-based user interface
- PostgreSQL database integration
- Modern responsive design

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

## Web Interface

The application provides a complete web-based user interface with the following pages:

### Registration Page
- **URL**: `http://localhost:8080/register`
- **Features**: User registration form with validation, password confirmation, and success/error messages
- **Access**: Public (no authentication required)

### Login Page
- **URL**: `http://localhost:8080/login-page`
- **Features**: Secure login form, remember me option, error handling
- **Access**: Public (no authentication required)

### Welcome Page
- **URL**: `http://localhost:8080/welcome`
- **Features**: Post-login success page with user information display
- **Access**: Requires authentication (redirected after successful login)

### Home Page
- **URL**: `http://localhost:8080/home`
- **Features**: Basic home page template
- **Access**: Requires authentication

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

**GET** `/register` - Registration page with form validation
**GET** `/login-page` - Login page with authentication
**GET** `/welcome` - Welcome page displayed after successful login
**GET** `/home` - Home page (requires authentication)

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

## User Flow

### Registration Flow
1. Navigate to `http://localhost:8080/register`
2. Fill in registration form (first name, last name, email, password)
3. Submit form - creates user account (disabled by default)
4. Receive confirmation email with verification token
5. Click confirmation link or visit `/api/v1/registration/confirm?token={token}`
6. Account becomes enabled and ready for login

### Login Flow
1. Navigate to `http://localhost:8080/login-page`
2. Enter email and password
3. Successful login redirects to welcome page
4. Welcome page displays user information and provides logout option

### Features
- **Form Validation**: Client-side and server-side validation
- **Error Handling**: User-friendly error messages for invalid credentials
- **Success Messages**: Confirmation for registration, logout, and email verification
- **Responsive Design**: Mobile-friendly interface using Bootstrap
- **Security**: CSRF protection, password encryption, session management

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

## Troubleshooting

### Common Issues

1. **Registration Page Not Accessible**
   - Ensure Spring Security configuration permits `/register` endpoint
   - Check that application is running on correct port

2. **Login Redirect Issues**
   - Verify login.html template exists in `/templates`
   - Check Spring Security form login configuration

3. **Database Connection Error**
   - Ensure PostgreSQL is running
   - Check database credentials in `application.yaml`
   - Verify database exists

4. **Email Not Sending**
   - Check mail server configuration
   - Ensure mail server is running on specified port
   - Verify email credentials

5. **Port Already in Use**
   - Change server port in `application.yaml`:
   ```yaml
   server:
     port: 8081
   ```

6. **Registration Form Not Working**
   - Check browser console for JavaScript errors
   - Verify API endpoint `/api/v1/registration` is accessible
   - Ensure CORS is properly configured

## License

This project is for educational purposes.
