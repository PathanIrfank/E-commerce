# Role-Based E-Commerce Platform

A production-ready, full-stack Role-Based Access Control (RBAC) E-Commerce Web Application built for the **Full Stack Developer Internship** task.

---

## 🚀 Live Demo & Repository Links

- **Live Frontend (Vercel)**: `https://your-app.vercel.app` *(Replace with your live Vercel URL upon deploy)*
- **Live Backend (Render)**: `https://your-backend.onrender.com` *(Replace with your live Render URL upon deploy)*
- **GitHub Repository**: `https://github.com/your-username/ecommerce-task`

---

## 🛠️ Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 19, Vite, Tailwind CSS v4, React Router DOM v7, Lucide Icons, Axios |
| **Backend** | Node.js, Express.js (REST API), JWT, Bcrypt.js, Multer |
| **Database** | MongoDB & Mongoose (Atlas in production / auto-in-memory fallback locally) |
| **Payments** | Razorpay (Test Mode) with backend HMAC-SHA256 signature verification |
| **Image Uploads** | Cloudinary direct multipart upload via Multer (with disk fallback for dev) |
| **Deployment** | Render (Backend Web Service) + Vercel (Frontend Single Page Application) |

---

## 🔑 Role-Based Test Credentials

The database comes pre-seeded with 3 dedicated accounts (or runs `npm run seed`):

| Role | Email | Password | Access Level & Permissions |
|---|---|---|---|
| **Admin** | `admin@example.com` | `admin123` | **Full platform control**: Manage all products, view & update any order status, manage registered users and switch roles, view platform sales stats. |
| **Sales Person** | `sales@example.com` | `sales123` | **Seller control**: Add, edit, and delete **only their own products** (enforced on backend); view customer orders containing their items and earnings. |
| **Customer (User)** | `user@example.com` | `user123` | **Customer storefront**: Browse, search & filter products; manage cart & wishlist; test Razorpay checkout; view past order history. |

> **Evaluator Tip**: The Login page includes **1-Click Demo Login** buttons for instant role evaluation without typing!

---

## 📋 Feature Completion Summary

| Feature | Implementation Summary | Status |
|---|---|:---:|
| **Authentication & RBAC** | JWT-based auth with bcrypt password hashing; role middleware guards (`protect` & `authorize`) restricting actions at the backend route level. | ✅ Complete |
| **Product CRUD** | Complete CRUD with backend ownership validation (`req.user._id === product.sellerId` or Admin); public search & category/price filtering. | ✅ Complete |
| **Cloudinary Image Upload** | Multipart form-data handling via Multer directly streaming to Cloudinary, saving secure image URL to DB (with local disk fallback for dev). | ✅ Complete |
| **Product Listing & Filters** | Responsive responsive product catalog with search, category pills, price range slider/inputs, and sorting. | ✅ Complete |
| **Cart & Wishlist** | Persistent cart synchronized with database for logged-in users; quantity adjustment; badge count; wishlist toggle. | ✅ Complete |
| **Razorpay Test Checkout** | End-to-end checkout: backend order generation (`amount` in paise), test checkout popup, backend HMAC-SHA256 signature verification, stock decrement, and cart clearing. | ✅ Complete |
| **Role Dashboards** | **User**: past order receipts; **Sales Person**: inventory management & itemized sales; **Admin**: platform KPI stats, all orders status updater, user role switcher. | ✅ Complete |
| **Deployments** | `render.yaml` configured for Render backend; `vercel.json` configured for Vercel SPA routing rewrites. | ✅ Complete |

---

## ⚡ Quick Start & Local Setup

### Prerequisites
- Node.js (v18+)
- npm (v9+)

### 1. Clone & Install Dependencies

```bash
# Clone repository
git clone https://github.com/your-username/ecommerce-task.git
cd ecommerce-task

# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

### 2. Environment Variables Setup

Create a `.env` file in `backend/` (refer to `backend/.env.example`):

```env
PORT=5000
NODE_ENV=development

