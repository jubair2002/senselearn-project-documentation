# Student API

Base URL: `/student/api`

All endpoints require authentication.

## Dashboard

### GET /stats

Get student dashboard statistics.

**Response:**
```json
{
  "success": true,
  "stats": {
    "enrolled_courses": 5,
    "pending_requests": 2,
    "completed_quizzes": 10,
    "total_quizzes": 15
  }
}
```

## Courses

### GET /courses

Get all available courses.

**Query Parameters:**
- `status` - Filter by enrollment status (optional)

**Response:**
```json
{
  "success": true,
  "courses": [
    {
      "id": 1,
      "name": "Introduction to Programming",
      "description": "Learn the basics...",
      "target_disability_types": "visual,hearing",
      "enrollment_status": "enrolled"
    }
  ]
}
```

### GET /courses/enrolled

Get courses the student is enrolled in.

**Response:**
```json
{
  "success": true,
  "courses": [...]
}
```

### GET /courses/:course_id/view

Get detailed course view.

**Response:**
```json
{
  "success": true,
  "course": {
    "id": 1,
    "name": "Course Name",
    "description": "...",
    "modules": [...],
    "tutors": [...]
  }
}
```

### POST /courses/:course_id/request

Request enrollment in a course.

**Request Body:**
```json
{
  "tutor_id": 5
}
```

**Response:**
```json
{
  "success": true,
  "message": "Course request submitted"
}
```

### GET /courses/:course_id/modules

Get modules for a course.

**Response:**
```json
{
  "success": true,
  "modules": [
    {
      "id": 1,
      "name": "Module 1",
      "description": "...",
      "order_index": 0,
      "files": [...]
    }
  ]
}
```

### GET /modules/:module_id/files

Get files for a module.

**Response:**
```json
{
  "success": true,
  "files": [
    {
      "id": 1,
      "file_name": "lecture.pdf",
      "file_type": "pdf",
      "file_size": 1024000,
      "url": "/uploads/..."
    }
  ]
}
```

## Progress Tracking

### POST /progress/file/:file_id/track

Track file viewing progress.

**Request Body:**
```json
{
  "course_id": 1
}
```

**Response:**
```json
{
  "success": true,
  "progress": {
    "view_count": 3,
    "last_viewed_at": "2024-01-01T12:00:00Z"
  }
}
```

### GET /courses/:course_id/progress

Get progress for a course.

**Response:**
```json
{
  "success": true,
  "progress": {
    "total_files": 20,
    "viewed_files": 15,
    "completion_percentage": 75
  }
}
```

## Profile

### GET /profile

Get student profile.

### POST /profile

Update student profile.

**Request Body:**
```json
{
  "full_name": "John Doe",
  "phone_number": "+1234567890",
  "disability_type": "visual"
}
```

## Quiz

See [Quiz API](quiz.md) for quiz-related endpoints.
