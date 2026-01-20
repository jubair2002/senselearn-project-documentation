# Authentication API

Base URL: `/api/auth`

## Endpoints

### GET /

Get authentication API information.

**Response:**
```json
{
  "status": "ok",
  "message": "Auth API is running",
  "endpoints": [
    "/api/auth/register",
    "/api/auth/login",
    "/api/auth/verify-email",
    "/api/auth/resend-otp",
    "/api/auth/forgot",
    "/api/auth/reset"
  ]
}
```

### POST /register

Register a new user account.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword",
  "full_name": "John Doe",
  "user_type": "student",
  "disability_type": "visual",  // Optional, for students
  "phone_number": "+1234567890",  // Optional
  "username": "johndoe"  // Optional
}
```

**Response (Success):**
```json
{
  "success": true,
  "message": "Registration successful. Please verify your email.",
  "pending_registration_id": 123
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": "Email already registered"
}
```

**Status Codes:**
- `200` - Success
- `400` - Bad request (validation error)
- `409` - Conflict (email already exists)
- `429` - Too many requests (rate limited)

### POST /login

Authenticate and log in a user.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response (Success):**
```json
{
  "success": true,
  "message": "Login successful",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "full_name": "John Doe",
    "user_type": "student",
    "email_verified": true
  }
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": "Invalid email or password"
}
```

**Status Codes:**
- `200` - Success
- `401` - Unauthorized (invalid credentials)
- `403` - Forbidden (account locked)
- `429` - Too many requests

### POST /logout

Log out the current user.

**Response:**
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

**Status Codes:**
- `200` - Success

### POST /verify-email

Verify email address with OTP.

**Request Body:**
```json
{
  "email": "user@example.com",
  "otp": "123456"
}
```

**Response (Success):**
```json
{
  "success": true,
  "message": "Email verified successfully"
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": "Invalid or expired OTP"
}
```

**Status Codes:**
- `200` - Success
- `400` - Bad request (invalid OTP)
- `404` - Not found (email/OTP not found)

### POST /resend-otp

Resend verification OTP.

**Request Body:**
```json
{
  "email": "user@example.com"
}
```

**Response:**
```json
{
  "success": true,
  "message": "OTP sent successfully"
}
```

**Status Codes:**
- `200` - Success
- `400` - Bad request
- `429` - Too many requests

### POST /forgot

Request password reset.

**Request Body:**
```json
{
  "email": "user@example.com"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Password reset code sent to email"
}
```

**Status Codes:**
- `200` - Success (always returns success for security)

### POST /reset

Reset password with reset code.

**Request Body:**
```json
{
  "email": "user@example.com",
  "code": "ABC123",
  "new_password": "newsecurepassword"
}
```

**Response (Success):**
```json
{
  "success": true,
  "message": "Password reset successful"
}
```

**Response (Error):**
```json
{
  "success": false,
  "error": "Invalid or expired reset code"
}
```

**Status Codes:**
- `200` - Success
- `400` - Bad request (invalid code)
- `404` - Not found

## Security Features

- **Rate Limiting**: All endpoints are rate limited
- **CSRF Protection**: Enabled for state-changing operations
- **Account Lockout**: After multiple failed login attempts
- **Password Validation**: Enforced password strength requirements
- **Input Validation**: All inputs are validated and sanitized

## Tutor Registration

For tutor registration, additional fields are required:

```json
{
  "email": "tutor@example.com",
  "password": "securepassword",
  "full_name": "Jane Tutor",
  "user_type": "tutor",
  "qualifications": "MSc in Education",
  "experience_years": 5,
  "subjects": "Mathematics, Physics",
  "hourly_rate": 50.00,
  "bio": "Experienced tutor..."
}
```
