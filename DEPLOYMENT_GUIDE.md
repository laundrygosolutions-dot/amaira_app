# Amaira – Deployment Guide for Beginners

This guide will help you move your Amaira web app from Replit to the internet using **Render** (free hosting) and **Supabase** (free database). Follow each step carefully.

---

## Step 1: Download the Project ZIP

1. On Replit, open your project.
2. In the left file panel, click the **three-dot menu (⋯)** at the top.
3. Click **"Download as zip"**.
4. Save the ZIP file to your computer (e.g., your Desktop).
5. **Unzip** the file – right-click it and choose "Extract All" (Windows) or double-click it (Mac).

---

## Step 2: Upload Code to GitHub

You need a GitHub account to host your code. GitHub is free.

### 2a. Create a GitHub account (if you don't have one)
1. Go to [https://github.com](https://github.com) and sign up for free.

### 2b. Create a new repository
1. After logging in, click the **green "New"** button (or go to [https://github.com/new](https://github.com/new)).
2. Name your repository (e.g., `amaira-app`).
3. Keep it **Private** (recommended) or Public.
4. Click **"Create repository"**.

### 2c. Upload your files
1. On your new repository page, click **"uploading an existing file"**.
2. Drag and drop **all the files from your unzipped project folder** into the upload area.
3. Wait for them to upload, then click **"Commit changes"**.

> ⚠️ **Important:** Do NOT upload the `node_modules` folder or the `dist` folder. These are generated automatically.

---

## Step 3: Create Your Supabase Database

Supabase gives you a free PostgreSQL database.

1. Go to [https://supabase.com](https://supabase.com) and sign up for free.
2. Click **"New Project"**.
3. Choose a name for your project (e.g., `amaira-db`).
4. Set a strong **database password** – write this down somewhere safe!
5. Choose a region close to you (e.g., US East, Europe West).
6. Click **"Create new project"** and wait about 2 minutes for it to set up.

### 3a. Get your Database URL
1. In your Supabase project, click **"Project Settings"** (gear icon on the left sidebar).
2. Click **"Database"**.
3. Scroll down to **"Connection string"** and select the **"URI"** tab.
4. Copy the connection string. It looks like:
   ```
   postgresql://postgres:[YOUR-PASSWORD]@db.xxxxxxxxxxxx.supabase.co:5432/postgres
   ```
5. Replace `[YOUR-PASSWORD]` with the password you set in Step 3.

### 3b. Set up the database tables
Your app uses Drizzle ORM to manage database tables. After deploying (Step 4), you'll run a migration command. Skip this for now.

---

## Step 4: Deploy on Render

Render is a free hosting service for web apps.

1. Go to [https://render.com](https://render.com) and sign up for free (you can sign up with your GitHub account).
2. Click **"New +"** then select **"Web Service"**.
3. Click **"Connect account"** to connect your GitHub account.
4. Find your `amaira-app` repository and click **"Connect"**.

### 4a. Configure the service
Fill in the settings as follows:

| Setting | Value |
|---|---|
| **Name** | `amaira-app` (or any name you like) |
| **Region** | Choose the closest to you |
| **Branch** | `main` |
| **Runtime** | `Node` |
| **Build Command** | `npm install && npm run build` |
| **Start Command** | `npm start` |
| **Plan** | Free |

5. Click **"Create Web Service"**.
6. Render will start building your app – this takes 2–5 minutes.

---

## Step 5: Add Your Environment Variables

Your app needs secret settings to connect to the database.

1. In your Render web service dashboard, click **"Environment"** in the left sidebar.
2. Click **"Add Environment Variable"** and add each of the following:

| Key | Value |
|---|---|
| `DATABASE_URL` | Paste your Supabase connection string from Step 3a |
| `NODE_ENV` | `production` |
| `PORT` | `3000` |

3. Click **"Save Changes"**. Render will automatically restart your app.

### 5a. Set up database tables
After your app is live, you need to create the tables in Supabase:

1. In your Render dashboard, go to your web service.
2. Click **"Shell"** in the top navigation.
3. Type this command and press Enter:
   ```
   npm run db:push
   ```
4. This creates all the required tables in your Supabase database.

---

## Step 6: Connect Your Custom Domain

Once your app is running, you can connect a custom domain (e.g., `www.amaira.com`).

### 6a. In Render
1. Go to your Render web service.
2. Click **"Custom Domains"** in the left sidebar.
3. Click **"Add Custom Domain"**.
4. Type your domain name (e.g., `www.amaira.com`) and click **"Save"**.
5. Render will show you a **CNAME value** – copy it (it looks like `amaira-app.onrender.com`).

### 6b. In your Domain Provider (e.g., GoDaddy, Namecheap)
1. Log in to where you bought your domain.
2. Go to **DNS Settings** for your domain.
3. Add a new **CNAME record**:
   - **Name/Host**: `www`
   - **Value/Points to**: the CNAME value from Render
   - **TTL**: Leave as default
4. Save the record.
5. Wait 15–60 minutes for the domain to start working.

> 💡 **Tip:** You can also set up a free SSL certificate (HTTPS) – Render does this automatically once the domain is connected.

---

## Summary of What You'll Have

- ✅ Your Amaira app running live on the internet
- ✅ A Supabase PostgreSQL database storing your data permanently
- ✅ Automatic HTTPS (secure connection)
- ✅ Your own custom domain (optional)

---

## Need Help?

- **Render Docs:** [https://render.com/docs](https://render.com/docs)
- **Supabase Docs:** [https://supabase.com/docs](https://supabase.com/docs)
- **Drizzle ORM Docs:** [https://orm.drizzle.team](https://orm.drizzle.team)

---

## Local Development (Running on Your Computer)

If you want to run the app on your own computer:

1. Install **Node.js** from [https://nodejs.org](https://nodejs.org) (choose the LTS version).
2. Open a terminal (Command Prompt on Windows, Terminal on Mac).
3. Navigate to your project folder:
   ```
   cd path/to/your/amaira-app
   ```
4. Copy `.env.example` to `.env` and fill in your Supabase DATABASE_URL:
   ```
   cp .env.example .env
   ```
5. Run these commands:
   ```
   npm install
   npm run build
   npm start
   ```
6. Open your browser and go to `http://localhost:3000`
