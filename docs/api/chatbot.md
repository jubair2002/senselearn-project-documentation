# Chatbot API

Base URL: `/student/chatbot/api`

All endpoints require authentication and student role.

## Conversations

### GET /conversations

Get all conversations for the current student.

**Query Parameters:**
- `archived` - Filter by archived status (true/false)

**Response:**
```json
{
  "success": true,
  "conversations": [
    {
      "id": 1,
      "title": "Question about Python",
      "created_at": "2024-01-01T12:00:00Z",
      "updated_at": "2024-01-01T12:30:00Z",
      "is_archived": false,
      "message_count": 5
    }
  ]
}
```

### POST /conversations

Create a new conversation.

**Request Body:**
```json
{
  "title": "New Conversation"  // Optional, auto-generated if not provided
}
```

**Response:**
```json
{
  "success": true,
  "conversation": {
    "id": 1,
    "title": "New Conversation",
    "created_at": "2024-01-01T12:00:00Z"
  }
}
```

### GET /conversations/:conversation_id/messages

Get messages for a conversation.

**Response:**
```json
{
  "success": true,
  "messages": [
    {
      "id": 1,
      "role": "user",
      "content": "What is Python?",
      "created_at": "2024-01-01T12:00:00Z"
    },
    {
      "id": 2,
      "role": "assistant",
      "content": "Python is a high-level programming language...",
      "created_at": "2024-01-01T12:00:05Z"
    }
  ]
}
```

### POST /conversations/:conversation_id/messages

Send a message in a conversation.

**Request Body:**
```json
{
  "content": "What is Python?",
  "conversation_id": 1  // Optional, will use URL parameter if not provided
}
```

**Response:**
```json
{
  "success": true,
  "message": {
    "id": 3,
    "role": "user",
    "content": "What is Python?",
    "created_at": "2024-01-01T12:35:00Z"
  },
  "response": {
    "id": 4,
    "role": "assistant",
    "content": "Python is a high-level programming language...",
    "created_at": "2024-01-01T12:35:05Z"
  }
}
```

### POST /conversations/:conversation_id/archive

Archive a conversation.

**Response:**
```json
{
  "success": true,
  "message": "Conversation archived"
}
```

### DELETE /conversations/:conversation_id

Delete a conversation.

**Response:**
```json
{
  "success": true,
  "message": "Conversation deleted"
}
```

## Documents

### POST /documents/upload

Upload a document for chatbot context.

**Request:** Multipart form data
- `file` - Document file (PDF, DOCX, TXT)
- `conversation_id` - Optional conversation ID

**Response:**
```json
{
  "success": true,
  "document": {
    "id": 1,
    "original_filename": "lecture.pdf",
    "file_type": "pdf",
    "file_size": 1024000,
    "uploaded_at": "2024-01-01T12:00:00Z"
  }
}
```

### GET /documents/:document_id/audio

Get audio file for a document (text-to-speech).

**Response:**
- Returns audio file (MP3/WAV)
- Content-Type: `audio/mpeg` or `audio/wav`

**Status Codes:**
- `200` - Success
- `404` - Document or audio not found
- `500` - Audio generation failed

## Features

- **Context-Aware Responses**: Uses uploaded documents and course materials
- **Text-to-Speech**: Generate audio from documents
- **Conversation History**: Persistent conversation storage
- **Document Processing**: Supports PDF, DOCX, and TXT files
