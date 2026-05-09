# Deploy to Railway (Free Hosting)

## What you need
- A free GitHub account → https://github.com
- A free Railway account → https://railway.app

---

## Step 1 — Push code to GitHub

1. Go to https://github.com and sign up (free)
2. Click the **+** icon → **New repository**
3. Name it `attendance-app`, set to **Private**, click **Create repository**
4. Download your code from Replit:
   - Click the 3 dots (⋯) menu in Replit
   - Click **Download as ZIP**
   - Extract the ZIP on your computer
5. Install Git on your computer → https://git-scm.com/downloads
6. Open a terminal/command prompt in the extracted folder and run:
   ```
   git init
   git add .
   git commit -m "initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/attendance-app.git
   git push -u origin main
   ```
   (Replace YOUR_USERNAME with your GitHub username)

---

## Step 2 — Deploy to Railway

1. Go to https://railway.app and sign up with GitHub (free)
2. Click **New Project** → **Deploy from GitHub repo**
3. Select your `attendance-app` repository
4. Railway will detect it automatically — click **Deploy**

---

## Step 3 — Add PostgreSQL Database

1. In your Railway project, click **New** → **Database** → **Add PostgreSQL**
2. Click on the PostgreSQL service → go to **Variables** tab
3. Copy the `DATABASE_URL` value

---

## Step 4 — Set Environment Variables

Click on your app service → **Variables** tab → add these:

| Variable | Value |
|----------|-------|
| `DATABASE_URL` | (paste from PostgreSQL above) |
| `SESSION_SECRET` | any long random text like `myapp-super-secret-key-2024-xyz` |
| `NODE_ENV` | `production` |
| `PORT` | `8080` |

---

## Step 5 — Set Up the Database

1. In Railway, click on your app service
2. Click **Settings** → scroll to **Deploy** → find the terminal/shell option
3. Or use Railway CLI: `railway run pnpm --filter @workspace/db run push`
4. Then run: `railway run pnpm --filter @workspace/db run seed`

This creates all the tables and sets up the admin account.

---

## Step 6 — Get Your Live URL

1. Click on your app service in Railway
2. Go to **Settings** → **Networking** → **Generate Domain**
3. You'll get a URL like `https://attendance-app-production.up.railway.app`
4. Your app is LIVE! Login with `ADMIN-001` / `admin123`

---

## Step 7 — Connect Your A2Hosting Domain (Free!)

Railway lets you add a custom domain for FREE.

### In Railway:
1. Go to your app service → **Settings** → **Networking**
2. Click **Custom Domain** → type your domain (e.g. `attendance.yourdomain.com`)
3. Railway will show you a DNS record like:
   ```
   Type: CNAME
   Name: attendance
   Value: something.railway.app
   ```

### In A2Hosting cPanel:
1. Log in to A2Hosting cPanel
2. Find **Zone Editor** or **DNS Zone Editor**
3. Click **Manage** next to your domain
4. Click **Add Record**:
   - **Type**: CNAME
   - **Name**: attendance (or @ for root domain)
   - **Record**: (paste the railway.app value)
5. Click **Add Record** → Save

Wait 10-30 minutes → your domain is now pointing to Railway!

---

## Login Credentials

- **Admin**: `ADMIN-001` / `admin123`
- **Members**: User ID shown in Members page / password shown when creating member
