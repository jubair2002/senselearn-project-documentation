# Notifications System

## Overview

The notification system provides real-time updates to users about important events and activities.

## Notification Types

- `info` - General information
- `success` - Success messages
- `warning` - Warnings
- `error` - Error notifications

## Notification Events

### Student Notifications

- Course enrollment approved/rejected
- New course available
- Quiz assigned
- Quiz results available
- Course request status updates

### Tutor Notifications

- New course assignment
- Student enrollment request
- Course request response
- Verification status updates

### Admin Notifications

- New tutor registration
- Verification requests
- System alerts

## Notification Model

```python
id: Integer
user_id: Integer (Foreign Key)
title: String(255)
message: Text
notification_type: String(50)  # info, success, warning, error
is_read: Boolean
created_at: DateTime
read_at: DateTime (nullable)
link: String(500) (nullable)  # Optional link to related page
```

## Features

### Read/Unread Status

- Notifications start as unread
- Mark as read when user views them
- Track read timestamp

### Links

Notifications can include optional links to:
- Course pages
- Quiz pages
- Profile pages
- Dashboard sections

### Filtering

- Filter by read/unread status
- Filter by notification type
- Sort by creation date

## API Endpoints

Notifications are typically accessed through:

- Dashboard APIs (included in dashboard responses)
- Dedicated notification endpoints (if implemented)

## Best Practices

1. **Keep messages concise**: Short, actionable messages
2. **Use appropriate types**: Match notification type to importance
3. **Include links**: Link to relevant pages when possible
4. **Auto-mark read**: Mark as read when user clicks link
5. **Cleanup old notifications**: Archive or delete old notifications
