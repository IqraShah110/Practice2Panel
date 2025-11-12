# 🔐 Authentication System Configuration Guide

## ✅ You Need to Configure BOTH

### 1. **Backend** (Render Backend Service)
**Environment Variable:**
- **Key**: `FRONTEND_URL`
- **Value**: `https://practice2panel-frontend.onrender.com` (your actual frontend URL)

**Why?**
- For CORS (Cross-Origin Resource Sharing) - allows frontend to make API calls
- For Google OAuth redirects - tells Google where to send users after login
- For session cookies - ensures cookies work between frontend and backend

### 2. **Frontend** (Render Frontend Service - Static Site)
**Environment Variable:**
- **Key**: `REACT_APP_API_URL`
- **Value**: `https://practice2panel-7bj2.onrender.com` (your backend URL)

**Why?**
- Tells frontend where to send API requests
- Used for login, signup, and all API calls

**Note:** The frontend code has auto-detection, so this is optional but recommended.

---

## 📋 Step-by-Step Configuration

### Step 1: Backend Configuration (Render)

1. Go to **Render Dashboard** → **Backend Service** (practice2panel-7bj2)
2. Click **Environment** tab
3. Add/Update environment variable:
   ```
   Key: FRONTEND_URL
   Value: https://practice2panel-frontend.onrender.com
   ```
   (Replace with your actual frontend URL if different)
4. Click **Save Changes**
5. Backend will auto-redeploy

### Step 2: Frontend Configuration (Render)

1. Go to **Render Dashboard** → **Frontend Service** (Static Site)
2. Click **Environment** tab
3. Add/Update environment variable:
   ```
   Key: REACT_APP_API_URL
   Value: https://practice2panel-7bj2.onrender.com
   ```
4. Click **Save Changes**
5. **Important:** Click **Manual Deploy** → **Clear build cache & deploy**
   (This rebuilds the frontend with the new environment variable)

### Step 3: Google OAuth Configuration

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. **APIs & Services** → **Credentials**
3. Click on your **OAuth 2.0 Client ID**
4. **Authorized JavaScript origins:**
   - Add: `https://practice2panel-frontend.onrender.com`
5. **Authorized redirect URIs:**
   - Add: `https://practice2panel-frontend.onrender.com/auth/google/callback`
6. Click **Save**

---

## ✅ Verification Checklist

After configuration, verify:

- [ ] Backend has `FRONTEND_URL` environment variable set
- [ ] Frontend has `REACT_APP_API_URL` environment variable set
- [ ] Frontend was rebuilt after adding environment variable
- [ ] Google OAuth redirect URI is updated
- [ ] Both services are deployed and running

---

## 🔍 How to Check if It's Working

1. **Open frontend URL** in browser
2. **Open browser console** (F12)
3. **Try to login:**
   - Check Network tab
   - Look for requests to: `https://practice2panel-7bj2.onrender.com/api/auth/login`
   - Should NOT see CORS errors
   - Should NOT see 404 errors

4. **Check for errors:**
   - If you see CORS error → Backend `FRONTEND_URL` is wrong
   - If you see 404 → Frontend `REACT_APP_API_URL` is wrong
   - If you see connection error → Backend is not running

---

## 📝 Summary

**Backend needs:**
- ✅ `FRONTEND_URL` = Your frontend URL (for CORS & OAuth)

**Frontend needs:**
- ✅ `REACT_APP_API_URL` = Your backend URL (for API calls)

**Google Cloud needs:**
- ✅ Frontend URL in Authorized origins
- ✅ Frontend callback URL in Redirect URIs

---

## 🚨 Common Issues

### Issue 1: CORS Error
**Solution:** Check backend `FRONTEND_URL` matches your actual frontend URL exactly (including https://)

### Issue 2: 404 on API calls
**Solution:** Check frontend `REACT_APP_API_URL` matches your backend URL exactly

### Issue 3: Google OAuth not working
**Solution:** 
- Check Google Cloud Console redirect URI matches exactly
- Check backend `FRONTEND_URL` is set correctly
- Make sure frontend URL in Google Console matches your actual deployed frontend URL

### Issue 4: Frontend still using old API URL
**Solution:** Rebuild frontend after adding environment variable (Clear build cache & deploy)

