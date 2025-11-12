# 🔧 Backend Deployment Timeout Fix

## Problem
Build successful but deployment timed out. This usually means:
- App is crashing on startup
- Health check endpoint not responding
- Database connection failing
- Missing environment variables

---

## ✅ Step-by-Step Fix

### Step 1: Check Backend Logs

1. Go to **Render Dashboard** → **Backend Service**
2. Click **"Logs"** tab
3. Look for error messages at the bottom
4. **Common errors to look for:**
   - Database connection errors
   - Missing environment variables
   - Import errors
   - Port binding errors

**What to do:**
- Copy any error messages you see
- Look for lines that say "Error", "Exception", "Failed"

---

### Step 2: Verify Environment Variables

1. Go to **Backend Service** → **Environment** tab
2. **Check these are set:**
   - ✅ `DATABASE_URL` (or PGDATABASE, PGUSER, PGPASSWORD, PGHOST, PGPORT)
   - ✅ `SECRET_KEY`
   - ✅ `FRONTEND_URL`
   - ✅ `GOOGLE_CLIENT_ID` (if using Google OAuth)
   - ✅ `GOOGLE_CLIENT_SECRET` (if using Google OAuth)
   - ✅ `PORT` (optional, defaults to 5000)

**If any are missing:**
- Add them
- Save
- Service will auto-redeploy

---

### Step 3: Check Health Check Configuration

1. Go to **Backend Service** → **Settings** tab
2. Scroll to **"Health Check Path"**
3. **Should be:** `/api/health` or `/`
4. If blank or wrong, set it to: `/api/health`

---

### Step 4: Verify Start Command

1. Go to **Backend Service** → **Settings** tab
2. Check **"Start Command"**
3. **Should be:**
   ```
   cd Backend && gunicorn app:app --bind 0.0.0.0:$PORT
   ```
   OR if Root Directory is `Backend`:
   ```
   gunicorn app:app --bind 0.0.0.0:$PORT
   ```

**If wrong:**
- Update it
- Save
- Service will redeploy

---

### Step 5: Check Root Directory

1. Go to **Backend Service** → **Settings** tab
2. Check **"Root Directory"**
3. **Should be:** `Backend` (or blank if using full paths in commands)

---

### Step 6: Test Database Connection

If you see database errors in logs:

1. Verify `DATABASE_URL` is correct
2. Test the database connection:
   - Go to your database provider (Render PostgreSQL, etc.)
   - Check if database is running
   - Verify connection string format

---

### Step 7: Manual Redeploy

After fixing issues:

1. Go to **Backend Service** → **"Manual Deploy"** button (top right)
2. Select **"Clear build cache & deploy"**
3. Wait for deployment
4. Check logs again

---

## 🔍 Common Issues & Fixes

### Issue 1: Database Connection Error
**Error in logs:** `Failed to connect`, `Connection refused`
**Fix:** 
- Check `DATABASE_URL` is correct
- Verify database is running
- Check database allows connections from Render

### Issue 2: Missing Environment Variable
**Error in logs:** `KeyError`, `Environment variable not set`
**Fix:**
- Add missing environment variable
- Redeploy

### Issue 3: Port Binding Error
**Error in logs:** `Address already in use`, `Port in use`
**Fix:**
- Make sure Start Command uses `$PORT` (not hardcoded port)
- Use: `gunicorn app:app --bind 0.0.0.0:$PORT`

### Issue 4: Import Error
**Error in logs:** `ModuleNotFoundError`, `ImportError`
**Fix:**
- Check `requirements.txt` has all packages
- Verify Root Directory is correct

### Issue 5: Health Check Failing
**Symptom:** Deployment times out, no errors in logs
**Fix:**
- Set Health Check Path to `/api/health`
- Make sure health endpoint returns 200 status
- Check health endpoint doesn't require authentication

---

## ✅ Quick Checklist

- [ ] Checked logs for errors
- [ ] All environment variables set
- [ ] Health Check Path is `/api/health`
- [ ] Start Command uses `$PORT`
- [ ] Root Directory is `Backend` (or blank)
- [ ] Database connection working
- [ ] Manual redeploy after fixes

---

## 📝 Next Steps

1. **Check logs first** - This will tell you exactly what's wrong
2. **Fix the issue** based on error message
3. **Redeploy** manually
4. **Monitor logs** during deployment

**Most likely causes:**
- Missing `DATABASE_URL`
- Wrong Start Command
- Health check path not set
- Database connection failing

Check the logs and share the error message if you need more help!

