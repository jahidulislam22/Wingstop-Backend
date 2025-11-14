# Vercel Deployment Guide

This guide will help you deploy the Rivo Middleware backend to Vercel.

## Prerequisites

1. A [Vercel account](https://vercel.com/signup) (free tier works fine)
2. Vercel CLI installed (optional, but recommended): `npm i -g vercel`
3. Your environment variables ready (see below)

## Required Environment Variables

Before deploying, you need to set up the following environment variables in your Vercel project:

```bash
RIVO_API_KEY=your_rivo_merchant_api_key
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_SECURE=false
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password
EMAIL_FROM_NAME=Rivo Loyalty
```

## Deployment Methods

### Method 1: Deploy via Vercel Dashboard (Easiest)

1. **Push your code to GitHub** (if not already done):
   ```bash
   git add .
   git commit -m "Prepare for Vercel deployment"
   git push origin main
   ```

2. **Import project to Vercel**:
   - Go to [Vercel Dashboard](https://vercel.com/dashboard)
   - Click "Add New..." → "Project"
   - Import your GitHub repository
   - Vercel will auto-detect the configuration from `vercel.json`

3. **Set Environment Variables**:
   - In your Vercel project settings, go to "Settings" → "Environment Variables"
   - Add all the required variables listed above
   - Make sure to add them for "Production", "Preview", and "Development" environments

4. **Deploy**:
   - Click "Deploy"
   - Wait for the build to complete
   - Your API will be live at: `https://your-project.vercel.app`

### Method 2: Deploy via Vercel CLI

1. **Login to Vercel**:
   ```bash
   vercel login
   ```

2. **Deploy to production**:
   ```bash
   vercel --prod
   ```

3. **Add environment variables** (one-time setup):
   ```bash
   vercel env add RIVO_API_KEY
   vercel env add EMAIL_HOST
   vercel env add EMAIL_PORT
   vercel env add EMAIL_SECURE
   vercel env add EMAIL_USER
   vercel env add EMAIL_PASS
   vercel env add EMAIL_FROM_NAME
   ```
   
   Follow the prompts to enter each value.

4. **Redeploy after adding environment variables**:
   ```bash
   vercel --prod
   ```

## Testing Your Deployment

Once deployed, test your API endpoints:

1. **Health check**:
   ```bash
   curl https://your-project.vercel.app/
   ```

2. **Test customer endpoint**:
   ```bash
   curl https://your-project.vercel.app/customers
   ```

3. **Test rewards endpoint**:
   ```bash
   curl https://your-project.vercel.app/rewards
   ```

## API Endpoints

Your deployed API will have the following endpoints:

- `GET /` - API information
- `GET /customers` - Fetch all customers from Rivo
- `GET /points/:email` - Get customer points by email
- `GET /rewards` - Fetch all rewards from Rivo
- `POST /redeem-points` - Redeem customer points
- `POST /notify-point-redemption` - Webhook endpoint for Rivo notifications

## Configuring Rivo Webhook

After deployment, configure your Rivo webhook URL:

1. Log in to your Rivo dashboard
2. Go to Settings → Webhooks
3. Add webhook URL: `https://your-project.vercel.app/notify-point-redemption`
4. Select the "Point Redemption" event
5. Save the webhook configuration

## Updating Your Deployment

To update your deployment:

1. **Push changes to GitHub**:
   ```bash
   git add .
   git commit -m "Your update message"
   git push origin main
   ```

2. Vercel will automatically deploy the new version

Or using CLI:
```bash
vercel --prod
```

## Troubleshooting

### Issue: API returns 500 errors

**Solution**: Check your environment variables are set correctly in Vercel dashboard.

### Issue: Email notifications not working

**Solutions**:
- Verify your email credentials are correct
- For Gmail, make sure you're using an [App Password](https://support.google.com/accounts/answer/185833)
- Check that `EMAIL_SECURE` is set to `false` for port 587

### Viewing Logs

View your deployment logs:
- **Dashboard**: Go to your project → Deployments → Click on deployment → Functions tab
- **CLI**: `vercel logs <deployment-url>`

## Local Development

To run the API locally:

1. Create a `.env` file in the root directory with your environment variables
2. Run: `npm run dev` or `npm start`
3. API will be available at: `http://localhost:5000`

The same `index.js` file is used for both local and production - no separate files needed!

## Security Notes

- Never commit your `.env` file to Git
- Use strong API keys and tokens
- Regularly rotate your credentials
- Monitor your Vercel deployment logs for suspicious activity

## Support

For issues with:
- **Vercel deployment**: Check [Vercel Documentation](https://vercel.com/docs)
- **Rivo API**: Contact Rivo support
- **Shopify API**: Check [Shopify API Documentation](https://shopify.dev/docs)

## Project Structure

```
├── index.js              # Main API file (works for both local and Vercel)
├── vercel.json           # Vercel configuration
├── .vercelignore         # Files to exclude from deployment
├── package.json          # Package configuration
├── .env                  # Local environment variables (not committed)
└── DEPLOYMENT.md         # This file
```

## How It Works

The `index.js` file is designed to work in **both environments**:

- **Local Development**: When you run `npm run dev`, the server starts on `localhost:5000`
- **Vercel Deployment**: Vercel imports the file and uses the exported Express app as a serverless function

The logic is simple:
```javascript
// Only start server if NOT running on Vercel
if (process.env.VERCEL !== '1') {
  app.listen(PORT, () => {
    console.log('Server started...');
  });
}

// Always export for Vercel
export default app;
```

