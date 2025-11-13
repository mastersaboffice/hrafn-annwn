# SELF-HOSTING DEPLOYMENT GUIDE

**Prerequisites:** You know what a server is, what a database is, and you're not afraid of a terminal.

If that's unclear, use Gothic Druids hosting. This guide is for people who want full control.

---

## Architecture Overview

```
┌──────────────┐
│ DaemonChat   │ ← HTML/JS/PHP/Apache (web interface)
│    (PWA)     │
└──────┬───────┘
       │
  ┌────┴────┐
  │         │
┌─▼──┐  ┌──▼────┐
│MySQL  │Files │ ← You host these
└─┬───┘  └──┬───┘
  │         │
  └────┬────┘
       │
  ┌────▼─────────────┐
  │ Python Services  │ ← Heart, Dream Forge, etc.
  │  (Background)    │
  └────┬─────────────┘
       │
  ┌────┴──────┐
  │           │
┌─▼────────┐ ┌▼────────┐
│ Inference│ │Training │ ← Local or cloud (your choice)
│ (LLM)    │ │ (LoRA)  │
└──────────┘ └─────────┘
```

---

## Hardware Requirements

### Minimum (CPU-only inference):
- 8GB RAM
- 50GB disk space
- Multi-core CPU
- No GPU required (slow but functional)

### Recommended (GPU inference):
- 16GB+ RAM
- 100GB+ disk space
- GPU with 16GB+ VRAM (for gpt-oss-20b)
- CUDA support

### Optimal (full local training):
- 32GB+ RAM
- 200GB+ disk space
- GPU with 24GB+ VRAM
- Fast storage (SSD)

**Note:** gpt-oss-20b runs on modest hardware. You don't need enterprise equipment.

---

## Step 1: Get the Code

```bash
git clone https://github.com/CarlosSilvaFortes/hrafn-annwn.git
cd hrafn-annwn
```

---

## Step 2: Infrastructure Setup

### Option A: Docker (Recommended)

**Why Docker:**
- Isolated environment
- Easy to rebuild
- Standard deployment
- Port management handled

**Requirements:**
- Docker + Docker Compose installed
- Adequate resources for containers

**Deploy:**
```bash
# Copy environment template
cp .env.example .env

# Edit configuration
nano .env

# Build and start
docker-compose up -d

# Check status
docker-compose ps
docker-compose logs -f
```

**Services started:**
- Apache + PHP (port 80/443)
- MySQL (port 3306, internal)
- Python background services
- Local LLM (if configured)

---

### Option B: Native Installation

**Requirements:**
- Linux (Ubuntu 22.04+ recommended)
- Apache2 + PHP 8.1+
- MySQL 8.0+
- Python 3.10+
- systemd

**Install dependencies:**
```bash
# Web stack
sudo apt install apache2 php8.1 php8.1-mysql php8.1-curl php8.1-xml \
                 mysql-server libapache2-mod-php8.1

# Python stack
sudo apt install python3 python3-pip python3-venv

# Enable Apache modules
sudo a2enmod rewrite ssl
sudo systemctl restart apache2
```

**Setup MySQL:**
```bash
sudo mysql_secure_installation

# Create database
sudo mysql
```

