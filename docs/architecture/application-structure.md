# Application Structure

## Project Layout

```
senseLearn-learning-hub/
├── app.py                 # Application entry point
├── requirements.txt       # Python dependencies
├── pytest.ini            # Pytest configuration
├── mkdocs.yml            # Documentation configuration
├── docs/                  # Documentation files
├── migrations/           # Database migrations (Alembic)
├── src/                   # Source code
│   ├── __init__.py       # Application factory
│   ├── config.py         # Configuration management
│   ├── admin/            # Admin module
│   │   ├── routes.py     # Admin routes
│   │   └── __init__.py
│   ├── auth/             # Authentication module
│   │   ├── models.py     # User and auth models
│   │   ├── routes.py     # Auth routes (register, login, etc.)
│   │   ├── utils.py      # Auth utilities
│   │   ├── email_service.py  # Email sending
│   │   └── __init__.py
│   ├── student/          # Student module
│   │   ├── routes.py     # Student routes
│   │   └── __init__.py
│   ├── tutor/            # Tutor module
│   │   ├── routes.py     # Tutor routes
│   │   ├── course_routes.py  # Course management routes
│   │   └── __init__.py
│   ├── quiz/             # Quiz system
│   │   ├── models.py     # Quiz models
│   │   ├── student_routes.py  # Student quiz routes
│   │   ├── tutor_routes.py   # Tutor quiz routes
│   │   └── __init__.py
│   ├── chatbot/          # Chatbot module
│   │   ├── models.py     # Chatbot models
│   │   ├── routes.py     # Chatbot routes
│   │   ├── service.py    # Chatbot service logic
│   │   ├── document_processor.py  # Document processing
│   │   ├── tts_service.py  # Text-to-speech service
│   │   └── __init__.py
│   ├── notifications/    # Notification system
│   │   ├── routes.py     # Notification routes
│   │   ├── service.py    # Notification service
│   │   └── __init__.py
│   ├── security/         # Security features
│   │   ├── csrf.py       # CSRF protection
│   │   ├── rate_limiter.py  # Rate limiting
│   │   ├── account_lockout.py  # Account lockout
│   │   ├── password_validator.py  # Password validation
│   │   ├── input_validator.py  # Input validation
│   │   ├── security_headers.py  # Security headers
│   │   ├── security_logger.py  # Security logging
│   │   ├── security_init.py  # Security initialization
│   │   └── __init__.py
│   ├── common/           # Common utilities
│   │   ├── decorators.py  # Common decorators
│   │   ├── file_utils.py  # File utilities
│   │   ├── utils.py      # General utilities
│   │   └── __init__.py
│   └── utils/            # Additional utilities
│       └── make_video_streamable.py
├── templates/            # HTML templates
│   ├── admin/
│   ├── student/
│   ├── tutor/
│   └── *.html
├── static/               # Static files
│   ├── css/
│   └── js/
├── test/                # Test files
│   ├── test_auth.py
│   ├── test_student.py
│   ├── test_tutor.py
│   ├── test_admin.py
│   └── test_integration.py
└── security_test/       # Security test scripts
```

## Application Factory Pattern

The application uses Flask's application factory pattern (`create_app()` function) for:

- Better testability
- Multiple app instances
- Configuration flexibility
- Blueprint registration

## Blueprint Organization

The application is organized into blueprints:

- `auth_bp` - Authentication routes (`/api/auth`)
- `student_bp` - Student routes (`/student`)
- `tutor_bp` - Tutor routes (`/tutor`)
- `admin_bp` - Admin routes (`/admin`)
- `quiz_bp` - Quiz routes (`/quiz/api`)
- `chatbot_bp` - Chatbot routes (`/student/chatbot`)

## Database Layer

- **ORM**: SQLAlchemy
- **Migrations**: Flask-Migrate (Alembic)
- **Connection Pooling**: Configured for performance
- **Indexes**: Optimized for common queries

## Security Layer

Security features are initialized in `src/security/security_init.py`:

- CSRF protection
- Rate limiting
- Account lockout
- Input validation
- Security headers
- Security logging

## File Handling

- Upload directory: Configurable via `UPLOAD_DIR`
- File serving: Secure file serving with permission checks
- Video streaming: HTTP Range request support
- File types: Configurable allowed extensions

## Request Flow

1. Request arrives at Flask app
2. Security middleware checks (CSRF, rate limit, etc.)
3. Route handler processes request
4. Database queries executed
5. Response returned with security headers
