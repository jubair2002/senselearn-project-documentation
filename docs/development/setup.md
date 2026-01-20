# Development Setup

## Prerequisites

- Python 3.9 or higher
- MySQL 5.7 or higher
- Git
- Virtual environment tool (venv)

## Initial Setup

### 1. Clone Repository

```bash
git clone <repository-url>
cd senseLearn-learning-hub
```

### 2. Create Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Database Setup

```bash
# Create database
mysql -u root -p
CREATE DATABASE senselearn_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
EXIT;
```

### 5. Configure Environment

Create `.env` file (see [Environment Variables](../getting-started/environment-variables.md)):

```env
SECRET_KEY=development-secret-key
DB_USER=root
DB_PASSWORD=yourpassword
DB_HOST=localhost
DB_PORT=3306
DB_NAME=senselearn_db
FLASK_ENV=development
FLASK_DEBUG=true
```

### 6. Initialize Database

```bash
# Run migrations
flask db upgrade

# Or create tables directly
python3 -c "from src import create_app, db; app = create_app(); app.app_context().push(); db.create_all()"
```

### 7. Run Development Server

```bash
python3 app.py
```

Application will be available at `http://localhost:5000`

## Development Tools

### Code Formatting

Use Python's built-in formatter or black:

```bash
pip install black
black src/
```

### Linting

Use flake8 or pylint:

```bash
pip install flake8
flake8 src/
```

### Testing

Run tests with pytest:

```bash
pytest
pytest --cov=src  # With coverage
pytest test/test_auth.py  # Specific test file
```

## IDE Setup

### VS Code

Recommended extensions:
- Python
- Pylance
- Python Docstring Generator
- SQLAlchemy

### PyCharm

- Configure Python interpreter to use venv
- Set up database connection
- Configure Flask run configuration

## Database Migrations

### Create Migration

```bash
flask db migrate -m "Description of changes"
```

### Apply Migration

```bash
flask db upgrade
```

### Rollback Migration

```bash
flask db downgrade
```

## Debugging

### Flask Debug Mode

Set in `.env`:
```env
FLASK_DEBUG=true
```

### SQL Query Logging

Set in `.env`:
```env
SQLALCHEMY_ECHO=true
```

### Debugger

Use Python debugger:
```python
import pdb; pdb.set_trace()
```

## Common Issues

### Port Already in Use

```bash
# Find process using port 5000
lsof -ti:5000 | xargs kill -9
```

### Database Connection Error

- Check MySQL is running
- Verify credentials in `.env`
- Check database exists

### Import Errors

- Ensure virtual environment is activated
- Check Python path
- Verify all dependencies installed

## Project Structure

See [Application Structure](../architecture/application-structure.md) for detailed project layout.
