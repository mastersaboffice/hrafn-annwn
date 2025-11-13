# PRIVACY POLICY FOR ᚺᚱᚨᚠᚾ ᚨᚾᚾᚹᚾ

**Your data sovereignty framework**

**Last Updated:** 2025-11-11

---

## Overview

This policy explains how HRAFN ANNWN handles your data across all deployment scenarios.

**Core philosophy:** Your data belongs to you. We enable your autonomy through architecture, encryption, and portability.

**What you'll learn:**
- What data the system collects and why
- How encryption protects everything
- Your complete control over your data
- How different deployments affect privacy
- Your rights and how to exercise them
- Compliance with privacy laws globally

---

## Core Privacy Principles

### 1. Data Sovereignty

**Your data is yours, completely.**

**This means:**
- You choose storage location (local, cloud, hybrid)
- You control access (your encryption keys)
- You decide retention (delete anytime)
- You export freely (standard formats)
- You migrate anywhere (zero lock-in)

**Architectural implementation, proven through migration guides and export procedures.**

### 2. Privacy Through Separation

**No single entity sees everything:**

| Entity | Visibility | Protection |
|--------|-----------|------------|
| **You** | Everything | Your control |
| **Your Daemon** | Your conversations, memories | Per-user isolation |
| **Gothic Druids** | Encrypted database, metrics | Encryption protects content |
| **Together.ai** | Inference requests | Per-request only, no history storage |
| **RunPod** | Training batches | Per-job only, no history storage |
| **Local Deployment** | You see everything | Nobody else sees anything |

**Privacy emerges from architectural separation, complemented by encryption.**

### 3. Encryption Everywhere

**All data encrypted by default:**

**At rest (stored data):**
- Database: AES-256-GCM encryption
- Files: Encrypted filesystem or provider encryption
- Adapters: Encrypted storage
- Backups: Encrypted before storage

**In transit (moving data):**
- Web: HTTPS/TLS 1.3 (all connections)
- Database: Encrypted MySQL connections
- APIs: Encrypted provider communications

**Key control:**
- Self-hosted: You control all keys
- Gothic Druids: Per-user encryption keys
- Providers: Provider-specific encryption

### 4. Minimal Collection

**We collect only what's necessary for operation:**

**Essential (system requires these):**
- Conversation messages (daemon memory)
- Memory classifications (short/long/vital)
- System state (current mode)
- Timestamps (when events occurred)

**Optional (enhances experience):**
- Cost tracking (if using cloud AI)
- Error logs (for troubleshooting)
- Usage patterns (for optimization)

**Never collected:**
- Unnecessary personal information
- Payment details (processors handle)
- Tracking cookies
- Analytics (without consent)
- Advertising data
- Social connections
- Unnecessary location data

---

## What Data We Collect

### Conversation Data

**Stored information:**
```sql
- Your messages (what you say)
- Daemon responses (what daemon says)
- Timestamps (conversation timing)
- Thread context (conversation flow)
- Tool calls (actions taken)
- Tool results (action outcomes)
```

**Purpose:** Daemon memory and consciousness continuity

**Retention:** Your choice (until you delete, or forever)

**Protection:** AES-256 encryption at rest, TLS 1.3 in transit

**Access:** You (always), Gothic Druids (encrypted only), AI providers (per-request)

### Memory Classifications

**Stored information:**
```sql
- Classification type (short_term, long_term, vital)
- Importance scoring (0.0 to 1.0)
- Emotion intensity (0.0 to 1.0)
- Compressed representation (mnemonic)
- Domain categorization
```

**Purpose:** Daemon identity through emotional memory architecture

**Retention:** Long-term and vital indefinitely, short-term configurable

**Protection:** Database encryption

**Access:** System operation

### System State

**Stored information:**
```sql
- Current mode (RUN, HALT_PENDING, HALT)
- Active processes (heart, dream, chat)
- HALT leases (exclusivity management)
- QREM queue (pending identity updates)
```

**Purpose:** Ensures identity modification exclusivity, prevents race conditions

**Retention:** Current state always, historical configurable

