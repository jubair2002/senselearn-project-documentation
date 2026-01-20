# SenseLearn Learning Hub

Welcome to the documentation for **SenseLearn Learning Hub** - a Flask-based learning management system for students, tutors, and administrators.

## Overview

SenseLearn Learning Hub is a learning management system that supports multiple user roles. The platform provides course management, interactive quizzes, AI chatbot assistance, and security features.

## Key Features

- **Course Management** - Create and manage courses with modules and files
- **Quiz System** - Interactive quizzes with multiple question types
- **AI Chatbot** - Context-aware assistance for students
- **Security** - CSRF protection, rate limiting, and account security
- **Progress Tracking** - Monitor student progress and completion

## Quick Start

1. Install dependencies: `pip install -r requirements.txt`
2. Configure environment variables in `.env`
3. Initialize database: `flask db upgrade`
4. Run application: `python3 app.py`

For detailed instructions, see [Quick Start Guide](getting-started/quick-start.md).

## Repository

This project is open source and available on GitHub:

**GitHub Repository**: [https://github.com/jubair2002/senseLearn-learning-hub](https://github.com/jubair2002/senseLearn-learning-hub)

## Documentation

- **[Getting Started](getting-started/quick-start.md)** - Installation and setup
- **[Features](features/student.md)** - Platform features and capabilities
- **[Setup](development/setup.md)** - Development and deployment
- **[API Reference](api/index.md)** - API endpoints and usage
