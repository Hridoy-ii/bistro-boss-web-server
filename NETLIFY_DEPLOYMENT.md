# Bistro Boss Server - Netlify Deployment Guide

## Preparation Steps

1. **Set Environment Variables in Netlify**
   - Go to your Netlify project settings
   - Navigate to "Build & deploy" → "Environment"
   - Add these variables:
     - `NODE_ENV` = `production`
     - `PORT` = `8888` (or your preferred port)
     - `DB_USER` = Your MongoDB username
     - `DB_PASS` = Your MongoDB password
     - `ACCESS_TOKEN_SECRET` = Your JWT secret
     - `MAILGUN_API_KEY` = Your Mailgun API key
     - `MAILGUN_DOMAIN` = Your Mailgun domain
     - `STRIPE_SECRET_KEY` = Your Stripe secret key

2. **Deploy to Netlify**

   ### Option A: Using Netlify CLI
   ```bash
   npm install -g netlify-cli
   netlify login
   netlify deploy --prod
   ```

   ### Option B: Using Git (Recommended)
   - Push this repo to GitHub
   - Login to [netlify.com](https://netlify.com)
   - Click "New site from Git"
   - Select your GitHub repository
   - Configure build settings:
     - Build command: `npm install`
     - Functions directory: `.`
     - Publish directory: `public`
   - Add all environment variables
   - Deploy

3. **API Endpoints**
   
   All endpoints will be available at:
   ```
   https://your-site.netlify.app/.netlify/functions/index
   ```

   Examples:
   - `POST https://your-site.netlify.app/.netlify/functions/index/jwt` - Generate JWT
   - `GET https://your-site.netlify.app/.netlify/functions/index/menu` - Get menu items
   - `POST https://your-site.netlify.app/.netlify/functions/index/users` - Create user

4. **Verify Deployment**
   
   After deployment, test with:
   ```bash
   curl https://your-site.netlify.app/.netlify/functions/index/
   # Should return: "boss is sitting"
   ```

## Local Development

To test locally before deployment:
```bash
npm run dev
# Server runs on http://localhost:5000
```

## Troubleshooting

- **Function not found**: Ensure `netlify.toml` is in the root directory
- **Environment variables not working**: Check Netlify UI - variables should be deployed with the site
- **CORS issues**: Already configured to allow all origins
- **MongoDB connection timeout**: Ensure MongoDB IP whitelist includes Netlify's IP ranges (recomm: 0.0.0.0/0 for testing)

## Notes

- The server uses `serverless-http` to wrap Express for serverless deployment
- MongoDB connection is established on function startup
- CORS is enabled for all origins (consider restricting in production)
- Mailgun email confirmations are sent on payment
