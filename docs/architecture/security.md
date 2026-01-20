# Security

## Security Features

The application implements multiple layers of security:

### CSRF Protection
- Cross-Site Request Forgery protection on all forms
- Token-based validation

### Rate Limiting
- API endpoint rate limiting
- Login attempt throttling

### Account Lockout
- Automatic account lockout after multiple failed login attempts
- Configurable lockout duration

### Input Validation
- Comprehensive input sanitization
- SQL injection prevention
- XSS protection

### Security Headers
- Content Security Policy
- X-Frame-Options
- X-Content-Type-Options
