# 🚀 OpenAlgo Hosting on whitecollartrades.com - Quick Start

## Summary
Your deployment files are now in GitHub. Choose your hosting platform below:

---

## ⭐ FASTEST OPTION: Railway.app (Recommended)

### Setup (5 minutes):
1. Go to **https://railway.app**
2. Click "New Project"
3. Select "Deploy from GitHub"
4. Choose `Whitecollartrades/OA`
5. Railway auto-detects the app as Python
6. Go to **Variables** tab and add:
   - `FLASK_ENV` = `production`
   - `FLASK_DEBUG` = `False`
   - Other API keys and secrets from your `.env`

7. Go to **Settings** → **Domain** 
8. Click "Generate Domain" or add custom domain `whitecollartrades.com`

9. **Done!** Every `git push` auto-deploys 🎉

**Cost:** Free tier available, ~$12/month for production

---

## Alternative: Render.com

Similar setup process:
1. https://render.com/dashboard
2. "New Web Service"
3. Connect GitHub repo
4. Set start command: `gunicorn -w 1 -b 0.0.0.0:$PORT --worker-class eventlet app:app`
5. Add variables
6. Add custom domain

---

## What You Need to Configure:

Before deploying, make sure you have:

### Required Environment Variables:
```
✅ FLASK_ENV = production
✅ FLASK_DEBUG = False
✅ FLASK_HOST_IP = 0.0.0.0
✅ BROKER_API_KEY = your_key
✅ BROKER_API_SECRET = your_secret
✅ APP_KEY = your_app_key (generate new)
✅ API_KEY_PEPPER = your_pepper (generate new)
```

### Database:
- Local SQLite works fine for testing
- For production: Use PostgreSQL (platforms provide free tier)

### Domain:
- Update DNS records to point to your platform's domain
- Most platforms provide free SSL/HTTPS

---

## Useful Commands

Monitor deployment:
```bash
# Check latest deployment status
git log --oneline -5

# View deployment logs (platform-specific)
# Railway: railway logs
# Render: Check dashboard
```

---

## After Deployment

1. ✅ Visit `https://whitecollartrades.com`
2. ✅ Create admin account
3. ✅ Configure broker API keys
4. ✅ Set up trading strategies

---

## Need Help?

- Railway Docs: https://docs.railway.app
- Render Docs: https://render.com/docs
- OpenAlgo Docs: Check README.md

See `DEPLOYMENT.md` for detailed instructions on all options.
