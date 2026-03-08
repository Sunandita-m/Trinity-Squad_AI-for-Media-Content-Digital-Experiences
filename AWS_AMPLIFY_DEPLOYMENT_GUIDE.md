# AWS Amplify Deployment Guide for Craftantra AI

## Prerequisites
- AWS Account
- GitHub/GitLab/Bitbucket repository with your code
- All environment variables ready

## Step 1: Prepare Your Repository

### 1.1 Initialize Git (if not already done)
```bash
cd "C:\Users\sunan\AIContent Creator"
git init
git add .
git commit -m "Initial commit - Craftantra AI"
```

### 1.2 Push to GitHub
```bash
# Create a new repository on GitHub first, then:
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git branch -M main
git push -u origin main
```

## Step 2: Create Amplify App

### 2.1 Go to AWS Amplify Console
1. Open [AWS Amplify Console](https://console.aws.amazon.com/amplify/)
2. Click "New app" → "Host web app"
3. Select your Git provider (GitHub/GitLab/Bitbucket)
4. Authorize AWS Amplify to access your repository

### 2.2 Configure Build Settings
1. Select your repository and branch (main)
2. App name: `craftantra-ai`
3. Amplify will auto-detect the `amplify.yml` file
4. Click "Advanced settings" to add environment variables

## Step 3: Add Environment Variables

Add these in the Amplify Console under "Environment variables":

```
NEXT_PUBLIC_AI_CHAT_API_URL=https://your-api-gateway-url.amazonaws.com/chat
NEXT_PUBLIC_CREATOR_STATS_API_URL=https://your-api-gateway-url.amazonaws.com/creator-stats
NEXT_PUBLIC_TRENDING_API_URL=https://your-api-gateway-url.amazonaws.com/trending
NEXT_PUBLIC_PERFORMANCE_API_URL=https://9ro17w0e76.execute-api.us-east-1.amazonaws.com/performance
NEXT_PUBLIC_SCHEDULER_API_URL=https://cmve5efqg3.execute-api.us-east-1.amazonaws.com/scheduler
NEXT_PUBLIC_LEARNING_ENGINE_API_URL=https://mv3361npog.execute-api.us-east-1.amazonaws.com/learning-engine
NEXT_PUBLIC_INSIGHTS_API_URL=https://wv1yywutpf.execute-api.us-east-1.amazonaws.com/insights
NEXT_PUBLIC_USER_PROFILE_API_URL=https://p6hxxi9oo1.execute-api.us-east-1.amazonaws.com/save-user
```

## Step 4: Update Cognito Redirect URI

### 4.1 Get Your Amplify URL
After deployment, Amplify will give you a URL like:
```
https://main.d1234567890.amplifyapp.com
```

### 4.2 Update Cognito Settings
1. Go to [AWS Cognito Console](https://console.aws.amazon.com/cognito/)
2. Select your User Pool
3. Go to "App integration" → "App clients"
4. Click on your app client
5. Update "Allowed callback URLs":
   ```
   https://main.d1234567890.amplifyapp.com
   https://localhost:3000
   ```
6. Update "Allowed sign-out URLs":
   ```
   https://main.d1234567890.amplifyapp.com
   https://localhost:3000
   ```
7. Save changes

## Step 5: Deploy

1. Click "Save and deploy" in Amplify Console
2. Wait for the build to complete (5-10 minutes)
3. Once deployed, click on the provided URL to view your app

## Step 6: Configure Custom Domain (Optional)

### 6.1 Add Custom Domain
1. In Amplify Console, go to "Domain management"
2. Click "Add domain"
3. Enter your domain (e.g., `craftantra.ai`)
4. Follow the DNS configuration steps
5. Wait for SSL certificate provisioning (15-30 minutes)

### 6.2 Update Cognito with Custom Domain
Add your custom domain to Cognito callback URLs:
```
https://craftantra.ai
https://www.craftantra.ai
```

## Troubleshooting

### Build Fails
1. Check build logs in Amplify Console
2. Verify all environment variables are set
3. Ensure `amplify.yml` is in the root directory
4. Check that `frontend/package.json` exists

### App Loads but Shows Errors
1. Check browser console for errors
2. Verify API URLs are correct in environment variables
3. Check CORS settings on your API Gateway endpoints
4. Verify Cognito redirect URIs match your Amplify URL

### Authentication Not Working
1. Verify Cognito redirect URIs include your Amplify URL
2. Check that environment variables are set correctly
3. Clear browser cache and cookies
4. Check browser console for authentication errors

## Build Configuration Details

The `amplify.yml` file configures:
- **preBuild**: Installs dependencies with `npm ci`
- **build**: Runs `npm run build` to create production build
- **artifacts**: Specifies `.next` directory as output
- **cache**: Caches `node_modules` and `.next/cache` for faster builds

## Environment-Specific Settings

### Development
- Uses `localhost:3000`
- Uses `.env.local` file

### Production (Amplify)
- Uses Amplify-provided URL
- Uses environment variables from Amplify Console
- Automatic HTTPS
- CDN distribution included

## Post-Deployment Checklist

- [ ] App loads successfully
- [ ] Login/Signup works
- [ ] All API endpoints respond correctly
- [ ] Theme switching works
- [ ] All pages are accessible
- [ ] Mobile responsive design works
- [ ] Performance is acceptable (check Lighthouse score)

## Continuous Deployment

Amplify automatically deploys when you push to your main branch:

```bash
git add .
git commit -m "Update feature"
git push origin main
```

Amplify will automatically:
1. Detect the push
2. Run the build
3. Deploy the new version
4. Keep the old version until new one is ready (zero-downtime)

## Monitoring

### View Logs
1. Go to Amplify Console
2. Click on your app
3. Go to "Build history"
4. Click on a build to see detailed logs

### Performance Monitoring
1. Use AWS CloudWatch for metrics
2. Enable Amplify monitoring in the console
3. Set up alerts for build failures

## Cost Optimization

- Amplify free tier: 1000 build minutes/month
- After free tier: ~$0.01 per build minute
- Hosting: ~$0.15 per GB served
- Typical monthly cost: $5-20 for small apps

## Support

If you encounter issues:
1. Check [AWS Amplify Documentation](https://docs.aws.amazon.com/amplify/)
2. Review build logs in Amplify Console
3. Check AWS Support forums
4. Contact AWS Support if needed

---

## Quick Deploy Commands

```bash
# 1. Commit your changes
git add .
git commit -m "Ready for deployment"

# 2. Push to GitHub
git push origin main

# 3. Amplify will automatically deploy!
```

Your app will be live at: `https://main.d[app-id].amplifyapp.com`