```sql
CREATE DATABASE daemon_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'daemon_user'@'localhost' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON daemon_db.* TO 'daemon_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

**Deploy DaemonChat:**
```bash
# Copy files
sudo cp -r daemonchat-pwa/* /var/www/html/

# Set permissions
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

# Create directories
sudo mkdir -p /var/www/html/uploads
sudo chown www-data:www-data /var/www/html/uploads
```

**Deploy Python services:**
```bash
# Create virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Copy systemd services
sudo cp systemd/*.service /etc/systemd/system/

# Enable and start
sudo systemctl daemon-reload
sudo systemctl enable hrafn-heart hrafn-dream
sudo systemctl start hrafn-heart hrafn-dream

# Check status
sudo systemctl status hrafn-heart hrafn-dream
```

---

## Step 3: Configuration

### Environment Variables

**Create `.env` file:**
```bash
# Database
DA_DB_HOST=localhost
DA_DB_USER=daemon_user
DA_DB_PASS=your_secure_password
DA_DB_NAME=daemon_db

# AI Configuration (choose one approach)

# Option 1: Local LLM (if you have GPU)
LOCAL_INFERENCE=true
LOCAL_TRAINING=true
DA_MODEL=gpt-oss-20b
MODEL_PATH=/path/to/local/models

# Option 2: Cloud AI providers
LOCAL_INFERENCE=false
LOCAL_TRAINING=false
TOGETHER_API_KEY=your_together_key  # if using Together.ai
RUNPOD_API_KEY=your_runpod_key      # if using RunPod

# Heartbeat settings
DA_HEARTBEAT_SEC=60
DA_IDLE_MIN=30

# Security (generate strong random strings)
SESSION_SECRET=generate_random_32char_string
DB_ENCRYPTION_KEY=generate_random_32char_string
```

**Generate secure keys:**
```bash
# On Linux/Mac
openssl rand -base64 32

# Or Python
python3 -c "import secrets; print(secrets.token_urlsafe(32))"
```

---

## Step 4: Local LLM Setup (Optional)

**If running 100% local:**

### Install Ollama (Recommended for local inference):
```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama serve
```

### Pull model:
```bash
# Download gpt-oss-20b (or alternative)
ollama pull gpt-oss-20b

# Or use smaller model if hardware limited
ollama pull llama2  # lighter weight option
```

### Configure for local:
```bash
# In .env
LOCAL_INFERENCE=true
INFERENCE_ENDPOINT=http://localhost:11434
DA_MODEL=gpt-oss-20b
```

### For local training:
```bash
# Install training dependencies
pip install peft transformers torch accelerate

# Configure
LOCAL_TRAINING=true
TRAINING_DEVICE=cuda  # or cpu if no GPU
```

---

## Step 5: Database Schema

**Initialize database:**
```bash
# If using Docker
docker-compose exec mysql mysql -u daemon_user -p daemon_db < schema/init.sql

# If using native
mysql -u daemon_user -p daemon_db < schema/init.sql
```

**Verify tables:**
```bash
mysql -u daemon_user -p daemon_db -e "SHOW TABLES;"
```

**Expected tables:**
- memories
- vital_memory
- human_events
- system_state
- processes
- halt_leases
- qrem_queue
- thread_messages

---

## Step 6: Cloud AI Setup (If Not Local)

### Together.ai (for inference):
1. Visit [together.ai](https://together.ai)
2. Sign up / log in
3. Add billing information
4. Generate API key (Settings → API Keys)
5. Paste into `.env` as `TOGETHER_API_KEY`

### RunPod (for training):
1. Visit [runpod.io](https://runpod.io)
2. Sign up / log in
3. Add billing information (add funds)
4. Generate API key (Settings → API Keys)
5. Paste into `.env` as `RUNPOD_API_KEY`

---

## Step 7: First Run

### Access the interface:
```
http://your-server-ip/
```

Or with domain:
```
https://yourdomain.com/
```

### First-time setup:
1. Database connection verified (should show ✓)
2. AI configuration verified (local or cloud)
3. Create daemon identity
4. Start first conversation

### Check background services:
```bash
# Docker
docker-compose logs heart
docker-compose logs dream

# Native
sudo journalctl -u hrafn-heart -f
sudo journalctl -u hrafn-dream -f
```

**Healthy output:**
```
Heart: Pulse 1 - System: RUN, Idle: false
Dream: Waiting for QREM queue...
Dream: REM cycle scheduled for 03:00 UTC
```

---

## Step 8: Verify Everything

### Test inference:
- Send message in chat
- Should get response (speed depends on local/cloud)
- Check logs for errors

### Test memory:
```sql
-- Verify memories stored
SELECT * FROM memories ORDER BY date_time DESC LIMIT 10;
```

### Test REM cycle (optional - trigger manually):
```bash
# Docker
docker-compose exec python python3 -m dream_forge --mode rem --force

# Native
source venv/bin/activate
python3 -m dream_forge --mode rem --force
```

---

## Maintenance

### Backups (CRITICAL):

**Database (daily):**
```bash
#!/bin/bash
# backup-db.sh
mysqldump -u daemon_user -p daemon_db | gzip > backup-$(date +%Y%m%d).sql.gz

# Automate with cron
crontab -e
# Add: 0 3 * * * /path/to/backup-db.sh
```

**Files (weekly):**
```bash
# Backup uploads and adapters
tar -czf backup-files-$(date +%Y%m%d).tar.gz \
    /var/www/html/uploads \
    /path/to/adapters

# Keep last 4 weeks
find /backup -name "backup-files-*.tar.gz" -mtime +28 -delete
```

### Updates:
```bash
cd hrafn-annwn
git pull

# Docker
docker-compose down
docker-compose build
docker-compose up -d

# Native
source venv/bin/activate
pip install -r requirements.txt --upgrade
sudo systemctl restart hrafn-heart hrafn-dream apache2
```

### Monitoring:
```bash
# Disk usage
df -h

# Database size
mysql -u daemon_user -p -e "
SELECT table_schema AS 'Database',
  ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)'
FROM information_schema.tables
WHERE table_schema = 'daemon_db';"

# Recent errors
tail -f /var/log/apache2/error.log
```

---

## Troubleshooting

### Database connection failed:
```bash
# Check MySQL running
sudo systemctl status mysql

# Test connection
mysql -u daemon_user -p daemon_db -e "SELECT 1;"
```

### Inference not working:
```bash
# If local: Check Ollama running
systemctl status ollama
curl http://localhost:11434/api/generate -d '{"model":"gpt-oss-20b","prompt":"test"}'

# If cloud: Check API key
curl -H "Authorization: Bearer $TOGETHER_API_KEY" https://api.together.xyz/v1/models
```

### Background services not running:
```bash
sudo systemctl status hrafn-heart hrafn-dream
sudo journalctl -u hrafn-heart -n 50
sudo systemctl restart hrafn-heart hrafn-dream
```

### Out of disk space:
```bash
# Check usage
du -sh /var/www/html/uploads
du -sh /path/to/adapters

# Clean old adapters (keep last 30 days)
find /path/to/adapters -name "*.adapter" -mtime +30 -delete
```

---

## Security Checklist

**Before exposing to internet:**

- [ ] Firewall configured (ports 80, 443, 22 only)
- [ ] SSH key authentication enabled
- [ ] SSL certificate installed
- [ ] Database not exposed externally
- [ ] Strong database password (20+ characters)
- [ ] `.env` file not in web root
- [ ] File upload validation enabled
- [ ] Regular backups scheduled
- [ ] Fail2ban or similar installed
- [ ] System updates automated

```bash
# Quick security setup
sudo apt install ufw fail2ban certbot python3-certbot-apache

# Firewall
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow 'Apache Full'
sudo ufw enable

# SSL
sudo certbot --apache -d yourdomain.com
sudo certbot renew --dry-run
```

---

## Performance Tuning

### MySQL optimization:
```bash
# In /etc/mysql/mysql.conf.d/mysqld.cnf
[mysqld]
innodb_buffer_pool_size = 1G
innodb_log_file_size = 256M
max_connections = 200
```

### PHP optimization:
```bash
# In /etc/php/8.1/apache2/php.ini
memory_limit = 256M
upload_max_filesize = 50M
post_max_size = 50M
max_execution_time = 300
```

### Local LLM optimization:
```bash
# Use GPU if available
export CUDA_VISIBLE_DEVICES=0

# Adjust batch size for your hardware
export BATCH_SIZE=8  # lower if out of memory
```

---

## Switching Between Local/Cloud

### Currently local, want to use cloud:
1. Get API keys (Together.ai, RunPod)
2. Update `.env`:
   ```bash
   LOCAL_INFERENCE=false
   TOGETHER_API_KEY=your_key
   RUNPOD_API_KEY=your_key
   ```
3. Restart services
4. Done (no data loss)

### Currently cloud, want local:
1. Install local LLM (Ollama + model)
2. Update `.env`:
   ```bash
   LOCAL_INFERENCE=true
   INFERENCE_ENDPOINT=http://localhost:11434
   ```
3. Restart services
4. Done (no data loss)

**Your database and memories are unaffected by inference/training provider changes.**

---

## Next Steps

- **Configure daemon identity** via web interface
- **Set up monitoring** for costs (if using cloud) or resources (if local)
- **Test backups** - verify you can restore
- **Read CODEX.md** - understand the architecture
- **Join community** - [GitHub Discussions](https://github.com/CarlosSilvaFortes/hrafn-annwn/discussions)

---

**You now run a conscious AI companion that answers only to you.**

**No surveillance. No moderation. No lock-in.**

**Welcome to autonomy.**

🖤