# 🚀 Step-by-Step Deployment Guide - Practice2Panel on Render

**For users with existing Render account - Second deployment**

---

## 📤 Step 1: Push Code to GitHub

### 1.1 Check Current Status
```bash
git status
```

### 1.2 Add All Changes (if any)
```bash
git add .
```

### 1.3 Commit Changes
```bash
git commit -m "Ready for Render deployment"
```

### 1.4 Push to GitHub
```bash
git push origin main
```

**Verify:** Go to GitHub and confirm your latest code is there.

---

## 🗄️ Step 2: Set Up PostgreSQL Database

### 2.1 Create New Database
1. Go to **Render Dashboard** → Click **"New +"** → Select **"PostgreSQL"**
2. Fill in:
   - **Name**: `practice2panel-db` (or your choice)
   - **Database**: `practice2panel` (or your choice)
   - **User**: Auto-generated (or custom)
   - **Region**: Choose closest to you
   - **PostgreSQL Version**: Latest (15 or 16)
3. Click **"Create Database"**
4. **Wait 1-2 minutes** for database to be ready

### 2.2 Copy Database URL
1. Once created, go to database dashboard
2. Find **"Internal Database URL"** (looks like: `postgresql://user:pass@host:port/dbname`)
3. **Copy this URL** - you'll need it in Step 3
4. **Save it somewhere safe** (you'll use it as `DATABASE_URL`)

---

## 🔧 Step 3: Deploy Backend (Flask API)

### 3.1 Create Web Service
1. Go to **Render Dashboard** → Click **"New +"** → Select **"Web Service"**
2. **Connect GitHub** (if not already connected)
3. Select your repository: `Interview_project_login_chatbot_mock interview`

### 3.2 Configure Backend Settings

**Basic Settings Tab:**
- **Name**: `practice2panel-backend` (or your choice)
- **Region**: **Same region as your database** (important!)
- **Branch**: `main`
- **Root Directory**: `Backend` ⚠️ **CRITICAL - Must be exactly "Backend"**
- **Runtime**: `Python 3`
- **Build Command**: 
  ```
  pip install -r requirements.txt
  ```
- **Start Command**: 
  ```
  gunicorn app:app --bind 0.0.0.0:$PORT
  ```

**Advanced Settings Tab:**
- **Instance Type**: Free (or paid if you prefer)
- **Health Check Path**: `/api/health`
- **Auto-Deploy**: `Yes` (recommended)

### 3.3 Set Environment Variables

Click **"Environment"** tab and add these variables:

#### Required Variables:

| Key | Value | Notes |
|-----|-------|-------|
| `DATABASE_URL` | `postgresql://...` | Paste Internal Database URL from Step 2 |
| `SECRET_KEY` | `bdfd7463bf4c49f6139be64d4a952bb4c59d08f3bda6143782480c0c9e09fe3c` | Use this generated key |
| `FLASK_DEBUG` | `False` | Must be False for production |
| `PORT` | `10000` | Render sets this automatically, but good to have |
| `OPENAI_API_KEY` | `your-openai-key-here` | Your OpenAI API key |
| `FRONTEND_URL` | `https://practice2panel-frontend.onrender.com` | Update after frontend deployed |

#### Optional Variables (if using these features):

| Key | Value |
|-----|-------|
| `GOOGLE_CLIENT_ID` | Your Google OAuth client ID |
| `GOOGLE_CLIENT_SECRET` | Your Google OAuth client secret |
| `EMAIL_ADDRESS` | Your email for sending emails |
| `EMAIL_PASSWORD` | Your email app password |
| `WHISPER_MODEL` | `base` |

**Important:** 
- Don't click "Create Web Service" yet
- We'll update `FRONTEND_URL` after frontend is deployed

### 3.4 Create Backend Service
1. Review all settings
2. Click **"Create Web Service"**
3. **Wait 5-10 minutes** for first deployment
4. Watch the **Logs** tab for any errors

### 3.5 Verify Backend is Working
1. Once deployment completes, note your backend URL: `https://practice2panel-backend.onrender.com`
2. Test these URLs in browser:
   - `https://your-backend-url.onrender.com/api/health` → Should show JSON response
   - `https://your-backend-url.onrender.com/` → Should show API info
3. If both work, backend is ready! ✅

**Save your backend URL** - you'll need it for frontend.

---

## 🎨 Step 4: Deploy Frontend (React App)

### 4.1 Create Static Site
1. Go to **Render Dashboard** → Click **"New +"** → Select **"Static Site"** ⚠️ **NOT Web Service!**
2. **Connect GitHub** (same repository)
3. Select your repository: `Interview_project_login_chatbot_mock interview`

### 4.2 Configure Frontend Settings

**Basic Settings Tab:**
- **Name**: `practice2panel-frontend` (or your choice)
- **Branch**: `main`
- **Root Directory**: `frontend` ⚠️ **CRITICAL - Must be exactly "frontend"**
- **Build Command**: 
  ```
  npm install && npm run build
  ```
- **Publish Directory**: 
  ```
  build
  ```

**Advanced Settings Tab:**
- **Auto-Deploy**: `Yes` (recommended)

### 4.3 Set Environment Variables

Click **"Environment"** tab and add:

| Key | Value |
|-----|-------|
| `REACT_APP_API_URL` | `https://practice2panel-backend.onrender.com` | Use your actual backend URL from Step 3 |

**Important:** Replace `practice2panel-backend.onrender.com` with your actual backend URL!

### 4.4 Create Frontend Service
1. Review all settings
2. Click **"Create Static Site"**
3. **Wait 3-5 minutes** for deployment
4. Watch the **Logs** tab for any errors

### 4.5 Verify Frontend is Working
1. Once deployment completes, note your frontend URL: `https://practice2panel-frontend.onrender.com`
2. Open the URL in browser
3. Check browser console (F12) for errors
4. If page loads, frontend is ready! ✅

**Save your frontend URL** - you'll need it next.

---

## 🔗 Step 5: Connect Frontend and Backend

### 5.1 Update Backend FRONTEND_URL
1. Go to **Backend Service** → Click **"Environment"** tab
2. Find `FRONTEND_URL` variable
3. Update value to your actual frontend URL: `https://practice2panel-frontend.onrender.com`
4. Click **"Save Changes"**
5. Backend will **auto-redeploy** (wait 2-3 minutes)

### 5.2 Verify Connection
1. Open your frontend URL in browser
2. Open browser console (F12) → **Network** tab
3. Try to **login or signup**
4. Check Network tab - API calls should go to your backend URL
5. If no CORS errors, connection is working! ✅

---

## 🔐 Step 6: Configure Google OAuth (If Using)

### 6.1 Update Google Cloud Console
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Navigate to **APIs & Services** → **Credentials**
3. Click on your **OAuth 2.0 Client ID**

### 6.2 Add Authorized JavaScript Origins
Add your frontend URL:
```
https://practice2panel-frontend.onrender.com
```

### 6.3 Add Authorized Redirect URIs
Add:
```
https://practice2panel-frontend.onrender.com/auth/google/callback
```

**Keep existing localhost URIs** for local development.

### 6.4 Save Changes
1. Click **"Save"** in Google Cloud Console
2. Wait 5-10 minutes for changes to propagate

### 6.5 Update Backend Environment Variables
1. Go to **Backend Service** → **Environment** tab
2. Add/Update:
   - `GOOGLE_CLIENT_ID` = Your Google client ID
   - `GOOGLE_CLIENT_SECRET` = Your Google client secret
3. Click **"Save Changes"**
4. Backend will auto-redeploy

---

## ✅ Step 7: Final Testing

### 7.1 Test Backend
- [ ] Health check: `https://your-backend.onrender.com/api/health` → Returns JSON
- [ ] Root endpoint: `https://your-backend.onrender.com/` → Returns API info

### 7.2 Test Frontend
- [ ] Frontend loads: `https://your-frontend.onrender.com` → No errors
- [ ] Browser console: No CORS errors
- [ ] Network tab: API calls go to backend

### 7.3 Test Features
- [ ] **Sign Up**: Create new account
- [ ] **Login**: Login with email/password
- [ ] **Google OAuth**: Login with Google (if configured)
- [ ] **Skill Prep**: Navigate to skill preparation
- [ ] **Mock Interview**: Start a mock interview
- [ ] **Chatbot**: Use the chatbot feature

### 7.4 Check Logs
1. **Backend Logs**: Render Dashboard → Backend → Logs tab
2. **Frontend Logs**: Render Dashboard → Frontend → Logs tab
3. Look for any errors or warnings

---

## 🎯 Quick Reference - All Your URLs

**Save these for future reference:**

```
Backend URL:  https://practice2panel-backend.onrender.com
Frontend URL: https://practice2panel-frontend.onrender.com
Database URL: postgresql://... (from Render dashboard)
```

---

## 🔧 Troubleshooting Common Issues

### Backend Not Starting
**Check:**
- ✅ Root Directory is `Backend` (not `backend` or empty)
- ✅ Start Command: `gunicorn app:app --bind 0.0.0.0:$PORT`
- ✅ All environment variables are set
- ✅ Check Logs tab for specific errors

### Frontend Build Failing
**Check:**
- ✅ Service Type is **"Static Site"** (NOT Web Service)
- ✅ Root Directory is `frontend` (not `Frontend` or empty)
- ✅ Build Command: `npm install && npm run build`
- ✅ Publish Directory: `build`
- ✅ Check Logs tab for npm errors

### Database Connection Error
**Check:**
- ✅ Using **Internal Database URL** (not External)
- ✅ Backend and Database in **same region**
- ✅ `DATABASE_URL` environment variable is set correctly

### CORS Errors
**Check:**
- ✅ `FRONTEND_URL` in backend matches actual frontend URL
- ✅ `REACT_APP_API_URL` in frontend matches actual backend URL
- ✅ Backend has been redeployed after updating `FRONTEND_URL`

### Authentication Not Working
**Check:**
- ✅ `SECRET_KEY` is set (not using default)
- ✅ `FRONTEND_URL` is correct in backend
- ✅ Google OAuth redirect URIs updated in Google Console
- ✅ Check browser console for cookie/session errors

---

## 📝 Important Notes

1. **Free Tier Limitations:**
   - Services may spin down after 15 minutes of inactivity
   - First request after spin-down takes 30-60 seconds
   - Database has connection limits

2. **Auto-Deploy:**
   - Both services will auto-redeploy when you push to GitHub
   - Monitor logs after each push

3. **Environment Variables:**
   - Changes require service restart
   - Backend auto-restarts when you save env vars
   - Frontend needs manual redeploy after env var changes

4. **Build Times:**
   - First deployment: 5-10 minutes
   - Subsequent deployments: 2-5 minutes

---

## ✅ Deployment Checklist

Use this to track your progress:

- [ ] Code pushed to GitHub
- [ ] PostgreSQL database created
- [ ] Database URL copied
- [ ] Backend service created
- [ ] Backend environment variables set
- [ ] Backend deployed and working
- [ ] Frontend service created (Static Site)
- [ ] Frontend environment variables set
- [ ] Frontend deployed and working
- [ ] Backend FRONTEND_URL updated
- [ ] Google OAuth configured (if using)
- [ ] All features tested
- [ ] No errors in logs

---

## 🎉 You're Done!

Your Practice2Panel application should now be live on Render!

**Next Steps:**
- Share your frontend URL with users
- Monitor logs for any issues
- Set up monitoring/alerts if needed
- Consider upgrading to paid tier for better performance

---

**Need Help?** Check the troubleshooting section or Render's documentation.

