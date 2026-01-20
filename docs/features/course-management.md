# Course Management

## Overview

The course management system allows admins to create courses, tutors to manage course content, and students to access learning materials.

## Course Creation (Admin)

Admins can create courses with:

- Course name and description
- Target disability types (visual, hearing, motor, cognitive, other, or "All")
- Automatic assignment of tutors

## Course Structure

```
Course
├── Modules (Lessons)
│   ├── Module Files
│   │   ├── PDFs
│   │   ├── Presentations (PPTX)
│   │   ├── Documents (DOCX)
│   │   ├── Images
│   │   └── Videos
│   └── Quizzes
└── Students (Enrolled)
```

## Module Management (Tutor)

Tutors assigned to courses can:

### Create Modules
- Add modules with name and description
- Set order/index for sequencing
- Organize content logically

### Upload Files
- Upload multiple file types:
  - PDF documents
  - PowerPoint presentations
  - Word documents
  - Images
  - Videos
- Files are organized by module
- Automatic file type detection
- File size validation

### Manage Content
- Edit module descriptions
- Reorder modules
- Delete modules (with confirmation)
- Delete files

## Student Enrollment

### Enrollment Methods

1. **Admin Assignment**: Admin directly enrolls students
2. **Course Request**: Student requests enrollment, tutor approves/rejects
3. **Tutor Assignment**: Tutor manually enrolls students

### Enrollment Status

- `enrolled` - Active enrollment
- `pending` - Awaiting approval
- `rejected` - Request denied

## File Access

Students can:

- View course files
- Download materials
- Track viewing progress
- Access videos with streaming support

## Progress Tracking

The system tracks:

- **File Views**: Which files students have viewed
- **View Count**: How many times each file was accessed
- **First Viewed**: Timestamp of first access
- **Last Viewed**: Timestamp of most recent access
- **Completion Percentage**: Based on total files vs viewed files

Tutors and admins can view:

- Individual student progress
- Course-wide progress statistics
- File-by-file viewing data

## Course Quizzes

Quizzes can be attached to:

- Entire courses (course-level quizzes)
- Specific modules (module-level quizzes)

See [Quiz System](quiz.md) for details.

## API Endpoints

### Admin
- `POST /admin/api/courses` - Create course
- `POST /admin/api/courses/:id/tutors` - Assign tutor
- `POST /admin/api/courses/:id/students` - Enroll student

### Tutor
- `GET /tutor/api/courses` - List assigned courses
- `POST /tutor/api/courses/:id/modules` - Create module
- `POST /tutor/api/modules/:id/files` - Upload file
- `GET /tutor/api/courses/:id/students/progress` - View progress

### Student
- `GET /student/api/courses` - List available courses
- `POST /student/api/courses/:id/request` - Request enrollment
- `GET /student/api/courses/:id/modules` - View modules
- `GET /student/api/modules/:id/files` - View files
- `POST /student/api/progress/file/:id/track` - Track viewing
