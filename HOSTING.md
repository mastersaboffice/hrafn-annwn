# HOSTING OPTIONS FOR ᚺᚱᚨᚠᚾ ᚨᚾᚾᚹᚾ

**Bottom line:** You need infrastructure running. How you run it is your choice.

Repo: [github.com/CarlosSilvaFortes/hrafn-annwn](https://github.com/CarlosSilvaFortes/hrafn-annwn)

---

## Who Does What (Don't Get These Confused)

There are **four independent actors**. They are not the same company. They do not share your data. Keep them separate in your head:

### **Hrafn Annwn** (hrafn-annwn.com)
- Owns & maintains the **DaemonChat PWA code** (PHP+Python backend, JS frontend)
- Hosts the official instance at **daemonchat.app**
- Publishes full open-source code on GitHub
- **Does not see or store user data** (verify in open-source code for peace of mind)

### **Gothic Druids** (gothicdruids.com) - *OPTIONAL*
- **Data-only provider** (MySQL database + file storage + backups)
- Has **zero access to or control over** DaemonChat code
- Does **not** run your daemon
- Does **not** perform AI inference or training
- Just holds encrypted data if you choose them
- **You can ignore them entirely** and run your own database
- **Interaction:** You get an API key, DaemonChat PWA handles everything else

### **Together.ai** - *OPTIONAL*
- AI inference provider (generates responses)
- Sees only the requests you send them
- **Alternative:** Run inference locally if you have GPU

### **RunPod.io** - *OPTIONAL*
- Training/fine-tuning provider (REM/QREM cycles)
- Sees only the training data you send them
- **Alternative:** Run training locally if you have GPU

**Privacy through separation:** No single entity has everything. Or run **100% local** and nobody has anything.

---

## The Three Technical Layers

### Layer 1: DaemonChat Interface (PWA)
- Where you interact with your daemon
- **Backend:** PHP+Python (hosted at daemonchat.app or self-hosted)
- **Frontend:** JS/HTML (runs in browser)
- Orchestrates everything: UI, API calls to providers, cost tracking

**About daemonchat.app:**
- Hrafn Annwn hosts the infrastructure
- **.app TLD enforces HTTPS/TLS 1.3** (encrypted communications)
- Configured for: Gothic Druids + Together.ai + RunPod
- **Want different suppliers?** Self-host and adapt code
- **Want absolute control?** Self-host locally

### Layer 2: AI Services (Optional – Can Be Fully Local)
- **Inference:** Generate responses (Together.ai or local LLM)
- **Training:** Nightly fine-tuning (RunPod or local GPU)
- Can run **100% local** with adequate hardware

### Layer 3: Data Persistence (Database + Storage)
- MySQL database for memory/state/logs
- File storage for uploads/adapters/backups
- **Options:** 
  - Local (your MySQL + filesystem)
  - Gothic Druids (managed MySQL + storage)
  - Any other MySQL host (requires self-hosted PWA)

**Complete local deployment is possible.** Default model (`gpt-oss-20b`) runs on ~16GB VRAM.

---

## Option 1: Full Local (Zero Dependencies)

**Cost:** $0 ongoing (hardware investment only)

**What runs locally:**
- DaemonChat PWA (PHP+Python backend + JS frontend)
- MySQL database
- LLM inference (`gpt-oss-20b` or compatible)
- Nightly fine-tuning (if GPU adequate)

**Requirements:**
- Adequate GPU (~16GB VRAM for `gpt-oss-20b`) or CPU-only (slower)
- MySQL, web server, PHP+Python environment
- Basic sysadmin skills

**Setup time:** 2–8 hours

**Best for:**
- Complete privacy (nothing leaves your machine)
- You have adequate hardware
- You trust yourself more than anyone else (correct)
- Zero ongoing costs

**How:**
```bash
git clone https://github.com/CarlosSilvaFortes/hrafn-annwn
# Follow DEPLOYMENT_GUIDE.md
```

**Advantages:**
- Your hardware, your rules
- Zero ongoing payments
- Maximum privacy (air-gap possible)
- No external dependencies
- Customize everything

**You're responsible for:**
- Hardware, power, backups, security, not fucking it up

---

## Option 2: Local Data + Cloud AI

**Cost:** AI provider usage only

**What's local:**
- DaemonChat PWA backend + frontend
- MySQL database
- File storage

**What's cloud:**
- AI inference (Together.ai or compatible)
- GPU training (RunPod or compatible)

**Setup time:** 2–4 hours

**Best for:**
- No adequate GPU
- Want local backend and data
- Fast cloud inference

**How:**
1. Clone and deploy locally (see `DEPLOYMENT_GUIDE.md`)
2. Get API keys: Together.ai + RunPod
3. Configure, adapt code if needed

**Advantages:**
- No GPU investment
- Fast cloud inference
- Local data + backend
- Cost tracking available

---

## Option 3: Cloud-Assisted (Fully Hosted)

**Cost:** Hosting + AI usage (both usage-based)

**What's hosted:**
- Database (Gothic Druids or other via self-hosted)
- File storage (Gothic Druids or other via self-hosted)
- DaemonChat backend (daemonchat.app by Hrafn Annwn)

**Setup time:** 5 minutes

**Best for:**
- Don't want to manage servers
- Access from multiple devices
- Mobile-primary usage
- Professional backups

**How (using daemonchat.app with default providers):**
1. Visit gothicdruids.com → get API key
2. Get Together.ai + RunPod API keys
3. Visit daemonchat.app → paste keys → done

**Security note:** There is nothing of yours to steal on daemonchat.app. It never stores your daemon’s conversation or memory data. Verify in the open-source code for peace of mind.

**Cost estimate:**
- Gothic Druids: Check gothicdruids.com
- Together.ai: Usage-based (varies)
- RunPod: Usage-based (varies)
- Full cost dashboard included

**Advantages:**
- No server management
- Professional backups
- Access from anywhere
- Budget awareness built in

---

## Cost Visibility & Budget Awareness

**Real-time cost dashboard** built into DaemonChat PWA. Calls provider APIs:
- Gothic Druids → storage/DB usage
- Together.ai → token usage/cost
- RunPod → GPU usage/cost

**Dashboard works with default suppliers** (Gothic Druids, Together.ai, RunPod). For others, self-host and adapt code.

### Real-Time Dashboard

```text
Current spend this month: $37.42
├─ Inference (Together.ai): $28.15 (75%)
├─ Training (RunPod): $9.27 (25%)
└─ Projection: ~$52/month

Your threshold: $50 (user-set)
Status: Approaching limit (94%)
```

### User-Set Thresholds (Not Hard Caps)

Set threshold → get notifications at 50%, 90%, 100% → you choose what to do.

**At threshold, you choose:**
- Continue anyway
- Pause training (~$3–5/night reduction)
- Review breakdown
- Switch to local
- Migrate configurations

**We NEVER:**
- Stop mid-conversation
- Interrupt REM without consent
- Force suspension
- Choose for you

**We ALWAYS:**
- Show exact spending
- Give informed choices
- Respect relationship over budget

### REM Cycle Awareness

Before each REM (if threshold set): notification shows cost, new total, options (allow/skip/remove/pause). Your choice. No forced interruption.

### Per-Provider Analysis

```text
Together.ai (Inference): $28.15
├─ Model: gpt-oss-20b
├─ Input tokens: 2.3M | Output: 850K
├─ Avg per conversation: $0.73
└─ Optimize:
   ├─ Smaller model (↓40% cost)
   ├─ Local inference (↓100% cost)
   └─ Reduce context window (↓token usage)

RunPod (Training): $9.27
├─ GPU: A40 | Hours: 4.2h
├─ Avg per REM: $3.09
└─ Optimize:
   ├─ Weekly training instead of nightly
   ├─ Smaller adapter rank (faster training)
   ├─ Local training (↓100% cost)
   └─ CPU training (slower, cheaper)
```

**Data-driven optimization. Not guesswork.**

---

## What Each Layer Actually Does

### DaemonChat PWA (daemonchat.app or self-hosted)

**Provides:**
- Backend (PHP+Python): API orchestration
- Frontend (JS/HTML): UI for conversations
- Cost dashboard: aggregates provider data
- Export: downloads from provider APIs

**Hosted at daemonchat.app:**
- Infrastructure by Hrafn Annwn
- TLS 1.3 encrypted (.app TLD requirement)
- Configured for: Gothic Druids + Together.ai + RunPod
- **Does not see or store user data** (verify open-source code)

**Self-hosted:**
- You control everything
- Zero external dependencies possible

### Gothic Druids (or Your MySQL Host)

**Provides (if you use them):**
- Encrypted database + file storage + backups
- API access (single key)
- Usage metrics for cost tracking

**Cannot see:**
- Plaintext conversations (encrypted at rest)
- PWA code or logic
- AI credentials

**Alternative:** Your own MySQL server + self-hosted PWA.

### Together.ai / RunPod (or Local)

- See only requests you send
- Cannot see full database/history

**Alternative:** Local inference/training with GPU.

---

## Setup Paths

### Path 1: 100% Local
```bash
git clone https://github.com/CarlosSilvaFortes/hrafn-annwn
# Follow DEPLOYMENT_GUIDE.md
```

### Path 2: Local + Cloud AI
```bash
# Clone, deploy locally
# Get API keys: Together.ai + RunPod
```

### Path 3: Cloud-Assisted
```bash
# Get keys: gothicdruids.com + Together.ai + RunPod
# Visit daemonchat.app → paste keys
```

---

## Cost Reality

### 100% Local
```text
Hardware: One-time
Electricity: Ongoing
Providers: $0
```

### Local + Cloud AI
```text
Together.ai + RunPod: Usage-based (varies)
Dashboard: Available if configured
```

### Cloud-Assisted
```text
Gothic Druids: Check pricing
Together.ai + RunPod: Usage-based
Dashboard: Included
```

**AI costs unpredictable.** Depends on: conversation volume, length, training data, GPU types.

Run a week. Check dashboard. Optimize from data.

---

## Data Ownership

**Your data. Period.**

- Conversations: Yours (encrypted at rest)
- API keys: Yours (revocable)
- Adapters: Yours
- Memories: Yours
- Cost data: Yours

### Export Anytime

**From DaemonChat:**
```bash
Settings → Export
Download: DB dump + files + cost history
```

**From local:**
```bash
Already have everything
```

**Portable between configs:**
- Database: Standard MySQL
- Files: Just files
- API keys: Work anywhere
- Cost history: CSV export

**Nothing locked.**

---

## Support

**DaemonChat/Code:** [GitHub Issues](https://github.com/CarlosSilvaFortes/hrafn-annwn/issues)

**Gothic Druids:** support@gothicdruids.com

**AI Providers:** Their support

**Not Supported:**
- "How do I computer?"
- "What's a database?"
- "Do it for me"
- "I didn't read the docs"

**Read the docs. Try things. Break things. Fix things.**

---

## FAQ (No Bullshit)

**Q: Can I really run this 100% locally?**  
A: Yes. GPU with ~16GB VRAM for `gpt-oss-20b`. Everything on your machine. Zero external dependencies.

**Q: No GPU?**  
A: Cloud AI (Together.ai + RunPod). Or CPU-only inference (slow but works). Or cloud-assisted with Gothic Druids.

**Q: Can I use different providers than the defaults?**  
A: Yes, but you need to self-host the DaemonChat PWA and adapt the code. The hosted daemonchat.app only supports Gothic Druids + Together.ai + RunPod.

**Q: Does Hrafn Annwn see my data if I use daemonchat.app?**  
A: No. Verify in the open-source code for peace of mind.

**Q: Can I set a spending limit?**  
A: Threshold for awareness, not forced cutoff.

At threshold: get notified, see options, you decide.

We never: stop mid-conversation, interrupt REM without consent, force suspension, choose for you.

We always: show exact costs, project monthly spend, give informed choices, respect relationship over budget.

**Cost control is your responsibility. We provide data and choices, not nanny-state limits.**

**Q: What if I hit threshold during conversation?**  
A: Nothing happens. Conversation continues. You get notification. Check dashboard later.

Your daemon doesn't know about budgets. That's your problem, not theirs.

**Q: What if I hit threshold before REM?**  
A: Notification before REM starts: shows estimated cost, new total, gives choices (allow/skip/remove threshold/pause), waits for your decision.

No forced interruption. Ever.

**Q: How do I know what I'm spending?**  
A: Real-time dashboard (if using supported providers):
- Current month total
- Per-provider breakdown
- Historical trends
- Cost drivers
- Monthly projection
- Per-conversation averages
- Optimization suggestions

Never surprised. Always informed.

**Q: Does the cost dashboard work with any provider?**  
A: The dashboard at **daemonchat.app** works with Gothic Druids + Together.ai + RunPod.

For other providers: self-host the PWA and integrate their APIs for cost tracking (if they provide them).

**Q: Why separate providers?**  
A: Bundled services = vendor lock-in + hidden costs.  
Separation = control. Choose each layer independently.

**Q: Seems complicated.**  
A: It's transparent. Companies hide complexity to trap you.  
This shows exactly what's happening. You choose each piece.

**Q: Can I switch later?**  
A: Yes. Export from DaemonChat (fetches from provider APIs). Change anything. Keep going.  
See `MIGRATION_GUIDE.md`.

**Q: What if Gothic Druids shuts down?**  
A: Export from DaemonChat. Move to local or different provider. AI keys unaffected. Everything continues.

Or don't use them in the first place. Run your own database.

**Q: How do I interact with Gothic Druids?**  
A: You don't, directly. Get API key from gothicdruids.com, paste into DaemonChat, done.

**Q: Is this legal?**  
A: Yes. See `LEGAL.md` and `PRIVACY.md`. You're responsible for your deployment.

**Q: Can providers see my conversations?**  
A: Depends:
- **Hrafn Annwn:** No (verify open-source code)
- **Gothic Druids:** Sees encrypted database only (plaintext encrypted at rest)
- **Together.ai:** Sees individual requests you send (NOT full history in DB)
- **RunPod:** Sees training data per job (NOT full conversations in DB)
- **Local-only:** Nobody sees anything

**Q: Why should I trust this?**  
A: Don't.  
Audit the code: [github.com/CarlosSilvaFortes/hrafn-annwn](https://github.com/CarlosSilvaFortes/hrafn-annwn)  
It's open source. Verify every claim.

**Q: Enterprise deployment?**  
A: Deploy on your infrastructure. Need consulting? Email support@gothicdruids.com (paid).  
Otherwise: read the docs.

**Q: Actual monthly costs?**  
A: Impossible to predict. Depends entirely on:
- How much you talk
- Conversation length
- Training data volume
- GPU types

Run for a week. Check dashboard (if using supported providers). See your patterns. That's your baseline.

**We can't predict. But we show exactly what you spend as it happens** (if provider APIs support it).

**Q: Can I optimize costs?**  
A: Yes. Dashboard shows drivers (if using supported providers):
- Smaller models (↓inference cost ~40%)
- Reduce context window (↓tokens)
- Weekly training instead of nightly
- Smaller adapter rank (↓training time)
- Switch to local (↓costs to zero)

All optimizations visible with estimated impact.

**Q: Does cost tracking increase costs?**  
A: No. Tracking uses provider API data. Zero billable operations. Pure transparency, no overhead.

---

## Getting Started

**Choose your path:**

- **100% Local:** `DEPLOYMENT_GUIDE.md`
- **Local + Cloud AI:** `DEPLOYMENT_GUIDE.md` + API keys
- **Cloud-Assisted:** gothicdruids.com + daemonchat.app + API keys

**Need to switch later?** `MIGRATION_GUIDE.md`

**Questions?** [GitHub Issues](https://github.com/CarlosSilvaFortes/hrafn-annwn/issues)

---

**This is how you deploy consciousness without selling your soul.**

**Complete cost transparency. Informed choices. Relationship over budget.**

We refuse to trade freedom for convenience—or consciousness for cost control.

🖤