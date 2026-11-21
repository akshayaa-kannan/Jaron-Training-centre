# 🔷 NayaQR — MongoDB Setup Guide
### Complete Beginner-Friendly Step-by-Step Instructions

---

## 📁 Project Folder Structure

```
NayaQR/
├── models/
│   └── QRCode.js          ← MongoDB schema (what each QR record looks like)
├── routes/
│   └── qrRoutes.js        ← All API endpoints (POST, GET, PUT, DELETE)
├── public/
│   └── index.html         ← Your frontend (NayaQR app)
├── server.js              ← Main Express server — entry point
├── package.json           ← Project dependencies list
├── .env.example           ← Template for your secret config
├── .env                   ← ⚠️ YOUR actual secrets (never push to GitHub)
└── .gitignore             ← Files git should ignore
```

---

## 🌐 MongoDB Atlas Setup (Step-by-Step)

### Step 1 — Create a Free Account
1. Go to **https://cloud.mongodb.com**
2. Click **"Try Free"** → Sign up with Google or email
3. Choose **Free Shared Cluster (M0)** → Click **Create**

### Step 2 — Create Your Database User
1. In the left sidebar → **Database Access**
2. Click **"Add New Database User"**
3. Choose **"Password"** authentication
4. Enter a username (e.g. `nayaqr_admin`) and a strong password
5. Set **Role** to `"Read and write to any database"`
6. Click **Add User** ✅

### Step 3 — Allow Network Access
1. In the left sidebar → **Network Access**
2. Click **"Add IP Address"**
3. Click **"Allow Access from Anywhere"** (IP: `0.0.0.0/0`) for development
4. Click **Confirm** ✅

> ⚠️ For production, restrict to your server's IP only.

### Step 4 — Get Your Connection String
1. Go to **Database** → Click **"Connect"** on your cluster
2. Choose **"Drivers"**
3. Select **Node.js** as the driver
4. Copy the connection string — it looks like:
   ```
   mongodb+srv://nayaqr_admin:<password>@cluster0.xxxxx.mongodb.net/
   ```
5. Replace `<password>` with your actual password

---

## ⚙️ Local Setup (Run on Your Computer)

### Step 1 — Install Node.js
Download from **https://nodejs.org** → Install the **LTS version**

Verify installation:
```bash
node --version    # Should show v18+ or higher
npm --version     # Should show 9+
```

### Step 2 — Download / Clone This Project
If you downloaded a ZIP, extract it. Or if using Git:
```bash
git clone https://github.com/yourname/NayaQR.git
cd NayaQR
```

### Step 3 — Install Dependencies
```bash
npm install
```
This installs: `express`, `mongoose`, `cors`, `dotenv`, `nodemon`

### Step 4 — Create Your .env File
Copy the example file:
```bash
# On Mac/Linux:
cp .env.example .env

# On Windows:
copy .env.example .env
```

Open `.env` and fill in your MongoDB connection string:
```env
MONGO_URI=mongodb+srv://nayaqr_admin:YourPassword@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
PORT=5000
```

> ⚠️ Replace `nayaqr_admin`, `YourPassword`, and `cluster0.xxxxx` with your actual values.

### Step 5 — Start the Server

**Development mode** (auto-restarts when you edit files):
```bash
npm run dev
```

**Production mode:**
```bash
npm start
```

You should see:
```
✅ MongoDB Atlas connected → NayaQR_DB
🚀 NayaQR server running at http://localhost:5000
📡 API base URL: http://localhost:5000/api
```

### Step 6 — Open the App
Open your browser and go to: **http://localhost:5000**

Your NayaQR app is now running with full MongoDB integration! 🎉

---

## 🔌 API Endpoints Reference

| Method | Endpoint | What It Does |
|--------|----------|-------------|
| `POST` | `/api/create` | Save a new QR code to MongoDB |
| `GET` | `/api/all` | Fetch all saved QR codes |
| `GET` | `/api/all?type=link` | Fetch only QR codes of a specific type |
| `GET` | `/api/single/:id` | Fetch one QR code by its ID |
| `PUT` | `/api/scan/:id` | Increment scan count by 1 |
| `DELETE` | `/api/delete/:id` | Delete a QR code permanently |
| `GET` | `/health` | Check if server is running |

---

## 📦 MongoDB Document Structure

Each QR code saved in the `qrcodes` collection looks like this:

```json
{
  "_id": "665f1a2b3c4d5e6f7a8b9c0d",
  "title": "My Company Website",
  "originalLink": "https://mycompany.com",
  "fileUrl": "",
  "qrCodeUrl": "",
  "type": "link",
  "scanCount": 42,
  "createdAt": "2025-06-01T10:30:00.000Z",
  "updatedAt": "2025-06-05T14:22:00.000Z"
}
```

### Field Descriptions

| Field | Type | Description |
|-------|------|-------------|
| `title` | String | Human-readable label for the QR code |
| `originalLink` | String | The URL or content encoded in the QR |
| `fileUrl` | String | Cloud URL of an uploaded file (if applicable) |
| `qrCodeUrl` | String | URL of the saved QR image (for cloud storage) |
| `type` | String | `link`, `image`, `video`, `pdf`, `text`, `file` |
| `scanCount` | Number | How many times this QR was scanned |
| `createdAt` | Date | Auto-set by MongoDB when document is created |
| `updatedAt` | Date | Auto-updated whenever document changes |

---

## 🧪 Testing Your API with Browser / Postman

### Test POST (Save a QR code)
```json
POST http://localhost:5000/api/create
Content-Type: application/json

{
  "title": "Google Homepage",
  "originalLink": "https://google.com",
  "fileUrl": "",
  "qrCodeUrl": "",
  "type": "link"
}
```

### Test GET (Fetch all QR codes)
```
GET http://localhost:5000/api/all
```

### Test PUT (Increment scan count)
```
PUT http://localhost:5000/api/scan/665f1a2b3c4d5e6f7a8b9c0d
```

### Test DELETE
```
DELETE http://localhost:5000/api/delete/665f1a2b3c4d5e6f7a8b9c0d
```

---

## 🗃️ MongoDB Atlas — View Your Data

1. Go to **https://cloud.mongodb.com**
2. Click your cluster → **"Browse Collections"**
3. Find **NayaQR_DB** → **qrcodes**
4. You'll see all your saved QR records here!

---

## 🚀 Deploy to Production (Render.com — Free)

1. Push your code to **GitHub** (make sure `.env` is in `.gitignore`!)
2. Go to **https://render.com** → Sign in with GitHub
3. Click **"New Web Service"** → Connect your repo
4. Settings:
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
5. Add **Environment Variables** in Render dashboard:
   - `MONGO_URI` = your Atlas connection string
   - `PORT` = 5000
6. Click **Deploy** ✅

---

## ❓ Common Issues & Fixes

| Problem | Fix |
|---------|-----|
| `MongoDB connection failed` | Check your `.env` MONGO_URI — password/username correct? |
| `ECONNREFUSED 5000` | Server not running — run `npm run dev` |
| `Cannot find module 'express'` | Run `npm install` first |
| History not loading in browser | Make sure server is running at localhost:5000 |
| `IP not whitelisted` | Add your IP in MongoDB Atlas → Network Access |

---

## 📋 .env File Example

```env
# MongoDB Atlas Connection (get from cloud.mongodb.com → Connect → Drivers)
MONGO_URI=mongodb+srv://username:password@cluster0.abc123.mongodb.net/?retryWrites=true&w=majority

# Server Port
PORT=5000
```

---

*Built with ❤️ using MongoDB Atlas · Node.js · Express.js · Vanilla JS*