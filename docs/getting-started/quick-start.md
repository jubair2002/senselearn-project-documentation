# Quick Start Guide

Get the SenseLearn Learning Hub up and running in minutes.

## Prerequisites

- Python 3.9 or higher
- MySQL 5.7 or higher
- pip (Python package manager)

## Step 1: Clone and Setup

```bash
# Navigate to project directory
cd senseLearn-learning-hub

# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## Step 2: Database Setup

```bash
# Create MySQL database
mysql -u root -p
CREATE DATABASE senselearn_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
EXIT;
```

## Step 3: Configure Environment

Create a `.env` file in the project root (see [Environment Variables](environment-variables.md)):

```env
SECRET_KEY=your-secret-key
DB_USER=root
DB_PASSWORD=yourpassword
DB_HOST=localhost
DB_PORT=3306
DB_NAME=senselearn_db
# ... other variables
```

## Step 4: Initialize Database

```bash
# Run migrations
flask db upgrade

# Or create tables directly
python3 -c "from src import create_app, db; app = create_app(); app.app_context().push(); db.create_all()"
```

## Step 5: Run the Application

```bash
python3 app.py
```

The application will be available at `http://localhost:5000`

## Step 6: Create Admin Account

The first admin account must be created manually in the database or through the admin interface if available.

## Next Steps

- Read the [Configuration Guide](configuration.md)
- Explore [Features](../features/student.md)
- Check [API Documentation](../api/index.md)
