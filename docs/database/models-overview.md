# Database Models Overview

The SenseLearn Learning Hub uses SQLAlchemy ORM with MySQL database. All models are defined in various `models.py` files.

## Model Locations

- **User & Authentication**: `src/auth/models.py`
- **Quiz System**: `src/quiz/models.py`
- **Chatbot**: `src/chatbot/models.py`

## Core Models

### User Management
- `User` - Main user model (students, tutors, admins)
- `PasswordResetCode` - Password reset tokens
- `EmailVerificationOTP` - Email verification OTPs
- `PendingRegistration` - Temporary registration data

### Course Management
- `Course` - Course entity
- `CourseModule` - Course modules/lessons
- `ModuleFile` - Files within modules
- `CourseStudent` - Student enrollments
- `CourseRequest` - Student course requests
- `StudentFileProgress` - File viewing progress tracking
- `course_tutors` - Many-to-many relationship table

### Quiz System
- `Quiz` - Quiz entity
- `Question` - Quiz questions
- `QuestionOption` - Multiple choice options
- `QuizAttempt` - Student quiz attempts
- `Answer` - Student answers to questions

### Chatbot
- `ChatConversation` - Chat conversations
- `ChatMessage` - Individual messages
- `ChatbotDocument` - Uploaded documents

### Other
- `TutorDocument` - Tutor verification documents
- `Notification` - User notifications

## Relationships

### User Relationships
- User → CourseStudent (one-to-many)
- User → CourseRequest (one-to-many)
- User → TutorDocument (one-to-many)
- User → ChatConversation (one-to-many)
- User → QuizAttempt (one-to-many)
- User → Notification (one-to-many)

### Course Relationships
- Course → CourseModule (one-to-many)
- Course → CourseStudent (one-to-many)
- Course → CourseRequest (one-to-many)
- Course → Quiz (one-to-many)
- Course ↔ User (many-to-many via course_tutors)

### Module Relationships
- CourseModule → ModuleFile (one-to-many)
- CourseModule → Quiz (one-to-many)

### Quiz Relationships
- Quiz → Question (one-to-many)
- Quiz → QuizAttempt (one-to-many)
- Question → QuestionOption (one-to-many)
- Question → Answer (one-to-many)
- QuizAttempt → Answer (one-to-many)

## Indexes

The database uses strategic indexes for performance:

- Foreign key columns are indexed
- Frequently queried columns (status, created_at, etc.)
- Composite indexes for common query patterns
- Unique constraints where appropriate

## Database Migrations

Migrations are managed using Flask-Migrate (Alembic). See [Database Migrations](../development/migrations.md) for details.
