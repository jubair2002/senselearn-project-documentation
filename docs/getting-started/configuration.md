# Configuration

## Environment Variables

The application uses environment variables for configuration. Create a `.env` file in the root directory.

### Required Variables

- `DATABASE_URL`: MySQL database connection string
- `SECRET_KEY`: Flask secret key for session management

### Optional Variables

- `MAIL_SERVER`: SMTP server for email functionality
- `MAIL_PORT`: SMTP port
- `MAIL_USERNAME`: Email username
- `MAIL_PASSWORD`: Email password

## Database Configuration

The application uses Flask-SQLAlchemy with MySQL. Configure your database connection in the `.env` file:

```
DATABASE_URL=mysql+pymysql://username:password@host:port/database_name
```

## Security Settings

Security features are configured in `src/security/` directory. Key components include:

- CSRF protection
- Rate limiting
- Account lockout
- Password validation
- Input validation
