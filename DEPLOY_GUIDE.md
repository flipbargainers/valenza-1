# URBAN EDGES — Full Stack Setup & Deployment Guide

## Your Two Files

| File | Purpose |
|------|---------|
| `index.html` | Customer-facing storefront |
| `admin.html` | Your private admin dashboard |

---

## 🚀 WHERE TO PUBLISH (Recommended Options)

### Option 1: Shopify (Best for serious e-commerce — RECOMMENDED)

The easiest path if you want real payments, inventory management, and a proper backend.

- **Cost**: From $39/month
- **What to do**: Use your HTML design as a Shopify theme (custom Liquid templates), or use it as a reference for a Shopify theme builder like Replo or PageFly
- **Dashboard**: Shopify Admin at `yourstore.myshopify.com/admin`
- **Pros**: Real checkout, Stripe/PayPal built-in, inventory, analytics, mobile app
- **URL**: https://shopify.com

---

### Option 2: Netlify (Free hosting, instant deploy)

Perfect for launching the front-end now while you build out the backend.

1. Go to https://netlify.com — sign up free
2. Drag and drop your folder (containing `index.html` and `admin.html`) onto the Netlify dashboard
3. Done! You get a live URL in 30 seconds
4. **Your store**: `yoursite.netlify.app`
5. **Your admin**: `yoursite.netlify.app/admin.html`
6. Connect a custom domain (e.g. `urbanedges.com`) for ~$15/year

**To protect admin.html on Netlify**: Add a `_headers` file:
```
/admin.html
  Basic-Auth: admin:yourpassword
```

---

### Option 3: GitHub Pages (Free, version controlled)

1. Create a GitHub account at https://github.com
2. Create a new repository called `urban-edges`
3. Upload both HTML files
4. Go to Settings → Pages → Deploy from main branch
5. Live at: `yourusername.github.io/urban-edges`

---

### Option 4: Vercel (Free, fastest)

1. Go to https://vercel.com
2. Import from GitHub (after doing Option 3 above)
3. Auto-deploys every time you push changes
4. Free custom domain support

---

### Option 5: Your own domain + hosting (Most control)

Buy hosting from:
- **Hostinger** (~$3/month) — very easy for beginners
- **SiteGround** (~$4/month) — more reliable
- **DigitalOcean** (~$6/month) — for developers

Then upload files via cPanel File Manager or FTP.

---

## 🔒 Securing Your Admin Dashboard

The `admin.html` file should NEVER be public. Here's how to secure it:

### On Netlify (easiest)
Create a file called `_headers` in your folder:
```
/admin.html
  X-Frame-Options: DENY
```
Then add password protection in Netlify Dashboard → Site Settings → Access control → Password protection.

### On any host with a backend (Node.js/PHP)
Move admin to a protected route that requires login:
```
/admin → requires session token
/store → public
```

---

## 🏗️ MAKING THIS A REAL FULL-STACK APP

To make the admin dashboard actually control the storefront (real database, real products), you need a backend. Here are your options:

### Option A: Supabase (FREE database, easiest)

1. Go to https://supabase.com — free tier available
2. Create a project and a `products` table with columns:
   - `id`, `name`, `category`, `price`, `original_price`, `images`, `stock`, `status`, `collection`
3. Use their JavaScript SDK in both HTML files:

```javascript
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('YOUR_URL', 'YOUR_ANON_KEY')

// In storefront — load products:
const { data: products } = await supabase.from('products').select('*')

// In admin — save product:
await supabase.from('products').insert({ name: 'Air Max', price: 189, ... })
```

4. Images → store in Supabase Storage (like S3, free 1GB)

### Option B: Firebase (Google's free backend)

- https://firebase.google.com
- Free Spark plan: 1GB storage, 50K reads/day
- Firestore for database, Firebase Storage for images

### Option C: PocketBase (Self-hosted, totally free)

- Download from https://pocketbase.io
- Single binary, runs on any server
- Has built-in admin UI at `/pocketbase/_/`

---

## 💳 ADDING REAL PAYMENTS

### Stripe (Recommended)
```javascript
// In checkout page, add Stripe.js
const stripe = Stripe('YOUR_PUBLISHABLE_KEY');
const { paymentMethod } = await stripe.createPaymentMethod({...})
// Send to your backend to charge
```
- Free to integrate, 2.9% + $0.30 per transaction
- Dashboard at https://dashboard.stripe.com

### PayPal Buttons
```html
<div id="paypal-button-container"></div>
<script src="https://www.paypal.com/sdk/js?client-id=YOUR_ID"></script>
<script>paypal.Buttons().render('#paypal-button-container');</script>
```

---

## 📦 RECOMMENDED STACK FOR A PRODUCTION STORE

| Layer | Tool | Cost |
|-------|------|------|
| Frontend Hosting | Netlify or Vercel | Free |
| Database | Supabase | Free up to 500MB |
| Image Storage | Supabase Storage | Free 1GB |
| Payments | Stripe | 2.9% per sale |
| Email | Resend.com | Free 3K/month |
| Domain | Namecheap | ~$12/year |
| **Total fixed cost** | | **~$12/year** |

---

## 🗂️ FILE STRUCTURE FOR A REAL PROJECT

```
urban-edges/
├── index.html          ← Your storefront
├── admin.html          ← Your dashboard  
├── _headers            ← Netlify security headers
├── assets/
│   ├── products/       ← Product images
│   └── hero/           ← Hero images
└── api/
    ├── products.js     ← Serverless functions (Netlify/Vercel)
    ├── orders.js
    └── checkout.js
```

---

## ⚡ QUICKEST WAY TO GO LIVE TODAY

1. Copy `index.html` and `admin.html` into a folder
2. Go to https://netlify.com/drop
3. Drag the folder onto the page
4. Share your live URL

That's it. You'll have a live store in under 2 minutes.
