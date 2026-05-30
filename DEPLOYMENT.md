# 🌐 Deploying the Enterprise Dynamic Pricing Engine (DPE) Live

This guide provides step-by-step instructions on how to deploy both the **FastAPI Backend** and the **Next.js Frontend** live to the web.

```
┌─────────────────────────────────┐
│     Next.js Frontend (Vercel)   │
│  https://dpe-web.vercel.app     │
└──────────────┬──────────────────┘
               │ (HTTPS REST API)
               ▼
┌─────────────────────────────────┐
│      FastAPI Backend (Render)   │
│  https://dpe-api.onrender.com   │
└─────────────────────────────────┘
```

---

## 🐍 Part 1: Deploy the FastAPI Backend to Render

[Render](https://render.com) is a great hosting platform for FastAPI and Python web services. We will deploy the backend with the built-in **In-Memory Database** configuration, which automatically seeds dynamic pricing data.

### Step 1: Sign up & Connect GitHub
1. Go to [Render](https://render.com) and sign up/log in (using your GitHub account).
2. Click **New +** in the top right, then select **Web Service**.
3. Choose **Build and deploy from a Git repository** and connect your repository: `ManneediRamcharan/dynamic-pricing-engine`.

### Step 2: Configure the Web Service
Configure the Render settings as follows:
* **Name**: `dynamic-pricing-backend` (or any name you prefer)
* **Region**: Choose the region closest to you
* **Branch**: `main`
* **Root Directory**: `backend` *(CRITICAL: This ensures Render only deploys the backend subfolder)*
* **Runtime**: `Python 3`
* **Build Command**: `pip install -r requirements.txt`
* **Start Command**: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`

### Step 3: Add Environment Variables
Click on the **Environment** tab on Render and add the following variables:
1. `USE_IN_MEMORY_DB` = `true` *(Enables auto-seeding mock database)*
2. `ENVIRONMENT` = `production`
3. `CORS_ORIGINS` = `*` *(Allows the Vercel frontend to query the API. You can lock this down to your Vercel URL later)*

### Step 4: Deploy
Click **Create Web Service**. Render will build and launch the FastAPI server.
* Once the logs show `Application startup complete.`, copy the service URL (e.g. `https://dynamic-pricing-backend.onrender.com`).
* Verify it is live by opening `https://<your-render-url>/api/health` in your browser. It should return `{"status":"operational", ...}`.

---

## ⚛️ Part 2: Deploy the Next.js Frontend to Vercel

[Vercel](https://vercel.com) is the native hosting platform for Next.js and is free, fast, and supports instant deployments.

### Step 1: Connect to Vercel
1. Go to [Vercel](https://vercel.com) and log in with your GitHub account.
2. Click **Add New...** -> **Project**.
3. Select the repository: `dynamic-pricing-engine`.

### Step 2: Configure Project Settings
In the configuration screen, make sure to set:
* **Framework Preset**: `Next.js`
* **Root Directory**: Click *Edit* and select the `frontend` folder *(CRITICAL: This ensures Vercel only deploys the Next.js frontend)*

### Step 3: Add Environment Variables
Expand the **Environment Variables** section and add:
* **Key**: `NEXT_PUBLIC_API_URL`
* **Value**: `https://<your-render-url>` *(The live URL of your Render backend from Part 1, without a trailing slash)*

### Step 4: Deploy
Click **Deploy**. Vercel will install dependencies, build the Next.js application, and host it live.
* Once completed, Vercel will give you a live URL (e.g., `https://dynamic-pricing-engine-frontend.vercel.app`).

---

## 🧪 Part 3: Verify the Live Deployment

1. Open your live Vercel frontend URL.
2. Check the **Command Center (Dashboard)**:
   - Verify that the KPIs (Active Products, Approvals Pending, etc.) load with data.
   - Verify that the Margin Trend and Live feed are loading and updating.
3. Test **AI Approvals**:
   - Navigate to **Inventory** -> click on **RL Decisions**.
   - Approve or reject a recommended price and verify it updates.
4. Check **Demand Forecasting**:
   - Go to **Forecast** page and select products from the dropdown. Verify that Prophet + XGBoost forecasting charts render properly.