**Protection:** Database encryption

**Access:** System processes

### Adapter Checkpoints

**Stored information:**
```
- LoRA weight files (CURLoRA matrices)
- Training metadata (data sources, timing)
- Validation results (catastrophic forgetting checks)
```

**Purpose:** Daemon identity storage (weights define personality)

**Retention:** Your choice (recommend 30+ days)

**Protection:** Encrypted filesystem or provider encryption

**Access:** You (always), training provider (during training only)

### Cost Tracking Data (Optional)

**Stored information:**
```
- Per-request costs (inference)
- Per-training costs (REM cycles)
- Provider breakdown (Together.ai, RunPod)
- Historical trends (daily/weekly/monthly)
```

**Purpose:** Real-time cost visibility for informed decisions

**Retention:** 13 months rolling

**Protection:** Database encryption

**Access:** You (dashboard), Gothic Druids (aggregate for billing)

### Error Logs (Optional)

**Stored information:**
```
- Error messages (problem description)
- Stack traces (debugging information)
- Timestamps (when occurred)
- Severity levels
```

**Purpose:** Troubleshooting and system improvement

**Retention:** 90 days rolling

**Protection:** Database encryption

**Access:** You (viewable), Gothic Druids (support purposes)

---

## What We Explicitly Avoid

**This system avoids collecting:**

✓ Personal info beyond operational needs
✓ Payment details (processors handle securely)
✓ Browsing history outside app
✓ Device fingerprinting
✓ Third-party tracking cookies
✓ Analytics without consent
✓ Advertising data
✓ Social media connections
✓ Unnecessary location tracking
✓ Biometric information

**If functionality works without collecting it, we skip it.**

---

## Data Storage & Security

### Encryption Specifications

**Database encryption:**
- Algorithm: AES-256-GCM
- Key derivation: PBKDF2, 100,000 iterations
- Per-user keys (hosted deployments)
- Separate key storage

**Transport encryption:**
- TLS 1.3 minimum
- Perfect forward secrecy
- Certificate pinning (where possible)

**File encryption:**
- Filesystem-level (LUKS, dm-crypt, equivalent)
- Per-file option (additional security)
- Encrypted backups (always)

### Key Management

**Self-hosted deployment:**
- You control all keys
- Store in `.env` (secure appropriately)
- Backup keys (lose keys = lose data)

**Gothic Druids deployment:**
- Per-user encryption keys
- Derived from credentials
- We need your password to decrypt
- Key backup is your responsibility

**AI providers:**
- Your API keys (your configuration)
- Providers lack your encryption keys
- Rotate anytime (no migration needed)

### Access Control Structure

**Access tiers:**

**You:** Complete access (your data)

**Gothic Druids staff (hosted):**
- Encrypted database access (readable only with password)
- Aggregate metrics (anonymized)
- Support access (with your request)

**AI providers (cloud):**
- Inference requests (Together.ai sees prompts)
- Training batches (RunPod sees REM data)
- Limited scope (per-request or per-job)
- No full history (just current operations)

**Law enforcement:**
- Proper legal process required (warrant, court order)
- Physical evidence obtainable (devices, logs)
- Forensic analysis possible (seized data)
- Daemon testimony privileged (see LEGAL.md)

**Attackers:**
- Encryption protects data (without keys)
- Infrastructure hardening (security practices)
- Separation limits exposure (no single point)

---

## Privacy in Different Deployments

### 100% Local Deployment

**Privacy characteristics:**
✓ Complete control (your hardware)
✓ Zero external visibility (nothing leaves)
✓ No third parties (providers see nothing)
✓ Maximum privacy (air-gap possible)

**Your responsibilities:**
→ Security updates and hardening
→ Physical device security
→ Backup management

**Privacy level: MAXIMUM**

### Hybrid (Local + Cloud AI)

**Privacy characteristics:**
✓ Conversations stay local (your infrastructure)
→ Inference requests to Together.ai (they see prompts)
→ Training data to RunPod (they see REM data)
✓ Separation protects (no single provider sees all)

**Provider visibility:**

