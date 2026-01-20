# Testing

## Test Structure

Tests are located in the `test/` directory:

- `test_auth.py` - Authentication tests
- `test_student.py` - Student functionality tests
- `test_tutor.py` - Tutor functionality tests
- `test_admin.py` - Admin functionality tests
- `test_integration.py` - Integration tests

## Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src

# Run specific test file
pytest test/test_auth.py
```

## Security Tests

Security tests are located in `security_test/` directory and can be run using:

```bash
./security_test/run_all_tests.sh
```
