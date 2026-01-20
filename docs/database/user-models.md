# User Models

## User Model

Main user model supporting students, tutors, and admins.

### Fields

```python
id: Integer (Primary Key)
email: String(255) - Unique, indexed
password_hash: String(255)
created_at: DateTime
user_type: String(20) - 'student', 'tutor', or 'admin'
full_name: String(255)
username: String(50) - Unique, nullable
phone_number: String(20) - Nullable

# Student-specific fields
disability_type: String(50) - Nullable

# Tutor-specific fields
qualifications: Text - Nullable
experience_years: Integer - Nullable
subjects: String(255) - Nullable
hourly_rate: Numeric(10, 2) - Nullable
bio: Text - Nullable
is_verified: Boolean - Default False

# Email verification
email_verified: Boolean - Default False
```

### Methods

- `is_student() -> bool` - Check if user is a student
- `is_tutor() -> bool` - Check if user is a tutor
- `is_admin() -> bool` - Check if user is an admin

### Relationships

- `reset_codes` - PasswordResetCode (one-to-many)
- `verification_otps` - EmailVerificationOTP (one-to-many)
- `documents` - TutorDocument (one-to-many, tutors only)
- `course_enrollments` - CourseStudent (one-to-many)
- `course_requests` - CourseRequest (one-to-many)
- `assigned_courses` - Course (many-to-many via course_tutors)
- `chat_conversations` - ChatConversation (one-to-many)
- `notifications` - Notification (one-to-many)

## PasswordResetCode Model

Stores password reset codes for users.

### Fields

```python
id: Integer (Primary Key)
user_id: Integer (Foreign Key -> users.id)
code: String(20) - Indexed
expires_at: DateTime
created_at: DateTime
```

### Class Methods

- `create_for_user(user, code, minutes_valid=15)` - Create reset code

## EmailVerificationOTP Model

Stores email verification OTPs.

### Fields

```python
id: Integer (Primary Key)
user_id: Integer (Foreign Key -> users.id) - Nullable, indexed
email: String(255) - Nullable, indexed
otp: String(20) - Indexed
expires_at: DateTime - Indexed
created_at: DateTime - Indexed
purpose: String(50) - Default 'verification', indexed
```

### Indexes

- Composite index on (email, purpose)
- Composite index on (user_id, purpose)
- Composite index on (email, otp, purpose)

### Class Methods

- `create_for_user(user, otp, minutes_valid=10, purpose='verification')`
- `create_for_email(email, otp, minutes_valid=10, purpose='verification')`

## PendingRegistration Model

Temporary storage for registration data before email verification.

### Fields

```python
id: Integer (Primary Key)
email: String(255) - Unique, indexed
registration_data: Text - JSON string
expires_at: DateTime
created_at: DateTime
```

### Methods

- `get_registration_data() -> dict` - Parse JSON registration data

## TutorDocument Model

Stores tutor verification documents.

### Fields

```python
id: Integer (Primary Key)
tutor_id: Integer (Foreign Key -> users.id) - Indexed
file_name: String(255)
file_path: String(500) - Unique
file_type: String(50) - Indexed (certificate, recommendation, etc.)
file_size: Integer
mime_type: String(100) - Nullable
uploaded_at: DateTime - Indexed
uploaded_by_admin: Boolean - Default False
```

### Indexes

- Composite index on (tutor_id, uploaded_at)
