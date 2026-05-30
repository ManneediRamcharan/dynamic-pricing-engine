# 🌐 Deploying the Enterprise Dynamic Pricing Engine (DPE) Live

This guide provides step-by-step instructions on how to deploy both the **FastAPI Backend** and the **Next.js Frontend** live to the web.

```
┌──────────────────────────────────────────────┐
│          Next.js Frontend (Vercel)           │
│  🔗 https://frontend-dynamic96.vercel.app     │
└──────────────┬───────────────────────────────┘
               │ (HTTPS REST API)
               ▼
┌──────────────────────────────────────────────┐
│           FastAPI Backend (Render)           │
│  🔗 https://dynamic-pricing-backend.onrender.com
└──────────────────────────────────────────────┘
```

---

## 🐍 Part 1: Deploy the FastAPI Backend to Render (FREE)

Render supports blueprint templates. We have already committed and pushed a `render.yaml` file to your GitHub repository, meaning you can deploy the backend with **1-click**.

### 1-Click Blueprint Deploy
1. Click the link below to deploy the backend service automatically:
   👉 **[Deploy to Render](https://render.com/deploy?repo=https://github.com/ManneediRamcharan/dynamic-pricing-engine)**
2. Log in with your GitHub account if prompted.
3. Review the parameters (Service Name: `dynamic-pricing-backend`, environment variables are already prefilled: `USE_IN_MEMORY_DB=true`, `ENVIRONMENT=production`, `CORS_ORIGINS=*`).
4. Click **Apply**.
5. Wait for the deploy to complete. Once the log says `Application startup complete.`, copy your live backend URL (e.g. `https://dynamic-pricing-backend-xxxx.onrender.com` or `https://dynamic-pricing-backend.onrender.com`).

---

## ⚛️ Part 2: Connect the Frontend (Vercel) to the Backend

We have already successfully built and deployed the Next.js frontend to Vercel at:
👉 **[https://frontend-dynamic96.vercel.app](https://frontend-dynamic96.vercel.app)**

Now, you just need to update the environment variable on Vercel to point it to your live backend.

### Step 1: Add Backend URL to Vercel Environment Variables
1. Go to your **[Vercel Dashboard](https://vercel.com/dashboard)**.
2. Click on the project **`frontend`** (or go directly to [Project Settings](https://vercel.com/dynamic96/frontend/settings)).
3. Go to the **Settings** tab -> **Environment Variables** (on the left menu).
4. Add a new variable:
   - **Key**: `NEXT_PUBLIC_API_URL`
   - **Value**: `https://<your-render-url>` *(Paste your live Render backend URL from Part 1, e.g. `https://dynamic-pricing-backend.onrender.com` without a trailing slash)*
   - Make sure **Production**, **Preview**, and **Development** are all checked.
5. Click **Save**.

### Step 2: Redeploy to apply changes
1. Go to the **Deployments** tab on Vercel.
2. Find the latest deployment, click the **three dots** on the right, and select **Redeploy**.
3. Once completed, your Next.js app will now communicate directly with your live Render backend!

---

## 🧪 Part 3: Verify the Live Deployment

1. Open your live Vercel frontend URL: **[https://frontend-dynamic96.vercel.app](https://frontend-dynamic96.vercel.app)**.
2. Check the **Command Center (Dashboard)**:
   - Verify that the KPIs (Active Products, Approvals Pending, etc.) load with data.
   - Verify that the Margin Trend and Live feed are loading and updating.
3. Test **AI Approvals**:
   - Navigate to **Inventory** -> click on **RL Decisions**.
   - Approve or reject a recommended price and verify it updates.
4. Check **Demand Forecasting**:
   - Go to **Forecast** page and select products from the dropdown. Verify that Prophet + XGBoost forecasting charts render properly.
