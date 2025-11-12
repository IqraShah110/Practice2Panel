# ⚡ Render Quick Start Checklist

**Quick reference for deploying Practice2Panel to Render**

---

## 🗄️ Database Setup

- [ ] Create PostgreSQL database on Render
- [ ] Copy Internal Database URL
- [ ] Database is running

---

## 🔧 Backend Deployment

### Service Configuration
- [ ] Service Type: **Web Service**
- [ ] Root Directory: `Backend`
- [ ] Build Command: `pip install -r requirements.txt`
- [ ] Start Command: `gunicorn app:app --bind 0.0.0.0:$PORT`
- [ ] Health Check Path: `/api/health`

### Environment Variables
- [ ] `DATABASE_URL` = (Internal Database URL)
- [ ] `SECRET_KEY` = (Generated - see below)
- [ ] `FLASK_DEBUG` = `False`
- [ ] `PORT` = `10000`
- [ ] `FRONTEND_URL` = (Update after frontend deployed)
- [ ] `OPENAI_API_KEY` = (Your OpenAI key)

### Generate SECRET_KEY
```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

**Generated Key:** `bdfd7463bf4c49f6139be64d4a952bb4c59d08f3bda6143782480c0c9e09fe3c`

---

## 🎨 Frontend Deployment

### Service Configuration
- [ ] Service Type: **Static Site** ⚠️ (NOT Web Service!)
- [ ] Root Directory: `frontend`
- [ ] Build Command: `npm install && npm run build`
- [ ] Publish Directory: `build`

### Environment Variables
- [ ] `REACT_APP_API_URL` = (Your backend URL)

---

## 🔗 Connect Services

- [ ] Update backend `FRONTEND_URL` with frontend URL
- [ ] Verify frontend `REACT_APP_API_URL` is correct

---

## ✅ Test

- [ ] Backend health check: `/api/health`
- [ ] Frontend loads correctly
- [ ] Login/Signup works
- [ ] API calls succeed

---

## 📋 URLs to Save

**Backend URL:** `https://____________________.onrender.com`  
**Frontend URL:** `https://____________________.onrender.com`  
**Database URL:** `postgresql://____________________`

---

**Full guide:** See `RENDER_DEPLOYMENT_GUIDE.md`