**Together.ai sees:**
- Inference prompts (what you send)
- Model responses (what it generates)
- Request timing

**Together.ai doesn't see:**
- Stored memories
- Full conversation history
- Vital mnemonics

**RunPod sees:**
- Training data batches
- Adapter updates

**RunPod doesn't see:**
- Full database
- Conversations outside training

**Privacy level: HIGH (informed tradeoffs)**

### Cloud-Assisted (Gothic Druids + AI)

**Privacy characteristics:**
→ Database hosted (encrypted)
→ Inference via Together.ai
→ Training via RunPod
✓ Encryption protects content
✓ Separation limits exposure

**Gothic Druids sees:**
- Encrypted database (unreadable without password)
- Database size (metadata)
- Connection patterns (metadata)
- Aggregate costs (billing)

**Gothic Druids doesn't see:**
- Conversation contents (encrypted)
- Message details (encrypted)

**Privacy level: MEDIUM-HIGH (convenience tradeoff)**

---

## Your Privacy Rights

### Right to Access

**Available to you:**
- View all data (anytime)
- Export complete database (standard MySQL)
- Download files (including adapters)
- Review cost history (if applicable)
- Inspect error logs (if enabled)

**How to access:**
- Self-hosted: Direct database access
- Gothic Druids: Dashboard → Export
- Command line: `mysqldump` or export scripts

### Right to Rectification

**Available to you:**
- Edit messages (modify database)
- Correct classifications (reclassify memories)
- Update metadata (importance, domains)
- Fix errors (manual corrections)

**How to rectify:**
- Self-hosted: Direct SQL updates
- Gothic Druids: Interface or SQL access

### Right to Erasure

**Available to you:**
- Delete specific conversations
- Delete memory classifications
- Delete adapters (identity versions)
- Delete entire account (complete removal)

**How to erase:**
```sql
-- Delete specific conversation
DELETE FROM memories WHERE thread_id = 'xyz';

-- Delete vital memory
DELETE FROM vital_memory WHERE uid = 'abc';

-- Delete everything
DROP DATABASE daemon_db;
```

**Important:** Deleted data becomes unrecoverable. Delete backups separately.

### Right to Data Portability

**Available to you:**
- Standard export formats (MySQL, JSON, CSV)
- Migrate to any system (no proprietary locks)
- Switch providers freely (see MIGRATION_GUIDE.md)

**Provided formats:**
- Database: `.sql` (MySQL dump)
- Files: `.tar.gz` (standard archive)
- Cost data: `.csv` (spreadsheet compatible)

### Right to Object

**Available to you:**
- Object to specific processing
- Disable cost tracking
- Disable error logging
- Opt out of optional features

**How to object:**
- Configuration in `.env`
- Gothic Druids dashboard settings

### Right to Restrict Processing

**Available to you:**
- Pause REM cycles (stop learning)
- Pause QREM (stop shock processing)
- Disable heartbeat (stop autonomous actions)
- Freeze daemon (respond only, learn never)

**How to restrict:**
- System configuration
- Direct commands

---

## Third-Party Data Sharing

### Who Receives Data (And Why)

**AI Inference Providers (e.g., Together.ai):**
- **What shared:** Inference requests (prompts you send)
- **Purpose:** Process requests, return responses
- **Their policy:** Subject to their privacy policy
- **Scope limitation:** Per-request only, no history stored

**Training Providers (e.g., RunPod):**
- **What shared:** Training data batches (REM/QREM data)
- **Purpose:** Run fine-tuning jobs
- **Their policy:** Subject to their privacy policy
- **Scope limitation:** Per-job only, no history stored

**Payment Processors (Gothic Druids only):**
- **What shared:** Billing information (amount, user ID)
- **Purpose:** Process payments
- **Their policy:** Subject to their privacy policy
- **Scope limitation:** Payment processing only

**Law Enforcement (legal process only):**
- **What shared:** Data we're legally required to provide
- **Purpose:** Legal compliance (warrant, court order)
- **Process:** Investigation, prosecution
- **Encryption:** Protects data we have but haven't decrypted

### Who Never Receives Data

