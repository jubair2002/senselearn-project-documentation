# Tutor API

Base URL: `/tutor/api`

All endpoints require authentication and tutor role.

## Dashboard

### GET /stats

Get tutor dashboard statistics.

**Response:**
```json
{
  "success": true,
  "stats": {
    "total_courses": 5,
    "total_students": 25,
    "pending_requests": 3
  }
}
```

## Courses

### GET /courses

Get courses assigned to the tutor.

**Response:**
```json
{
  "success": true,
  "courses": [
    {
      "id": 1,
      "name": "Course Name",
      "description": "...",
      "student_count": 10,
      "module_count": 5
    }
  ]
}
```

### GET /courses/:course_id

Get course details.

**Response:**
```json
{
  "success": true,
  "course": {
    "id": 1,
    "name": "Course Name",
    "description": "...",
    "modules": [...],
    "students": [...]
  }
}
```

### PUT /courses/:course_id/description

Update course description.

**Request Body:**
```json
{
  "description": "Updated description"
}
```

### GET /courses/:course_id/view

Get detailed course view with modules and files.

## Modules

### POST /courses/:course_id/modules

Create a new module.

**Request Body:**
```json
{
  "name": "Module Name",
  "description": "Module description",
  "order_index": 0
}
```

**Response:**
```json
{
  "success": true,
  "module": {
    "id": 1,
    "name": "Module Name",
    "course_id": 1
  }
}
```

### GET /courses/:course_id/modules

Get all modules for a course.

### POST /modules/:module_id/files

Upload a file to a module.

**Request:** Multipart form data
- `file` - File to upload
- `file_name` - Optional custom name

**Response:**
```json
{
  "success": true,
  "file": {
    "id": 1,
    "file_name": "lecture.pdf",
    "file_type": "pdf",
    "file_size": 1024000
  }
}
```

### GET /modules/:module_id/files

Get files for a module.

### DELETE /modules/:module_id/files/:file_id

Delete a file from a module.

## Course Requests

### GET /courses/:course_id/requests

Get course enrollment requests.

**Response:**
```json
{
  "success": true,
  "requests": [
    {
      "id": 1,
      "student": {
        "id": 5,
        "full_name": "Student Name",
        "email": "student@example.com"
      },
      "status": "pending",
      "requested_at": "2024-01-01T12:00:00Z"
    }
  ]
}
```

### POST /courses/:course_id/requests/:request_id

Respond to a course request.

**Request Body:**
```json
{
  "action": "accept"  // or "reject"
}
```

## Students

### GET /students

Get all students (for course assignment).

**Query Parameters:**
- `course_id` - Filter by course (optional)

### POST /courses/:course_id/students

Manually enroll a student.

**Request Body:**
```json
{
  "student_id": 5
}
```

### DELETE /courses/:course_id/students/:student_id

Remove a student from a course.

## Progress Tracking

### GET /courses/:course_id/students/progress

Get student progress for a course.

**Response:**
```json
{
  "success": true,
  "progress": [
    {
      "student_id": 5,
      "student_name": "John Doe",
      "total_files": 20,
      "viewed_files": 15,
      "completion_percentage": 75
    }
  ]
}
```

### GET /progress/all

Get progress for all courses.

## Documents

### GET /documents

Get tutor verification documents.

### POST /documents

Upload verification document.

**Request:** Multipart form data
- `file` - Document file
- `file_type` - Type (certificate, recommendation, etc.)

### DELETE /documents/:doc_id

Delete a document.

## Profile

### GET /profile

Get tutor profile.

### POST /profile

Update tutor profile.

**Request Body:**
```json
{
  "full_name": "Jane Tutor",
  "qualifications": "MSc in Education",
  "experience_years": 5,
  "subjects": "Math, Physics",
  "hourly_rate": 50.00,
  "bio": "Experienced tutor..."
}
```
