# API Reference

Welcome to the SenseLearn Learning Hub API documentation. All APIs follow RESTful conventions and return JSON responses.

## Base URLs

- **Authentication**: `/api/auth`
- **Student**: `/student/api`
- **Tutor**: `/tutor/api`
- **Admin**: `/admin/api`
- **Quiz**: `/quiz/api`
- **Chatbot**: `/student/chatbot/api`

## Authentication

Most endpoints require authentication. Include session cookies or use the authentication endpoints to obtain a session.

### Response Format

All API responses follow this format:

**Success:**
```json
{
  "success": true,
  "data": {...},
  "message": "Optional message"
}
```

**Error:**
```json
{
  "success": false,
  "error": "Error message",
  "code": "ERROR_CODE"  // Optional
}
```

### Status Codes

- `200` - Success
- `201` - Created
- `400` - Bad Request (validation error)
- `401` - Unauthorized (authentication required)
- `403` - Forbidden (insufficient permissions)
- `404` - Not Found
- `409` - Conflict (duplicate resource)
- `429` - Too Many Requests (rate limited)
- `500` - Internal Server Error

## API Documentation

### [Authentication API](authentication.md)
User registration, login, logout, email verification, and password reset.

**Key Endpoints:**
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Authenticate user
- `POST /api/auth/verify-email` - Verify email with OTP
- `POST /api/auth/forgot` - Request password reset
- `POST /api/auth/reset` - Reset password

### [Student API](student.md)
Student dashboard, course browsing, enrollment requests, and progress tracking.

**Key Endpoints:**
- `GET /student/api/stats` - Dashboard statistics
- `GET /student/api/courses` - List available courses
- `POST /student/api/courses/:id/request` - Request enrollment
- `GET /student/api/courses/:id/modules` - View course modules
- `POST /student/api/progress/file/:id/track` - Track file viewing

### [Tutor API](tutor.md)
Course management, module creation, file uploads, student management, and progress viewing.

**Key Endpoints:**
- `GET /tutor/api/courses` - List assigned courses
- `POST /tutor/api/courses/:id/modules` - Create module
- `POST /tutor/api/modules/:id/files` - Upload file
- `GET /tutor/api/courses/:id/students/progress` - View student progress
- `POST /tutor/api/courses/:id/requests/:id` - Respond to enrollment request

### [Admin API](admin.md)
User management, course creation, tutor verification, and system administration.

**Key Endpoints:**
- `GET /admin/api/stats` - Dashboard statistics
- `GET /admin/api/tutors` - List all tutors
- `POST /admin/api/verify-tutor` - Verify tutor account
- `POST /admin/api/courses` - Create course
- `POST /admin/api/create-account` - Create user account

### [Quiz API](quiz.md)
Quiz creation (tutors), quiz taking (students), question management, and attempt tracking.

**Key Endpoints:**
- `POST /quiz/api/courses/:id/quizzes` - Create quiz (tutor)
- `POST /quiz/api/quizzes/:id/start` - Start quiz attempt (student)
- `POST /quiz/api/attempts/:id/answer` - Submit answer
- `POST /quiz/api/attempts/:id/submit` - Submit quiz
- `GET /quiz/api/quizzes/:id/attempts` - View attempts

### [Chatbot API](chatbot.md)
Conversation management, messaging, document uploads, and text-to-speech.

**Key Endpoints:**
- `GET /student/chatbot/api/conversations` - List conversations
- `POST /student/chatbot/api/conversations/:id/messages` - Send message
- `POST /student/chatbot/api/documents/upload` - Upload document
- `GET /student/chatbot/api/documents/:id/audio` - Get audio file

## Rate Limiting

All endpoints are rate limited to prevent abuse. Rate limits vary by endpoint:

- **Authentication endpoints**: Stricter limits (e.g., 5 requests per minute)
- **General endpoints**: Standard limits (e.g., 60 requests per minute)

Rate limit headers are included in responses:
```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1640995200
```

## Security

### CSRF Protection
State-changing operations require CSRF tokens. Include CSRF token in requests:
```javascript
headers: {
  'X-CSRFToken': getCsrfToken()
}
```

### Account Lockout
After multiple failed login attempts, accounts are temporarily locked.

### Input Validation
All inputs are validated and sanitized. Invalid inputs return `400 Bad Request`.

## Error Handling

### Validation Errors
```json
{
  "success": false,
  "error": "Validation failed",
  "errors": {
    "email": "Invalid email format",
    "password": "Password must be at least 8 characters"
  }
}
```

### Authentication Errors
```json
{
  "success": false,
  "error": "Authentication required"
}
```

### Permission Errors
```json
{
  "success": false,
  "error": "Insufficient permissions"
}
```

## Pagination

List endpoints may support pagination:

**Query Parameters:**
- `page` - Page number (default: 1)
- `per_page` - Items per page (default: 20)

**Response:**
```json
{
  "success": true,
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 20,
    "total": 100,
    "pages": 5
  }
}
```

## Best Practices

1. **Handle Errors Gracefully**: Always check `success` field
2. **Use Appropriate HTTP Methods**: GET for retrieval, POST for creation
3. **Include Required Headers**: Content-Type, CSRF tokens
4. **Respect Rate Limits**: Implement exponential backoff
5. **Validate Inputs**: Client-side validation before API calls
6. **Handle Timeouts**: Set appropriate request timeouts
7. **Log Errors**: Log API errors for debugging

## Testing

API endpoints can be tested using:
- **cURL**: Command-line tool
- **Postman**: API testing tool
- **Browser DevTools**: Network tab for debugging
- **Python requests**: For automated testing

Example cURL request:
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"password"}'
```