# Optional: If left blank or unset, in-memory MongoDB will auto-boot for zero-config testing!
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/ecommerce?retryWrites=true&w=majority

JWT_SECRET=super_secret_jwt_key_minimum_32_chars

# Cloudinary (Optional in local dev; uses disk fallback if not provided)
CLOUDINARY_CLOUD_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_cloudinary_key
CLOUDINARY_API_SECRET=your_cloudinary_secret

# Razorpay Test Credentials (Optional in local dev; uses test simulator if not provided)
RAZORPAY_KEY_ID=rzp_test_your_key_id
RAZORPAY_KEY_SECRET=your_secret_key
```

Create a `.env` file in `frontend/` (refer to `frontend/.env.example`):

```env
VITE_API_BASE_URL=http://localhost:5000/api
```

### 3. Seed Initial Test Data

```bash
cd backend
npm run seed
```
*(Note: The server also auto-seeds these accounts and sample products on startup if the database is fresh!)*

### 4. Run Development Servers

**In Terminal 1 (Backend):**
```bash
cd backend
npm start
# Backend runs at http://localhost:5000
```

**In Terminal 2 (Frontend):**
```bash
cd frontend
npm run dev
# Frontend runs at http://localhost:5173
```

---

## 🛡️ Role & Permission Matrix (Backend Enforced)

| Action | Admin | Sales Person | User | Guest |
|---|:---:|:---:|:---:|:---:|
| Browse, Search & Filter Products | ✅ | ✅ | ✅ | ✅ |
| View Single Product Details | ✅ | ✅ | ✅ | ✅ |
| Manage Cart & Wishlist | ✅ | ✅ | ✅ | ⚠️ (Local) |
| Checkout with Razorpay (Test) | ✅ | ✅ | ✅ | ❌ |
| View Own Orders (`/api/orders/my-orders`) | ✅ | ✅ | ✅ | ❌ |
| Create Product (`POST /api/products`) | ✅ | ✅ | ❌ | ❌ |
| Update/Delete **Own** Product | ✅ | ✅ | ❌ | ❌ |
| Update/Delete **Others'** Product | ✅ | ❌ (403 Forbidden) | ❌ | ❌ |
| View Seller Orders (`/api/orders/seller-orders`) | ✅ | ✅ | ❌ | ❌ |
| View All Orders & Update Status (`/api/orders/admin/*`) | ✅ | ❌ (403 Forbidden) | ❌ | ❌ |
| Manage Users & Roles (`/api/auth/users/*`) | ✅ | ❌ (403 Forbidden) | ❌ | ❌ |
| View Platform Analytics (`/api/orders/admin/stats`) | ✅ | ❌ (403 Forbidden) | ❌ | ❌ |

---

## 📝 Mandatory Implementation Report

### 1. What Features Were Completed
1. **Role-Based Authentication (RBAC)**: Secure password hashing with bcrypt, JSON Web Token generation on login/register, role identification, and backend middleware guards (`protect` and `authorize`).
2. **Product Catalog & Management**: Search by keyword in name/description, category filtering, min/max price range filter, sorting by price/newest, single product details view, and product creation/editing with image upload.
3. **Cloudinary Integration**: Direct image upload using Multer and `multer-storage-cloudinary`, storing CDN URLs in the database. Also includes a disk storage fallback so local offline development is never blocked.
4. **Persistent Cart & Wishlist**: DB-backed cart and wishlist for authenticated users, instant badge counters, real-time quantity increments/decrements, subtotal recalculation, and line item removal.
5. **Razorpay Test Integration**: Server-side Razorpay order generation with amount in paise, test mode checkout popup, cryptographic HMAC-SHA256 signature verification (`crypto.createHmac`), stock decrement upon purchase, and automatic cart clearing.
6. **Role Dashboards**:
   - **Customer**: View previous order history, itemized receipts, delivery status badges.
   - **Sales Person**: Inventory table with Add/Edit/Delete actions (restricted to own items), and itemized customer orders containing their products with earnings totals.
   - **Admin**: Platform KPI dashboard (Total Revenue, Orders, Catalog items, Users), order status updater (Paid/Processing/Delivered/Cancelled), product management, and user role promotion/demotion.

### 2. How Each Feature Was Implemented
- **Backend Architecture**: Layered Express architecture (`models/`, `controllers/`, `routes/`, `middleware/`, `config/`).
- **Permission Enforcement**: Backend routes enforce ownership using `product.sellerId.toString() === req.user._id.toString()`. Admins bypass ownership checks, while unauthorized sellers receive an immediate `403 Forbidden` response.
- **Frontend Architecture**: React 19 Single Page Application with React Router DOM 7, scoped Context Providers (`AuthContext`, `CartContext`, `WishlistContext`), Axios request interceptors that attach the `Bearer <token>`, and responsive Tailwind CSS components.

### 3. Challenges Faced & Solutions
- **Challenge 1**: Ensuring local evaluation works smoothly even if third-party keys (MongoDB Atlas, Cloudinary, Razorpay) are not yet configured.
  - *Solution*: Designed intelligent fallbacks: `mongodb-memory-server` boots an in-memory database automatically if no `MONGO_URI` is provided; Multer falls back to local uploads if Cloudinary keys are missing; and a sandbox checkout mode allows testing the signature verification pipeline even without personal Razorpay keys.
- **Challenge 2**: Mongoose 9.x deprecating `next` callbacks in `async` pre-save hooks (`TypeError: next is not a function`).
  - *Solution*: Modernized the user schema pre-save password hashing hook to use `async/await` without `next()`.
- **Challenge 3**: Enforcing Sales Person order visibility so sellers only view items they sell, not the entire customer basket.
  - *Solution*: Created `getSellerOrders` in `orderController.js` which filters order product line items by `sellerId: req.user._id` and calculates the seller's specific subtotal.

### 4. Known Limitations & Assumptions
- Razorpay test keys simulate currency in INR (₹).
- Single currency model (INR) used across the application.
- Stock is decremented upon verified payment; order cancellation does not currently auto-restock items.

---

## 🚢 Deployment Guide

### Backend to Render
1. Create a new **Web Service** on [Render](https://render.com).
2. Connect your GitHub repository.
3. Configure the service:
   - **Root Directory**: `backend`
   - **Build Command**: `npm install`
   - **Start Command**: `node server.js`
4. Add Environment Variables in the Render dashboard:
   - `NODE_ENV`: `production`
   - `PORT`: `5000`
   - `MONGO_URI`: *Your MongoDB Atlas connection string*
   - `JWT_SECRET`: *A secure random string*
   - `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`
   - `RAZORPAY_KEY_ID`, `RAZORPAY_KEY_SECRET`
5. Deploy and copy your live backend URL (e.g. `https://ecommerce-backend.onrender.com`).

### Frontend to Vercel
1. Import your GitHub repository in [Vercel](https://vercel.com).
2. Configure project settings:
   - **Root Directory**: `frontend`
   - **Framework Preset**: `Vite`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
3. Add Environment Variable:
   - `VITE_API_BASE_URL`: `https://ecommerce-backend.onrender.com/api`
4. Deploy. The included `frontend/vercel.json` ensures all client-side routes (`/admin/dashboard`, `/sales/dashboard`, etc.) resolve properly.
![Uploading Screenshot 2026-09-04 151711.png…]()

---

## 🌿 Git Workflow & Pull Request Practice

To satisfy Task 9's git workflow practice requirement:
```bash
# 1. Initialize repo
git init
git add .
git commit -m "feat: initial project scaffolding with frontend and backend"

# 2. Create feature branch
git checkout -b feature/role-based-dashboards

# 3. Work on feature and commit
git add .
git commit -m "feat: implement admin and sales person dashboards with role enforcement"

# 4. Merge via Pull Request on GitHub or locally:
git checkout main
git merge feature/role-based-dashboards
```
