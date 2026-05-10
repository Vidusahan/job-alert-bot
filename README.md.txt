# Job Alert Bot 🤖

Automated job alert bot that monitors LinkedIn every 12 hours and sends
new job listings directly to Telegram. Built with n8n, Docker, and
deployed on a DigitalOcean cloud VM.

![Workflow](screenshots/workflow.png)

## How it works

| Node | What it does |
|------|-------------|
| Schedule Trigger | Fires every 12 hours automatically |
| HTTP Request | Fetches LinkedIn job listings |
| Setup Message | Filters by keywords and formats the alert |
| Check Duplicates | Prevents re-sending already seen jobs |
| Telegram | Sends the formatted alert to your phone |

## Sample alert

![Telegram Alert](screenshots/alerts.png)

## Tech stack

- **n8n** — workflow automation engine
- **Docker** — containerised deployment
- **Telegram Bot API** — instant alert delivery
- **DigitalOcean** — 24/7 cloud VM 

## Setup instructions

1. Clone this repo
```bash
   git clone https://github.com/Vidusahan/job-alert-bot.git
   cd job-alert-bot
```

2. Run n8n with Docker
```bash
   docker run -d \
     --name n8n \
     --restart unless-stopped \
     -p 5678:5678 \
     -v /root/n8n-data:/home/node/.n8n \
     -e N8N_BASIC_AUTH_ACTIVE=true \
     -e N8N_BASIC_AUTH_USER=admin \
     -e N8N_BASIC_AUTH_PASSWORD=yourpassword \
     -e GENERIC_TIMEZONE=Asia/Colombo \
     -e N8N_SECURE_COOKIE=false \
     n8nio/n8n
```

3. Open `http://localhost:5678` and create your owner account

4. Import the workflow
   - Go to your workflow canvas
   - Click the menu → Import from file
   - Select `workflow/job-alert-bot.json`

5. Add your Telegram credential
   - Open the Telegram node
   - Add your bot token from @BotFather

6. Click **Publish** — the bot runs every 12 hours automatically

## Deployment

Running 24/7 on a DigitalOcean Droplet in the Singapore region.
Auto-restarts on VM reboot via Docker restart policy.

## Keywords monitored

`MLOps` · `DevOps` · `AI Engineer` · `Cloud Engineer` · `Platform Engineer`

## Next improvements

- Add more job sources (topjobs.lk, xpress.jobs, jobpal.lk)
- Redis persistent deduplication across restarts  
- Daily digest summary instead of per-job alerts
- Salary range filtering