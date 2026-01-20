# Chatbot Models

## ChatConversation Model

Stores chatbot conversations for students.

### Fields

```python
id: Integer (Primary Key)
student_id: Integer (Foreign Key -> users.id) - Indexed
title: String(255) - Nullable (auto-generated from first message)
created_at: DateTime - Indexed
updated_at: DateTime - Indexed
is_archived: Boolean - Default False, indexed
```

### Relationships

- `student` - User
- `messages` - ChatMessage (one-to-many)
- `documents` - ChatbotDocument (one-to-many)

### Methods

- `to_dict() -> dict` - Convert to dictionary

### Indexes

- Composite index on (student_id, updated_at)

## ChatMessage Model

Individual messages within conversations.

### Fields

```python
id: Integer (Primary Key)
conversation_id: Integer (Foreign Key -> chat_conversations.id) - Indexed
role: String(20) - Indexed ('user' or 'assistant')
content: Text
created_at: DateTime - Indexed
message_metadata: Text - Nullable (JSON string)
```

### Roles

- `user` - Student message
- `assistant` - Bot response

### Relationships

- `conversation` - ChatConversation

### Methods

- `to_dict() -> dict` - Convert to dictionary

### Indexes

- Composite index on (conversation_id, created_at)

## ChatbotDocument Model

Documents uploaded to chatbot for text-to-speech.

### Fields

```python
id: Integer (Primary Key)
conversation_id: Integer (Foreign Key -> chat_conversations.id) - Nullable, indexed
student_id: Integer (Foreign Key -> users.id) - Indexed
original_filename: String(255)
file_path: String(500)
file_type: String(50) - pdf, docx, txt, etc.
file_size: Integer
extracted_text: Text - Nullable
audio_path: String(500) - Nullable
uploaded_at: DateTime - Indexed
```

### Relationships

- `conversation` - ChatConversation (nullable)
- `student` - User

### Indexes

- Composite index on (student_id, uploaded_at)
- Index on conversation_id
