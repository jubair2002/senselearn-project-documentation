# Architecture Overview

## Project Structure

```
senseLearn-learning-hub/
├── app.py                 # Application entry point
├── src/                   # Source code
│   ├── admin/            # Admin routes and functionality
│   ├── auth/             # Authentication system
│   ├── chatbot/          # Chatbot integration
│   ├── common/           # Common utilities
│   ├── notifications/    # Notification system
│   ├── quiz/             # Quiz system
│   ├── security/         # Security features
│   ├── student/          # Student routes
│   └── tutor/            # Tutor routes
├── templates/            # HTML templates
├── static/               # Static files (CSS, JS)
├── migrations/           # Database migrations
└── test/                 # Test files
```

## Key Components

### Authentication System (`src/auth/`)
- User registration and login
- Email verification
- Password reset functionality

### Security Layer (`src/security/`)
- CSRF protection
- Rate limiting
- Account lockout
- Input validation
- Security headers

### Course Management (`src/tutor/`)
- Course creation and editing
- Module management
- File upload and processing

### Quiz System (`src/quiz/`)
- Quiz creation for tutors
- Quiz taking for students
- Progress tracking

### Chatbot (`src/chatbot/`)
- Document processing
- AI-powered responses
- Text-to-speech integration