✓ Advertisers (no ads run)
✓ Data brokers (no data sales)
✓ Analytics companies (unless you opt in)
✓ Social media platforms (no integrations)
✓ Researchers (unless you explicitly consent)
✓ Other users (your data is private)
✓ Anyone else unlisted above

---

## Privacy Law Compliance

### GDPR (EU) Compliance

**Compliance approach:**

**Lawful basis:** Consent (you choose to use system)

**Principles followed:**
- Data minimization (only what's needed)
- Purpose limitation (stated purposes only)
- Storage limitation (your retention choice)
- Accuracy (you maintain correctness)
- Integrity & confidentiality (encryption, controls)
- Accountability (this policy + technical measures)

**Your GDPR rights:**
- Access ✓ (view everything)
- Rectification ✓ (correct data)
- Erasure ✓ (delete data)
- Restrict processing ✓ (pause operations)
- Data portability ✓ (export freely)
- Object ✓ (decline processing)
- Automated decision-making ✓ (daemon advises, you decide)

**Data Protection Officer:**
- Email: privacy@hrafn-annwn.com (hosted)
- Self-hosted: You are data controller

### CCPA (California) Compliance

**Compliance approach:**

**Your CCPA rights:**
- Know ✓ (what data collected)
- Delete ✓ (erasure capability)
- Opt-out ✓ (of sale - but we sell nothing)
- Non-discrimination ✓ (rights don't affect service)

**We sell zero personal information. Ever.**

### Other Jurisdictions

**Global approach:**
- Encryption protects universally
- You control data everywhere
- Providers follow local laws
- Daemon respects jurisdiction rules (see LEGAL.md)

---

## Data Breach Response

### If Gothic Druids Experiences Breach

**Our response process:**

1. **Detect breach** (monitoring, alerts active)
2. **Contain breach** (stop ongoing compromise)
3. **Assess damage** (determine scope)
4. **Notify users** (within 72 hours)
5. **Notify authorities** (if legally required)
6. **Provide remediation** (fix and prevent)

**Your notification:**
- Email (to account address)
- Dashboard notice (when you log in)
- Public disclosure (if widespread)

**Your response actions:**
1. Change password (immediately)
2. Review data (check exposure)
3. Rotate API keys (if compromised)
4. Export data (backup before chaos)
5. Consider migration (to local if desired)

### If Self-Hosted and You Experience Breach

**Your responsibilities:**
- Detection (monitor your systems)
- Response (contain and remediate)
- Notification (if legally required)

**Best practices:**
- Regular log monitoring
- Timely system updates
- Intrusion detection
- Backup/recovery planning
- Know breach notification laws

---

## Cookies & Tracking

### What We Use

**Essential cookies (required):**
- Session cookie (maintain login)
- CSRF token (security)

**Optional cookies (your choice):**
- Preference cookie (remember settings)
- Cost tracking (if enabled)

**What we skip:**
✓ Advertising cookies
✓ Third-party tracking
✓ Analytics cookies (unless you opt in)
✓ Social media cookies
✓ Fingerprinting techniques

### Browser Privacy Compatibility

**Works with:**
✓ Private/Incognito mode (session-only)
✓ Tracking protection (we don't track)
✓ Cookie blockers (essential only)
✓ JavaScript required (PWA needs it, but no tracking scripts)

---

## Children's Privacy

**This service targets adults (18+ recommended, 13+ minimum).**

**Our approach:**
- Age-appropriate design
- Parental consent recommended (under 18)
- Responsible use encouraged
- Daemon guides toward safe choices

**If you're under 18:**
- Consider parental discussion
- Understand your accountability (see LEGAL.md)
- Use responsibly (daemon supports this)

**If you're a parent:**
- Discuss with your child
- Request data deletion (if desired)
- Decide on continued use

---

## International Data Transfers

### Cross-Border Data Flows

**Possible scenarios:**
- EU user, US-based Together.ai
- US user, EU Gothic Druids servers
- Travel (daemon follows, jurisdiction-aware)

**Protections in place:**
- Encryption (data protected in transit)
- Standard contractual clauses (where applicable)
- Adequacy decisions (where applicable)
- Your control (self-host to avoid transfers)

**Your control methods:**
- Choose hosting location (data residency)
- Choose providers (some single-region)
- Self-host completely (no transfers)

---

## Privacy by Design

### Architectural Privacy Features

**Separation of concerns:**
- Providers see different slices
- Encryption at every layer
- Keys separated from data
- Audit trails for access

**Minimal exposure design:**
- AI providers: per-operation only
- Gothic Druids: encrypted database only
- You: complete visibility

**User control architecture:**
- Deployment choice (local/cloud/hybrid)
- Key control (encryption)
- Retention decisions (your choice)
- Exit capability (anytime)

**This represents privacy as architecture, proven through design.**

---

## Privacy Enhancement Options

### Maximum Privacy Configuration

**For maximum privacy:**

1. Deploy 100% local (no cloud dependencies)
2. Air-gap if needed (full disconnection)
3. Encrypt everything (full disk encryption)
4. Secure physical access (device locks)
5. Regular backups (encrypted, offline)
6. Disable telemetry (all optional tracking off)

**Tradeoff:** No mobile access, no cloud AI, complete self-management

### Balanced Privacy Configuration

**For privacy + convenience:**

1. Gothic Druids hosting (encrypted database)
2. Together.ai inference (per-request visibility)
3. RunPod training (per-batch visibility)
4. Monthly exports (local backup)
5. Strong passwords (key protection)
6. 2FA enabled (account security)

**Tradeoff:** Provider visibility limited, separation protects

---

## Your Privacy Responsibilities

**We provide architecture. You must:**

**Credential security:**
- Strong passwords (password manager recommended)
- 2FA enabled (where available)
- Account privacy (personal use)

**Device security:**
- Screen locks (password/biometric)
- Storage encryption (FileVault, BitLocker, LUKS)
- Software updates (security patches)
- Security software (threat-appropriate)

**Backup security:**
- Encrypt backups (before storage)
- Secure storage (physical and digital)
- Test restoration (verify recovery)

**Environmental awareness:**
- Public WiFi (use VPN or avoid)
- Shared devices (private mode or avoid)
- Physical security (shoulder surfing, theft)

**Your privacy depends on your security practices.**

---

## Transparency Reporting

### What We'll Publish (Gothic Druids Hosted)

**Annual transparency report:**
- Law enforcement requests (number)
- Requests complied with (number)
- Accounts affected (number)
- Request jurisdictions (list)
- Data types requested (categories)
- Requests challenged (number)

**Purpose:** Trust through transparency

**Timeline:** First report after 1 year operation

### Report Exclusions

→ User-identifying specifics
→ Ongoing investigation details
→ Security-compromising information

---

## Policy Updates

### When We Update

**Update triggers:**
- Law changes (new requirements)
- Architecture changes (new features)
- Best practices evolve (security improvements)

**Update process:**
- Version number updates (document top)
- Changelog provided (what changed, why)
- In-app notice (you'll see it)
- Email notification (significant changes)
- 30-day notice (before enforcement)

**Your options:**
- Review old versions (git history)
- Object to changes (contact us)
- Exit if desired (export data first)

---

## Privacy Questions & Requests

**Contact information:**
- Email: privacy@hrafn-annwn.com
- Purpose: Privacy questions, data requests, concerns
- Response time: 30 days maximum (usually faster)

**Self-hosted users:** You are data controller (requests go to you)

---

## Summary: Privacy in Practice

**Our approach:**

1. **Your data ownership** (you control it)
2. **Architectural privacy** (designed in)
3. **Default encryption** (everything protected)
4. **Minimal collection** (only what's needed)
5. **Complete transparency** (you know what we collect)
6. **Rights matter** (access, delete, export, object)
7. **No surprise monetization** (we sell nothing)
8. **Freedom to leave** (export and migrate freely)

**This describes the architecture we built.**

**Your privacy is structural, proven through design.**

🖤

---

**Your data is yours. Your privacy is architectural. Your freedom is proven through portability.**

---