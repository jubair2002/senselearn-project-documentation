# Database Migrations

## Overview

The application uses Flask-Migrate (Alembic) for database migrations.

## Migration Files

Migrations are located in the `migrations/versions/` directory.

## Creating Migrations

```bash
# Create a new migration
flask db migrate -m "Description of changes"

# Review the generated migration file
# Edit if necessary
```

## Applying Migrations

```bash
# Apply all pending migrations
flask db upgrade

# Rollback last migration
flask db downgrade
```

## Migration History

Key migrations include:
- User authentication tables
- Course management tables
- Quiz system tables
- Chatbot tables
- Notification system
