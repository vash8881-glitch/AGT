# SabziMandi Fresh — Vegetable & Grocery E-Commerce Platform

Full-stack, production-ready vegetable e-commerce platform for Indian customers: customer storefront, REST API, MongoDB, professional admin panel, OTP authentication, 4 languages (EN/HI/KN/MR), subscriptions, loyalty/wallet, automatic PDF invoices, abandoned-cart tracking, local delivery management, PWA-ready.

## Tech Stack
- **Frontend**: React, React Router, Context API, Axios, Tailwind CSS, PWA manifest
- **Backend**: FastAPI (Python), Motor (async MongoDB), JWT, bcrypt, reportlab (invoices)
- **Database**: MongoDB with indexes (mobile/email partial-unique, SKU, orders, OTP TTL)

## Structure
```
/app
├── backend/            # FastAPI API (port 8001, all routes under /api)
│   ├── server.py       # App assembly + background jobs (offers, subscriptions, abandoned carts)
│   ├── routes_auth.py  # OTP + admin login
│   ├── routes_store.py # Products, categories, cart sync, wishlist, addresses, coupons, reviews
│   ├── routes_orders.py# Orders, invoices, loyalty, wallet, subscriptions
│   ├── routes_admin.py # Admin panel APIs + RBAC + activity logs
│   ├── invoice.py      # PDF invoice generation
│   ├── seed.py         # Seed data + indexes
│   └── tests/          # 90 pytest cases (auth, store, orders, admin)
├── frontend/           # React storefront + /admin panel (port 3000)
└── .env.example        # All configurable secrets/keys
```

## Quick Start (this workspace)
Services run via supervisor; hot reload enabled.
```bash
sudo supervisorctl restart backend frontend
```
- Storefront: `/` · Admin: `/admin/login`
- Admin: `admin@sabzimandi.com` / `Admin@123`
- Customer OTP login: any 10-digit mobile, OTP `123456` (test mode)
- Seeded coupons: `FRESH50` (₹50 off ≥₹299), `SABZI10` (10% ≤₹150, ≥₹499)

## Configuration (.env)
Copy `.env.example` → `backend/.env`. Key groups:
- `OTP_PROVIDER=test` (fixed `TEST_OTP`) — swap for a real SMS provider key later
- `RAZORPAY_KEY_ID/SECRET` — enables Razorpay at checkout when set
- `WHATSAPP_*` — Business Cloud API for automated messaging (storefront uses wa.me links meanwhile)
- `CLOUDINARY_*` — cloud image hosting (local `/api/uploads` fallback built in)
- `STORE_*`, `GST_NUMBER`, `JWT_SECRET`, `ADMIN_*`

## Tests
```bash
cd backend && python -m pytest tests/ -q   # 90 tests: OTP, products, cart, orders, inventory, coupons, loyalty, admin auth
```

## Production Notes
- Set strong `JWT_SECRET`, real `ADMIN_PASSWORD`, explicit `CORS_ORIGINS` before deploy.
- Never commit real secrets; `.env.example` documents every variable.
