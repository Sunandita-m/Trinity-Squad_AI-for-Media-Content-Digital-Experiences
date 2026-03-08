# AWS Amplify Git Deployment (Recommended Method)

## ⚠️ Why NOT Drag & Drop?

Your app has:
- Dynamic routes (`/dashboard/overview`, `/ai-mentor/chat`, etc.)
- Server-side rendering (SSR)
- Authentication with Cognito
- API integrations

**Drag & drop only works for static sites.** Your app needs Git-based deployment.

---

## ✅ Correct Method: Git-Based Deployment

### Step 1: Create GitHub Repository

1. Go to [GitHub](https://github.com)
2. Click "New repository"
3. Name: `craftantra-ai`
4. Keep it Private or Public
5. Don't initialize with README
6. Click "Create repository"

### Step 2: Push Your Code to GitHub

Open PowerShell in your project folder:

```powershell
cd "C:\Users\sunan\AIContent Creator"

# Initialize git (if not already done)
git init

# Add all files
git add .

# Commit
git commit -m "Initial commit - Craftantra AI"

# Add GitHub remote (replace with YOUR repository URL)
git remote add origin https://github.com/YOUR_USERNAME/craftantra-ai.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### Step 3: Deploy on AWS Amplify

1. Go to [AWS Amplify Console](https://console.aws.amazon.com/amplify/)
2. Click **"New app"** → **"Host web app"**
3. Select **"GitHub"**
4. Click **"Continue"**
5. Authorize AWS Amplify to access your GitHub
6. Select your repository: `craftantra-ai`
7. Select branch: `main`
8. Click **"Next"**

### Step 4: Configure Build Settings

Amplify will auto-detect your `amplify.yml` file.

**Verify it shows:**
```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - cd frontend
        - npm ci
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: frontend/.next
    files:
      - '**/*'
  cache:
    paths:
      - frontend/node_modules/**/*
      - frontend/.next/cache/**/*
```

Click **"Next"**

### Step 5: Add Environment Variables

Click **"Advanced settings"**

Add these environment variables:

```
NEXT_PUBLIC_PERFORMANCE_API_URL=https://9ro17w0e76.execute-api.us-east-1.amazonaws.com/performance
NEXT_PUBLIC_SCHEDULER_API_URL=https://cmve5efqg3.execute-api.us-east-1.amazonaws.com/scheduler
NEXT_PUBLIC_LEARNING_ENGINE_API_URL=https://mv3361npog.execute-api.us-east-1.amazonaws.com/learning-engine
NEXT_PUBLIC_INSIGHTS_API_URL=https://wv1yywutpf.execute-api.us-east-1.amazonaws.com/insights
NEXT_PUBLIC_USER_PROFILE_API_URL=https://p6hxxi9oo1.execute-api.us-east-1.amazonaws.com/save-user
```

Click **"Save and deploy"**

### Step 6: Wait for Build

- Build takes 5-10 minutes
- You'll see: Provision → Build → Deploy → Verify
- Once complete, you'll get a URL like: `https://main.d1234567890.amplifyapp.com`

### Step 7: Update Cognito Redirect URI

1. Copy your Amplify URL
2. Go to [AWS Cognito Console](https://console.aws.amazon.com/cognito/)
3. Select your User Pool
4. Go to "App integration" → "App clients"
5. Click your app client
6. Update **"Allowed callback URLs"**:
   ```
   https://main.d1234567890.amplifyapp.com
   https://localhost:3000
   ```
7. Update **"Allowed sign-out URLs"**:
   ```
   https://main.d1234567890.amplifyapp.com
   https://localhost:3000
   ```
8. Click **"Save changes"**

### Step 8: Test Your App

1. Visit your Amplify URL
2. Click "Login" or "Sign Up"
3. Test authentication
4. Test all features

---

## 🔄 Continuous Deployment

Every time you push to GitHub, Amplify automatically rebuilds:

```powershell
git add .
git commit -m "Update feature"
git push origin main
```

Amplify will:
- Detect the push
- Build automatically
- Deploy new version
- Zero downtime

---

## 🐛 Troubleshooting

### Build Fails

**Check build logs:**
1. Go to Amplify Console
2. Click on your app
3. Go to "Build history"
4. Click on the failed build
5. Read the error logs

**Common issues:**
- Missing environment variables
- Wrong `amplify.yml` path
- Node version mismatch
- Missing dependencies

**Fix:**
- Verify `amplify.yml` is in root directory
- Check all environment variables are set
- Ensure `frontend/package.json` exists

### App Loads but Authentication Fails

**Check:**
- Cognito redirect URIs include your Amplify URL
- Environment variables are correct
- Browser console for errors

### API Calls Fail

**Check:**
- CORS settings on API Gateway
- API URLs in environment variables
- Network tab in browser DevTools

---

## 📊 Monitoring

### View Build Logs
Amplify Console → Your App → Build history → Click build

### View Runtime Logs
Amplify Console → Your App → Monitoring

### Performance
- Amplify includes CloudFront CDN
- Automatic HTTPS
- Global distribution

---

## 💰 Cost

- **Free tier**: 1000 build minutes/month, 15 GB served/month
- **After free tier**: ~$0.01/build minute, ~$0.15/GB served
- **Typical cost**: $5-20/month for small apps

---

## ✅ Advantages of Git-Based Deployment

✅ Automatic deployments on push
✅ Full Next.js features (SSR, API routes, dynamic routes)
✅ Environment variables management
✅ Build logs and monitoring
✅ Rollback to previous versions
✅ Preview deployments for branches
✅ Custom domains with SSL
✅ CDN included

---

## 🚫 Why Drag & Drop Won't Work

❌ No server-side rendering
❌ No dynamic routes
❌ No API routes
❌ No authentication flows
❌ Static export only
❌ Limited features

Your app needs Git-based deployment!

---

## 📝 Quick Commands

```powershell
# First time setup
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/YOUR_USERNAME/craftantra-ai.git
git push -u origin main

# Future updates
git add .
git commit -m "Your update message"
git push origin main
```

---

## 🎯 Summary

1. ✅ Push code to GitHub
2. ✅ Connect GitHub to Amplify
3. ✅ Add environment variables
4. ✅ Deploy automatically
5. ✅ Update Cognito redirect URIs
6. ✅ Test your app

**Your app will be live with full functionality!**
