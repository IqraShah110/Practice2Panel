# 🔐 Google OAuth: Updating URLs for Production

## Quick Answer

**NO** - Your `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` **do NOT change**.

**YES** - You **DO need to update** the Authorized URLs in Google Cloud Console.

---

## What Stays the Same ✅

- **GOOGLE_CLIENT_ID** - Same value, no change needed
- **GOOGLE_CLIENT_SECRET** - Same value, no change needed
- **Environment Variables** - Keep using the same values

---

## What Needs to Be Updated ⚠️

You need to add your **production URLs** to Google Cloud Console:

1. **Authorized JavaScript Origins**
2. **Authorized Redirect URIs**

---

## Step-by-Step: Update Google Cloud Console

### Step 1: Go to Google Cloud Console

1. Visit: [Google Cloud Console](https://console.cloud.google.com/)
2. Select your project
3. Navigate to: **APIs & Services** → **Credentials**

### Step 2: Find Your OAuth 2.0 Client

1. Look for **"OAuth 2.0 Client IDs"**
2. Click on your client (usually named something like "Web client" or your app name)

### Step 3: Update Authorized JavaScript Origins

**Add your production frontend URL:**

```
https://practice2panel-frontend.onrender.com
```

**Keep existing URLs:**
- `http://localhost:3000` (for local development)
- `http://127.0.0.1:3000` (for local development)

**Full list should look like:**
```
http://localhost:3000
http://127.0.0.1:3000
https://practice2panel-frontend.onrender.com
```

### Step 4: Update Authorized Redirect URIs

**Add your production callback URL:**

```
https://practice2panel-frontend.onrender.com/auth/google/callback
```

**Keep existing URLs:**
- `http://localhost:3000/auth/google/callback` (for local development)
- `http://127.0.0.1:3000/auth/google/callback` (for local development)

**Full list should look like:**
```
http://localhost:3000/auth/google/callback
http://127.0.0.1:3000/auth/google/callback
https://practice2panel-frontend.onrender.com/auth/google/callback
```

### Step 5: Save Changes

1. Click **"Save"** button
2. Wait 5-10 minutes for changes to propagate

---

## Backend Environment Variables

### No Changes Needed ✅

Your backend environment variables stay the same:

```env
GOOGLE_CLIENT_ID=your-existing-client-id-here
GOOGLE_CLIENT_SECRET=your-existing-client-secret-here
```

**These values don't change** - they're the same for all environments (local, staging, production).

---

## Why This Works

- **Client ID & Secret** = Your app's identity (same everywhere)
- **Authorized URLs** = Where Google allows your app to run (different per environment)

Think of it like:
- **Credentials** = Your house key (same key, works everywhere)
- **Authorized URLs** = List of addresses where you're allowed to use the key

---

## Complete Checklist

### Google Cloud Console:
- [ ] Added frontend URL to **Authorized JavaScript Origins**
- [ ] Added callback URL to **Authorized Redirect URIs**
- [ ] Kept localhost URLs for development
- [ ] Saved changes
- [ ] Waited 5-10 minutes for propagation

### Backend Environment Variables (Render):
- [ ] `GOOGLE_CLIENT_ID` is set (same value as before)
- [ ] `GOOGLE_CLIENT_SECRET` is set (same value as before)
- [ ] `FRONTEND_URL` is set to production frontend URL

### Frontend:
- [ ] Frontend is deployed and accessible
- [ ] Google OAuth button is visible

---

## Testing Google OAuth

After updating:

1. Go to your production frontend: `https://practice2panel-frontend.onrender.com`
2. Click **"Sign in with Google"** button
3. Should redirect to Google login
4. After login, should redirect back to your frontend
5. Should be logged in

---

## Troubleshooting

### Error: "redirect_uri_mismatch"
**Cause:** Redirect URI not added to Google Console  
**Fix:** Add `https://practice2panel-frontend.onrender.com/auth/google/callback` to Authorized Redirect URIs

### Error: "origin_mismatch"
**Cause:** JavaScript origin not added  
**Fix:** Add `https://practice2panel-frontend.onrender.com` to Authorized JavaScript Origins

### OAuth works locally but not in production
**Cause:** Production URLs not added to Google Console  
**Fix:** Follow Step 3 and Step 4 above

### Changes not taking effect
**Cause:** Google needs 5-10 minutes to propagate changes  
**Fix:** Wait a few minutes and try again

---

## Summary

✅ **Keep:** `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` (same values)  
✅ **Update:** Authorized URLs in Google Cloud Console (add production URLs)  
✅ **Result:** OAuth works in both local and production environments

---

**Your credentials are reusable - just add the new URLs!** 🎉

