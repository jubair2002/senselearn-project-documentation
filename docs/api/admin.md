# Admin API

Base URL: `/admin/api`

All endpoints require authentication and admin role.

## Dashboard

### GET /stats

Get admin dashboard statistics.

**Response:**
```json
{
  "success": true,
  "stats": {
    "total_tutors": 50,
    "total_students": 200,
    "total_courses": 30,
    "pending_verifications": 5
  }
}
```

## Tutors

### GET /tutors

Get all tutors with verification status.

**Query Parameters:**
- `verified` - Filter by verification status (true/false)

**Response:**
```json
{
  "success": true,
  "tutors": [
    {
      "id": 1,
      "email": "tutor@example.com",
      "full_name": "Jane Tutor",
      "is_verified": true,
      "document_count": 3,
      "created_at": "2024-01-01T12:00:00Z"
    }
  ]
}
```

### GET /tutors/:tutor_id/documents

Get documents for a tutor.

**Response:**
```json
{
  "success": true,
  "documents": [
    {
      "id": 1,
      "file_name": "certificate.pdf",
      "file_type": "certificate",
      "uploaded_at": "2024-01-01T12:00:00Z"
    }
  ]
}
```

### POST /verify-tutor

Verify a tutor account.

**Request Body:**
```json
{
  "tutor_id": 1
}
```

**Response:**
```json
{
  "success": true,
  "message": "Tutor verified successfully"
}
```

## Students

### GET /students

Get all students.

**Response:**
```json
{
  "success": true,
  "students": [
    {
      "id": 1,
      "email": "student@example.com",
      "full_name": "John Doe",
      "disability_type": "visual",
      "created_at": "2024-01-01T12:00:00Z"
    }
  ]
}
```

## Account Creation

### POST /create-account

Create a new user account (admin only).

**Request Body:**
```json
{
  "email": "user@example.com",
  "full_name": "User Name",
  "user_type": "student",
  "password": "temporarypassword",
  "disability_type": "visual",  // For students
  "qualifications": "...",  // For tutors
  "send_credentials": true  // Send credentials via email
}
```

**Response:**
```json
{
  "success": true,
  "message": "Account created successfully",
  "user": {
    "id": 1,
    "email": "user@example.com"
  }
}
```

## Courses

### GET /courses

Get all courses.

**Response:**
```json
{
  "success": true,
  "courses": [
    {
      "id": 1,
      "name": "Course Name",
      "description": "...",
      "target_disability_types": "visual,hearing",
      "tutor_count": 2,
      "student_count": 10,
      "created_at": "2024-01-01T12:00:00Z"
    }
  ]
}
```

### POST /courses

Create a new course.

**Request Body:**
```json
{
  "name": "Course Name",
  "description": "Course description",
  "target_disability_types": "visual,hearing"  // or "All"
}
```

**Response:**
```json
{
  "success": true,
  "course": {
    "id": 1,
    "name": "Course Name"
  }
}
```

### GET /courses/:course_id

Get course details.

### POST /courses/:course_id/tutors

Assign tutor to course.

**Request Body:**
```json
{
  "tutor_id": 5
}
```

### DELETE /courses/:course_id/tutors/:tutor_id

Remove tutor from course.

### POST /courses/:course_id/students

Enroll student in course.

**Request Body:**
```json
{
  "student_id": 10
}
```

### DELETE /courses/:course_id/students/:student_id

Remove student from course.
