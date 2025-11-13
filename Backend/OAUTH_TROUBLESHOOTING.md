# OAuth "deleted_client" Error - Troubleshooting Guide

## Current Client ID in your .env:
```
731124056099-ehod57r9sc72ldr7lht40i99p7fmoh6s.apps.googleusercontent.com
```

## Step-by-Step Fix:

### 1. Verify OAuth Client Exists in Google Cloud Console
1. Go to: https://console.cloud.google.com/apis/credentials
2. Find your OAuth 2.0 Client ID: `731124056099-ehod57r9sc72ldr7lht40i99p7fmoh6s`
3. **Check if it shows "Active" status**
   - If it's deleted or missing → Create a new one
   - If it exists but shows errors → Check the configuration below

### 2. Check OAuth Consent Screen Configuration
1. Go to: https://console.cloud.google.com/apis/credentials/consent
2. Verify:
   - ✅ **Publishing status**: Should be "Testing" or "In production"
   - ✅ **User type**: External (for testing) or Internal
   - ✅ **Scopes added**: 
     - `openid`
     - `https://www.googleapis.com/auth/userinfo.email`
     - `https://www.googleapis.com/auth/userinfo.profile`
   - ✅ **Test users**: Add `iqrashah.bscsf22dd@iba-suk.edu.pk` as a test user

### 3. Verify Redirect URIs Match EXACTLY
In Google Cloud Console → Your OAuth Client → Authorized redirect URIs:

**Must include these EXACT URLs (case-sensitive, no trailing slashes):**
```
http://localhost:3000/auth/google/callback
http://127.0.0.1:3000/auth/google/callback
```

**Common mistakes:**
- ❌ `http://localhost:3000/auth/google/callback/` (trailing slash)
- ❌ `http://localhost:3000/Auth/Google/Callback` (wrong case)
- ❌ `http://localhost:3000/google/callback` (wrong path)
- ✅ `http://localhost:3000/auth/google/callback` (CORRECT)

### 4. Verify Authorized JavaScript Origins
**Must include:**
```
http://localhost:3000
http://127.0.0.1:3000
```

### 5. If Client is Actually Deleted - Create New One

#### Create New OAuth Client:
1. Go to: https://console.cloud.google.com/apis/credentials
2. Click **+ CREATE CREDENTIALS** → **OAuth client ID**
3. Application type: **Web application**
4. Name: "Interview App OAuth Client"
5. **Authorized JavaScript origins:**
   ```
   http://localhost:3000
   http://127.0.0.1:3000
   ```
6. **Authorized redirect URIs:**
   ```
   http://localhost:3000/auth/google/callback
   http://127.0.0.1:3000/auth/google/callback
   ```
7. Click **CREATE**
8. **Copy the new Client ID and Client Secret**
9. Update your `.env` file:
   ```env
   GOOGLE_CLIENT_ID=<new-client-id>
   GOOGLE_CLIENT_SECRET=<new-client-secret>
   FRONTEND_URL=http://localhost:3000
   ```

### 6. Restart Your Flask Server
After making changes:
1. Stop your Flask server (Ctrl+C)
2. Restart it: `python app.py`
3. Clear browser cache or use incognito mode
4. Try Google login again

### 7. Check Browser Console for Errors
Open browser DevTools (F12) → Console tab
Look for any JavaScript errors related to OAuth

### 8. Verify Your Frontend is Running
Make sure your frontend is running on:
- `http://localhost:3000` (not 3001, 8080, etc.)

### 9. Test with Different Browser/Incognito
Sometimes cached OAuth tokens cause issues. Try:
- Incognito/Private browsing mode
- Different browser
- Clear browser cache

## Quick Verification Checklist:
- [ ] OAuth client exists and is "Active" in Google Cloud Console
- [ ] OAuth consent screen is configured (Testing or In production)
- [ ] Your email (`iqrashah.bscsf22dd@iba-suk.edu.pk`) is added as a test user
- [ ] Redirect URIs match EXACTLY (no trailing slashes, correct path)
- [ ] JavaScript origins are set correctly
- [ ] `.env` file has correct `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET`
- [ ] `.env` file has `FRONTEND_URL=http://localhost:3000` (no trailing slash)
- [ ] Flask server restarted after `.env` changes
- [ ] Frontend is running on `http://localhost:3000`

## Still Not Working?
1. **Double-check the Client ID** in Google Cloud Console matches your `.env` file
2. **Check if you're using the correct Google account** (the one added as test user)
3. **Wait a few minutes** - Google OAuth changes can take 5-10 minutes to propagate
4. **Check Google Cloud Console logs** for any errors

