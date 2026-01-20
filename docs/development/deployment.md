# Deployment Guide

## Production Checklist

- [ ] Set `FLASK_ENV=production`
- [ ] Set `FLASK_DEBUG=false`
- [ ] Use strong `SECRET_KEY`
- [ ] Set `SESSION_COOKIE_SECURE=true` (with HTTPS)
- [ ] Configure production database
- [ ] Set up email service (SMTP)
- [ ] Configure file upload directory
- [ ] Set up SSL/TLS certificates
- [ ] Configure reverse proxy (nginx)
- [ ] Set up process manager (systemd/supervisor)
- [ ] Configure logging
- [ ] Set up backups
- [ ] Configure monitoring

## Environment Variables

Production `.env` should include:

```env
# Flask
SECRET_KEY=<strong-random-key>
FLASK_ENV=production
FLASK_DEBUG=false

# Database
DB_USER=production_user
DB_PASSWORD=<secure-password>
DB_HOST=localhost
DB_PORT=3306
DB_NAME=senselearn_prod

# Security
SESSION_COOKIE_SECURE=true
SESSION_COOKIE_HTTPONLY=true
SESSION_COOKIE_SAMESITE=Lax

# URLs
APP_BASE_URL=https://yourdomain.com
API_BASE_URL=https://yourdomain.com/api

# Email
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your-email@gmail.com
SMTP_PASSWORD=<app-password>
SMTP_FROM_EMAIL=your-email@gmail.com
SMTP_USE_TLS=true

# File Uploads
UPLOAD_DIR=/var/www/uploads
MAX_FILE_SIZE=10485760
```

## Database Setup

### Production Database

```sql
CREATE DATABASE senselearn_prod CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'prod_user'@'localhost' IDENTIFIED BY 'secure_password';
GRANT ALL PRIVILEGES ON senselearn_prod.* TO 'prod_user'@'localhost';
FLUSH PRIVILEGES;
```

### Run Migrations

```bash
flask db upgrade
```

## Web Server Configuration

### Nginx Configuration

```nginx
server {
    listen 80;
    server_name yourdomain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name yourdomain.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static {
        alias /path/to/static;
        expires 30d;
    }
}
```

## Process Management

### Systemd Service

Create `/etc/systemd/system/senselearn.service`:

```ini
[Unit]
Description=SenseLearn Learning Hub
After=network.target

[Service]
User=www-data
WorkingDirectory=/path/to/senseLearn-learning-hub
Environment="PATH=/path/to/venv/bin"
ExecStart=/path/to/venv/bin/python app.py
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start:

```bash
sudo systemctl enable senselearn
sudo systemctl start senselearn
```

## Security Hardening

### Firewall

```bash
# Allow SSH, HTTP, HTTPS
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

### File Permissions

```bash
# Set proper permissions
chmod 600 .env
chmod -R 755 static
chmod -R 755 templates
chmod -R 700 uploads
```

### SSL/TLS

Use Let's Encrypt for free SSL:

```bash
sudo certbot --nginx -d yourdomain.com
```

## Monitoring

### Logging

Configure logging in production:

```python
import logging
from logging.handlers import RotatingFileHandler

if not app.debug:
    file_handler = RotatingFileHandler('logs/senselearn.log', maxBytes=10240, backupCount=10)
    file_handler.setFormatter(logging.Formatter(
        '%(asctime)s %(levelname)s: %(message)s [in %(pathname)s:%(lineno)d]'
    ))
    file_handler.setLevel(logging.INFO)
    app.logger.addHandler(file_handler)
```

### Health Checks

Create health check endpoint:

```python
@app.route('/health')
def health():
    return jsonify({"status": "healthy"}), 200
```

## Backup Strategy

### Database Backups

```bash
# Daily backup script
mysqldump -u user -p senselearn_prod > backup_$(date +%Y%m%d).sql
```

### File Backups

```bash
# Backup uploads directory
tar -czf uploads_backup_$(date +%Y%m%d).tar.gz uploads/
```

## Performance Optimization

### Database

- Enable query caching
- Add appropriate indexes
- Use connection pooling
- Monitor slow queries

### Static Files

- Serve static files via nginx
- Enable gzip compression
- Use CDN for assets (optional)

### Caching

- Cache frequently accessed data
- Use Redis for session storage (optional)
- Implement response caching

## Scaling

### Horizontal Scaling

- Use load balancer
- Multiple application instances
- Shared session storage (Redis)
- Shared file storage (NFS/S3)

### Vertical Scaling

- Increase server resources
- Optimize database queries
- Add caching layers

## Maintenance

### Regular Tasks

- Monitor logs
- Check disk space
- Review error rates
- Update dependencies
- Backup verification

### Updates

```bash
# Update dependencies
pip install --upgrade -r requirements.txt

# Run migrations
flask db upgrade

# Restart service
sudo systemctl restart senselearn
```
