# Environment Variables

This document describes all environment variables required for the SenseLearn Learning Hub application.

## Required Variables

### Flask Configuration

```env
SECRET_KEY=your-secret-key-here
FLASK_ENV=development  # or 'production'
FLASK_DEBUG=true  # or 'false'
```

### Database Configuration

```env
DB_USER=your_database_user
DB_PASSWORD=your_database_password
DB_HOST=localhost
DB_PORT=3306
DB_NAME=senselearn_db
```

### API Prefixes

```env
API_PREFIX=/api
AUTH_API_PREFIX=/api/auth
STUDENT_API_PREFIX=/api/student
TUTOR_API_PREFIX=/api/tutor
```

### Application URLs

```env
APP_BASE_URL=http://localhost:5000
API_BASE_URL=http://localhost:5000/api
```

## Optional Configuration

### Password Reset

```env
RESET_CODE_LENGTH=6
RESET_CODE_VALIDITY_MINUTES=15
```

### Password Validation

```env
MIN_PASSWORD_LENGTH=8
```

### User Types

```env
VALID_USER_TYPES=student,tutor,admin
DEFAULT_USER_TYPE=student
VALID_DISABILITY_TYPES=visual,hearing,motor,cognitive,other
```

### URL Prefixes

```env
STUDENT_URL_PREFIX=/student
TUTOR_URL_PREFIX=/tutor
```

### Success Messages

```env
MSG_STUDENT_REGISTER_SUCCESS=Registration successful! Please verify your email.
MSG_TUTOR_REGISTER_SUCCESS=Registration successful! Please verify your email.
MSG_LOGIN_SUCCESS=Login successful!
MSG_LOGOUT_SUCCESS=You have been logged out.
MSG_PASSWORD_RESET_SUCCESS=Password reset successful!
```

### Session Configuration

```env
SESSION_COOKIE_SECURE=false  # Set to 'true' in production with HTTPS
SESSION_COOKIE_HTTPONLY=true
SESSION_COOKIE_SAMESITE=Lax
```

### SQLAlchemy

```env
SQLALCHEMY_ECHO=false  # Set to 'true' for SQL query logging
```

### Email Configuration (SMTP)

```env
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your-email@gmail.com
SMTP_PASSWORD=your-app-password
SMTP_FROM_EMAIL=your-email@gmail.com
SMTP_USE_TLS=true
```

### OTP Configuration

```env
OTP_LENGTH=6
OTP_VALIDITY_MINUTES=10
```

### File Upload Configuration

```env
UPLOAD_DIR=/path/to/uploads
MAX_FILE_SIZE=10485760  # 10MB in bytes
ALLOWED_EXTENSIONS=pdf,doc,docx,jpg,jpeg,png,ppt,pptx,gif,txt,mp4,webm,avi,mov,mkv
```

## Example .env File

Create a `.env` file in the project root:

```env
# Flask
SECRET_KEY=your-super-secret-key-change-in-production
FLASK_ENV=development
FLASK_DEBUG=true

# Database
DB_USER=root
DB_PASSWORD=yourpassword
DB_HOST=localhost
DB_PORT=3306
DB_NAME=senselearn_db

# API
API_PREFIX=/api
AUTH_API_PREFIX=/api/auth
STUDENT_API_PREFIX=/api/student
TUTOR_API_PREFIX=/api/tutor

# URLs
APP_BASE_URL=http://localhost:5000
API_BASE_URL=http://localhost:5000/api

# Email
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your-email@gmail.com
SMTP_PASSWORD=your-app-password
SMTP_FROM_EMAIL=your-email@gmail.com
SMTP_USE_TLS=true

# File Uploads
UPLOAD_DIR=/Users/yourname/uploads
MAX_FILE_SIZE=10485760
ALLOWED_EXTENSIONS=pdf,doc,docx,jpg,jpeg,png,ppt,pptx,gif,txt,mp4,webm,avi,mov,mkv
```

## Security Notes

- **Never commit `.env` files to version control**
- Use strong, randomly generated `SECRET_KEY` in production
- Set `SESSION_COOKIE_SECURE=true` when using HTTPS
- Use environment-specific values for production deployments
