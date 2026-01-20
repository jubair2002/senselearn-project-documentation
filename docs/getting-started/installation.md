# Installation

This guide will help you set up the SenseLearn Learning Hub on your local machine.

## Prerequisites

- Python 3.9 or higher
- pip (Python package installer)
- MySQL database server

## Steps

1. **Clone the repository** (if applicable)
   ```bash
   git clone <repository-url>
   cd senseLearn-learning-hub
   ```

2. **Create a virtual environment**
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**
   Create a `.env` file in the root directory with your configuration:
   ```env
   DATABASE_URL=mysql+pymysql://user:password@localhost/dbname
   SECRET_KEY=your-secret-key-here
   ```

5. **Run database migrations**
   ```bash
   flask db upgrade
   ```

6. **Run the application**
   ```bash
   python3 app.py
   ```

The application will be available at `http://localhost:5000`
