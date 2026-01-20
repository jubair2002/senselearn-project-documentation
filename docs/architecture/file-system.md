# File System Architecture

## Upload Directory Structure

Files are organized in the uploads directory as follows:

```
uploads/
├── temp/                          # Temporary files (pending registrations)
│   └── email_at_domain_com/       # Email-based temp directories
│       └── document.pdf
├── tutors/                        # Tutor verification documents
│   └── {tutor_id}/
│       ├── certificate.pdf
│       └── recommendation.pdf
└── courses/                       # Course materials
    └── {course_id}/
        └── modules/
            └── {module_id}/
                ├── lecture.pdf
                ├── slides.pptx
                └── video.mp4
```

## File Serving

### Endpoint

`GET /uploads/<path:file_path>`

### Security

- **Authentication Required**: All file access requires login
- **Permission Checks**: 
  - Tutor files: Only tutor owner or admin can access
  - Course files: Only enrolled students, assigned tutors, or admins can access
- **Path Traversal Protection**: Normalized paths prevent directory traversal attacks

### Video Streaming

Video files support HTTP Range requests for efficient streaming:

- **Range Header Support**: `Range: bytes=start-end`
- **Partial Content**: Returns `206 Partial Content` for range requests
- **Accept-Ranges**: Header indicates byte-range support
- **Chunked Transfer**: 64KB chunks for efficient streaming

### File Types

Supported file types (configurable via `ALLOWED_EXTENSIONS`):

- **Documents**: PDF, DOC, DOCX, TXT
- **Images**: JPG, JPEG, PNG, GIF
- **Presentations**: PPT, PPTX
- **Videos**: MP4, WEBM, AVI, MOV, MKV

### File Metadata

Each file stores:
- Original filename
- File path (relative to uploads directory)
- File type/extension
- File size (bytes)
- MIME type
- Upload timestamp
- Uploader ID

## File Utilities

Located in `src/common/file_utils.py`:

- `save_uploaded_file()` - Save uploaded file securely
- `delete_file()` - Delete file with permission checks
- `get_file_url()` - Generate secure file URL

## Configuration

File upload settings in `.env`:

```env
UPLOAD_DIR=/path/to/uploads
MAX_FILE_SIZE=10485760  # 10MB
ALLOWED_EXTENSIONS=pdf,doc,docx,jpg,jpeg,png,ppt,pptx,gif,txt,mp4,webm,avi,mov,mkv
```

## Best Practices

1. **Never trust user-provided filenames**: Use `secure_filename()` from Werkzeug
2. **Validate file types**: Check both extension and MIME type
3. **Limit file size**: Enforce `MAX_FILE_SIZE`
4. **Store outside web root**: Keep uploads directory outside public web directory
5. **Use unique filenames**: Prevent filename collisions
6. **Check permissions**: Always verify user has access rights
