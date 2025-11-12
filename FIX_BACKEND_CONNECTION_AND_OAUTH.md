# 🔧 Fix Backend Connection & Google OAuth Issues

## Problem 1: "Cannot connect to server" Error

### Symptoms
- Error: "Cannot connect to server at https://practice2panel-backend.onrender.com"
- Happens when creating account or other API calls

### Possible Causes

1. **Backend is sleeping** (Free tier spins down after inactivity)
2. **CORS configuration issue**
3. **Network/connectivity issue**
4. **Backend crashed**

---

## Problem 2: Google OAuth Not Completing Login

### Symptoms
- Google login redirects back to home page
- Still asks for authentication (not actually logged in)
- No error message shown

### Possible Causes

1. **Session cookies not being set** (CORS/credentials issue)
2. **checkAuth not working** after OAuth callback
3. **Backend callback endpoint failing silently**
4. **State mismatch** in OAuth flow

---

## Step-by-Step Fixes

### Fix 1: Check Backend Status

1. **Test backend directly:**
   - Visit: `https://practice2panel-backend.onrender.com/api/health`
   - Should return: `{"success": true, "status": "healthy", ...}`
   - If 404 or error → Backend is down

2. **Check Render Dashboard:**
   - Go to **Backend Service** → **Logs** tab
   - Look for errors or crashes
   - Check if service is "Live" (green) or "Sleeping" (gray)

3. **If backend is sleeping:**
   - First request takes 30-60 seconds to wake up
   - Wait and try again
   - Or upgrade to paid tier for always-on

---

### Fix 2: Verify CORS Configuration

1. **Check Backend Environment Variables:**
   - Render Dashboard → Backend Service → Environment
   - Verify `FRONTEND_URL` is set to: `https://practice2panel-frontend-8ptb.onrender.com`
   - **No trailing slash!**

2. **Check Backend CORS Code:**
   - Should allow your frontend URL
   - Should have `supports_credentials=True`

3. **Test CORS:**
   - Open browser console (F12)
   - Try to create account
   - Check for CORS errors in console

---

### Fix 3: Fix Google OAuth Session Issue

The issue is likely that:
1. OAuth callback succeeds on backend
2. Session is created
3. But frontend `checkAuth` doesn't detect it (cookie issue)

#### Solution A: Check Session Cookies

1. **After Google OAuth redirect:**
   - Open browser DevTools (F12)
   - Go to **Application** tab → **Cookies**
   - Check if cookies are set for your backend domain
   - Look for session cookies

2. **If no cookies:**
   - CORS credentials not working
   - Check `FRONTEND_URL` in backend
   - Verify `credentials: 'include'` in frontend fetch calls

#### Solution B: Improve checkAuth After OAuth

The `checkAuth` might be called too quickly. Let's add a small delay:

---

## Quick Fixes to Apply

### Fix 1: Update GoogleCallback Component

Add better error handling and retry logic:

```javascript
// In GoogleCallback.js, after successful callback:
if (data.success) {
  // Wait a moment for session to be set
  await new Promise(resolve => setTimeout(resolve, 500));
  // Refresh auth state
  await checkAuth();
  // Double-check auth before navigating
  setTimeout(() => {
    checkAuth().then(() => {
      navigate('/');
    });
  }, 1000);
}
```

### Fix 2: Check Backend Logs for OAuth Errors

1. Go to **Render Dashboard** → **Backend Service** → **Logs**
2. Try Google OAuth login
3. Look for errors in logs around the callback
4. Common errors:
   - "Invalid state parameter"
   - "Authorization code expired"
   - "Failed to exchange authorization code"

### Fix 3: Verify Environment Variables

**Backend Environment Variables (Render):**
- ✅ `FRONTEND_URL` = `https://practice2panel-frontend-8ptb.onrender.com` (no trailing slash)
- ✅ `GOOGLE_CLIENT_ID` = (your client ID)
- ✅ `GOOGLE_CLIENT_SECRET` = (your client secret)
- ✅ `SECRET_KEY` = (strong key, not default)

---

## Debugging Steps

### Step 1: Test Backend Connection

1. Open browser console (F12)
2. Go to **Network** tab
3. Try to create account
4. Look for the request to `/api/auth/signup`
5. Check:
   - **Status code:** 200 = success, 500 = server error, (failed) = connection issue
   - **Request URL:** Should be backend URL
   - **Response:** Check what error message backend returns

### Step 2: Test Google OAuth Flow

1. Open browser console (F12)
2. Go to **Network** tab
3. Click "Sign in with Google"
4. Complete Google login
5. After redirect, check:
   - Request to `/api/auth/google/callback`
   - **Status:** Should be 200
   - **Response:** Should have `{"success": true, ...}`
   - **Cookies:** Check if session cookies are set

### Step 3: Check Browser Console Errors

Look for:
- CORS errors
- Network errors
- JavaScript errors
- Cookie warnings

---

## Common Issues & Solutions

### Issue 1: Backend Connection Timeout

**Symptom:** "Cannot connect to server"  
**Cause:** Backend sleeping (free tier)  
**Fix:** 
- Wait 30-60 seconds for first request
- Or upgrade to paid tier

### Issue 2: CORS Error

**Symptom:** CORS error in console  
**Cause:** `FRONTEND_URL` not set correctly  
**Fix:**
- Update `FRONTEND_URL` in backend environment variables
- Must match exact frontend URL (no trailing slash)
- Backend will auto-redeploy

### Issue 3: OAuth Callback Succeeds But Not Logged In

**Symptom:** Redirects to home but still asks for login  
**Cause:** Session cookie not being set/sent  
**Fix:**
- Check `FRONTEND_URL` is correct
- Verify `credentials: 'include'` in fetch calls
- Check browser allows cookies
- Try in incognito mode (to rule out cookie issues)

### Issue 4: OAuth State Mismatch

**Symptom:** "Invalid state parameter" error  
**Cause:** Session state lost  
**Fix:**
- Clear browser cookies
- Try again
- Check if session storage is working

---

## Immediate Actions

### 1. Check Backend Health
```
Visit: https://practice2panel-backend.onrender.com/api/health
```
- If it works → Backend is up
- If it fails → Backend is down, check Render logs

### 2. Verify Environment Variables
- Render Dashboard → Backend → Environment
- Check all required variables are set

### 3. Check Backend Logs
- Render Dashboard → Backend → Logs
- Look for errors when you try to:
  - Create account
  - Login with Google

### 4. Test in Browser Console
- Open DevTools (F12)
- Network tab
- Try the actions
- See what requests fail and why

---

## Next Steps

1. **Check backend health** (visit health endpoint)
2. **Check backend logs** (Render dashboard)
3. **Verify environment variables** (especially `FRONTEND_URL`)
4. **Test in browser console** (see Network tab errors)
5. **Share the specific error** you see in logs/console

Let me know what you find and I'll help fix the specific issue!

