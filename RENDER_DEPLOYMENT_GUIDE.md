# 🚀 Complete Render Deployment Guide - Practice2Panel

**Step-by-step guide to deploy your Practice2Panel application to Render**

---

## 📋 Prerequisites

Before starting, make sure you have:
- ✅ GitHub repository pushed (your code is on GitHub)
- ✅ Render account (sign up at [render.com](https://render.com))
- ✅ PostgreSQL database (Render provides this, or use external)
- ✅ OpenAI API key (for chatbot features)
- ✅ Google OAuth credentials (if using Google login)

---

## 🗄️ Step 1: Set Up PostgreSQL Database (If Not Already Done)

### 1.1 Create PostgreSQL Database on Render

1. Go to **Render Dashboard** → **"New +"** → **"PostgreSQL"**
2. Configure:
   - **Name**: `practice2panel-db` (or your preferred name)
   - **Database**: `practice2panel` (or your preferred name)
   - **User**: Auto-generated (or custom)
   - **Region**: Choose closest to your backend
   - **PostgreSQL Version**: Latest (15 or 16)
3. Click **"Create Database"**
4. **Important:** Wait for database to be created (takes 1-2 minutes)
5. **Copy the Internal Database URL** (you'll need this)

### 1.2 Note Your Database Connection String

After creation, you'll see:
- **Internal Database URL**: `postgresql://user:password@host:port/dbname`
- **External Database URL**: (for local connections if needed)

**Save the Internal Database URL** - you'll use it in Step 2.

---

## 🔧 Step 2: Deploy Backend (Flask API)

### 2.1 Create Web Service

1. Go to **Render Dashboard** → **"New +"** → **"Web Service"**
2. **Connect your GitHub repository**
3. Select your repository: `Interview_project_login_chatbot_mock interview` (or your repo name)

### 2.2 Configure Backend Settings

**Basic Settings:**
- **Name**: `practice2panel-backend` (or your preferred name)
- **Region**: Same as database (for better performance)
- **Branch**: `main`
- **Root Directory**: `Backend` ⚠️ **Important!**
- **Runtime**: `Python 3`
- **Build Command**: 
  ```
  pip install -r requirements.txt
  ```
- **Start Command**: 
  ```
  gunicorn app:app --bind 0.0.0.0:$PORT
  ```

**Advanced Settings:**
- **Instance Type**: Free tier (or paid for better performance)
- **Health Check Path**: `/api/health`
- **Auto-Deploy**: `Yes` (redeploys on git push)

### 2.3 Set Environment Variables

Go to **Environment** tab and add these variables:

#### Required Variables:

```env
# Database (use the Internal Database URL from Step 1)
DATABASE_URL=postgresql://user:password@host:port/dbname

# Flask Configuration
SECRET_KEY=your-generated-secret-key-here
FLASK_DEBUG=False
PORT=10000

# Frontend URL (update after frontend is deployed)
FRONTEND_URL=https://practice2panel-frontend.onrender.com

# OpenAI API (for chatbot and mock interviews)
OPENAI_API_KEY=your-openai-api-key-here
```

#### Optional Variables (if using these features):

```env
# Google OAuth (if using Google login)
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# Email Service (if using email verification)
EMAIL_ADDRESS=your-email@gmail.com
EMAIL_PASSWORD=your-app-password

# Whisper Model (voice processing)
WHISPER_MODEL=base
```

### 2.4 Generate SECRET_KEY

**Important:** Generate a strong SECRET_KEY before deploying:

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

Copy the output and use it as your `SECRET_KEY` value.

### 2.5 Deploy Backend

1. Click **"Create Web Service"**
2. Wait for deployment (5-10 minutes first time)
3. **Check the logs** for any errors
4. Once deployed, note your backend URL: `https://practice2panel-backend.onrender.com`

### 2.6 Verify Backend is Working

Test these URLs:
- `https://your-backend-url.onrender.com/api/health` - Should return JSON
- `https://your-backend-url.onrender.com/` - Should return API info

---

## 🎨 Step 3: Deploy Frontend (React App)

### 3.1 Create Static Site

1. Go to **Render Dashboard** → **"New +"** → **"Static Site"** ⚠️ **NOT Web Service!**
2. **Connect your GitHub repository** (same repo as backend)
3. Select your repository

### 3.2 Configure Frontend Settings

**Basic Settings:**
- **Name**: `practice2panel-frontend` (or your preferred name)
- **Branch**: `main`
- **Root Directory**: `frontend` ⚠️ **Important!**
- **Build Command**: 
  ```
  npm install && npm run build
  ```
- **Publish Directory**: 
  ```
  build
  ```

**Advanced Settings:**
- **Auto-Deploy**: `Yes` (redeploys on git push)

### 3.3 Set Environment Variables

Go to **Environment** tab and add:

```env
# Backend API URL (use your backend URL from Step 2)
REACT_APP_API_URL=https://practice2panel-backend.onrender.com
```

**Note:** Replace `practice2panel-backend.onrender.com` with your actual backend URL.

### 3.4 Deploy Frontend

1. Click **"Create Static Site"**
2. Wait for deployment (3-5 minutes)
3. Once deployed, note your frontend URL: `https://practice2panel-frontend.onrender.com`

---

## 🔗 Step 4: Connect Frontend and Backend

### 4.1 Update Backend Environment Variable

1. Go to **Backend Service** → **Environment** tab
2. Update `FRONTEND_URL` to your actual frontend URL:
   ```
   FRONTEND_URL=https://practice2panel-frontend.onrender.com
   ```
3. Click **"Save Changes"** (backend will auto-redeploy)

### 4.2 Update Frontend Environment Variable (if needed)

1. Go to **Frontend Service** → **Environment** tab
2. Verify `REACT_APP_API_URL` is set to your backend URL
3. If you changed it, click **"Manual Deploy"** → **"Clear build cache & deploy"**

---

## 🔐 Step 5: Configure Google OAuth (If Using)

### 5.1 Update Google Cloud Console

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. **APIs & Services** → **Credentials**
3. Click on your **OAuth 2.0 Client ID**

### 5.2 Add Authorized JavaScript Origins

Add your frontend URL:
```
https://practice2panel-frontend.onrender.com
```

### 5.3 Add Authorized Redirect URIs

Add:
```
https://practice2panel-frontend.onrender.com/auth/google/callback
```

**Keep existing localhost URIs** for local development.

### 5.4 Save Changes

Click **"Save"** in Google Cloud Console.

---

## ✅ Step 6: Verify Deployment

### 6.1 Test Backend

1. Open: `https://your-backend-url.onrender.com/api/health`
   - Should return: `{"success": true, "message": "API is running", ...}`
2. Open: `https://your-backend-url.onrender.com/`
   - Should return API information

### 6.2 Test Frontend

1. Open: `https://your-frontend-url.onrender.com`
2. Check browser console (F12) for errors
3. Test features:
   - ✅ Sign up / Login
   - ✅ Google OAuth (if configured)
   - ✅ Skill preparation
   - ✅ Mock interview
   - ✅ Chatbot

### 6.3 Check API Connection

1. Open browser console (F12)
2. Go to **Network** tab
3. Try logging in
4. Verify API calls go to: `https://your-backend-url.onrender.com/api/*`

---

## 🔧 Troubleshooting

### Backend Not Starting

**Check:**
1. **Logs** → Look for error messages
2. **Environment Variables** → All required vars set?
3. **Start Command** → Should be: `gunicorn app:app --bind 0.0.0.0:$PORT`
4. **Root Directory** → Should be: `Backend`
5. **Health Check Path** → Should be: `/api/health`

### Frontend Build Failing

**Check:**
1. **Service Type** → Must be **"Static Site"**, NOT "Web Service"
2. **Root Directory** → Should be: `frontend`
3. **Build Command** → Should be: `npm install && npm run build`
4. **Publish Directory** → Should be: `build`
5. **Logs** → Check for npm errors

### Database Connection Errors

**Check:**
1. **DATABASE_URL** → Using Internal Database URL?
2. **Database Status** → Is database running?
3. **Network** → Backend and database in same region?

### CORS Errors

**Check:**
1. **FRONTEND_URL** → Set correctly in backend?
2. **REACT_APP_API_URL** → Set correctly in frontend?
3. **Backend CORS** → Check `Backend/app.py` allowed origins

### Authentication Not Working

**Check:**
1. **SECRET_KEY** → Set and not using default?
2. **FRONTEND_URL** → Matches actual frontend URL?
3. **Google OAuth** → Redirect URIs updated in Google Console?
4. **Session Cookies** → Check browser console for cookie issues

---

## 📊 Environment Variables Summary

### Backend (Required)
```env
DATABASE_URL=postgresql://...
SECRET_KEY=...
FLASK_DEBUG=False
PORT=10000
FRONTEND_URL=https://...
OPENAI_API_KEY=...
```

### Backend (Optional)
```env
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...
EMAIL_ADDRESS=...
EMAIL_PASSWORD=...
WHISPER_MODEL=base
```

### Frontend (Required)
```env
REACT_APP_API_URL=https://...
```

---

## 🎯 Quick Reference

### Backend Service Settings
- **Type**: Web Service
- **Root Directory**: `Backend`
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `gunicorn app:app --bind 0.0.0.0:$PORT`
- **Health Check**: `/api/health`

### Frontend Service Settings
- **Type**: Static Site ⚠️ **NOT Web Service!**
- **Root Directory**: `frontend`
- **Build Command**: `npm install && npm run build`
- **Publish Directory**: `build`

---

## 🚀 After Deployment

### Monitor Your Services

1. **Render Dashboard** → Check service status
2. **Logs** → Monitor for errors
3. **Metrics** → Check performance

### Update Your Code

1. Push changes to GitHub
2. Render will auto-deploy (if auto-deploy is enabled)
3. Check logs after deployment

### Scale (If Needed)

- **Free Tier**: Limited resources, may spin down after inactivity
- **Paid Tier**: Always-on, better performance
- Upgrade in **Settings** → **Instance Type**

---

## 📝 Notes

- **Free Tier**: Services may spin down after 15 minutes of inactivity
- **First Request**: May take 30-60 seconds to wake up (free tier)
- **Database**: Free tier has connection limits
- **Build Time**: First deployment takes longer (5-10 min)

---

## ✅ Deployment Checklist

- [ ] PostgreSQL database created
- [ ] Backend deployed and working
- [ ] Frontend deployed and working
- [ ] Environment variables set correctly
- [ ] Google OAuth configured (if using)
- [ ] All features tested
- [ ] CORS working correctly
- [ ] Authentication working

---

**🎉 Congratulations! Your application should now be live on Render!**

For issues, check the troubleshooting section or Render's documentation.

