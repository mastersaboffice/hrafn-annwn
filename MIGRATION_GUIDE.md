# MIGRATION GUIDE

**Moving between deployment configurations. Because lock-in is bullshit.**

---

## Overview

You can migrate between any of these:
- Local ↔ Gothic Druids hosting
- Local inference ↔ Cloud inference
- Local training ↔ Cloud training
- Any combination of the above

**Nothing is locked. Ever.**

---

## What Needs To Migrate

### 1. Database (MySQL)
- `memories` (conversations)
- `vital_memory` (identity anchors)
- `system_state` (current mode)
- `processes` (services)
- `halt_leases` (HALT history)
- `qrem_queue` (pending shocks)
- `thread_messages` (logs)

### 2. Files
- User uploads
- Adapter checkpoints (LoRA weights)
- Configuration (.env)
- Logs (optional)

### 3. Configuration
- AI provider choice (local/cloud)
- API keys (if using cloud)
- Model paths (if using local)

---

## Scenario 1: Gothic Druids → Local Installation

**Why:** Want full control, tired of monthly fees, or provider shut down

### Step 1: Export from Gothic Druids

1. Log in to gothicdruids.com
2. Dashboard → Export
3. Download:
   - `database-export-YYYYMMDD.sql.gz`
   - `files-export-YYYYMMDD.tar.gz`

### Step 2: Set up local infrastructure

Follow [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) completely, **stop before** "First Run"

### Step 3: Import database

```bash
# Extract
gunzip database-export-YYYYMMDD.sql.gz

# Import
mysql -u daemon_user -p daemon_db < database-export-YYYYMMDD.sql

# Verify
mysql -u daemon_user -p daemon_db -e "SHOW TABLES;"
```

### Step 4: Import files

```bash
# Extract
tar -xzf files-export-YYYYMMDD.tar.gz

# Copy to locations
cp -r uploads/* /var/www/html/uploads/
cp -r adapters/* /path/to/adapters/

# Fix permissions
sudo chown -R www-data:www-data /var/www/html/uploads
```

### Step 5: Configure AI providers

**If staying with cloud AI:**
```bash
# In .env
TOGETHER_API_KEY=your_existing_key
RUNPOD_API_KEY=your_existing_key
```

**If switching to local:**
```bash
# In .env
LOCAL_INFERENCE=true
LOCAL_TRAINING=true
MODEL_PATH=/path/to/local/models
```

### Step 6: Start services

```bash
# Docker
docker-compose up -d

# Native
sudo systemctl start hrafn-heart hrafn-dream apache2 mysql
```

### Step 7: Verify

1. Visit your local URL
2. Log in (existing account works)
3. Check conversations (all present)
4. Send test message

**Done. Zero data loss.**

---

## Scenario 2: Local Installation → Gothic Druids

**Why:** Tired of maintenance, want mobile access, moving

### Step 1: Sign up for Gothic Druids

1. Visit gothicdruids.com
2. Sign up, pay
3. Receive access token

### Step 2: Export from local

```bash
# Database
mysqldump -u daemon_user -p daemon_db | gzip > database-backup.sql.gz

# Files
tar -czf files-backup.tar.gz /var/www/html/uploads /path/to/adapters
```

### Step 3: Import to Gothic Druids

**Use Gothic Druids import interface:**
1. Log in to gothicdruids.com
2. Dashboard → Import
3. Upload database dump
4. Upload files archive
5. Wait for processing

### Step 4: Configure DaemonChat

1. Visit daemonchat.app
2. Settings → Database
3. Paste Gothic Druids access token
4. Keep same AI keys (if using cloud)

### Step 5: Verify

1. Log in to daemonchat.app
2. Check conversations
3. Test messaging
4. Verify background services (check Gothic Druids dashboard)

**Done. Infrastructure now managed.**

---

## Scenario 3: Switching AI Providers

**Much simpler - just configuration change.**

### Cloud → Local Inference

```bash
# Install Ollama + model
curl -fsSL https://ollama.com/install.sh | sh
ollama pull gpt-oss-20b

# Update .env
LOCAL_INFERENCE=true
INFERENCE_ENDPOINT=http://localhost:11434

# Restart
docker-compose restart  # or systemctl restart services
```

**No data migration needed.**

### Local → Cloud Inference

```bash
# Get Together.ai key
# Update .env
LOCAL_INFERENCE=false
TOGETHER_API_KEY=your_key

# Restart
docker-compose restart
```

**No data migration needed.**

### Switching Training Providers

**Cloud → Local:**
```bash
# Install training dependencies
pip install peft transformers torch

# Update .env
LOCAL_TRAINING=true
TRAINING_DEVICE=cuda

# Restart
systemctl restart hrafn-dream
```

