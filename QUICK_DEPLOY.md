# 🚀 QUICK START: GitHub & Vercel Deployment

## Your Next Steps (In Order)

### 1️⃣ CREATE GITHUB REPO (2 minutes)
```
👉 Go to: https://github.com/new
- Name: cakesman-bakery
- Public (important!)
- Create repository
```

**Copy your repo URL:**
```
https://github.com/YOUR_USERNAME/cakesman-bakery.git
```

---

### 2️⃣ PUSH TO GITHUB (1 minute)

Replace `YOUR_USERNAME` in the command below, then run:

```powershell
cd d:\Cakesman-Bakery
git remote add origin https://github.com/IshaanIshu13/cakesman-bakery.git
git branch -M main
git push -u origin main
```

✅ Your code is now on GitHub!

---

### 3️⃣ DEPLOY BACKEND (5 minutes) - Choose ONE option:

#### Option A: RAILWAY (Easier) ⭐
```
1. Go to: https://railway.app
2. Sign up with GitHub
3. New Project → Deploy from GitHub → Select cakesman-bakery
4. Set Root Directory: backend
5. Add Variables:
   - MONGODB_URI: [your MongoDB connection string]
   - JWT_SECRET: [any random string]
   - PORT: 5001
6. Deploy!
```

**Your Backend URL will be:** `https://your-project.railway.app`

---

#### Option B: RENDER.COM
```
1. Go to: https://render.com
2. New Web Service → GitHub
3. Select cakesman-bakery
4. Root Directory: backend
5. Add same variables as above
6. Deploy!
```

---

### 4️⃣ CONFIGURE ENVIRONMENT VARIABLES

**Find your MongoDB connection string:**
- Go to https://cloud.mongodb.com
- Log in
- Select your cluster
- Click "Connect"
- Copy connection string
- Replace `<username>` and `<password>` with actual values

**Copy and save these:**
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/bakery?retryWrites=true&w=majority
JWT_SECRET=super_secret_key_12345
```

---

### 5️⃣ UPDATE FRONTEND CODE (2 minutes)

**File: `frontend/src/utils/api.js`**

Find this:
```javascript
const API_BASE_URL = "http://localhost:5001/api";
```

Replace with:
```javascript
const API_BASE_URL = process.env.REACT_APP_API_URL || "http://localhost:5001/api";
```

---

**File: `frontend/src/context/SocketContext.jsx`**

Find this:
```javascript
const socket = io("http://localhost:5001");
```

Replace with:
```javascript
const backendURL = process.env.REACT_APP_API_URL 
  ? process.env.REACT_APP_API_URL.replace("/api", "")
  : "http://localhost:5001";
const socket = io(backendURL);
```

---

**File: `backend/server.js`**

Find this:
```javascript
app.use(cors());
```

Replace with:
```javascript
const allowedOrigins = [
  "http://localhost:3000",
  "https://cakesman-bakery.vercel.app"
];

app.use(cors({
  origin: allowedOrigins,
  credentials: true,
  methods: ["GET", "POST", "PUT", "PATCH", "DELETE"],
  allowedHeaders: ["Content-Type", "Authorization"]
}));
```

---

### 6️⃣ PUSH UPDATED CODE

```powershell
cd d:\Cakesman-Bakery
git add .
git commit -m "Update API URLs for production"
git push origin main
```

---

### 7️⃣ DEPLOY FRONTEND TO VERCEL (3 minutes)

```
1. Go to: https://vercel.com
2. Click "New Project"
3. "Import Git Repository"
4. Select: cakesman-bakery
5. Framework: React
6. Root Directory: frontend
7. Build Command: npm run build
8. Click "Deploy"
```

**Once deployed, Vercel will give you a URL like:**
```
https://cakesman-bakery.vercel.app
```

---

### 8️⃣ SET ENVIRONMENT VARIABLES ON VERCEL

In Vercel Dashboard → Your Project → Settings → Environment Variables

Add:
```
REACT_APP_API_URL=https://your-railway-backend.railway.app
```

(Replace with your actual Railway/Render backend URL)

---

### 9️⃣ REDEPLOY FRONTEND

After adding env vars, go back to Deployments and click "Redeploy" on the latest deployment.

---

## ✅ DONE!

Your website is now live at:
```
🌐 https://cakesman-bakery.vercel.app
```

---

## 📍 IMPORTANT URLs TO SAVE

```
GitHub Repo: https://github.com/YOUR_USERNAME/cakesman-bakery
Vercel App: https://cakesman-bakery.vercel.app
Vercel Dashboard: https://vercel.com/dashboard
Railway Dashboard: https://railway.app
MongoDB: https://cloud.mongodb.com
```

---

## 🧪 TEST YOUR SITE

Visit: https://cakesman-bakery.vercel.app

Test these features:
- ✅ Homepage loads
- ✅ Browse products
- ✅ Add to cart
- ✅ Login/Signup
- ✅ Admin dashboard
- ✅ View orders
- ✅ View customers

---

## ⚡ QUICK TIPS

**If you get an error:**

1. **"Cannot fetch from API"**
   - Check Railway/Render is running
   - Verify backend URL in Vercel env vars

2. **"502 Bad Gateway"**
   - Backend might be cold-starting
   - Wait 30 seconds and refresh

3. **"Build failed"**
   - Check Vercel build logs
   - Make sure root directory is `frontend`

4. **"Socket connection failed"**
   - Remove `/api` from backend URL in env var
   - Should be: `https://your-backend.railway.app`

---

## 📞 Need Help?

**For each service:**
- Vercel Issues: https://vercel.com/docs
- Railway Issues: https://railway.app/docs
- GitHub Help: https://docs.github.com
- MongoDB: https://docs.mongodb.com/atlas

---

**Ready? Start with Step 1! 🚀**
