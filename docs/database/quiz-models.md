# Quiz Models

## Quiz Model

Quiz entity that can be attached to courses or modules.

### Fields

```python
id: Integer (Primary Key)
course_id: Integer (Foreign Key -> courses.id) - Indexed
module_id: Integer (Foreign Key -> course_modules.id) - Nullable, indexed
title: String(255)
description: Text - Nullable
instructions: Text - Nullable
time_limit_minutes: Integer - Nullable
passing_score: Numeric(5, 2) - Default 60.0
max_attempts: Integer - Default 1, nullable
is_active: Boolean - Default True, indexed
order_index: Integer - Default 0, indexed
created_at: DateTime - Indexed
created_by: Integer (Foreign Key -> users.id) - Nullable
```

### Relationships

- `course` - Course
- `module` - CourseModule (nullable)
- `creator` - User
- `questions` - Question (one-to-many)
- `attempts` - QuizAttempt (one-to-many)

### Methods

- `get_total_points() -> float` - Calculate total points
- `get_question_count() -> int` - Get question count

### Indexes

- Composite index on (course_id, module_id)
- Composite index on (course_id, is_active)

## Question Model

Quiz questions supporting multiple types.

### Fields

```python
id: Integer (Primary Key)
quiz_id: Integer (Foreign Key -> quizzes.id) - Indexed
question_type: String(50) - Indexed
question_text: Text
points: Numeric(5, 2) - Default 1.0
order_index: Integer - Default 0, indexed
created_at: DateTime
correct_answer: Text - Nullable
```

### Question Types

- `multiple_choice` - Has options, one correct answer
- `short_answer` - Text answer, case-insensitive comparison
- `true_false` - Boolean answer

### Relationships

- `quiz` - Quiz
- `options` - QuestionOption (one-to-many, for multiple_choice)
- `answers` - Answer (one-to-many)

### Methods

- `get_correct_answer() -> str` - Get correct answer as string

### Indexes

- Composite index on (quiz_id, order_index)

## QuestionOption Model

Options for multiple choice questions.

### Fields

```python
id: Integer (Primary Key)
question_id: Integer (Foreign Key -> quiz_questions.id) - Indexed
option_text: String(500)
is_correct: Boolean - Default False, indexed
order_index: Integer - Default 0, indexed
```

### Relationships

- `question` - Question

### Indexes

- Composite index on (question_id, order_index)
- Composite index on (question_id, is_correct)

## QuizAttempt Model

Student quiz attempts.

### Fields

```python
id: Integer (Primary Key)
quiz_id: Integer (Foreign Key -> quizzes.id) - Indexed
student_id: Integer (Foreign Key -> users.id) - Indexed
started_at: DateTime - Indexed
submitted_at: DateTime - Nullable, indexed
is_completed: Boolean - Default False, indexed
score: Numeric(5, 2) - Nullable
total_points: Numeric(5, 2) - Nullable
percentage_score: Numeric(5, 2) - Nullable
passed: Boolean - Nullable, indexed
time_taken_seconds: Integer - Nullable
```

### Relationships

- `quiz` - Quiz
- `student` - User
- `answers` - Answer (one-to-many)

### Indexes

- Composite index on (quiz_id, student_id)
- Composite index on (student_id, is_completed)
- Composite index on (quiz_id, is_completed)

## Answer Model

Student answers to quiz questions.

### Fields

```python
id: Integer (Primary Key)
attempt_id: Integer (Foreign Key -> quiz_attempts.id) - Indexed
question_id: Integer (Foreign Key -> quiz_questions.id) - Indexed
answer_text: Text - Nullable
option_id: Integer (Foreign Key -> question_options.id) - Nullable
is_correct: Boolean - Nullable
points_earned: Numeric(5, 2) - Nullable
answered_at: DateTime
```

### Relationships

- `attempt` - QuizAttempt
- `question` - Question
- `option` - QuestionOption (nullable)

### Indexes

- Composite index on (attempt_id, question_id)
