# Quiz API

Base URL: `/quiz/api`

## Tutor Endpoints

All tutor endpoints require authentication and tutor role.

### GET /courses/:course_id/quizzes

Get all quizzes for a course.

**Response:**
```json
{
  "success": true,
  "quizzes": [
    {
      "id": 1,
      "title": "Quiz 1",
      "description": "...",
      "module_id": null,
      "question_count": 10,
      "time_limit_minutes": 30,
      "is_active": true,
      "order_index": 0
    }
  ]
}
```

### POST /courses/:course_id/quizzes

Create a new quiz.

**Request Body:**
```json
{
  "title": "Quiz Title",
  "description": "Quiz description",
  "module_id": 5,  // Optional
  "instructions": "Read carefully...",
  "time_limit_minutes": 30,
  "passing_score": 60.0,
  "max_attempts": 3,
  "order_index": 0
}
```

**Response:**
```json
{
  "success": true,
  "quiz": {
    "id": 1,
    "title": "Quiz Title",
    "course_id": 1
  }
}
```

### GET /quizzes/:quiz_id

Get quiz details with questions.

**Response:**
```json
{
  "success": true,
  "quiz": {
    "id": 1,
    "title": "Quiz Title",
    "questions": [
      {
        "id": 1,
        "question_type": "multiple_choice",
        "question_text": "What is 2+2?",
        "points": 1.0,
        "options": [
          {"id": 1, "option_text": "3", "is_correct": false},
          {"id": 2, "option_text": "4", "is_correct": true}
        ]
      }
    ]
  }
}
```

### PUT /quizzes/:quiz_id

Update quiz.

**Request Body:**
```json
{
  "title": "Updated Title",
  "is_active": true
}
```

### DELETE /quizzes/:quiz_id

Delete a quiz.

### POST /quizzes/:quiz_id/questions

Add a question to a quiz.

**Request Body (Multiple Choice):**
```json
{
  "question_type": "multiple_choice",
  "question_text": "What is Python?",
  "points": 2.0,
  "order_index": 0,
  "options": [
    {"option_text": "A programming language", "is_correct": true},
    {"option_text": "A snake", "is_correct": false}
  ]
}
```

**Request Body (Short Answer):**
```json
{
  "question_type": "short_answer",
  "question_text": "What is the capital of France?",
  "points": 1.0,
  "correct_answer": "Paris",
  "order_index": 1
}
```

**Request Body (True/False):**
```json
{
  "question_type": "true_false",
  "question_text": "Python is a compiled language.",
  "points": 1.0,
  "correct_answer": "false",
  "order_index": 2
}
```

### GET /questions/:question_id

Get question details.

### PUT /questions/:question_id

Update a question.

### DELETE /questions/:question_id

Delete a question.

### GET /quizzes/:quiz_id/attempts

Get all quiz attempts.

**Response:**
```json
{
  "success": true,
  "attempts": [
    {
      "id": 1,
      "student": {
        "id": 5,
        "full_name": "John Doe"
      },
      "started_at": "2024-01-01T12:00:00Z",
      "submitted_at": "2024-01-01T12:30:00Z",
      "is_completed": true,
      "score": 85.0,
      "percentage_score": 85.0,
      "passed": true
    }
  ]
}
```

## Student Endpoints

All student endpoints require authentication and student role.

### GET /courses/:course_id/quizzes

Get available quizzes for a course.

**Response:**
```json
{
  "success": true,
  "quizzes": [
    {
      "id": 1,
      "title": "Quiz 1",
      "description": "...",
      "time_limit_minutes": 30,
      "max_attempts": 3,
      "attempts_remaining": 2,
      "has_active_attempt": false
    }
  ]
}
```

### POST /quizzes/:quiz_id/start

Start a quiz attempt.

**Response:**
```json
{
  "success": true,
  "attempt": {
    "id": 1,
    "quiz_id": 1,
    "started_at": "2024-01-01T12:00:00Z",
    "time_limit_minutes": 30
  }
}
```

### GET /attempts/:attempt_id/questions

Get questions for an active attempt.

**Response:**
```json
{
  "success": true,
  "questions": [
    {
      "id": 1,
      "question_type": "multiple_choice",
      "question_text": "What is 2+2?",
      "points": 1.0,
      "options": [
        {"id": 1, "option_text": "3"},
        {"id": 2, "option_text": "4"}
      ],
      "current_answer": null
    }
  ]
}
```

### POST /attempts/:attempt_id/answer

Submit an answer.

**Request Body (Multiple Choice):**
```json
{
  "question_id": 1,
  "option_id": 2
}
```

**Request Body (Short Answer):**
```json
{
  "question_id": 2,
  "answer_text": "Paris"
}
```

**Request Body (True/False):**
```json
{
  "question_id": 3,
  "answer_text": "false"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Answer saved"
}
```

### POST /attempts/:attempt_id/submit

Submit the quiz attempt.

**Response:**
```json
{
  "success": true,
  "attempt": {
    "id": 1,
    "is_completed": true,
    "score": 85.0,
    "total_points": 100.0,
    "percentage_score": 85.0,
    "passed": true,
    "submitted_at": "2024-01-01T12:30:00Z"
  }
}
```

### GET /quizzes/:quiz_id/attempts

Get student's attempts for a quiz.

**Response:**
```json
{
  "success": true,
  "attempts": [
    {
      "id": 1,
      "started_at": "2024-01-01T12:00:00Z",
      "submitted_at": "2024-01-01T12:30:00Z",
      "is_completed": true,
      "score": 85.0,
      "passed": true
    }
  ]
}
```
