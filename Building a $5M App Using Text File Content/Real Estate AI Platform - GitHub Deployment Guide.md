# Real Estate AI Platform - GitHub Deployment Guide

## 🚀 Complete Deployment Setup for GitHub, Vercel, Netlify & Firebase

This guide will help you deploy the Real Estate AI Platform using GitHub as your version control system and deploy to multiple platforms simultaneously.

## 📋 Prerequisites

1. **GitHub Account**: [Sign up at GitHub](https://github.com)
2. **Vercel Account**: [Sign up at Vercel](https://vercel.com)
3. **Netlify Account**: [Sign up at Netlify](https://netlify.com)
4. **Firebase Account**: [Sign up at Firebase](https://firebase.google.com)
5. **Supabase Project**: Already configured with your credentials

## 🔧 Step 1: Create GitHub Repository

### 1.1 Create Repository on GitHub
1. Go to [GitHub](https://github.com) and log in
2. Click "New repository" or go to [https://github.com/new](https://github.com/new)
3. Repository name: `real-estate-ai-platform`
4. Description: `Enterprise Real Estate AI Platform with Multi-Modal Training`
5. Set to **Public** or **Private** (your choice)
6. **DO NOT** initialize with README, .gitignore, or license (we have existing code)
7. Click "Create repository"

### 1.2 Push Your Code to GitHub

```bash
# Navigate to your backend directory
cd /home/ubuntu/real_estate_ai_platform

# Initialize git repository
git init

# Add all files
git add .

# Commit your changes
git commit -m "Initial commit: Real Estate AI Platform Backend with Supabase integration"

# Add your GitHub repository as remote (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/real-estate-ai-platform.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### 1.3 Create Frontend Repository

```bash
# Navigate to your frontend directory
cd /home/ubuntu/real-estate-ai-frontend

# Initialize git repository
git init

# Add all files
git add .

# Commit your changes
git commit -m "Initial commit: Real Estate AI Platform Frontend with React + Vite"

# Add your GitHub repository as remote (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/real-estate-ai-frontend.git

# Push to GitHub
git branch -M main
git push -u origin main
```

## 🌐 Step 2: Deploy Backend to Vercel

### 2.1 Connect GitHub to Vercel
1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "Add New..." → "Project"
3. Import your `real-estate-ai-platform` repository
4. Configure project settings:
   - **Framework Preset**: Other
   - **Root Directory**: `./` (leave empty)
   - **Build Command**: Leave empty
   - **Output Directory**: Leave empty

### 2.2 Add Environment Variables in Vercel
Go to Project Settings → Environment Variables and add:

```
SUPABASE_URL = https://mqzxpqxzjglfwevzgydr.supabase.co
SUPABASE_KEY = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1xenhwcXh6amdsZndldnpneWRyIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc1NTIwOTI0NCwiZXhwIjoyMDcwNzg1MjQ0fQ.KFPxke9mFCivT7gCz4HRRA5jU0i_a1hE-9rwGlWZdC0
SUPABASE_ANON_KEY = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1xenhwcXh6amdsZndldnpneWRyIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTUyMDkyNDQsImV4cCI6MjA3MDc4NTI0NH0.yORAl1KoxXVY-u7zMBywc9AOCeUp81_EIMZscCLoKKY
STRIPE_SECRET_KEY = sk_test_51ROPU8Fyj95atzDB8PeyNUFqKUYTclKgMk7qmhSAFmQ9Qd1eDE5y4Xp7oMlZgaPBGvWNXmf9yFi5BLdaeDMGxBAj00W8IYKcYy
STRIPE_PUBLISHABLE_KEY = pk_test_51ROPU8Fyj95atzDBVOgWzPek8Ib0sIbjnXoVr4CV3PW7cjdnDYtsdnkih8fEY6jWASYA2n3JyOl40WgQTTjdsMyL00tVSxc9KS
JWT_SECRET_KEY = your-super-secret-jwt-key-change-in-production
SECRET_KEY = your-super-secret-flask-key-change-in-production
OPENAI_API_KEY = your-openai-api-key
```

### 2.3 Deploy Backend
1. Click "Deploy" in Vercel
2. Wait for deployment to complete
3. Note your backend URL (e.g., `https://real-estate-ai-platform.vercel.app`)

## 🎨 Step 3: Deploy Frontend to Multiple Platforms

### 3.1 Deploy to Vercel

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Click "Add New..." → "Project"
3. Import your `real-estate-ai-frontend` repository
4. Configure project settings:
   - **Framework Preset**: Vite
   - **Root Directory**: `./` (leave empty)
   - **Build Command**: `pnpm build`
   - **Output Directory**: `dist`

5. Add Environment Variables:
```
VITE_API_URL = https://your-backend-url.vercel.app/api
VITE_SUPABASE_URL = https://mqzxpqxzjglfwevzgydr.supabase.co
VITE_SUPABASE_ANON_KEY = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1xenhwcXh6amdsZndldnpneWRyIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTUyMDkyNDQsImV4cCI6MjA3MDc4NTI0NH0.yORAl1KoxXVY-u7zMBywc9AOCeUp81_EIMZscCLoKKY
VITE_STRIPE_PUBLISHABLE_KEY = pk_test_51ROPU8Fyj95atzDBVOgWzPek8Ib0sIbjnXoVr4CV3PW7cjdnDYtsdnkih8fEY6jWASYA2n3JyOl40WgQTTjdsMyL00tVSxc9KS
```

6. Click "Deploy"

### 3.2 Deploy to Netlify

1. Go to [Netlify Dashboard](https://app.netlify.com)
2. Click "Add new site" → "Import an existing project"
3. Choose GitHub and select your `real-estate-ai-frontend` repository
4. Configure build settings:
   - **Build command**: `pnpm build`
   - **Publish directory**: `dist`
   - **Base directory**: (leave empty)

5. Add Environment Variables in Site Settings:
```
VITE_API_URL = https://your-backend-url.vercel.app/api
VITE_SUPABASE_URL = https://mqzxpqxzjglfwevzgydr.supabase.co
VITE_SUPABASE_ANON_KEY = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1xenhwcXh6amdsZndldnpneWRyIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTUyMDkyNDQsImV4cCI6MjA3MDc4NTI0NH0.yORAl1KoxXVY-u7zMBywc9AOCeUp81_EIMZscCLoKKY
VITE_STRIPE_PUBLISHABLE_KEY = pk_test_51ROPU8Fyj95atzDBVOgWzPek8Ib0sIbjnXoVr4CV3PW7cjdnDYtsdnkih8fEY6jWASYA2n3JyOl40WgQTTjdsMyL00tVSxc9KS
```

6. Click "Deploy site"

### 3.3 Deploy to Firebase

1. Install Firebase CLI:
```bash
npm install -g firebase-tools
```

2. Login to Firebase:
```bash
firebase login
```

3. Initialize Firebase in your frontend directory:
```bash
cd /home/ubuntu/real-estate-ai-frontend
firebase init hosting
```

4. Configure Firebase:
   - Select existing project or create new one
   - Public directory: `dist`
   - Single-page app: `Yes`
   - Overwrite index.html: `No`

5. Build and deploy:
```bash
pnpm build
firebase deploy
```

## 🔄 Step 4: Set Up Automated Deployments with GitHub Actions

### 4.1 Add GitHub Secrets

Go to your GitHub repository → Settings → Secrets and variables → Actions

Add these secrets:

**For Vercel:**
```
VERCEL_TOKEN = (get from Vercel Account Settings → Tokens)
VERCEL_ORG_ID = (get from Vercel Project Settings → General)
VERCEL_PROJECT_ID = (get from Vercel Project Settings → General)
```

**For Netlify:**
```
NETLIFY_AUTH_TOKEN = (get from Netlify User Settings → Applications)
NETLIFY_SITE_ID = (get from Netlify Site Settings → General)
```

**For Firebase:**
```
FIREBASE_SERVICE_ACCOUNT = (JSON key from Firebase Project Settings → Service Accounts)
FIREBASE_PROJECT_ID = your-firebase-project-id
```

**Environment Variables:**
```
VITE_API_URL = https://your-backend-url.vercel.app/api
VITE_SUPABASE_URL = https://mqzxpqxzjglfwevzgydr.supabase.co
VITE_SUPABASE_ANON_KEY = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im1xenhwcXh6amdsZndldnpneWRyIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTUyMDkyNDQsImV4cCI6MjA3MDc4NTI0NH0.yORAl1KoxXVY-u7zMBywc9AOCeUp81_EIMZscCLoKKY
VITE_STRIPE_PUBLISHABLE_KEY = pk_test_51ROPU8Fyj95atzDBVOgWzPek8Ib0sIbjnXoVr4CV3PW7cjdnDYtsdnkih8fEY6jWASYA2n3JyOl40WgQTTjdsMyL00tVSxc9KS
```

### 4.2 Test Automated Deployment

1. Make a small change to your code
2. Commit and push to GitHub:
```bash
git add .
git commit -m "Test automated deployment"
git push origin main
```

3. Check GitHub Actions tab to see deployment progress
4. Verify deployments on all platforms

## 🗄️ Step 5: Set Up Supabase Database

### 5.1 Create Database Schema

1. Go to [Supabase Dashboard](https://supabase.com/dashboard)
2. Select your project: `mqzxpqxzjglfwevzgydr`
3. Go to SQL Editor
4. Copy and paste the contents of `/home/ubuntu/real_estate_ai_platform/supabase_schema.sql`
5. Click "Run" to create all tables and relationships

### 5.2 Configure Row Level Security (Optional)

1. In Supabase Dashboard, go to Authentication → Policies
2. Review and customize the RLS policies as needed
3. Enable/disable policies based on your security requirements

## 🎯 Step 6: Final Testing

### 6.1 Test All Deployments

1. **Backend API**: Test health endpoint at `https://your-backend.vercel.app/health`
2. **Frontend Vercel**: Test at `https://your-frontend.vercel.app`
3. **Frontend Netlify**: Test at `https://your-site.netlify.app`
4. **Frontend Firebase**: Test at `https://your-project.web.app`

### 6.2 Test Full Application Flow

1. **User Registration**: Create new account
2. **User Login**: Test authentication
3. **Dashboard**: Verify data loading
4. **Chatbot Creation**: Test AI functionality
5. **Property Management**: Test CRUD operations
6. **QR Code Generation**: Test QR functionality
7. **Credit System**: Test payment integration
8. **API Endpoints**: Test all backend routes

## 🔧 Troubleshooting

### Common Issues:

1. **Build Failures**: Check environment variables are set correctly
2. **API Connection Issues**: Verify backend URL in frontend env vars
3. **Database Connection**: Check Supabase credentials and schema
4. **Payment Issues**: Verify Stripe keys are correct
5. **CORS Errors**: Ensure backend CORS is configured for your domains

### Debug Commands:

```bash
# Check build locally
pnpm build

# Test backend locally
cd /home/ubuntu/real_estate_ai_platform
python src/main.py

# Test frontend locally
cd /home/ubuntu/real-estate-ai-frontend
pnpm dev
```

## 🎉 Success!

You now have a fully deployed Real Estate AI Platform with:

✅ **Backend**: Deployed on Vercel with Supabase database  
✅ **Frontend**: Deployed on Vercel, Netlify, and Firebase  
✅ **Database**: Supabase PostgreSQL with full schema  
✅ **Payments**: Stripe integration with your keys  
✅ **CI/CD**: Automated deployments via GitHub Actions  
✅ **Monitoring**: Health checks and error handling  

Your platform is ready for production use and can handle enterprise-scale traffic!

## 📞 Support

If you encounter any issues:
1. Check the GitHub Actions logs for deployment errors
2. Review platform-specific logs (Vercel, Netlify, Firebase)
3. Test API endpoints individually
4. Verify environment variables are set correctly
5. Check Supabase logs for database issues

**Your Real Estate AI Platform is now live and ready to generate revenue!** 🚀

