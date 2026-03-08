# AWS Amplify Drag & Drop Deployment Guide

## Step 1: Build Your Project

Open PowerShell and run:

```powershell
cd "C:\Users\sunan\AIContent Creator\frontend"
npm run build
```

This will create an `out` folder with your static files.

## Step 2: Prepare the Zip File

### Option A: Using File Explorer (Easiest)

1. Navigate to: `C:\Users\sunan\AIContent Creator\frontend\out`
2. **Go INSIDE the `out` folder**
3. Select ALL files and folders inside (Ctrl+A)
4. Right-click → "Send to" → "Compressed (zipped) folder"
5. Name it: `build.zip`

### Option B: Using PowerShell

```powershell
cd "C:\Users\sunan\AIContent Creator\frontend\out"
Compress-Archive -Path * -DestinationPath ..\build.zip
```

The zip file will be at: `C:\Users\sunan\AIContent Creator\frontend\build.zip`

## Step 3: Upload to AWS Amplify

1. Go to [AWS Amplify Console](https://console.aws.amazon.com/amplify/)
2. Click **"New app"** → **"Deploy without Git provider"**
3. App name: `craftantra-ai`
4. Environment name: `production`
5. Drag and drop your `build.zip` file
6. Click **"Save and deploy"**

## Step 4: Get Your URL

After deployment completes, you'll get a URL like:
```
https://production.d1234567890.amplifyapp.com
```

## Step 5: Update Cognito Redirect URI

1. Copy your Amplify URL
2. Go to [AWS Cognito Console](https://console.aws.amazon.com/cognito/)
3. Select your User Pool
4. Go to "App integration" → "App clients"
5. Click your app client
6. Update **"Allowed callback URLs"**:
   ```
   https://production.d1234567890.amplifyapp.com
   https://localhost:3000
   ```
7. Update **"Allowed sign-out URLs"**:
   ```
   https://production.d1234567890.amplifyapp.com
   https://localhost:3000
   ```
8. Click **"Save changes"**

## Step 6: Test Your App

Visit your Amplify URL and test all features.

---

## 🔄 Updating Your App

When you make changes:

1. **Build again:**
   ```powershell
   cd "C:\Users\sunan\AIContent Creator\frontend"
   npm run build
   ```

2. **Create new zip:**
   - Go to `frontend\out` folder
   - Select all files inside
   - Create new zip

3. **Upload to Amplify:**
   - Go to Amplify Console
   - Click your app
   - Click **"Deploy updates"**
   - Upload new zip

---

## ⚠️ Limitations of Static Export

Your app will work, but with these limitations:

- ❌ No server-side rendering (SSR)
- ❌ No API routes
- ❌ No dynamic routes with parameters
- ✅ Client-side routing works
- ✅ API calls to external endpoints work
- ✅ Authentication works (client-side)
- ✅ All React features work

---

## 📁 What's in the `out` folder?

After build, you'll see:
```
out/
├── index.html
├── _next/
│   ├── static/
│   └── ...
├── dashboard/
│   └── overview.html
├── ai-mentor/
│   └── chat.html
└── ...
```

Each page becomes an HTML file.

---

## 🐛 Troubleshooting

### Build Fails

**Error: "Page ... is missing 'generateStaticParams()'"**

This means some pages use dynamic routes. You need to either:
1. Remove those pages, or
2. Add `generateStaticParams()` to them

**Error: "Image Optimization using the default loader is not compatible"**

Already fixed with `images: { unoptimized: true }` in config.

### Zip Upload Fails

**Error: "Invalid zip structure"**

Make sure you:
- Zip the CONTENTS of `out` folder, not the `out` folder itself
- Your zip should have `index.html` at root, not `out/index.html`

### App Loads but Shows 404

- Check that all routes are static
- Verify `trailingSlash: true` is in config
- Clear browser cache

---

## ✅ Quick Checklist

- [ ] `next.config.mjs` has `output: 'export'`
- [ ] Run `npm run build` in `frontend` folder
- [ ] `out` folder is created
- [ ] Zip CONTENTS of `out` folder (not the folder itself)
- [ ] Upload to Amplify drag & drop
- [ ] Update Cognito redirect URIs
- [ ] Test the app

---

## 💡 Pro Tip

To verify your zip is correct:
1. Extract it to a test folder
2. You should see `index.html` immediately
3. NOT `out/index.html`

---

## 📝 Environment Variables

Since this is static export, environment variables are baked into the build.

Make sure your `.env.local` has all API URLs before building:

```env
NEXT_PUBLIC_PERFORMANCE_API_URL=https://9ro17w0e76.execute-api.us-east-1.amazonaws.com/performance
NEXT_PUBLIC_SCHEDULER_API_URL=https://cmve5efqg3.execute-api.us-east-1.amazonaws.com/scheduler
NEXT_PUBLIC_LEARNING_ENGINE_API_URL=https://mv3361npog.execute-api.us-east-1.amazonaws.com/learning-engine
NEXT_PUBLIC_INSIGHTS_API_URL=https://wv1yywutpf.execute-api.us-east-1.amazonaws.com/insights
NEXT_PUBLIC_USER_PROFILE_API_URL=https://p6hxxi9oo1.execute-api.us-east-1.amazonaws.com/save-user
```

If you change environment variables, you must rebuild and re-upload.

---

## 🎯 Summary

```powershell
# 1. Build
cd frontend
npm run build

# 2. Zip (go inside out folder first)
cd out
# Select all files → Right-click → Compress

# 3. Upload to Amplify drag & drop

# 4. Update Cognito redirect URIs

# Done!
```

Your app will be live at your Amplify URL!
