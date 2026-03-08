# Pre-Deployment Checklist

## ✅ Code Ready
- [ ] All features working locally
- [ ] No console errors
- [ ] Build succeeds: `npm run build`
- [ ] All environment variables in `.env.local`

## ✅ Git Repository
- [ ] Code pushed to GitHub/GitLab/Bitbucket
- [ ] `.gitignore` excludes `.env.local` and `node_modules`
- [ ] `amplify.yml` in root directory
- [ ] Latest changes committed

## ✅ AWS Setup
- [ ] AWS account created
- [ ] Cognito User Pool configured
- [ ] All API Gateway endpoints deployed
- [ ] API URLs documented

## ✅ Environment Variables Ready
Copy these to Amplify Console:
```
NEXT_PUBLIC_PERFORMANCE_API_URL=https://9ro17w0e76.execute-api.us-east-1.amazonaws.com/performance
NEXT_PUBLIC_SCHEDULER_API_URL=https://cmve5efqg3.execute-api.us-east-1.amazonaws.com/scheduler
NEXT_PUBLIC_LEARNING_ENGINE_API_URL=https://mv3361npog.execute-api.us-east-1.amazonaws.com/learning-engine
NEXT_PUBLIC_INSIGHTS_API_URL=https://wv1yywutpf.execute-api.us-east-1.amazonaws.com/insights
NEXT_PUBLIC_USER_PROFILE_API_URL=https://p6hxxi9oo1.execute-api.us-east-1.amazonaws.com/save-user
```

## ✅ Cognito Configuration
- [ ] Client ID: `6778f85j4o120rhfkifu12nqsf`
- [ ] Domain: `https://us-east-1tz3f1ef7v.auth.us-east-1.amazoncognito.com`
- [ ] Callback URLs will be updated after Amplify deployment

## 🚀 Deployment Steps

1. **Push to GitHub**
   ```bash
   git add .
   git commit -m "Ready for Amplify deployment"
   git push origin main
   ```

2. **Create Amplify App**
   - Go to AWS Amplify Console
   - Click "New app" → "Host web app"
   - Connect your repository

3. **Configure Build**
   - Amplify auto-detects `amplify.yml`
   - Add environment variables
   - Click "Save and deploy"

4. **Update Cognito**
   - Copy your Amplify URL
   - Add to Cognito callback URLs
   - Add to Cognito sign-out URLs

5. **Test Deployment**
   - Visit Amplify URL
   - Test login/signup
   - Test all features

## 📝 Post-Deployment

- [ ] App loads successfully
- [ ] Authentication works
- [ ] All APIs respond
- [ ] Theme switching works
- [ ] Mobile responsive
- [ ] Custom domain configured (optional)

## 🔧 If Build Fails

1. Check build logs in Amplify Console
2. Verify `amplify.yml` is correct
3. Ensure all dependencies in `package.json`
4. Check Node version compatibility
5. Verify environment variables are set

## 📞 Support Resources

- AWS Amplify Docs: https://docs.aws.amazon.com/amplify/
- Next.js Deployment: https://nextjs.org/docs/deployment
- Your deployment guide: `AWS_AMPLIFY_DEPLOYMENT_GUIDE.md`
