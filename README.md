# Quantum Scribe Documentation
## Complete Setup and Customization Guide for Open WebUI

---

## Table of Contents

1. [Introduction](#introduction)
2. [Initial Setup](#initial-setup)
3. [Branding Customization](#branding-customization)
4. [Database Configuration](#database-configuration)
5. [System Requirements](#system-requirements)
6. [Development Environment](#development-environment)
7. [Production Deployment](#production-deployment)
8. [Troubleshooting](#troubleshooting)
9. [Maintenance & Backup](#maintenance--backup)

---

## Introduction

This documentation covers the complete setup and customization of Open WebUI, rebranded as **Quantum Scribe** - a company knowledge base system designed for 30+ users.

### What is Quantum Scribe?

Quantum Scribe is a customized version of Open WebUI configured as a corporate knowledge base system with:
- Custom branding and identity
- PostgreSQL database for better performance
- Optimized for 30+ concurrent users
- Development and production configurations

---

## Initial Setup

### Prerequisites

**Required Software:**
- Python 3.11+
- Node.js 18+ and npm
- Git
- PostgreSQL 16+
- Code editor (VS Code recommended)

**System Requirements:**
- Windows 10/11, macOS, or Linux
- Minimum 8GB RAM (16GB recommended)
- 50GB free disk space

### Clone the Repository
```bash
git clone https://github.com/aiikendoit/quantum-scribe.git
cd quantum-scribe
```

### Install Dependencies

**Backend:**
```bash
cd backend
pip install -r requirements.txt
pip install psycopg2-binary  # For PostgreSQL support
cd ..
```

**Frontend:**
```bash
npm install
```

---

## Branding Customization

### 1. Change Application Name

**Method 1: Environment Variable (Recommended)**

Create or edit `.env` file in the project root:

```bash
WEBUI_NAME=Quantum Scribe
```

**Method 2: Modify Source Code**

Edit `backend/open_webui/env.py` (around line 103-105):

**Original:**
```python
WEBUI_NAME = os.environ.get("WEBUI_NAME", "Open WebUI")
if WEBUI_NAME != "Open WebUI":
    WEBUI_NAME += " (Open WebUI)"
```

**Modified:**
```python
WEBUI_NAME = os.environ.get("WEBUI_NAME", "Quantum Scribe")
```

This removes the "(Open WebUI)" suffix and sets "Quantum Scribe" as the default name.

### 2. Replace Icons and Logos

#### File Locations

Based on the Open WebUI structure, you need to replace files in these directories:

**Primary Source Files:**
```
./static/static/
├── favicon.png
├── favicon.svg
├── favicon.ico
├── favicon-96x96.png
├── favicon-dark.png
├── apple-touch-icon.png
└── logo.png
```

**Backend Static Files:**
```
./backend/open_webui/static/
├── favicon.png
├── favicon.svg
├── favicon.ico
├── favicon-96x96.png
├── favicon-dark.png
├── apple-touch-icon.png
└── logo.png
```

#### Required Asset Sizes

Prepare these files for your custom branding:

| File | Size | Purpose |
|------|------|---------|
| `favicon.ico` | 16x16, 32x32, 48x48 | Browser tab icon |
| `favicon.png` | 32x32 or 64x64 | PNG favicon |
| `favicon.svg` | Scalable | Vector favicon |
| `favicon-96x96.png` | 96x96 | High-res favicon |
| `favicon-dark.png` | 32x32 or 64x64 | Dark mode favicon |
| `apple-touch-icon.png` | 180x180 | iOS home screen |
| `logo.png` | 512x512+ | Main application logo |

#### Asset Replacement Script

**For Windows (Git Bash):**

Create `replace-branding.sh`:

```bash
#!/bin/bash

# Configuration
BRANDING_DIR="./quantum-scribe-branding"

echo "🎨 Replacing Open WebUI branding with Quantum Scribe..."

# Create backup
echo "Creating backup..."
mkdir -p backups/branding-$(date +%Y%m%d-%H%M%S)
cp -r static/static/* backups/branding-$(date +%Y%m%d-%H%M%S)/ 2>/dev/null

# Function to copy files
copy_branding() {
    local dest=$1
    echo "  → Updating $dest"
    
    cp "$BRANDING_DIR/favicon.png" "$dest/" 2>/dev/null
    cp "$BRANDING_DIR/favicon.svg" "$dest/" 2>/dev/null
    cp "$BRANDING_DIR/favicon.ico" "$dest/" 2>/dev/null
    cp "$BRANDING_DIR/favicon-96x96.png" "$dest/" 2>/dev/null
    cp "$BRANDING_DIR/favicon-dark.png" "$dest/" 2>/dev/null
    cp "$BRANDING_DIR/apple-touch-icon.png" "$dest/" 2>/dev/null
    cp "$BRANDING_DIR/logo.png" "$dest/" 2>/dev/null
}

# Replace in source directories
copy_branding "./static/static"
copy_branding "./backend/open_webui/static"

# Optional: Replace in build directories
if [ -d "./build/static" ]; then
    copy_branding "./build/static"
fi

if [ -d "./.svelte-kit/output/client/static" ]; then
    copy_branding "./.svelte-kit/output/client/static"
fi

echo "✅ Branding replacement complete!"
echo "⚠️  Remember to rebuild: npm run build"
```

**Usage:**
```bash
chmod +x replace-branding.sh
./replace-branding.sh
```

**For Windows (PowerShell):**

Create `replace-branding.ps1`:

```powershell
# Configuration
$BrandingDir = ".\quantum-scribe-branding"

Write-Host "🎨 Replacing branding..." -ForegroundColor Cyan

# Destinations
$destinations = @(
    ".\static\static",
    ".\backend\open_webui\static"
)

# Files to copy
$files = @(
    "favicon.png",
    "favicon.svg", 
    "favicon.ico",
    "favicon-96x96.png",
    "favicon-dark.png",
    "apple-touch-icon.png",
    "logo.png"
)

foreach ($dest in $destinations) {
    Write-Host "  → Updating $dest" -ForegroundColor Yellow
    foreach ($file in $files) {
        Copy-Item "$BrandingDir\$file" "$dest\$file" -Force -ErrorAction SilentlyContinue
    }
}

Write-Host "✅ Complete! Run 'npm run build' to apply changes." -ForegroundColor Green
```

**Usage:**
```powershell
.\replace-branding.ps1
```

#### Update Manifest File

Edit `static/manifest.json` or `static/site.webmanifest`:

```json
{
  "name": "Quantum Scribe",
  "short_name": "QScribe",
  "description": "AI-powered company knowledge base",
  "icons": [
    {
      "src": "/static/favicon-96x96.png",
      "sizes": "96x96",
      "type": "image/png"
    },
    {
      "src": "/static/apple-touch-icon.png",
      "sizes": "180x180",
      "type": "image/png"
    }
  ],
  "start_url": "/",
  "display": "standalone",
  "theme_color": "#your-brand-color",
  "background_color": "#ffffff"
}
```

#### Update Favicon URL in Configuration

Edit `backend/open_webui/env.py` (around line 106):

```python
WEBUI_FAVICON_URL = "/favicon.png"
# Or use external URL:
# WEBUI_FAVICON_URL = "https://yourdomain.com/favicon.png"
```

### 3. Generating Assets from Logo

**Using Online Tools:**
- https://realfavicongenerator.net/ - Generate all favicon sizes
- https://www.favicon-generator.org/ - Simple favicon generator

**Using ImageMagick (Command Line):**

```bash
# Install ImageMagick
# Windows: Download from https://imagemagick.org/
# Linux: sudo apt install imagemagick

# Generate different sizes from your logo
convert logo.png -resize 16x16 favicon-16.png
convert logo.png -resize 32x32 favicon-32.png
convert logo.png -resize 96x96 favicon-96x96.png
convert logo.png -resize 192x192 icon-192.png
convert logo.png -resize 512x512 icon-512.png
convert logo.png -resize 180x180 apple-touch-icon.png

# Create .ico from multiple sizes
convert favicon-16.png favicon-32.png favicon.ico
```

---

## Database Configuration

### Default: SQLite

Open WebUI uses SQLite by default:

```python
# In backend/open_webui/env.py (line 246)
DATABASE_URL = os.environ.get("DATABASE_URL", f"sqlite:///{DATA_DIR}/webui.db")
```

**SQLite Characteristics:**
- ✅ Zero configuration
- ✅ Perfect for development/testing
- ✅ Single file database
- ❌ Limited concurrent writes
- ❌ Not ideal for 30+ users

### Recommended: PostgreSQL

For production with 30+ users, PostgreSQL is strongly recommended.

#### PostgreSQL Installation (Windows)

**1. Download and Install:**
- Download from: https://www.postgresql.org/download/windows/
- Or EnterpriseDB: https://www.enterprisedb.com/downloads/postgres-postgresql-downloads
- Install PostgreSQL 16.x (recommended)
- Set a strong password for postgres user
- Default port: 5432

**2. Add to PATH (Git Bash):**

```bash
echo 'export PATH="/c/Program Files/PostgreSQL/16/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Verify
psql --version
```

#### PostgreSQL Installation (Linux)

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install postgresql postgresql-contrib

# Start and enable service
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Verify
psql --version
```

#### Create Database and User

**1. Connect to PostgreSQL:**

```bash
# Windows
psql -U postgres

# Linux
sudo -u postgres psql
```

**2. Run these SQL commands:**

```sql
-- Create database
CREATE DATABASE openwebui;

-- Create user
CREATE USER openwebui_user WITH PASSWORD 'your_secure_password_here';

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE openwebui TO openwebui_user;

-- Connect to the database
\c openwebui

-- Grant schema privileges (PostgreSQL 15+)
GRANT ALL ON SCHEMA public TO openwebui_user;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO openwebui_user;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO openwebui_user;

-- Set default privileges for future tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO openwebui_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON SEQUENCES TO openwebui_user;

-- Exit
\q
```

**3. Test Connection:**

```bash
psql -U openwebui_user -d openwebui -h localhost
# Enter password when prompted
# You should see: openwebui=>
\q
```

#### Install Python PostgreSQL Driver

```bash
cd /path/to/open-webui
pip install psycopg2-binary

# If you encounter errors, try:
pip install psycopg2
```

#### Configure Open WebUI for PostgreSQL

**Method 1: Using .env file (Recommended)**

Create/edit `.env` in project root:

```bash
# Environment
ENV=dev

# Branding
WEBUI_NAME=Quantum Scribe

# PostgreSQL Database Configuration
DATABASE_URL=postgresql://openwebui_user:your_secure_password_here@localhost:5432/openwebui

# Database Pool Settings (for better performance)
DATABASE_POOL_SIZE=20
DATABASE_POOL_MAX_OVERFLOW=10
DATABASE_POOL_TIMEOUT=30
DATABASE_POOL_RECYCLE=3600

# Enable migrations
ENABLE_DB_MIGRATIONS=true

# Logging
GLOBAL_LOG_LEVEL=INFO
```

**Method 2: Using separate environment variables**

```bash
# In .env file
ENV=dev
WEBUI_NAME=Quantum Scribe

DATABASE_TYPE=postgresql
DATABASE_USER=openwebui_user
DATABASE_PASSWORD=your_secure_password_here
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=openwebui

DATABASE_POOL_SIZE=20
DATABASE_POOL_MAX_OVERFLOW=10
```

#### PostgreSQL Optimization

Edit PostgreSQL configuration (`postgresql.conf`):

**Location:**
- Windows: `C:\Program Files\PostgreSQL\16\data\postgresql.conf`
- Linux: `/etc/postgresql/16/main/postgresql.conf`

**Recommended Settings for 30 Users:**

```conf
# Connection settings
max_connections = 100

# Memory settings (adjust based on your RAM)
shared_buffers = 4GB                # 25% of RAM
effective_cache_size = 12GB         # 75% of RAM
maintenance_work_mem = 1GB
work_mem = 64MB

# Performance
random_page_cost = 1.1              # For SSD
effective_io_concurrency = 200      # For SSD

# Write-Ahead Logging
wal_buffers = 16MB
checkpoint_completion_target = 0.9
```

After editing, restart PostgreSQL:

```bash
# Windows
net stop postgresql-x64-16
net start postgresql-x64-16

# Linux
sudo systemctl restart postgresql
```

#### Database Management

**Useful PostgreSQL Commands:**

```bash
# Start PostgreSQL (Windows)
net start postgresql-x64-16

# Stop PostgreSQL
net stop postgresql-x64-16

# Connect to database
psql -U openwebui_user -d openwebui -h localhost

# Backup database
pg_dump -U openwebui_user -d openwebui -F c -b -v -f openwebui_backup.dump

# Restore database
pg_restore -U openwebui_user -d openwebui -v openwebui_backup.dump

# Check database size
psql -U openwebui_user -d openwebui -c "SELECT pg_size_pretty(pg_database_size('openwebui'));"

# Vacuum/optimize database
psql -U openwebui_user -d openwebui -c "VACUUM ANALYZE;"
```

**SQL Queries:**

```sql
-- Connect to database
\c openwebui

-- List all tables
\dt

-- Check user count
SELECT COUNT(*) FROM "user";

-- Check table sizes
SELECT 
    schemaname,
    tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;

-- Exit
\q
```

---

## System Requirements

### For 30-50 User Deployment

#### Minimum Configuration
```
CPU:     4-6 cores (Intel i5/i7 or AMD Ryzen 5/7)
RAM:     16 GB
Storage: 500 GB SSD
Network: 100 Mbps+
OS:      Ubuntu Server 22.04 LTS or Windows Server 2019+
```

#### Recommended Configuration
```
CPU:     8-12 cores (Intel Xeon or AMD EPYC)
RAM:     32 GB DDR4 ECC
Storage: 1 TB NVMe SSD
Network: 1 Gbps
OS:      Ubuntu Server 22.04 LTS
```

#### Optimal Configuration (Future-proof)
```
CPU:     16+ cores (Intel Xeon Gold or AMD EPYC)
RAM:     64 GB DDR4 ECC
Storage: 2 TB NVMe SSD (RAID 1 for redundancy)
Network: 1 Gbps+
GPU:     Optional: NVIDIA RTX 4060/4070 (12-16GB VRAM) for local AI models
OS:      Ubuntu Server 22.04 LTS
Backup:  NAS with 4TB for automated backups
UPS:     1500VA for power protection
```

### Resource Estimation

**Memory Usage Breakdown:**
```
Base System:           2-4 GB
Open WebUI Backend:    2-4 GB
PostgreSQL:            2-8 GB
Redis Cache:           1-2 GB
Vector Database:       4-8 GB
Document Processing:   4-8 GB
Buffer/OS:            4-8 GB
-----------------------------------
Total Minimum:        16 GB
Recommended:          32 GB
Optimal:              64 GB
```

**Storage Breakdown:**
```
Operating System:           50 GB
Open WebUI Application:     10 GB
Database:                   50-100 GB
Vector Embeddings:          100-500 GB
Document Storage:           Variable
Logs & Backups:            100-200 GB
-----------------------------------
Minimum:                   500 GB
Recommended:               1 TB
Optimal:                   2 TB+
```

**Per-User Estimation:**
- Active user session: ~50-200 MB RAM
- 30 concurrent users: ~6 GB RAM
- Database per user: ~100-500 MB storage

### Deployment Architectures

#### Option 1: Single Server (Simple)
```
┌─────────────────────────────────┐
│   Single Server                 │
│   - Open WebUI                  │
│   - PostgreSQL                  │
│   - Redis                       │
│   - Vector DB                   │
└─────────────────────────────────┘

Pros: Simple, cost-effective
Cons: Single point of failure
Best for: <50 users, development/testing

Recommended Specs:
- CPU: 12 cores
- RAM: 32 GB
- Storage: 1 TB NVMe SSD
```

#### Option 2: Separated Services (Recommended)
```
┌──────────────────┐   ┌──────────────────┐
│  App Server      │   │  Database Server │
│  - Open WebUI    │───│  - PostgreSQL    │
│  - Redis         │   │  - Vector DB     │
└──────────────────┘   └──────────────────┘

Pros: Better performance, scalability
Cons: More complex, higher cost
Best for: 30-100 users, production

App Server Specs:
- CPU: 8-12 cores
- RAM: 24 GB
- Storage: 500 GB SSD

Database Server Specs:
- CPU: 8 cores
- RAM: 32 GB
- Storage: 1 TB SSD (RAID 1)
```

### Cost Estimation

**Cloud Hosting (AWS/Azure/GCP):**

| Configuration | Monthly Cost |
|---------------|-------------|
| Minimum (4 cores, 16GB) | $150-200 |
| Recommended (8 cores, 32GB) | $300-400 |
| Optimal (16 cores, 64GB) | $600-800 |

**On-Premise Server:**

| Configuration | One-time Cost |
|---------------|---------------|
| Minimum | $2,000-3,000 |
| Recommended | $4,000-6,000 |
| Optimal | $8,000-12,000 |

---

## Development Environment

### Hot Reload Setup

Hot reload allows you to see changes immediately without restarting the server.

#### Enable Hot Reload for Backend

```bash
cd backend

# Start with hot reload enabled
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload

# With specific reload directory
python -m uvicorn open_webui.main:app \
  --host 0.0.0.0 \
  --port 8080 \
  --reload \
  --reload-dir ./open_webui \
  --log-level debug
```

**Verify hot reload is working:**

When you start the server, you should see:
```
INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
INFO:     Started reloader process [xxxxx] using WatchFiles
INFO:     Started server process [xxxxx]
```

The key indicator is: **"Started reloader process using WatchFiles"**

**Test it:**
1. Make a small change to any Python file
2. Save the file
3. Watch the terminal - you should see:
```
INFO:     WatchFiles detected changes in 'filename.py'. Reloading...
```

#### Enable Hot Reload for Frontend

```bash
# In project root directory
npm run dev
```

This starts the Vite/SvelteKit dev server with hot module replacement (HMR).

### Development Workflow

**Recommended Terminal Setup:**

**Terminal 1 - PostgreSQL (optional monitoring):**
```bash
# Check PostgreSQL status
psql -U openwebui_user -d openwebui

# Monitor connections
SELECT * FROM pg_stat_activity;
```

**Terminal 2 - Backend:**
```bash
cd backend
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload
```

**Terminal 3 - Frontend:**
```bash
npm run dev
```

### Environment Modes

Set in `.env` file:

```bash
# Development mode (hot reload, debug logs)
ENV=dev

# Test mode
ENV=test

# Production mode (optimized, no debug)
ENV=prod
```

### Complete Development .env File

```bash
# ======================
# ENVIRONMENT
# ======================
ENV=dev

# ======================
# BRANDING
# ======================
WEBUI_NAME=Quantum Scribe

# ======================
# DATABASE
# ======================
DATABASE_URL=postgresql://openwebui_user:your_password@localhost:5432/openwebui
DATABASE_POOL_SIZE=20
DATABASE_POOL_MAX_OVERFLOW=10
DATABASE_POOL_TIMEOUT=30
DATABASE_POOL_RECYCLE=3600
ENABLE_DB_MIGRATIONS=true

# ======================
# LOGGING
# ======================
GLOBAL_LOG_LEVEL=DEBUG

# ======================
# DEVELOPMENT OPTIONS
# ======================
ENABLE_VERSION_UPDATE_CHECK=false
SAFE_MODE=false

# ======================
# PERFORMANCE
# ======================
UVICORN_WORKERS=1
ENABLE_REALTIME_CHAT_SAVE=false
ENABLE_QUERIES_CACHE=true

# ======================
# REDIS (Optional)
# ======================
# REDIS_URL=redis://localhost:6379
# ENABLE_WEBSOCKET_SUPPORT=true
# WEBSOCKET_MANAGER=redis
```

### Build Commands

```bash
# Clean build
rm -rf .svelte-kit build

# Build frontend
npm run build

# Build backend (if packaging)
cd backend
python setup.py build
```

---

## Production Deployment

### Production .env Configuration

```bash
# ======================
# ENVIRONMENT
# ======================
ENV=prod

# ======================
# BRANDING
# ======================
WEBUI_NAME=Quantum Scribe

# ======================
# DATABASE
# ======================
DATABASE_URL=postgresql://openwebui_user:STRONG_PASSWORD@localhost:5432/openwebui
DATABASE_POOL_SIZE=30
DATABASE_POOL_MAX_OVERFLOW=15
DATABASE_POOL_TIMEOUT=30
DATABASE_POOL_RECYCLE=3600
DATABASE_ENABLE_SESSION_SHARING=true

# ======================
# SECURITY
# ======================
WEBUI_SECRET_KEY=your-very-strong-secret-key-here
WEBUI_AUTH=true
ENABLE_SIGNUP_PASSWORD_CONFIRMATION=true
ENABLE_PASSWORD_VALIDATION=true

# ======================
# LOGGING
# ======================
GLOBAL_LOG_LEVEL=INFO
ENABLE_AUDIT_LOGS_FILE=true
AUDIT_LOG_LEVEL=REQUEST

# ======================
# PERFORMANCE
# ======================
UVICORN_WORKERS=4
ENABLE_COMPRESSION_MIDDLEWARE=true
ENABLE_REALTIME_CHAT_SAVE=true
ENABLE_QUERIES_CACHE=true

# ======================
# REDIS
# ======================
REDIS_URL=redis://localhost:6379
ENABLE_WEBSOCKET_SUPPORT=true
WEBSOCKET_MANAGER=redis

# ======================
# BACKUP
# ======================
# Configure automatic backups externally
```

### Running in Production

**Option 1: Using systemd (Linux)**

Create `/etc/systemd/system/quantum-scribe.service`:

```ini
[Unit]
Description=Quantum Scribe (Open WebUI)
After=network.target postgresql.service

[Service]
Type=notify
User=www-data
Group=www-data
WorkingDirectory=/var/www/quantum-scribe
Environment="PATH=/var/www/quantum-scribe/venv/bin"
EnvironmentFile=/var/www/quantum-scribe/.env
ExecStart=/var/www/quantum-scribe/venv/bin/uvicorn open_webui.main:app \
    --host 0.0.0.0 \
    --port 8080 \
    --workers 4
Restart=always

[Install]
WantedBy=multi-user.target
```

**Enable and start:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable quantum-scribe
sudo systemctl start quantum-scribe
sudo systemctl status quantum-scribe
```

**Option 2: Using Gunicorn (Production WSGI)**

```bash
pip install gunicorn

# Run with Gunicorn
gunicorn open_webui.main:app \
    --workers 4 \
    --worker-class uvicorn.workers.UvicornWorker \
    --bind 0.0.0.0:8080 \
    --timeout 120 \
    --access-logfile - \
    --error-logfile -
```

**Option 3: Using PM2 (Alternative)**

```bash
npm install -g pm2

# Create ecosystem file
pm2 ecosystem

# Edit ecosystem.config.js
module.exports = {
  apps: [{
    name: 'quantum-scribe',
    script: 'python',
    args: '-m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --workers 4',
    cwd: '/path/to/quantum-scribe',
    env: {
      ENV: 'prod'
    }
  }]
};

# Start
pm2 start ecosystem.config.js
pm2 save
pm2 startup
```

### Nginx Reverse Proxy

Create `/etc/nginx/sites-available/quantum-scribe`:

```nginx
upstream quantum_scribe {
    server 127.0.0.1:8080;
}

server {
    listen 80;
    server_name quantum-scribe.yourcompany.com;

    # Redirect HTTP to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name quantum-scribe.yourcompany.com;

    # SSL Configuration
    ssl_certificate /etc/letsencrypt/live/quantum-scribe.yourcompany.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/quantum-scribe.yourcompany.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    # Logging
    access_log /var/log/nginx/quantum-scribe-access.log;
    error_log /var/log/nginx/quantum-scribe-error.log;

    # Max upload size
    client_max_body_size 100M;

    location / {
        proxy_pass http://quantum_scribe;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
        proxy_read_timeout 300s;
        proxy_connect_timeout 75s;
    }

    # WebSocket support
    location /ws {
        proxy_pass http://quantum_scribe;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_read_timeout 86400;
    }

    # Static files
    location /static {
        alias /var/www/quantum-scribe/static;
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

Enable the site:
```bash
sudo ln -s /etc/nginx/sites-available/quantum-scribe /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### SSL Certificate (Let's Encrypt)

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Obtain certificate
sudo certbot --nginx -d quantum-scribe.yourcompany.com

# Auto-renewal is set up automatically
# Test renewal
sudo certbot renew --dry-run
```

---

## Troubleshooting

### PostgreSQL Issues

#### Issue: Connection Refused

```bash
# Check if PostgreSQL is running
# Windows:
Get-Service -Name postgresql*

# Linux:
sudo systemctl status postgresql

# Start if not running
# Windows:
net start postgresql-x64-16

# Linux:
sudo systemctl start postgresql
```

#### Issue: Authentication Failed

Edit `pg_hba.conf`:

**Windows:** `C:\Program Files\PostgreSQL\16\data\pg_hba.conf`  
**Linux:** `/etc/postgresql/16/main/pg_hba.conf`

Change:
```
host    all             all             127.0.0.1/32            scram-sha-256
```

To:
```
host    all             all             127.0.0.1/32            md5
```

Restart PostgreSQL after changes.

#### Issue: Permission Denied

```sql
-- Reconnect as postgres user
psql -U postgres

-- Regrant permissions
\c openwebui
GRANT ALL ON SCHEMA public TO openwebui_user;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO openwebui_user;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO openwebui_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL ON TABLES TO openwebui_user;
\q
```

#### Issue: Database Migration Fails

```bash
# Drop and recreate database
psql -U postgres -c "DROP DATABASE openwebui;"
psql -U postgres -c "CREATE DATABASE openwebui;"
psql -U postgres -c "GRANT ALL PRIVILEGES ON DATABASE openwebui TO openwebui_user;"

# Restart Open WebUI - it will recreate tables
```

### Application Issues

#### Issue: Hot Reload Not Working

```bash
# Make sure you're using --reload flag
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload

# Specify reload directory
python -m uvicorn open_webui.main:app \
  --host 0.0.0.0 \
  --port 8080 \
  --reload \
  --reload-dir ./open_webui
```

#### Issue: Frontend Not Loading

```bash
# Clear build cache
rm -rf .svelte-kit build node_modules/.vite

# Reinstall and rebuild
npm install
npm run build
npm run dev
```

#### Issue: Icons/Logos Not Updating

```bash
# Clear browser cache (Hard refresh)
# Chrome/Edge: Ctrl + Shift + R
# Firefox: Ctrl + F5

# Clear SvelteKit cache
rm -rf .svelte-kit build

# Rebuild
npm run build

# Check if files are in correct locations
ls -la static/static/
ls -la backend/open_webui/static/
```

#### Issue: Database Pool Exhausted

Increase pool size in `.env`:

```bash
DATABASE_POOL_SIZE=50
DATABASE_POOL_MAX_OVERFLOW=20
```

### Performance Issues

#### High Memory Usage

```bash
# Check PostgreSQL memory
SELECT * FROM pg_stat_activity;

# Reduce worker count
UVICORN_WORKERS=2  # Instead of 4

# Optimize PostgreSQL
# Edit postgresql.conf
shared_buffers = 2GB  # Reduce if necessary
work_mem = 32MB      # Reduce if necessary
```

#### Slow Response Times

```bash
# Enable query caching
ENABLE_QUERIES_CACHE=true

# Add Redis
REDIS_URL=redis://localhost:6379

# Check database indexes
psql -U openwebui_user -d openwebui
\di
```

### Logging and Debugging

```bash
# Enable debug logging
GLOBAL_LOG_LEVEL=DEBUG

# Enable audit logging
ENABLE_AUDIT_STDOUT=true
ENABLE_AUDIT_LOGS_FILE=true
AUDIT_LOG_LEVEL=REQUEST_RESPONSE

# View logs
tail -f backend/data/audit.log

# Check PostgreSQL logs
# Windows: C:\Program Files\PostgreSQL\16\data\log\
# Linux: /var/log/postgresql/
```

---

## Maintenance & Backup

### Regular Maintenance Tasks

#### Daily
- Monitor system resources (CPU, RAM, Disk)
- Check application logs for errors
- Verify backup completion

#### Weekly
- Review user activity logs
- Check database size and growth
- Optimize PostgreSQL (VACUUM)

#### Monthly
- Update dependencies
- Review and archive old logs
- Test backup restoration
- Security updates

### Database Backup

#### Automated Daily Backup (Linux)

Create `/usr/local/bin/backup-quantum-scribe.sh`:

```bash
#!/bin/bash

# Configuration
BACKUP_DIR="/var/backups/quantum-scribe"
DB_NAME="openwebui"
DB_USER="openwebui_user"
DATE=$(date +%Y%m%d_%H%M%S)
RETENTION_DAYS=30

# Create backup directory
mkdir -p $BACKUP_DIR

# Backup database
pg_dump -U $DB_USER -d $DB_NAME -F c -b -v -f "$BACKUP_DIR/db_backup_$DATE.dump"

# Backup data directory
tar -czf "$BACKUP_DIR/data_backup_$DATE.tar.gz" /var/www/quantum-scribe/backend/data

# Remove old backups
find $BACKUP_DIR -name "*.dump" -mtime +$RETENTION_DAYS -delete
find $BACKUP_DIR -name "*.tar.gz" -mtime +$RETENTION_DAYS -delete

echo "Backup completed: $DATE"
```

Make executable and add to cron:

```bash
chmod +x /usr/local/bin/backup-quantum-scribe.sh

# Add to crontab (run daily at 2 AM)
crontab -e
0 2 * * * /usr/local/bin/backup-quantum-scribe.sh >> /var/log/quantum-scribe-backup.log 2>&1
```

#### Manual Backup

```bash
# Database backup
pg_dump -U openwebui_user -d openwebui -F c -b -v -f openwebui_backup_$(date +%Y%m%d).dump

# Data directory backup
tar -czf data_backup_$(date +%Y%m%d).tar.gz backend/data

# Full application backup
tar -czf quantum-scribe_full_backup_$(date +%Y%m%d).tar.gz \
  --exclude=node_modules \
  --exclude=.svelte-kit \
  --exclude=build \
  /path/to/quantum-scribe
```

#### Restore from Backup

```bash
# Restore database
pg_restore -U openwebui_user -d openwebui -v openwebui_backup_20240101.dump

# Restore data directory
tar -xzf data_backup_20240101.tar.gz -C backend/

# Restart application
sudo systemctl restart quantum-scribe
```

### Update Procedure

#### Update Open WebUI

```bash
# Navigate to project
cd /path/to/quantum-scribe

# Backup first!
./backup-quantum-scribe.sh

# Stop application
sudo systemctl stop quantum-scribe

# Pull latest changes
git fetch origin
git pull origin main

# Update backend dependencies
cd backend
pip install -r requirements.txt --upgrade

# Update frontend dependencies
cd ..
npm install
npm run build

# Run database migrations
python -m alembic upgrade head

# Restart application
sudo systemctl start quantum-scribe
sudo systemctl status quantum-scribe
```

### Monitoring

#### System Monitoring

```bash
# Check system resources
htop

# Check disk usage
df -h

# Check PostgreSQL status
systemctl status postgresql

# Check application status
systemctl status quantum-scribe

# Check PostgreSQL connections
psql -U openwebui_user -d openwebui -c "SELECT count(*) FROM pg_stat_activity;"
```

#### Application Monitoring

```bash
# View application logs
journalctl -u quantum-scribe -f

# View PostgreSQL logs
tail -f /var/log/postgresql/postgresql-16-main.log

# View Nginx logs
tail -f /var/log/nginx/quantum-scribe-access.log
tail -f /var/log/nginx/quantum-scribe-error.log

# Check database size
psql -U openwebui_user -d openwebui -c "SELECT pg_size_pretty(pg_database_size('openwebui'));"
```

#### Performance Monitoring

```bash
# PostgreSQL query performance
psql -U openwebui_user -d openwebui

SELECT 
    query, 
    calls, 
    total_time, 
    mean_time, 
    max_time
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;
```

### Security Best Practices

1. **Change Default Passwords**
   - PostgreSQL postgres user
   - Database user password
   - WEBUI_SECRET_KEY

2. **Enable HTTPS**
   - Use Let's Encrypt SSL certificates
   - Force HTTPS redirects

3. **Regular Updates**
   - Keep OS updated
   - Update PostgreSQL
   - Update Python packages
   - Update Node.js packages

4. **Access Control**
   - Use firewall (UFW on Linux)
   - Limit PostgreSQL to localhost
   - Use strong passwords
   - Enable fail2ban

5. **Backup Encryption**
   - Encrypt backup files
   - Store backups off-site
   - Test restoration regularly

---

## Quick Reference

### Common Commands

```bash
# Start development
npm run dev  # Frontend
python -m uvicorn open_webui.main:app --reload  # Backend

# Build for production
npm run build

# Start PostgreSQL
net start postgresql-x64-16  # Windows
sudo systemctl start postgresql  # Linux

# Connect to database
psql -U openwebui_user -d openwebui

# Backup database
pg_dump -U openwebui_user -d openwebui -F c -f backup.dump

# View logs
tail -f backend/data/audit.log
journalctl -u quantum-scribe -f  # systemd

# Check status
systemctl status quantum-scribe
systemctl status postgresql
systemctl status nginx
```

### File Locations

```
Project Structure:
├── .env                          # Environment configuration
├── backend/
│   ├── open_webui/
│   │   ├── env.py               # Configuration file
│   │   └── static/              # Backend static files
│   └── data/
│       └── webui.db             # SQLite database (if used)
├── static/
│   └── static/                  # Frontend static assets
├── quantum-scribe-branding/     # Custom branding assets
└── backups/                     # Backup directory

Configuration Files:
- .env                           # Main environment config
- postgresql.conf                # PostgreSQL configuration
- pg_hba.conf                    # PostgreSQL authentication
- /etc/nginx/sites-available/    # Nginx configuration
- /etc/systemd/system/           # systemd service files
```

### Environment Variables Quick Reference

```bash
# Core
ENV=dev|test|prod
WEBUI_NAME=Quantum Scribe

# Database
DATABASE_URL=postgresql://user:pass@host:port/dbname
DATABASE_POOL_SIZE=20

# Performance
UVICORN_WORKERS=4
ENABLE_QUERIES_CACHE=true

# Security
WEBUI_SECRET_KEY=your-secret-key
WEBUI_AUTH=true

# Logging
GLOBAL_LOG_LEVEL=INFO|DEBUG
ENABLE_AUDIT_LOGS_FILE=true
```

---

## Support and Resources

### Official Documentation
- Open WebUI: https://docs.openwebui.com
- PostgreSQL: https://www.postgresql.org/docs/
- SvelteKit: https://kit.svelte.dev/docs

### Community
- Open WebUI GitHub: https://github.com/open-webui/open-webui
- Discord: https://discord.gg/5rJgQTnV4s

### Tools
- pgAdmin: https://www.pgadmin.org/
- DBeaver: https://dbeaver.io/
- Postman: https://www.postman.com/

---

## Changelog

### Version 1.0.0 (Initial Release)
- Complete branding customization to Quantum Scribe
- PostgreSQL database configuration
- System requirements documentation
- Development and production deployment guides
- Maintenance and backup procedures

---

**Document Version:** 1.0.0  
**Last Updated:** February 2026  
**Maintained by:** [Your Company Name]

---

*This documentation is based on Open WebUI and customized for Quantum Scribe deployment.*
