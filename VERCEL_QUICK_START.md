# 🚀 Quick Start: Deploy to Vercel in 5 Minutes

## Step 1: Push to GitHub (if not already done)

```bash
git add .
git commit -m "Add Vercel deployment configuration"
git push origin main
```

## Step 2: Import to Vercel

1. Go to https://vercel.com/dashboard
2. Click **"Add New..."** → **"Project"**
3. Select your repository
4. Click **"Import"**

## Step 3: Add Environment Variables

In Vercel project settings, add these environment variables:

```
SHOPIFY_STORE=your-store.myshopify.com
SHOPIFY_ACCESS_TOKEN=shpat_xxxxxxxxxxxxx
RIVO_API_KEY=your_rivo_merchant_api_key
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_SECURE=false
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password
EMAIL_FROM_NAME=Rivo Loyalty
```

💡 **Tip**: Add them for Production, Preview, and Development environments.

## Step 4: Deploy!

Click **"Deploy"** and wait ~1 minute.

## Step 5: Test Your API

Your API will be live at: `https://your-project-name.vercel.app`

Test it:
```bash
curl https://your-project-name.vercel.app/
```

## Step 6: Configure Rivo Webhook

1. Go to Rivo Dashboard → Settings → Webhooks
2. Add URL: `https://your-project-name.vercel.app/notify-point-redemption`
3. Select "Point Redemption" event
4. Save

## ✅ You're Done!

Your API is now live and ready to handle requests.

For detailed documentation, see [DEPLOYMENT.md](./DEPLOYMENT.md)

## Quick Test Commands

```bash
# Check API status
curl https://your-project-name.vercel.app/

# Get customers
curl https://your-project-name.vercel.app/customers

# Get rewards
curl https://your-project-name.vercel.app/rewards

# Get customer points
curl https://your-project-name.vercel.app/points/customer@example.com
```

## 💡 How It Works

The same `index.js` file works for **both** local development and Vercel deployment:

- **Local**: Runs `app.listen()` when you use `npm run dev`
- **Vercel**: Imports and uses the exported Express app as a serverless function

No need for separate files! 🎉

## Need Help?

- 📚 Full deployment guide: [DEPLOYMENT.md](./DEPLOYMENT.md)
- 🔧 Vercel docs: https://vercel.com/docs
- 📧 Issues? Check the Vercel deployment logs

