# 🚀 LearnSign Deployment Guide

## Complete Step-by-Step Deployment on Render (Free)

---

## 📋 **Prerequisites**

- [x] GitHub account with your code pushed
- [x] MongoDB Atlas account with connection string
- [x] Render account (sign up at https://render.com - it's free!)

---

## 🎯 **Deployment Strategy**

We'll deploy 3 services on Render:

1. **Frontend + Express Server** (`index.js`) - Port 3000
2. **API Server** (`api.js`) - Port 4000  
3. **Python FastAPI** (`sign_recognition/`) - Port 8000

---

## 📝 **Step 1: Prepare Your Repository**

### 1.1 Make sure all changes are pushed to GitHub

```bash
cd /Users/amithkm/Desktop/hackathon
git add -A
git commit -m "Ready for deployment"
git push origin main
```

### 1.2 Verify your `.gitignore` excludes sensitive files

✅ Already done - your `.gitignore` is configured correctly!

---

## 🌐 **Step 2: Deploy API Server on Render**

### 2.1 Create API Service

1. Go to https://dashboard.render.com
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository: `amithkm9/final`
4. Configure the service:

   **Basic Settings:**
   - **Name:** `learnsign-api`
   - **Region:** `Oregon (US West)` or closest to you
   - **Branch:** `main`
   - **Root Directory:** Leave empty
   - **Runtime:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `node api.js`
   - **Instance Type:** `Free`

   **Environment Variables:** (Click "Advanced" → "Add Environment Variable")
   
   Add these one by one:
   
   | Key | Value |
   |-----|-------|
   | `NODE_ENV` | `production` |
   | `API_PORT` | `4000` |
   | `MONGODB_URI` | `your-mongodb-atlas-connection-string` |
   | `DB_NAME` | `learnsign` |

   **Important:** Replace `your-mongodb-atlas-connection-string` with your actual MongoDB Atlas URI like:
   ```
   mongodb+srv://username:password@cluster.mongodb.net/learnsign?retryWrites=true&w=majority
   ```

5. Click **"Create Web Service"**
6. Wait 3-5 minutes for deployment
7. **Copy the URL** (e.g., `https://learnsign-api.onrender.com`)

---

## 🖥️ **Step 3: Deploy Frontend on Render**

### 3.1 Create Frontend Service

1. Click **"New +"** → **"Web Service"** again
2. Select your repository: `amithkm9/final`
3. Configure the service:

   **Basic Settings:**
   - **Name:** `learnsign-frontend`
   - **Region:** `Oregon (US West)` (same as API)
   - **Branch:** `main`
   - **Root Directory:** Leave empty
   - **Runtime:** `Node`
   - **Build Command:** `npm install`
   - **Start Command:** `node index.js`
   - **Instance Type:** `Free`

   **Environment Variables:**
   
   | Key | Value |
   |-----|-------|
   | `NODE_ENV` | `production` |
   | `PORT` | `3000` |
   | `MONGODB_URI` | `your-mongodb-atlas-connection-string` (same as API) |
   | `DB_NAME` | `learnsign` |
   | `API_URL` | `https://learnsign-api.onrender.com` (from Step 2.7) |
   | `SESSION_SECRET` | `your-random-secret-key-here` |

   **Generate a random SESSION_SECRET:**
   ```bash
   # Run this in terminal to generate a random secret
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```

4. Click **"Create Web Service"**
5. Wait 3-5 minutes for deployment
6. **Copy the URL** (e.g., `https://learnsign-frontend.onrender.com`)

---

## 🐍 **Step 4: Deploy Python API on Render**

### 4.1 Create Python Service

1. Click **"New +"** → **"Web Service"** again
2. Select your repository: `amithkm9/final`
3. Configure the service:

   **Basic Settings:**
   - **Name:** `learnsign-python-api`
   - **Region:** `Oregon (US West)` (same region)
   - **Branch:** `main`
   - **Root Directory:** `sign_recognition`
   - **Runtime:** `Python 3`
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `uvicorn translate_api:app --host 0.0.0.0 --port 8000`
   - **Instance Type:** `Free`

   **Environment Variables:**
   
   | Key | Value |
   |-----|-------|
   | `PYTHON_VERSION` | `3.11.0` |
   | `PORT` | `8000` |

4. Click **"Create Web Service"**
5. Wait 5-10 minutes (Python takes longer)
6. **Copy the URL** (e.g., `https://learnsign-python-api.onrender.com`)

---

## 🔗 **Step 5: Connect All Services**

### 5.1 Update Frontend Environment Variables

Go back to your **Frontend service** (`learnsign-frontend`):

1. Click on the service
2. Go to **"Environment"** tab
3. Add/Update these variables:

   | Key | Value |
   |-----|-------|
   | `PYTHON_API_URL` | `https://learnsign-python-api.onrender.com` |
   | `TRANSLATE_API_URL` | `https://learnsign-python-api.onrender.com` |
   | `NUMBERS_LETTERS_API_URL` | `https://learnsign-python-api.onrender.com` |

4. Click **"Save Changes"**
5. Service will auto-redeploy

---

## ✅ **Step 6: Verify Deployment**

### 6.1 Test Each Service

**API Server:**
```
Visit: https://learnsign-api.onrender.com/health
Should see: "API is healthy"
```

**Python API:**
```
Visit: https://learnsign-python-api.onrender.com/
Should see: API documentation
```

**Frontend:**
```
Visit: https://learnsign-frontend.onrender.com
Should see: Your homepage
```

### 6.2 Test User Registration

1. Go to your frontend URL
2. Click **"Login"** or **"Sign Up"**
3. Create a new account
4. If successful → ✅ Deployment complete!

---

## 🐛 **Troubleshooting**

### Issue: "Registration Failed"

**Check:**
1. MongoDB Atlas connection string is correct
2. MongoDB Atlas allows connections from anywhere (IP: `0.0.0.0/0`)
3. API service logs show no errors

**Fix:**
```
Go to MongoDB Atlas → Network Access → Add IP Address → Allow Access from Anywhere
```

### Issue: "Service Unavailable"

**Reason:** Free tier services sleep after 15 minutes of inactivity

**Fix:** 
- First request will take 30-60 seconds to wake up
- Subsequent requests will be fast

### Issue: "Cannot connect to API"

**Check:**
1. `API_URL` in frontend environment variables
2. API service is running (check logs)
3. All services are in the same region

---

## 📊 **Monitor Your Services**

### View Logs

1. Go to Render Dashboard
2. Click on any service
3. Click **"Logs"** tab
4. See real-time logs

### Check Status

- **Green dot** = Running
- **Yellow dot** = Building
- **Red dot** = Failed

---

## 💰 **Cost Breakdown**

**Render Free Tier:**
- ✅ 3 Web Services: **FREE**
- ✅ 750 hours/month per service
- ✅ Auto-sleep after 15 min inactivity
- ✅ SSL certificates included

**MongoDB Atlas:**
- ✅ 512 MB storage: **FREE**
- ✅ Shared cluster

**Total Cost: $0/month** 🎉

---

## 🔄 **Continuous Deployment**

**Automatic Updates:**

Every time you push to GitHub `main` branch:
1. Render detects the change
2. Automatically rebuilds and redeploys
3. Your site updates in 3-5 minutes

**Manual Deploy:**
1. Go to service in Render
2. Click **"Manual Deploy"** → **"Deploy latest commit"**

---

## 🎯 **Your Deployed URLs**

After deployment, you'll have:

```
Frontend:    https://learnsign-frontend.onrender.com
API:         https://learnsign-api.onrender.com
Python API:  https://learnsign-python-api.onrender.com
```

**Share your frontend URL with users!** 🚀

---

## 📝 **Quick Checklist**

- [ ] All code pushed to GitHub
- [ ] MongoDB Atlas connection string ready
- [ ] Render account created
- [ ] API service deployed
- [ ] Frontend service deployed
- [ ] Python API service deployed
- [ ] All environment variables configured
- [ ] Services connected to each other
- [ ] Test registration/login works
- [ ] Test video watching
- [ ] Test quiz functionality

---

## 🆘 **Need Help?**

If you encounter issues:

1. **Check Render Logs:**
   - Go to service → Logs tab
   - Look for error messages

2. **Check MongoDB Atlas:**
   - Verify connection string
   - Check network access settings

3. **Test Locally First:**
   ```bash
   npm start
   node api.js
   ```

4. **Common Fixes:**
   - Redeploy service
   - Check environment variables
   - Verify all URLs are correct

---

## 🎉 **Congratulations!**

Your LearnSign platform is now live and accessible worldwide! 🌍

Share your URL and start teaching sign language! 🤟

