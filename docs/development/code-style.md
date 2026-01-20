# Code Style Guide

## Python Style

Follow PEP 8 style guide with these additions:

### Naming Conventions

- **Classes**: `PascalCase` (e.g., `User`, `CourseModule`)
- **Functions/Methods**: `snake_case` (e.g., `get_user`, `create_course`)
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `MAX_FILE_SIZE`)
- **Private**: Prefix with `_` (e.g., `_internal_method`)

### Type Hints

Use type hints for function signatures:

```python
def create_user(email: str, password: str) -> User:
    """Create a new user."""
    ...
```

### Docstrings

Use Google-style docstrings:

```python
def process_file(file_path: str) -> dict:
    """
    Process an uploaded file.
    
    Args:
        file_path: Path to the file to process
        
    Returns:
        Dictionary with file metadata
        
    Raises:
        FileNotFoundError: If file doesn't exist
    """
    ...
```

## Flask-Specific

### Route Decorators

```python
@blueprint.route('/endpoint', methods=['POST'])
@login_required
def endpoint():
    """Endpoint description."""
    ...
```

### Error Handling

Return JSON for API routes:

```python
try:
    result = process_data()
    return jsonify({"success": True, "data": result}), 200
except ValueError as e:
    return jsonify({"success": False, "error": str(e)}), 400
```

## Database

### Query Optimization

- Use `filter_by()` for simple queries
- Use `joinedload()` to avoid N+1 queries
- Add indexes for frequently queried columns
- Use `db.session.get()` for primary key lookups

### Transactions

```python
try:
    # Database operations
    db.session.commit()
except Exception:
    db.session.rollback()
    raise
```

## Security

### Input Validation

Always validate and sanitize user input:

```python
from src.security.input_validator import validate_input

email = validate_input(request.json.get('email'), 'email')
```

### Password Handling

Never store plaintext passwords:

```python
from src.auth.utils import hash_password, verify_password

password_hash = hash_password(password)
is_valid = verify_password(password, password_hash)
```

## File Organization

### Module Structure

```
module_name/
├── __init__.py      # Blueprint registration
├── models.py        # Database models
├── routes.py        # Route handlers
├── utils.py         # Utility functions
└── service.py       # Business logic (if needed)
```

### Import Order

1. Standard library imports
2. Third-party imports
3. Local application imports

```python
# Standard library
import os
from datetime import datetime

# Third-party
from flask import Flask, jsonify
from sqlalchemy import Column

# Local
from src import db
from src.auth.models import User
```

## Comments

- Use comments to explain "why", not "what"
- Keep comments up-to-date with code
- Remove commented-out code

## Testing

### Test Structure

```python
def test_function_name():
    """Test description."""
    # Arrange
    setup_data()
    
    # Act
    result = function_to_test()
    
    # Assert
    assert result == expected
```

## Best Practices

1. **Keep functions small**: Single responsibility
2. **Use meaningful names**: Self-documenting code
3. **Handle errors gracefully**: Proper error messages
4. **Log important events**: Use Flask's logger
5. **Document complex logic**: Add comments where needed
6. **Follow DRY**: Don't repeat yourself
7. **Test edge cases**: Include error scenarios
