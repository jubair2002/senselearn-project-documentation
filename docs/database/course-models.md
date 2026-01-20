# Course Models

## Course Model

Main course entity created by admins.

### Fields

```python
id: Integer (Primary Key)
name: String(255) - Indexed
description: Text - Nullable
target_disability_types: String(255) - Nullable (comma-separated)
created_at: DateTime - Indexed
created_by: Integer (Foreign Key -> users.id) - Indexed, nullable
```

### Relationships

- `creator` - User (admin who created)
- `tutors` - User (many-to-many via course_tutors)
- `modules` - CourseModule (one-to-many)
- `enrollments` - CourseStudent (one-to-many)
- `requests` - CourseRequest (one-to-many)
- `quizzes` - Quiz (one-to-many)
- `student_progress` - StudentFileProgress (one-to-many)

## CourseModule Model

Course modules/lessons created by tutors.

### Fields

```python
id: Integer (Primary Key)
course_id: Integer (Foreign Key -> courses.id) - Indexed
name: String(255)
description: Text - Nullable
order_index: Integer - Default 0, indexed
created_at: DateTime - Indexed
created_by: Integer (Foreign Key -> users.id) - Nullable
```

### Relationships

- `course` - Course
- `creator` - User (tutor who created)
- `files` - ModuleFile (one-to-many)
- `quizzes` - Quiz (one-to-many)

### Indexes

- Composite index on (course_id, order_index)

## ModuleFile Model

Files within course modules.

### Fields

```python
id: Integer (Primary Key)
module_id: Integer (Foreign Key -> course_modules.id) - Indexed
file_name: String(255)
file_path: String(500) - Unique
file_type: String(50) - Indexed (pdf, pptx, doc, etc.)
file_size: Integer
mime_type: String(100) - Nullable
uploaded_at: DateTime - Indexed
uploaded_by: Integer (Foreign Key -> users.id) - Nullable
```

### Relationships

- `module` - CourseModule
- `uploader` - User (tutor who uploaded)
- `student_progress` - StudentFileProgress (one-to-many)

### Indexes

- Composite index on (module_id, uploaded_at)

## CourseStudent Model

Student enrollment in courses.

### Fields

```python
id: Integer (Primary Key)
course_id: Integer (Foreign Key -> courses.id) - Indexed
student_id: Integer (Foreign Key -> users.id) - Indexed
status: String(20) - Default 'enrolled', indexed
assigned_by: Integer (Foreign Key -> users.id) - Nullable
assigned_at: DateTime - Indexed
```

### Status Values

- `enrolled` - Student is enrolled
- `pending` - Enrollment pending approval
- `rejected` - Enrollment rejected

### Relationships

- `course` - Course
- `student` - User
- `assigner` - User (admin or tutor who assigned)

### Constraints

- Unique constraint on (course_id, student_id)

### Indexes

- Composite index on (course_id, status)

## CourseRequest Model

Student requests to join courses.

### Fields

```python
id: Integer (Primary Key)
course_id: Integer (Foreign Key -> courses.id) - Indexed
student_id: Integer (Foreign Key -> users.id) - Indexed
tutor_id: Integer (Foreign Key -> users.id) - Indexed
status: String(20) - Default 'pending', indexed
requested_at: DateTime - Indexed
responded_at: DateTime - Nullable
```

### Status Values

- `pending` - Request pending tutor review
- `accepted` - Request accepted
- `rejected` - Request rejected

### Relationships

- `course` - Course
- `student` - User
- `tutor` - User (tutor who reviews)

### Indexes

- Composite index on (tutor_id, status)
- Composite index on (course_id, student_id)

## course_tutors Table

Many-to-many association table for Course-Tutor relationships.

### Columns

```python
course_id: Integer (Foreign Key -> courses.id) - Primary Key, indexed
tutor_id: Integer (Foreign Key -> users.id) - Primary Key, indexed
assigned_at: DateTime
assigned_by: Integer (Foreign Key -> users.id)
```

### Indexes

- Composite index on (course_id, tutor_id)

## StudentFileProgress Model

Tracks student progress on course files.

### Fields

```python
id: Integer (Primary Key)
student_id: Integer (Foreign Key -> users.id) - Indexed
file_id: Integer (Foreign Key -> module_files.id) - Indexed
course_id: Integer (Foreign Key -> courses.id) - Indexed
first_viewed_at: DateTime
last_viewed_at: DateTime
view_count: Integer - Default 1
```

### Relationships

- `student` - User
- `file` - ModuleFile
- `course` - Course

### Constraints

- Unique constraint on (student_id, file_id)

### Indexes

- Composite index on (student_id, course_id)
- Composite index on (course_id, student_id)
