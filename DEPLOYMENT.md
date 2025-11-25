# OpenAlgo Deployment Guide for whitecollartrades.com

## Quick Deployment Options

### Option 1: Railway.app (⭐ Recommended - Fastest)

**Pros:**
- Auto-deploys from GitHub
- Free tier available
- Great for Python Flask apps
- Simple setup (5 minutes)

**Steps:**
1. Go to https://railway.app
2. Sign up with GitHub
3. Click "New Project" → "Deploy from GitHub repo"
4. Select `Whitecollartrades/OA`
5. Configure environment variables in Railway dashboard:
   ```
   FLASK_ENV=production
   FLASK_DEBUG=False
   FLASK_HOST_IP=0.0.0.0
   DATABASE_URL=postgresql://...  (Railway provides free PostgreSQL)
   ```
6. Connect custom domain at: https://railway.app/project/{id}/settings/domains
7. Every `git push` to main auto-deploys! 🚀

---

### Option 2: Render.com (⭐ Good Alternative)

**Pros:**
- Free tier with limitations
- Easy GitHub integration
- Good performance

**Steps:**
1. Go to https://render.com
2. Sign up with GitHub
3. Click "New +" → "Web Service"
4. Connect your GitHub repo
5. Set build command: `pip install -r requirements.txt`
6. Set start command: `gunicorn -w 1 -b 0.0.0.0:$PORT --worker-class eventlet app:app`
7. Add environment variables
8. Connect custom domain

---

### Option 3: DigitalOcean App Platform

**Pros:**
- $12/month for App Platform
- Full control
- Easy scaling

**Steps:**
1. Create DigitalOcean account
2. Create new App
3. Connect GitHub repo
4. Set runtime to Python 3.12
5. Configure environment
6. Connect domain

---

### Option 4: Self-Hosted with GitHub Actions

**For your own server (VPS/Dedicated Server):**

Create `.github/workflows/deploy-ssh.yml`:

```yaml
name: Deploy via SSH

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SSH_HOST }}
          username: ${{ secrets.SSH_USER }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /home/user/openalgo
            git pull origin main
            pip install -r requirements.txt
            systemctl restart openalgo
```

---

## Domain Configuration (whitecollartrades.com)

### Point your domain to the hosting platform:

1. **If using Railway/Render/Heroku:**
   - Get the platform's domain name
   - Add CNAME record in your domain registrar:
     ```
     whitecollartrades.com  →  yourapp.railway.app
     ```
   - Or add platform-provided nameservers

2. **If self-hosted:**
   - Point A record to your server IP:
     ```
     whitecollartrades.com  →  123.45.67.89
     ```
   - Set up SSL with Let's Encrypt (free)

---

## Environment Variables Needed

Create these in your hosting platform's dashboard:

```
FLASK_ENV=production
FLASK_DEBUG=False
FLASK_HOST_IP=0.0.0.0
FLASK_PORT=5000
DATABASE_URL=your_database_url
BROKER_API_KEY=your_key
BROKER_API_SECRET=your_secret
APP_KEY=your_app_key
API_KEY_PEPPER=your_pepper
```

---

## Database Setup

For production, use PostgreSQL instead of SQLite:

```
# Railway/Render/DigitalOcean provide free PostgreSQL
# Update your .env:
DATABASE_URL=postgresql://user:pass@host:5432/openalgo
LATENCY_DATABASE_URL=postgresql://user:pass@host:5432/latency
LOGS_DATABASE_URL=postgresql://user:pass@host:5432/logs
```

---

## Monitoring & Logs

- Railway: Dashboard → Logs tab
- Render: Logs visible in dashboard
- DigitalOcean: App Platform → Logs

---

## SSL/HTTPS

All platforms provide free SSL certificates automatically ✅

---

## Questions?

Refer to platform-specific docs:
- Railway: https://docs.railway.app
- Render: https://render.com/docs
- DigitalOcean: https://docs.digitalocean.com/products/app-platform/
