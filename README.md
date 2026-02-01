<div align="center">

# 🚀 Quantum Scribe Documentation

### Complete Setup and Customization Guide for Open WebUI

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Python](https://img.shields.io/badge/python-3.11+-blue.svg)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791.svg)

*A customized Open WebUI deployment configured as a company knowledge base system*

[Quick Start](#-quick-start) • [Installation](#-installation) • [Configuration](#-configuration) • [Deployment](#-production-deployment)

</div>

---

## 📑 Table of Contents

- [Introduction](#-introduction)
- [Quick Start](#-quick-start)
- [Installation](#-installation)
  - [Prerequisites](#prerequisites)
  - [Clone & Setup](#clone--setup)
  - [Frontend Setup](#frontend-setup)
  - [Backend Setup](#backend-setup)
- [Branding Customization](#-branding-customization)
- [Database Configuration](#-database-configuration)
- [System Requirements](#-system-requirements)
- [Development Environment](#-development-environment)
- [Production Deployment](#-production-deployment)
- [Troubleshooting](#-troubleshooting)
- [Maintenance & Backup](#-maintenance--backup)

---

## 🎯 Introduction

**Quantum Scribe** is a customized version of [Open WebUI](https://github.com/open-webui/open-webui) configured as a corporate knowledge base system.

### ✨ Features

- 🎨 **Custom Branding** - Complete visual identity customization
- 🗄️ **PostgreSQL Database** - Production-ready database configuration
- 👥 **Multi-User Support** - Optimized for 30+ concurrent users
- 🔧 **Hot Reload Development** - Fast development workflow
- 🔒 **Security Focused** - Enterprise-grade security settings
- 📊 **Performance Optimized** - Configured for optimal performance

### 🎯 Use Case

This deployment is designed for companies needing:
- Centralized AI-powered knowledge base
- Document Q&A capabilities
- Team collaboration features
- Secure on-premise or cloud deployment

---

## ⚡ Quick Start

> **TL;DR** - Get up and running in 5 minutes

```bash
# 1. Clone the repository
git clone https://github.com/open-webui/open-webui.git
cd open-webui

# 2. Setup environment
cp -RPp .env.example .env

# 3. Install frontend dependencies
npm install --force

# 4. Setup backend (choose one)
# Option A: Using Conda
conda create --name open-webui python=3.11
conda activate open-webui
pip install -r backend/requirements.txt -U

# Option B: Using venv
python3 -m venv venv
source venv/bin/activate  # Linux/macOS
# .\venv\Scripts\activate  # Windows
pip install -r backend/requirements.txt -U

# 5. Run development servers
# Terminal 1 - Backend
cd backend
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload

# Terminal 2 - Frontend
npm run dev
```

🎉 Open your browser to `http://localhost:5173`

---

## 📦 Installation

### Prerequisites

Make sure you have these installed:

| Software | Version | Purpose |
|----------|---------|---------|
| **Python** | 3.11+ | Backend runtime |
| **Node.js** | 18+ | Frontend build tools |
| **npm** | 9+ | Package manager |
| **Git** | Latest | Version control |
| **PostgreSQL** | 16+ | Database (recommended) |

<details>
<summary><b>📋 Check your versions</b></summary>

```bash
python --version   # Should be 3.11+
node --version     # Should be 18+
npm --version      # Should be 9+
git --version      # Any recent version
psql --version     # Should be 16+ (optional for dev)
```

</details>

### Clone & Setup

**Step 1: Clone the repository**

```bash
git clone https://github.com/open-webui/open-webui.git
cd open-webui
```

**Step 2: Create environment file**

```bash
# Copy example environment file
cp -RPp .env.example .env

# Edit .env with your preferred editor
nano .env  # or vim, code, etc.
```

**Step 3: Basic .env configuration**

```bash
# Add these to your .env file
WEBUI_NAME=Quantum Scribe
ENV=dev
```

### Frontend Setup

**Install dependencies:**

```bash
cd open-webui
npm install --force
```

> 💡 **Note:** The `--force` flag is sometimes needed to resolve dependency conflicts.

**Build frontend (optional for production):**

```bash
npm run build
```

**Start development server:**

```bash
npm run dev
```

The frontend will be available at `http://localhost:5173`

### Backend Setup

You can use either **Conda** (recommended) or **venv** (Python built-in).

<details open>
<summary><b>Option A: Using Conda (Recommended)</b></summary>

**1. Install Miniconda**

Download from: https://docs.conda.io/en/latest/miniconda.html

**2. Create and activate environment:**

```bash
# Create environment with Python 3.11
conda create --name open-webui python=3.11

# Activate environment
conda activate open-webui

# Initialize conda (first time only)
conda init
```

**3. Install dependencies:**

```bash
cd backend
pip install -r requirements.txt -U
```

**4. Run the backend:**

```bash
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload
```

</details>

<details>
<summary><b>Option B: Using Python venv</b></summary>

**1. Create virtual environment:**

```bash
python3 -m venv venv
```

**2. Activate virtual environment:**

**Linux/macOS:**
```bash
source venv/bin/activate
```

**Windows (CMD):**
```cmd
.\venv\Scripts\activate
```

**Windows (PowerShell):**
```powershell
.\venv\Scripts\Activate.ps1
```

**Windows (Git Bash):**
```bash
source venv/Scripts/activate
```

**3. Install dependencies:**

```bash
cd backend
pip install -r requirements.txt -U
```

**4. Run the backend:**

```bash
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload
```

</details>

### Alternative: Using Shell Scripts

**Linux/macOS:**

```bash
cd backend
sh dev.sh
```

**Windows:**

```bash
cd backend
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload
```

### ✅ Verify Installation

1. **Backend is running:** http://localhost:8080
2. **Frontend is running:** http://localhost:5173
3. **Check logs for errors** in both terminal windows

---

## 🎨 Branding Customization

### 1. Change Application Name

#### Method 1: Environment Variable (Recommended)

Edit your `.env` file:

```bash
WEBUI_NAME=Quantum Scribe
```

#### Method 2: Modify Source Code (Permanent)

**Location:** `backend/open_webui/env.py` (around line 103)

**Original code:**
```python
WEBUI_NAME = os.environ.get("WEBUI_NAME", "Open WebUI")
if WEBUI_NAME != "Open WebUI":
    WEBUI_NAME += " (Open WebUI)"
```

**Replace with:**
```python
# Default app name changed to Quantum Scribe
WEBUI_NAME = os.environ.get("WEBUI_NAME", "Quantum Scribe")

# Comment out the suffix logic
# if WEBUI_NAME != "Open WebUI":
#     WEBUI_NAME += " (Open WebUI)"
```

> ✅ **Result:** Your app will now display "Quantum Scribe" instead of "Open WebUI"

### 2. Replace Icons, Images, and Logos

#### Find Current Assets

Use this command to locate all icon and logo files:

```bash
find . -name "favicon*" -o -name "logo*" -o -name "*icon*" | grep -E "\.(png|svg|ico|jpg)$"
```

#### Key Directories

**Frontend source files (primary):**
```
open-webui/static/static/
├── favicon.png
├── favicon.svg
├── favicon.ico
├── favicon-96x96.png
├── favicon-dark.png
├── apple-touch-icon.png
└── logo.png
```

**Backend static files (built):**
```
open-webui/backend/open_webui/static/
├── favicon.png
├── favicon.svg
├── favicon.ico
├── favicon-96x96.png
├── favicon-dark.png
├── apple-touch-icon.png
└── logo.png
```

> ⚠️ **Important:** The backend directory is populated during build. Always update the frontend source first!

#### Required Asset Specifications

Create these files for your custom branding:

| File | Dimensions | Purpose |
|------|-----------|---------|
| `favicon.ico` | 16×16, 32×32, 48×48 | Multi-size browser favicon |
| `favicon.png` | 32×32 or 64×64 | PNG favicon fallback |
| `favicon.svg` | Scalable | Vector favicon (modern browsers) |
| `favicon-96x96.png` | 96×96 | High-resolution favicon |
| `favicon-dark.png` | 32×32 or 64×64 | Dark mode variant |
| `apple-touch-icon.png` | 180×180 | iOS/iPadOS home screen |
| `logo.png` | 512×512+ | Main application logo |

#### Asset Replacement Workflow

**Step 1: Prepare your assets**

```bash
# Create a branding directory
mkdir quantum-scribe-branding

# Place all your custom assets here:
# - favicon.ico
# - favicon.png
# - favicon.svg
# - favicon-96x96.png
# - favicon-dark.png
# - apple-touch-icon.png
# - logo.png
```

**Step 2: Backup original assets**

```bash
# Create backup directory
mkdir -p backups/original-branding-$(date +%Y%m%d)

# Backup current assets
cp -r static/static/* backups/original-branding-$(date +%Y%m%d)/
```

**Step 3: Replace assets (Manual Method)**

```bash
# Copy your assets to frontend source
cp quantum-scribe-branding/* static/static/

# The backend directory will be updated on next build
```

**Step 4: Clean and rebuild**

```bash
# Clean previous builds
rm -rf .svelte-kit build

# Rebuild frontend
npm run build

# Or if in dev mode with hot reload
npm run dev
```

<details>
<summary><b>🤖 Automated Replacement Script</b></summary>

Create `replace-branding.sh`:

```bash
#!/bin/bash

# Configuration
BRANDING_DIR="./quantum-scribe-branding"
BACKUP_DIR="./backups/branding-$(date +%Y%m%d-%H%M%S)"

echo "🎨 Replacing branding assets..."

# Create backup
echo "📦 Creating backup..."
mkdir -p "$BACKUP_DIR"
cp -r static/static/* "$BACKUP_DIR/" 2>/dev/null

# Function to copy branding files
copy_branding() {
    local dest=$1
    echo "  → Updating $dest"
    
    for file in favicon.png favicon.svg favicon.ico favicon-96x96.png \
                favicon-dark.png apple-touch-icon.png logo.png; do
        if [ -f "$BRANDING_DIR/$file" ]; then
            cp "$BRANDING_DIR/$file" "$dest/$file"
            echo "    ✓ $file"
        fi
    done
}

# Replace in source directory
copy_branding "./static/static"

echo ""
echo "✅ Branding assets replaced!"
echo "📝 Next steps:"
echo "   1. Run: rm -rf .svelte-kit build"
echo "   2. Run: npm run build"
echo "   3. Restart your servers"
```

**Make executable and run:**

```bash
chmod +x replace-branding.sh
./replace-branding.sh
```

</details>

<details>
<summary><b>🪟 PowerShell Script (Windows)</b></summary>

Create `replace-branding.ps1`:

```powershell
# Configuration
$BrandingDir = ".\quantum-scribe-branding"
$BackupDir = ".\backups\branding-$(Get-Date -Format 'yyyyMMdd-HHmmss')"

Write-Host "🎨 Replacing branding assets..." -ForegroundColor Cyan

# Create backup
Write-Host "📦 Creating backup..." -ForegroundColor Yellow
New-Item -ItemType Directory -Path $BackupDir -Force | Out-Null
Copy-Item ".\static\static\*" $BackupDir -Recurse -Force -ErrorAction SilentlyContinue

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

# Copy to source directory
Write-Host "  → Updating .\static\static" -ForegroundColor Yellow
foreach ($file in $files) {
    $source = Join-Path $BrandingDir $file
    if (Test-Path $source) {
        Copy-Item $source ".\static\static\$file" -Force
        Write-Host "    ✓ $file" -ForegroundColor Green
    }
}

Write-Host ""
Write-Host "✅ Branding assets replaced!" -ForegroundColor Green
Write-Host "📝 Next steps:" -ForegroundColor Cyan
Write-Host "   1. Run: Remove-Item -Recurse -Force .svelte-kit,build"
Write-Host "   2. Run: npm run build"
Write-Host "   3. Restart your servers"
```

**Run in PowerShell:**

```powershell
.\replace-branding.ps1
```

</details>

#### Update Web Manifest

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

Edit `backend/open_webui/env.py` (line 106):

```python
# Use local path
WEBUI_FAVICON_URL = "/favicon.png"

# Or use external URL:
# WEBUI_FAVICON_URL = "https://yourdomain.com/favicon.png"
```

### 3. Generate Assets from Logo

<details>
<summary><b>🎨 Using Online Tools</b></summary>

- **RealFaviconGenerator:** https://realfavicongenerator.net/
  - Generates all sizes
  - Provides optimized output
  - Includes web manifest

- **Favicon Generator:** https://www.favicon-generator.org/
  - Simple and quick
  - Basic size generation

</details>

<details>
<summary><b>🖼️ Using ImageMagick (Command Line)</b></summary>

**Install ImageMagick:**

```bash
# Windows: Download from https://imagemagick.org/
# Linux:
sudo apt install imagemagick
# macOS:
brew install imagemagick
```

**Generate all sizes from your logo:**

```bash
# Navigate to your logo directory
cd path/to/your/logo

# Generate different sizes
convert logo.png -resize 16x16 favicon-16.png
convert logo.png -resize 32x32 favicon-32.png
convert logo.png -resize 96x96 favicon-96x96.png
convert logo.png -resize 180x180 apple-touch-icon.png
convert logo.png -resize 192x192 icon-192.png
convert logo.png -resize 512x512 icon-512.png

# Create multi-size .ico file
convert favicon-16.png favicon-32.png favicon.ico

# Create SVG favicon (if you have Inkscape)
# Manual conversion recommended for best quality
```

</details>

### ✅ Verify Branding Changes

**Checklist:**

- [ ] Application name shows "Quantum Scribe"
- [ ] Browser tab shows custom favicon
- [ ] Logo appears in navbar/sidebar
- [ ] Dark mode favicon works (if applicable)
- [ ] iOS home screen icon displays correctly
- [ ] No console errors about missing images

**Clear cache and test:**

```bash
# Hard refresh in browser
# Chrome/Edge: Ctrl + Shift + R
# Firefox: Ctrl + F5
# Safari: Cmd + Option + R

# Or clear browser cache completely
```

---

## 🗄️ Database Configuration

### Overview

Open WebUI supports multiple databases, but for production deployments with 30+ users, **PostgreSQL is strongly recommended**.

| Database | Use Case | Concurrent Users | Performance |
|----------|----------|------------------|-------------|
| **SQLite** | Development, Testing | 1-10 | ⚠️ Limited |
| **PostgreSQL** | Production | 30+ | ✅ Excellent |
| **MySQL/MariaDB** | Alternative | 20+ | ✅ Good |

### Default: SQLite

**Location:** `backend/open_webui/env.py` (line 246)

```python
DATABASE_URL = os.environ.get("DATABASE_URL", f"sqlite:///{DATA_DIR}/webui.db")
```

**SQLite Characteristics:**

✅ **Pros:**
- Zero configuration required
- Perfect for development/testing
- Single file database
- Easy to backup (just copy the file)

❌ **Cons:**
- Limited concurrent write operations
- Not recommended for 30+ users
- No built-in replication
- Performance degrades with many simultaneous users

### Recommended: PostgreSQL

For production with 30+ users, PostgreSQL is the recommended choice.

<details open>
<summary><h3>🐘 PostgreSQL Installation</h3></summary>

#### Windows Installation

**1. Download PostgreSQL**

- Official: https://www.postgresql.org/download/windows/
- EnterpriseDB: https://www.enterprisedb.com/downloads/postgres-postgresql-downloads

**2. Install PostgreSQL 16.x**

- Run the installer
- Choose installation directory: `C:\Program Files\PostgreSQL\16`
- **Set a strong password** for the `postgres` user (remember this!)
- Port: `5432` (default)
- Locale: Default
- Skip Stack Builder (optional)

**3. Add PostgreSQL to PATH**

**For Git Bash:**
```bash
echo 'export PATH="/c/Program Files/PostgreSQL/16/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**For Windows PowerShell (as Administrator):**
```powershell
$env:Path += ";C:\Program Files\PostgreSQL\16\bin"
[Environment]::SetEnvironmentVariable("Path", $env:Path, [EnvironmentVariableTarget]::Machine)
```

**4. Verify installation:**
```bash
psql --version
# Should output: psql (PostgreSQL) 16.x
```

#### Linux Installation

**Ubuntu/Debian:**
```bash
# Update package list
sudo apt update

# Install PostgreSQL and contrib packages
sudo apt install postgresql postgresql-contrib

# Start and enable service
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Verify installation
psql --version
```

**RHEL/CentOS/Fedora:**
```bash
# Install PostgreSQL
sudo dnf install postgresql-server postgresql-contrib

# Initialize database
sudo postgresql-setup --initdb

# Start and enable service
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

#### macOS Installation

**Using Homebrew:**
```bash
# Install PostgreSQL
brew install postgresql@16

# Start service
brew services start postgresql@16

# Verify
psql --version
```

</details>

<details open>
<summary><h3>🔧 PostgreSQL Configuration</h3></summary>

#### Create Database and User

**1. Connect to PostgreSQL:**

**Windows:**
```bash
psql -U postgres
# Enter the password you set during installation
```

**Linux:**
```bash
sudo -u postgres psql
```

**2. Run these SQL commands:**

```sql
-- Create database
CREATE DATABASE openwebui;

-- Create user with secure password
CREATE USER openwebui_user WITH PASSWORD 'your_secure_password_here';

-- Grant privileges on database
GRANT ALL PRIVILEGES ON DATABASE openwebui TO openwebui_user;

-- Connect to the database
\c openwebui

-- Grant schema privileges (PostgreSQL 15+)
GRANT ALL ON SCHEMA public TO openwebui_user;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO openwebui_user;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO openwebui_user;

-- Set default privileges for future tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public 
    GRANT ALL ON TABLES TO openwebui_user;
ALTER DEFAULT PRIVILEGES IN SCHEMA public 
    GRANT ALL ON SEQUENCES TO openwebui_user;

-- Verify privileges
\du openwebui_user

-- Exit
\q
```

**3. Test the connection:**

```bash
psql -U openwebui_user -d openwebui -h localhost
# Enter the password you created
# You should see: openwebui=>

# Test a simple query
SELECT version();

# Exit
\q
```

#### Install Python PostgreSQL Driver

```bash
# Make sure your virtual environment is activated
# conda activate open-webui  # or source venv/bin/activate

# Install psycopg2 (PostgreSQL adapter)
pip install psycopg2-binary

# If you encounter compilation errors, try:
pip install psycopg2

# Verify installation
python -c "import psycopg2; print(psycopg2.__version__)"
```

</details>

<details open>
<summary><h3>⚙️ Configure Open WebUI for PostgreSQL</h3></summary>

#### Method 1: Using .env File (Recommended)

Edit your `.env` file in the project root:

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
# POSTGRESQL DATABASE
# ======================
DATABASE_URL=postgresql://openwebui_user:your_secure_password_here@localhost:5432/openwebui

# Database Connection Pool Settings
DATABASE_POOL_SIZE=20
DATABASE_POOL_MAX_OVERFLOW=10
DATABASE_POOL_TIMEOUT=30
DATABASE_POOL_RECYCLE=3600

# Enable automatic migrations
ENABLE_DB_MIGRATIONS=true

# ======================
# LOGGING
# ======================
GLOBAL_LOG_LEVEL=INFO
```

> 🔒 **Security Note:** Never commit your `.env` file with real passwords to version control!

#### Method 2: Using Separate Environment Variables

Alternatively, you can use individual variables:

```bash
# Database Configuration
DATABASE_TYPE=postgresql
DATABASE_USER=openwebui_user
DATABASE_PASSWORD=your_secure_password_here
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=openwebui

# Connection Pool Settings
DATABASE_POOL_SIZE=20
DATABASE_POOL_MAX_OVERFLOW=10
DATABASE_POOL_TIMEOUT=30
DATABASE_POOL_RECYCLE=3600
```

</details>

<details>
<summary><h3>🚀 Start Open WebUI with PostgreSQL</h3></summary>

**1. Make sure PostgreSQL is running:**

**Windows:**
```bash
# Check status
sc query postgresql-x64-16

# Start if not running
net start postgresql-x64-16
```

**Linux:**
```bash
sudo systemctl status postgresql
sudo systemctl start postgresql
```

**2. Start the backend:**

```bash
cd backend
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload
```

**3. Check the logs:**

You should see messages like:
```
INFO:     Connected to PostgreSQL database
INFO:     Running database migrations...
INFO:     Database initialized successfully
```

**4. Verify in PostgreSQL:**

```bash
psql -U openwebui_user -d openwebui -h localhost
```

```sql
-- List all tables
\dt

-- You should see tables like:
-- auth, user, chat, document, etc.

-- Check table contents
SELECT COUNT(*) FROM "user";

-- Exit
\q
```

</details>

<details>
<summary><h3>⚡ PostgreSQL Performance Optimization</h3></summary>

#### For 30-User Deployment

Edit PostgreSQL configuration file:

**Location:**
- **Windows:** `C:\Program Files\PostgreSQL\16\data\postgresql.conf`
- **Linux:** `/etc/postgresql/16/main/postgresql.conf`

**Recommended Settings:**

```conf
# ======================
# CONNECTION SETTINGS
# ======================
max_connections = 100
superuser_reserved_connections = 3

# ======================
# MEMORY SETTINGS
# ======================
# Adjust based on your total RAM
shared_buffers = 4GB                # 25% of RAM
effective_cache_size = 12GB         # 75% of RAM
maintenance_work_mem = 1GB
work_mem = 64MB

# ======================
# QUERY TUNING
# ======================
random_page_cost = 1.1              # For SSD storage
effective_io_concurrency = 200      # For SSD storage

# ======================
# WRITE-AHEAD LOGGING
# ======================
wal_buffers = 16MB
checkpoint_completion_target = 0.9
wal_compression = on

# ======================
# AUTOVACUUM
# ======================
autovacuum = on
autovacuum_max_workers = 3
autovacuum_naptime = 1min

# ======================
# LOGGING
# ======================
logging_collector = on
log_directory = 'log'
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'
log_rotation_age = 1d
log_rotation_size = 100MB
log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '
log_timezone = 'UTC'
```

**Apply changes:**

**Windows:**
```bash
# Restart PostgreSQL service
net stop postgresql-x64-16
net start postgresql-x64-16
```

**Linux:**
```bash
sudo systemctl restart postgresql
```

**Verify settings:**
```sql
psql -U postgres

-- Check current settings
SHOW shared_buffers;
SHOW effective_cache_size;
SHOW max_connections;

\q
```

</details>

### Database Comparison

<details>
<summary><b>📊 Feature Comparison Table</b></summary>

| Feature | SQLite | PostgreSQL | MySQL/MariaDB |
|---------|--------|------------|---------------|
| **Setup Complexity** | ✅ None | ⚠️ Moderate | ⚠️ Moderate |
| **Concurrent Writes** | ❌ Limited | ✅ Excellent | ✅ Good |
| **Concurrent Reads** | ✅ Good | ✅ Excellent | ✅ Good |
| **Max Users** | ~10 | 1000+ | 500+ |
| **Replication** | ❌ No | ✅ Yes | ✅ Yes |
| **Full-Text Search** | ⚠️ Basic | ✅ Advanced | ✅ Good |
| **JSON Support** | ⚠️ Limited | ✅ Advanced | ✅ Good |
| **ACID Compliance** | ✅ Yes | ✅ Yes | ✅ Yes |
| **Backup/Restore** | ✅ File copy | ✅ Advanced tools | ✅ Advanced tools |
| **Performance (30 users)** | ❌ Poor | ✅ Excellent | ✅ Good |
| **Production Ready** | ❌ No | ✅ Yes | ✅ Yes |

</details>

### Database Management Commands

<details>
<summary><b>🛠️ Useful PostgreSQL Commands</b></summary>

```bash
# ============================================
# SERVICE MANAGEMENT
# ============================================

# Windows
net start postgresql-x64-16        # Start
net stop postgresql-x64-16         # Stop
net restart postgresql-x64-16      # Restart
sc query postgresql-x64-16         # Check status

# Linux
sudo systemctl start postgresql    # Start
sudo systemctl stop postgresql     # Stop
sudo systemctl restart postgresql  # Restart
sudo systemctl status postgresql   # Check status

# ============================================
# CONNECTION
# ============================================

# Connect to database
psql -U openwebui_user -d openwebui -h localhost

# Connect as superuser
psql -U postgres

# ============================================
# DATABASE OPERATIONS
# ============================================

# Inside psql:

# List databases
\l

# Connect to database
\c openwebui

# List tables
\dt

# Describe table
\d table_name

# List users/roles
\du

# Show current database
SELECT current_database();

# Show database size
SELECT pg_size_pretty(pg_database_size('openwebui'));

# Show table sizes
SELECT 
    schemaname,
    tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;

# Check active connections
SELECT * FROM pg_stat_activity;

# Terminate idle connections
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE datname = 'openwebui'
  AND state = 'idle'
  AND state_change < now() - interval '1 hour';

# Exit psql
\q

# ============================================
# BACKUP
# ============================================

# Backup database (binary format - recommended)
pg_dump -U openwebui_user -d openwebui -F c -b -v -f backup_$(date +%Y%m%d).dump

# Backup database (SQL format)
pg_dump -U openwebui_user -d openwebui > backup_$(date +%Y%m%d).sql

# Backup all databases
pg_dumpall -U postgres > all_databases_$(date +%Y%m%d).sql

# ============================================
# RESTORE
# ============================================

# Restore from binary dump
pg_restore -U openwebui_user -d openwebui -v backup_20240101.dump

# Restore from SQL dump
psql -U openwebui_user -d openwebui < backup_20240101.sql

# ============================================
# MAINTENANCE
# ============================================

# Vacuum database (reclaim space)
psql -U openwebui_user -d openwebui -c "VACUUM VERBOSE;"

# Analyze database (update statistics)
psql -U openwebui_user -d openwebui -c "ANALYZE VERBOSE;"

# Vacuum and analyze together
psql -U openwebui_user -d openwebui -c "VACUUM ANALYZE VERBOSE;"

# Reindex database
psql -U openwebui_user -d openwebui -c "REINDEX DATABASE openwebui;"

# ============================================
# MONITORING
# ============================================

# Check database size
psql -U openwebui_user -d openwebui -c \
  "SELECT pg_size_pretty(pg_database_size('openwebui'));"

# Check table sizes
psql -U openwebui_user -d openwebui -c \
  "SELECT tablename, pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) 
   FROM pg_tables WHERE schemaname = 'public' 
   ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;"

# Check connection count
psql -U postgres -c \
  "SELECT count(*) FROM pg_stat_activity WHERE datname = 'openwebui';"

# Check slow queries
psql -U postgres -c \
  "SELECT pid, now() - pg_stat_activity.query_start AS duration, query 
   FROM pg_stat_activity 
   WHERE state = 'active' AND now() - pg_stat_activity.query_start > interval '5 seconds';"
```

</details>

---

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

## 💻 Development Environment

### Hot Reload Setup

Hot reload allows you to see changes instantly without manually restarting servers - essential for efficient development!

#### ✅ What is Hot Reload?

| Component | Technology | What it does |
|-----------|------------|--------------|
| **Backend** | Uvicorn `--reload` | Watches Python files, restarts on changes |
| **Frontend** | Vite/SvelteKit HMR | Updates browser instantly on file changes |

#### 🔥 Enable Hot Reload

<details open>
<summary><h4>Backend Hot Reload</h4></summary>

**Start backend with hot reload:**

```bash
cd backend

# Basic hot reload
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload

# With debug logging (recommended)
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload --log-level debug

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
INFO:     Started reloader process [12345] using WatchFiles  ← This is the key!
INFO:     Started server process [67890]
INFO:     Application startup complete.
```

**Test it:**

1. Make a change to any Python file (e.g., add a comment in `env.py`)
2. Save the file
3. Watch your terminal - you should see:

```
INFO:     WatchFiles detected changes in 'open_webui/env.py'. Reloading...
INFO:     Shutting down
INFO:     Finished server process [67890]
INFO:     Started server process [12345]
INFO:     Application startup complete.
```

✅ **Hot reload is working!** Your changes are automatically applied.

</details>

<details open>
<summary><h4>Frontend Hot Reload</h4></summary>

**Start frontend dev server:**

```bash
# From project root
cd open-webui
npm run dev
```

You should see:

```
  VITE v5.x.x  ready in xxx ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

**How it works:**

- Changes to `.svelte` files → Instant browser update (HMR)
- Changes to `.ts/.js` files → Instant browser update
- Changes to styles → Instant browser update
- No manual refresh needed! 🎉

**Verify:**

1. Open `http://localhost:5173` in your browser
2. Edit any `.svelte` file in `src/`
3. Save the file
4. Watch your browser update automatically

</details>

### 🖥️ Development Workflow

#### Recommended Terminal Setup

**Open 2 terminals** for efficient development:

```bash
# ============================================
# TERMINAL 1: Backend (with hot reload)
# ============================================
cd /path/to/open-webui/backend

# Activate your environment
conda activate open-webui
# or: source venv/bin/activate

# Start backend with hot reload
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload --log-level debug

# ============================================
# TERMINAL 2: Frontend (with hot reload)
# ============================================
cd /path/to/open-webui

# Start frontend dev server
npm run dev
```

**Access your application:**

- **Frontend (Dev):** http://localhost:5173 ← Use this during development
- **Backend API:** http://localhost:8080
- **API Docs:** http://localhost:8080/docs (Swagger UI)

#### Optional: Database Monitoring Terminal

```bash
# ============================================
# TERMINAL 3: Database Monitoring (Optional)
# ============================================

# Watch PostgreSQL activity
watch -n 2 'psql -U openwebui_user -d openwebui -c "SELECT count(*) FROM pg_stat_activity;"'

# Or connect interactively
psql -U openwebui_user -d openwebui
```

### 🔧 Environment Modes

Configure via `.env` file:

```bash
# Development mode (verbose logging, hot reload friendly)
ENV=dev

# Test mode (for running tests)
ENV=test

# Production mode (optimized, minimal logging)
ENV=prod
```

### 📝 Complete Development .env File

Create/edit `.env` in project root:

```bash
# ==============================================
# ENVIRONMENT
# ==============================================
ENV=dev

# ==============================================
# BRANDING
# ==============================================
WEBUI_NAME=Quantum Scribe

# ==============================================
# DATABASE
# ==============================================
# Development: Use SQLite (fast, zero-config)
DATABASE_URL=sqlite:///./backend/data/webui.db

# Production: Use PostgreSQL
# DATABASE_URL=postgresql://openwebui_user:your_password@localhost:5432/openwebui

# Connection Pool Settings
DATABASE_POOL_SIZE=20
DATABASE_POOL_MAX_OVERFLOW=10
DATABASE_POOL_TIMEOUT=30
DATABASE_POOL_RECYCLE=3600
ENABLE_DB_MIGRATIONS=true

# ==============================================
# LOGGING (Verbose for development)
# ==============================================
GLOBAL_LOG_LEVEL=DEBUG

# ==============================================
# DEVELOPMENT OPTIONS
# ==============================================
# Disable version check (faster startup)
ENABLE_VERSION_UPDATE_CHECK=false

# Disable safe mode
SAFE_MODE=false

# ==============================================
# PERFORMANCE (Development)
# ==============================================
# Single worker for easier debugging
UVICORN_WORKERS=1

# Disable realtime save (use manual save)
ENABLE_REALTIME_CHAT_SAVE=false

# Enable query caching
ENABLE_QUERIES_CACHE=true

# ==============================================
# FEATURES
# ==============================================
# Enable/disable features as needed
ENABLE_WEBSOCKET_SUPPORT=true
ENABLE_COMPRESSION_MIDDLEWARE=true

# ==============================================
# SECURITY (Development)
# ==============================================
# Use a simple secret key for dev
WEBUI_SECRET_KEY=dev-secret-key-change-in-production

# Enable auth (recommended even for dev)
WEBUI_AUTH=true

# ==============================================
# REDIS (Optional - for WebSocket)
# ==============================================
# REDIS_URL=redis://localhost:6379
# WEBSOCKET_MANAGER=redis
```

### 🏗️ Build Commands

<details>
<summary><h4>Development Build</h4></summary>

```bash
# Just run dev servers (no build needed)
# Terminal 1:
cd backend
python -m uvicorn open_webui.main:app --reload

# Terminal 2:
npm run dev
```

</details>

<details>
<summary><h4>Production Build</h4></summary>

```bash
# Clean previous builds
rm -rf .svelte-kit build

# Build frontend
npm run build

# The built files will be in:
# - ./build/ (static files)
# - ./.svelte-kit/output/ (SvelteKit output)

# Backend doesn't need building for Python
# Just run with production settings:
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --workers 4
```

</details>

<details>
<summary><h4>Clean Build (When Things Break)</h4></summary>

```bash
# Stop all running servers first!

# Clean ALL build artifacts
rm -rf .svelte-kit build node_modules/.vite

# Reinstall dependencies (if needed)
npm install

# Rebuild
npm run build

# Or for development
npm run dev
```

</details>

### 🐛 Development Tips

<details>
<summary><h4>Debugging Backend</h4></summary>

**Enable debug logging:**

```bash
# In .env
GLOBAL_LOG_LEVEL=DEBUG

# Or run with debug flag
python -m uvicorn open_webui.main:app --reload --log-level debug
```

**Use Python debugger:**

```python
# Add to any Python file
import pdb; pdb.set_trace()

# Or use breakpoint() (Python 3.7+)
breakpoint()
```

**VS Code debugging:**

Create `.vscode/launch.json`:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: FastAPI",
            "type": "python",
            "request": "launch",
            "module": "uvicorn",
            "args": [
                "open_webui.main:app",
                "--reload",
                "--host", "0.0.0.0",
                "--port", "8080"
            ],
            "jinja": true,
            "justMyCode": false,
            "cwd": "${workspaceFolder}/backend"
        }
    ]
}
```

</details>

<details>
<summary><h4>Debugging Frontend</h4></summary>

**Browser DevTools:**

- Press `F12` to open DevTools
- Check Console for errors
- Check Network tab for API calls
- Use Vue/Svelte DevTools extension

**Svelte debugging:**

```svelte
<script>
  // Add console logs
  console.log('Component data:', data);
  
  // Use reactive statements for debugging
  $: console.log('Value changed:', someValue);
</script>
```

**Enable source maps:**

Source maps are enabled by default in dev mode, allowing you to debug original source code.

</details>

### 🔄 Common Development Workflows

<details>
<summary><h4>Making Changes to Backend</h4></summary>

1. **Edit Python files** in `backend/open_webui/`
2. **Save the file**
3. **Uvicorn automatically reloads** (watch terminal)
4. **Refresh browser** (if needed)
5. **Check logs** for any errors

**Example: Add a new API endpoint**

```python
# backend/open_webui/routers/custom.py
from fastapi import APIRouter

router = APIRouter()

@router.get("/hello")
async def hello():
    return {"message": "Hello from Quantum Scribe!"}
```

**Register the router** in `backend/open_webui/main.py`

**Test:** http://localhost:8080/hello

</details>

<details>
<summary><h4>Making Changes to Frontend</h4></summary>

1. **Edit `.svelte` files** in `src/`
2. **Save the file**
3. **Vite HMR updates browser instantly**
4. **Check browser console** for any errors

**Example: Customize homepage**

```svelte
<!-- src/routes/+page.svelte -->
<script>
  let title = "Welcome to Quantum Scribe";
</script>

<h1>{title}</h1>
<p>Your AI-powered knowledge base</p>
```

**Save and watch the browser update automatically! 🎉**

</details>

<details>
<summary><h4>Database Schema Changes</h4></summary>

1. **Modify models** in `backend/open_webui/models/`
2. **Create migration** (if using Alembic)
3. **Apply migration**
4. **Restart backend**

**Or for development:**

```bash
# Drop and recreate database
rm backend/data/webui.db
# Restart backend - tables will be recreated
```

</details>

### 📊 Development Tools

**Recommended VS Code Extensions:**

```json
{
  "recommendations": [
    "ms-python.python",
    "ms-python.vscode-pylance",
    "svelte.svelte-vscode",
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "ms-azuretools.vscode-docker"
  ]
}
```

**Browser Extensions:**

- [Svelte DevTools](https://chrome.google.com/webstore) - For debugging Svelte components
- [React DevTools](https://chrome.google.com/webstore) - If using React components
- [Pesticide](https://chrome.google.com/webstore) - CSS debugging

**API Testing:**

- **Built-in:** http://localhost:8080/docs (Swagger UI)
- **Postman:** https://www.postman.com/
- **HTTPie:** https://httpie.io/

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

## 📚 Quick Reference

### Common Commands Cheat Sheet

<details>
<summary><h4>🚀 Development Servers</h4></summary>

```bash
# Start backend (hot reload)
cd backend
python -m uvicorn open_webui.main:app --host 0.0.0.0 --port 8080 --reload

# Start frontend (hot reload)
npm run dev

# Linux alternative (backend)
cd backend
sh dev.sh
```

</details>

<details>
<summary><h4>🏗️ Build Commands</h4></summary>

```bash
# Clean build artifacts
rm -rf .svelte-kit build

# Build frontend for production
npm run build

# Install/update dependencies
npm install --force
pip install -r backend/requirements.txt -U
```

</details>

<details>
<summary><h4>🗄️ PostgreSQL Commands</h4></summary>

```bash
# Start PostgreSQL
net start postgresql-x64-16                # Windows
sudo systemctl start postgresql            # Linux

# Connect to database
psql -U openwebui_user -d openwebui -h localhost

# Backup database
pg_dump -U openwebui_user -d openwebui -F c -f backup.dump

# Restore database
pg_restore -U openwebui_user -d openwebui -v backup.dump

# Check database size
psql -U openwebui_user -d openwebui -c "SELECT pg_size_pretty(pg_database_size('openwebui'));"
```

</details>

<details>
<summary><h4>🎨 Branding Commands</h4></summary>

```bash
# Find all icons and logos
find . -name "favicon*" -o -name "logo*" -o -name "*icon*" | grep -E "\.(png|svg|ico|jpg)$"

# Replace branding (after preparing assets)
cp quantum-scribe-branding/* static/static/
rm -rf .svelte-kit build
npm run build
```

</details>

<details>
<summary><h4>🐍 Python Environment</h4></summary>

```bash
# Conda
conda create --name open-webui python=3.11
conda activate open-webui
conda deactivate

# venv
python3 -m venv venv
source venv/bin/activate          # Linux/macOS
.\venv\Scripts\activate           # Windows
deactivate
```

</details>

### File Structure Reference

```
open-webui/
├── .env                              # Environment configuration
├── .env.example                      # Example environment file
├── package.json                      # Frontend dependencies
├── vite.config.ts                    # Vite configuration
│
├── backend/
│   ├── requirements.txt              # Python dependencies
│   ├── open_webui/
│   │   ├── main.py                   # FastAPI application entry
│   │   ├── env.py                    # ⭐ Configuration (WEBUI_NAME here)
│   │   ├── models/                   # Database models
│   │   ├── routers/                  # API endpoints
│   │   ├── utils/                    # Utility functions
│   │   └── static/                   # ⭐ Backend static files (built)
│   └── data/
│       └── webui.db                  # SQLite database (if used)
│
├── src/                              # Frontend source code
│   ├── lib/                          # Shared components
│   ├── routes/                       # Page components
│   └── app.html                      # HTML template
│
├── static/
│   └── static/                       # ⭐ Frontend static assets (source)
│       ├── favicon.png
│       ├── favicon.svg
│       ├── favicon.ico
│       ├── favicon-96x96.png
│       ├── favicon-dark.png
│       ├── apple-touch-icon.png
│       └── logo.png
│
├── build/                            # Production build output
├── .svelte-kit/                      # SvelteKit build cache
└── node_modules/                     # Node.js dependencies
```

### Key URLs

| Service | Development | Production |
|---------|-------------|------------|
| **Frontend** | http://localhost:5173 | https://your-domain.com |
| **Backend API** | http://localhost:8080 | https://your-domain.com/api |
| **API Docs** | http://localhost:8080/docs | - |
| **PostgreSQL** | localhost:5432 | localhost:5432 |

### Environment Variables Quick Reference

```bash
# Core Settings
ENV=dev|test|prod                     # Environment mode
WEBUI_NAME=Quantum Scribe             # Application name

# Database
DATABASE_URL=postgresql://user:pass@host:port/dbname
DATABASE_POOL_SIZE=20                 # Connection pool size

# Performance
UVICORN_WORKERS=1                     # Dev: 1, Prod: 4+
ENABLE_QUERIES_CACHE=true             # Enable query caching

# Security
WEBUI_SECRET_KEY=your-secret-key      # Change in production!
WEBUI_AUTH=true                       # Enable authentication

# Logging
GLOBAL_LOG_LEVEL=INFO|DEBUG           # Log verbosity
ENABLE_AUDIT_LOGS_FILE=true           # Enable audit logging
```

### Troubleshooting Quick Fixes

<details>
<summary><h4>❌ "Port already in use"</h4></summary>

**Backend (port 8080):**
```bash
# Windows
netstat -ano | findstr :8080
taskkill /PID <PID> /F

# Linux/macOS
lsof -i :8080
kill -9 <PID>
```

**Frontend (port 5173):**
```bash
# Windows
netstat -ano | findstr :5173
taskkill /PID <PID> /F

# Linux/macOS
lsof -i :5173
kill -9 <PID>
```

</details>

<details>
<summary><h4>❌ "Module not found" errors</h4></summary>

```bash
# Backend
pip install -r backend/requirements.txt -U

# Frontend
rm -rf node_modules package-lock.json
npm install --force
```

</details>

<details>
<summary><h4>❌ Hot reload not working</h4></summary>

```bash
# Make sure you're using --reload flag
python -m uvicorn open_webui.main:app --reload

# Frontend: make sure dev server is running
npm run dev
```

</details>

<details>
<summary><h4>❌ Database connection failed</h4></summary>

```bash
# Check PostgreSQL is running
net start postgresql-x64-16           # Windows
sudo systemctl start postgresql       # Linux

# Test connection
psql -U openwebui_user -d openwebui -h localhost

# Check .env DATABASE_URL is correct
```

</details>

<details>
<summary><h4>❌ Assets/icons not updating</h4></summary>

```bash
# Clear build cache
rm -rf .svelte-kit build

# Rebuild
npm run build

# Hard refresh browser
# Chrome/Edge: Ctrl + Shift + R
# Firefox: Ctrl + F5
```

</details>

---

## 🤝 Contributing

### Development Setup for Contributors

1. **Fork the repository**
2. **Clone your fork:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/open-webui.git
   cd open-webui
   ```
3. **Create a branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```
4. **Make your changes**
5. **Test thoroughly**
6. **Commit and push:**
   ```bash
   git add .
   git commit -m "Add your feature"
   git push origin feature/your-feature-name
   ```
7. **Create a Pull Request**

### Code Style

- **Python:** Follow PEP 8
- **JavaScript/Svelte:** Use Prettier (configured in project)
- **Commits:** Use conventional commits format

---

## 📖 Additional Resources

### Official Documentation
- **Open WebUI:** https://docs.openwebui.com
- **PostgreSQL:** https://www.postgresql.org/docs/
- **SvelteKit:** https://kit.svelte.dev/docs
- **FastAPI:** https://fastapi.tiangolo.com/

### Community
- **Open WebUI GitHub:** https://github.com/open-webui/open-webui
- **Discord:** https://discord.gg/5rJgQTnV4s
- **Discussions:** https://github.com/open-webui/open-webui/discussions

### Useful Tools
- **pgAdmin:** https://www.pgadmin.org/ (PostgreSQL GUI)
- **DBeaver:** https://dbeaver.io/ (Universal database tool)
- **Postman:** https://www.postman.com/ (API testing)
- **VS Code:** https://code.visualstudio.com/ (Recommended IDE)

---

## 📄 License

This documentation is based on [Open WebUI](https://github.com/open-webui/open-webui) which is licensed under the MIT License.

---

## 📝 Changelog

### Version 1.0.0 (February 2026)
- ✅ Complete installation guide with Conda and venv options
- ✅ Branding customization (Quantum Scribe)
- ✅ PostgreSQL database configuration
- ✅ System requirements for 30+ user deployment
- ✅ Development environment setup with hot reload
- ✅ Production deployment guides
- ✅ Troubleshooting section
- ✅ Maintenance and backup procedures
- ✅ Quick reference commands

---

<div align="center">

### 🚀 Quantum Scribe

**Built with ❤️ on top of [Open WebUI](https://github.com/open-webui/open-webui)**

**Document Version:** 1.0.0  
**Last Updated:** February 2026  

[![Open WebUI](https://img.shields.io/badge/Based%20on-Open%20WebUI-blue)](https://github.com/open-webui/open-webui)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL%2016-336791)](https://www.postgresql.org/)
[![Python](https://img.shields.io/badge/Python-3.11%2B-blue)](https://www.python.org/)
[![SvelteKit](https://img.shields.io/badge/Frontend-SvelteKit-FF3E00)](https://kit.svelte.dev/)

---

*For support, please open an issue on the [GitHub repository](https://github.com/open-webui/open-webui/issues)*

</div>
