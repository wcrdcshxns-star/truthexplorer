# 🇮🇳 Young India Writes

**India's national digital platform for original student poetry and short stories.**

> *"Every student has a voice. This platform gives it permanence."*

---

## 🗂️ Project Structure

```
young-india-writes/
├── index.html          ← Landing page (hero, featured, carousel, stories)
├── poems.html          ← All poems with filter (city · language · class)
├── stories.html        ← Short stories with genre filter
├── submit.html         ← 3-step submission form + plagiarism check
├── payment.html        ← Razorpay payment gateway
├── success.html        ← Post-payment success + confetti
├── about.html          ← Founder portfolio (Premanjan)
├── admin.html          ← Admin dashboard (review, approve, feature)
│
├── css/
│   └── styles.css      ← Complete stylesheet (dark mode, fonts, responsive)
│
├── js/
│   └── script.js       ← All JavaScript (modular, performant)
│
└── backend/
    ├── server.js        ← Node.js + Express API
    ├── package.json     ← Dependencies
    └── .env.template    ← Environment variables template
```

---

## ✨ Features

| Feature | Status |
|---|---|
| 🏠 Landing page (hero, stats counter, featured poems, carousel) | ✅ |
| 📜 Poems page with filter (city, language, class, search) | ✅ |
| 📚 Stories page with genre/length filters | ✅ |
| ✍️ 3-step submission form (author → content → payment) | ✅ |
| 🛡️ Anti-plagiarism check (Copyleaks API integration) | ✅ |
| 💳 Razorpay payment flow (₹49 → ₹57.82 with GST) | ✅ |
| 🎊 Success page with confetti animation | ✅ |
| 👤 Founder portfolio / About page (Premanjan) | ✅ |
| ⚙️ Admin dashboard (approve/reject/feature submissions) | ✅ |
| 🌙 Dark/Light mode toggle (persisted in localStorage) | ✅ |
| 🔤 3-font switcher (SF Pro / Times New Roman / Typewriter) | ✅ |
| 📱 Mobile-first responsive design + hamburger nav | ✅ |
| 🎠 Auto-playing carousel with touch swipe support | ✅ |
| 🖼️ Featured 3 poems with images + rank badges | ✅ |
| 🔐 Rate limiting + Razorpay signature verification | ✅ |
| 📧 Transactional email confirmation (Nodemailer) | ✅ |
| 🗄️ MongoDB schema (submissions, newsletter) | ✅ |
| 🚀 Production-ready backend (helmet, cors, sanitization) | ✅ |

---

## 🚀 Quick Start

### 1. Open in Browser (Frontend only)
Just open `index.html` in any browser. All pages work without the backend for demo purposes.

### 2. Full Stack Setup

**Prerequisites:** Node.js ≥ 18, MongoDB Atlas account, Razorpay account

```bash
# Install backend dependencies
cd backend
npm install

# Configure environment
cp .env.template .env
# Edit .env with your actual keys

# Start development server
npm run dev

# Start production server
npm start
```

---

## 🔑 API Keys You Need

### 1. Razorpay
1. Create account at [dashboard.razorpay.com](https://dashboard.razorpay.com)
2. Go to Settings → API Keys
3. Generate Test Mode keys (use `rzp_test_` for testing)
4. Add to `.env`:
   ```
   RAZORPAY_KEY_ID=rzp_test_XXXXXXXX
   RAZORPAY_KEY_SECRET=XXXXXXXXXXXXXXXXXXXXXXXXXX
   ```
5. In `payment.html`, replace `'rzp_test_REPLACE_WITH_YOUR_KEY'` with your Key ID

### 2. Copyleaks (Anti-Plagiarism)
1. Sign up at [app.copyleaks.com](https://app.copyleaks.com)
2. Generate an API key
3. Add to `.env`:
   ```
   COPYLEAKS_EMAIL=your@email.com
   COPYLEAKS_API_KEY=your-api-key
   ```
4. In `backend/server.js`, uncomment the Copyleaks API block and remove the demo simulation

### 3. MongoDB Atlas
1. Create cluster at [cloud.mongodb.com](https://cloud.mongodb.com)
2. Create database user and get connection string
3. Add to `.env`:
   ```
   MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/youngindiawrites
   ```

---

## 🌙 Dark Mode & Font Switching

These work automatically via localStorage — no configuration needed.

**Dark Mode:** Click the moon/sun icon in the nav  
**Fonts:** Click "Aa ▾" in the nav:
- `SF Pro Display` — Modern, clean (default)  
- `Times New Roman` — Classic literary feel  
- `Typewriter` — Courier Prime, nostalgic typewriter aesthetic

---

## 💳 Payment Flow

```
User fills form (3 steps)
    ↓
Plagiarism check passes (< 20% similarity)
    ↓
User agrees to originality declaration
    ↓
Frontend → POST /api/payment/create-order
    ↓
Razorpay checkout opens (UPI / Card / Net Banking)
    ↓
User pays ₹57.82
    ↓
Razorpay callback → Frontend
    ↓
Frontend → POST /api/payment/verify
    ↓
Backend verifies HMAC-SHA256 signature
    ↓
Submission activated → Email sent → success.html
```

---

## 🛡️ Anti-Plagiarism Flow

```
User writes content
    ↓
Clicks "Run Plagiarism Check"
    ↓
POST /api/plagiarism/check { text }
    ↓
Copyleaks API scans 60B+ pages
    ↓
score <= 20%  → PASS → User can proceed to payment
score > 20%   → FAIL → User must revise content
```

---

## ⚙️ Admin Dashboard

Access: `admin.html`

**In production, protect this route with:**
- HTTP Basic Auth (nginx)
- JWT token validation
- Or move to a separate subdomain

**Admin capabilities:**
- View all submissions with status filters
- Approve / Reject submissions
- Feature up to 3 poems (appear on homepage)
- View payment records
- Manage platform settings
- Plagiarism threshold control

**API calls use `x-admin-token` header:**
```javascript
fetch('/api/admin/submissions', {
  headers: { 'x-admin-token': process.env.ADMIN_SECRET }
})
```

---

## 🌐 Deployment

### Frontend (Vercel / Netlify)
```bash
# Just deploy the root directory
# All HTML/CSS/JS files are static
```

### Backend (Railway / Render / Heroku)
```bash
cd backend
# Set environment variables in dashboard
# Deploy from GitHub or CLI
```

### Nginx Configuration
```nginx
server {
    listen 443 ssl;
    server_name youngindiawr.in;

    location / {
        root /var/www/young-india-writes;
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## 📊 Scaling for March 21 Launch

The backend includes:
- Rate limiting (100 req/15min global, 5 req/hr for submissions)
- MongoDB indexes on frequently queried fields
- Text search index for poem/story search
- Graceful shutdown handler

For high traffic:
1. Enable MongoDB Atlas auto-scaling
2. Add Redis for session/rate-limit store
3. Use CloudFlare CDN for static assets
4. Enable Razorpay webhooks for payment confirmation fallback

---

## 📬 Contact

**Founder:** Premanjan  
**Email:** premanjan@youngindiawr.in  
**Platform:** [youngindiawr.in](https://youngindiawr.in)

---

*Made with ❤️ in India · Launching March 21, 2025 · International Poetry Day*
