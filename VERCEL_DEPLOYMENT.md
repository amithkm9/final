# 🚀 Deploy LearnSign on Vercel (Simplest Method)

## ⚡ Quick Deployment on Vercel

**Note:** Vercel is best for the frontend. We'll use Render for the backend APIs (free tier).

---

## 📋 **What You'll Deploy Where**

- **Vercel** → Frontend (`index.js`) - Super fast, global CDN
- **Render** → Backend APIs (`api.js` + Python) - Free, always-on

---

## 🎯 **Step-by-Step Instructions**

### **Part 1: Deploy Backend on Render (5 minutes)**

#### 1.1 Deploy API Server

1. Go to https://dashboard.render.com (sign up if needed)
2. Click **"New +"** → **"Web Service"**
3. Connect GitHub: `amithkm9/final`
4. Settings:
   - **Name:** `learnsign-api`
   - **Build:** `npm install`
   - **Start:** `node api.js`
   - **Free tier**

5. **Environment Variables:**
   ```
   NODE_ENV=production
   API_PORT=4000
   MONGODB_URI=your-mongodb-atlas-uri
   DB_NAME=learnsign
   ```

6. Click **"Create"**
7. **Copy the URL:** `https://learnsign-api.onrender.com`

#### 1.2 Deploy Python API

1. Click **"New +"** → **"Web Service"**
2. Connect GitHub: `amithkm9/final`
3. Settings:
   - **Name:** `learnsign-python`
   - **Root Directory:** `sign_recognition`
   - **Build:** `pip install -r requirements.txt`
   - **Start:** `uvicorn translate_api:app --host 0.0.0.0 --port 8000`
   - **Free tier**

4. Click **"Create"**
5. **Copy the URL:** `https://learnsign-python.onrender.com`

---

### **Part 2: Deploy Frontend on Vercel (2 minutes)**

#### 2.1 Connect to Vercel

1. Go to https://vercel.com (sign up with GitHub)
2. Click **"Add New"** → **"Project"**
3. Import `amithkm9/final` repository
4. Click **"Import"**

#### 2.2 Configure Project

**Framework Preset:** Other

**Build Settings:**
- **Build Command:** `npm install`
- **Output Directory:** (leave empty)
- **Install Command:** `npm install`

**Root Directory:** (leave empty)

#### 2.3 Add Environment Variables

Click **"Environment Variables"** and add:

```
NODE_ENV=production
PORT=3000
MONGODB_URI=your-mongodb-atlas-connection-string
DB_NAME=learnsign
API_URL=https://learnsign-api.onrender.com
PYTHON_API_URL=https://learnsign-python.onrender.com
TRANSLATE_API_URL=https://learnsign-python.onrender.com
NUMBERS_LETTERS_API_URL=https://learnsign-python.onrender.com
SESSION_SECRET=your-random-secret-key
```

**Generate SESSION_SECRET:**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

#### 2.4 Deploy

1. Click **"Deploy"**
2. Wait 2-3 minutes
3. **Your site is live!** 🎉

**URL:** `https://your-project-name.vercel.app`

---

## ✅ **Verify Everything Works**

### Test Checklist:

1. **Visit your Vercel URL**
   - Homepage loads ✅
   
2. **Test Registration**
   - Create account ✅
   - Login works ✅

3. **Test Features**
   - Watch videos ✅
   - Take quiz ✅
   - Dashboard shows data ✅

---

## 🔧 **If Login/Registration Fails**

### Check MongoDB Atlas:

1. Go to MongoDB Atlas → **Network Access**
2. Click **"Add IP Address"**
3. Select **"Allow Access from Anywhere"** (`0.0.0.0/0`)
4. Click **"Confirm"**

### Check Vercel Environment Variables:

1. Go to Vercel Dashboard → Your Project
2. Click **"Settings"** → **"Environment Variables"**
3. Verify all variables are set correctly
4. Click **"Redeploy"** if you made changes

### Check Render Services:

1. Go to Render Dashboard
2. Check both services are **"Live"** (green dot)
3. Click on API service → **"Logs"** → Check for errors

---

## 🔄 **Auto-Deploy on Git Push**

**Vercel:**
- Automatically deploys when you push to `main` branch
- Takes 2-3 minutes

**Render:**
- Automatically deploys when you push to `main` branch  
- Takes 3-5 minutes

**To deploy changes:**
```bash
git add -A
git commit -m "Your changes"
git push origin main
```

---

## 💰 **Cost**

- **Vercel:** FREE (Hobby plan)
- **Render:** FREE (2 services)
- **MongoDB Atlas:** FREE (512MB)

**Total: $0/month** 🎉

---

## 🎯 **Your Live URLs**

After deployment:

```
🌐 Frontend:   https://your-project.vercel.app
🔧 API:        https://learnsign-api.onrender.com
🐍 Python API: https://learnsign-python.onrender.com
```

**Share your Vercel URL with users!**

---

## 🆘 **Common Issues & Fixes**

### Issue: "ECONNREFUSED 127.0.0.1:4000"

**Fix:** Make sure `API_URL` environment variable is set in Vercel to your Render API URL

### Issue: "Service Unavailable" (Render)

**Reason:** Free tier sleeps after 15 min inactivity

**Fix:** First request takes 30s to wake up (normal behavior)

### Issue: "MongoDB connection failed"

**Fix:** 
1. Check `MONGODB_URI` is correct
2. Verify MongoDB Atlas network access allows all IPs
3. Make sure database name is `learnsign`

---

## 🎉 **Done!**

Your LearnSign platform is now deployed on:
- ⚡ **Vercel** (fast global CDN)
- 🔧 **Render** (reliable backend)
- 💾 **MongoDB Atlas** (cloud database)

All for **FREE**! 🚀