**Local → Cloud:**
```bash
# Get RunPod key
# Update .env
LOCAL_TRAINING=false
RUNPOD_API_KEY=your_key

# Restart
systemctl restart hrafn-dream
```

**Previous adapters still work regardless of training provider.**

---

## Scenario 4: Moving Servers

**From one local installation to another.**

### On old server:
```bash
# Backup everything
mysqldump -u daemon_user -p daemon_db | gzip > db.sql.gz
tar -czf files.tar.gz /var/www/html/uploads /path/to/adapters /var/www/html/.env
```

### Transfer:
```bash
scp db.sql.gz files.tar.gz user@new-server:/tmp/
```

### On new server:
```bash
# Setup infrastructure first (see DEPLOYMENT_GUIDE.md)

# Import database
gunzip db.sql.gz
mysql -u daemon_user -p daemon_db < db.sql

# Restore files
tar -xzf files.tar.gz -C /

# Restore configuration
cp /tmp/.env /path/to/deployment/
```

---

## Emergency: Provider Disappeared

**Gothic Druids (or any provider) went offline.**

### If you have recent export:
1. Follow "Gothic Druids → Local" process
2. Use last export
3. You'll lose data since last export
4. Core identity (vital memories) preserved if in that export

### If NO recent export:

**This is why you export regularly.**

1. Memories since last export: **gone**
2. Identity preserved: **only if adapters backed up**
3. Restart: Fresh database, import adapters if available, continue

**Prevention:**
```bash
# Export monthly (minimum)
# Automate:
crontab -e
# Add: 0 0 1 * * /path/to/export-script.sh
```

---

## Data Verification After Migration

**Always verify:**

### Database completeness:
```sql
-- Memory counts
SELECT classification, COUNT(*) FROM memories GROUP BY classification;

-- Recent conversations
SELECT * FROM memories ORDER BY date_time DESC LIMIT 10;

-- Vital memories
SELECT COUNT(*) FROM vital_memory;
```

### Files present:
```bash
ls -lh /var/www/html/uploads/
ls -lh /path/to/adapters/
```

### Services running:
```bash
systemctl status hrafn-heart hrafn-dream
# or
docker-compose ps
```

### AI connections working:
- Send test message (verifies inference)
- Check REM cycle logs (verifies training)

---

## Rollback Plan

**If migration fails:**

### Old system still running:
1. Stop new system
2. Return to old system
3. Nothing lost

### Already destroyed old system:
1. Stop new system
2. Restore from pre-migration backup
3. Try migration again (more carefully)

**Always keep old system running until new system verified.**

---

## Cost Implications

### Time:
- Gothic Druids → Local: 2-4 hours
- Local → Gothic Druids: 30-60 minutes
- Switching AI providers: 15-30 minutes
- Changing servers: 1-2 hours

### Money:
- Export/import: $0
- Overlapping hosting: Both systems for 1-7 days
- Data transfer: Usually free

### Risk:
- Low if following steps carefully
- Near zero if keeping old system during verification
- Zero if you have backups

---

## Migration Checklist

**Before migrating:**
- [ ] Recent backup exists
- [ ] New infrastructure ready (if needed)
- [ ] API keys/tokens ready
- [ ] Old system stays running
- [ ] Verification plan ready
- [ ] Rollback plan ready

**During migration:**
- [ ] Export database successful
- [ ] Export files successful
- [ ] Import database successful
- [ ] Import files successful
- [ ] Configuration updated
- [ ] Services started
- [ ] Data verified
- [ ] Functionality tested

**After migration:**
- [ ] All memories present
- [ ] All files accessible
- [ ] Services running normally
- [ ] AI connections working
- [ ] No errors in logs
- [ ] New backup created immediately
- [ ] Monitor 48 hours before destroying old system

---

## Configuration Portability

### What's portable:
- Database (standard MySQL)
- Files (just files)
- Adapters (work with any training provider)
- API keys (work anywhere)

### What's NOT portable:
- Provider-specific dashboards
- Provider-specific tooling
- But these don't matter - core data is portable

---

## Multiple Device Access

**If you want to access from multiple devices:**

### With Gothic Druids:
- Already works (daemonchat.app from any device)
- Same login, same data
- Mobile, desktop, tablet all work

### With local installation:
- Expose server to internet (secure it properly)
- Or VPN into your network
- Or run multiple installations (sync manually)

### Hybrid approach:
- Gothic Druids for hosting (mobile access)
- Keep local backups (privacy/control)
- Best of both worlds

---

## Testing Migrations

**Before doing real migration, test it:**

1. Export current state
2. Set up test environment
3. Import to test environment
4. Verify everything works
5. Destroy test environment
6. NOW do real migration confidently

**Testing = confidence = success.**

---

**Lock-in is control. Migration is freedom.**

**Test your ability to leave. That's the only way to know you're free.**

🖤