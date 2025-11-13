# 🔧 Troubleshooting: Why App Won't Run

## Common Issues and Solutions

### 1. Database Connection Error

**Error:** `Missing required environment variables`

**Solution:**
- Ensure `DATABASE_URL` is set in your `.env` file (for local) or Render environment variables (for deployment)
- Format: `postgresql://user:password@host:port/dbname?sslmode=require`

**Note:** The app will still start even if database is not available - it will initialize on first use.

### 2. Port Already in Use

**Error:** `Address already in use` or `Port 5000 is already in use`

**Solution:**
```bash
# Find process using port 5000
netstat -ano | findstr :5000

# Kill the process (replace PID with actual process ID)
taskkill /PID <PID> /F

# Or use a different port
# Set PORT=5001 in .env file
```

### 3. Missing Dependencies

**Error:** `ModuleNotFoundError`

**Solution:**
```bash
cd Backend
pip install -r requirements.txt
```

### 4. Import Errors

**Error:** `ImportError` or `ModuleNotFoundError`

**Solution:**
- Ensure you're running from the correct directory
- Check all imports in `app.py` are available
- Verify `requirements.txt` has all dependencies

### 5. App Starts But Routes Don't Work

**Check:**
- Is the app actually running? Check the terminal output
- Are you accessing the correct URL? (http://localhost:5000/api/health)
- Check CORS settings if accessing from frontend
- Verify environment variables are loaded

### 6. Render Deployment Issues

**Common Problems:**

1. **Build Fails:**
   - Check build logs in Render dashboard
   - Verify `requirements.txt` is correct
   - Ensure Python version is compatible

2. **App Crashes on Start:**
   - Check Render logs for error messages
   - Verify all environment variables are set
   - Ensure `DATABASE_URL` is correct
   - Check start command: `gunicorn app:app --bind 0.0.0.0:$PORT`

3. **Database Connection Fails:**
   - Verify `DATABASE_URL` is set correctly
   - Check if database is accessible from Render
   - Ensure SSL mode is included: `?sslmode=require`

4. **Health Check Fails:**
   - Root route `/` is now available for Render health checks
   - Also check `/api/health` endpoint

## Quick Diagnostic Commands

### Test App Import
```bash
cd Backend
python -c "from app import app; print('App loaded successfully')"
```

### Test Database Connection
```bash
cd Backend
python -c "from db_handler import get_pg_connection; conn = get_pg_connection(); print('Database connected!')"
```

### Check Routes
```bash
cd Backend
python -c "from app import app; print('Routes:', [rule.rule for rule in app.url_map.iter_rules()])"
```

### Test Server Start
```bash
cd Backend
python start_server.py
```

## Expected Behavior

### On Local Start:
```
🚀 Starting Practice2Panel API server...
📍 Server will run on: http://localhost:5000
🔧 Debug mode: True
📊 Health check: http://localhost:5000/api/health
❓ Questions API: http://localhost:5000/api/questions/<interview_type>/<skill>
============================================================
⚠ Database not available on startup: [error message]
INFO:app:Database will be initialized on first connection
 * Running on http://0.0.0.0:5000
```

### On Render:
- Build should complete successfully
- App should start with gunicorn
- Health check at `/` should return 200
- Database should connect automatically

## Still Not Working?

1. **Check Logs:**
   - Local: Check terminal output
   - Render: Check Render dashboard logs

2. **Verify Environment:**
   - `.env` file exists in `Backend/` directory
   - All required variables are set
   - No typos in variable names

3. **Test Individual Components:**
   - Database connection
   - Flask app creation
   - Route registration

4. **Common Mistakes:**
   - Wrong directory (should be in `Backend/` for server)
   - Missing `.env` file
   - Incorrect `DATABASE_URL` format
   - Port conflicts

---

**If you're still having issues, share the specific error message!**

