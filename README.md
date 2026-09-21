# Hi, I'm Niyi 👋

Full-stack developer near Edmonton, Alberta. I build and ship production web apps end to end — Node/Express backends, Postgres, Stripe payments, and the Linux servers they run on.

**Currently open to junior / entry-level developer roles anywhere in Canada (remote or relocating).**

---

## What I've shipped

Everything below is live, takes real payments, and is deployed and maintained by me.

### 🚗 AutoVINReveal — [autovinreveal.com](https://www.autovinreveal.com)
Vehicle history report platform. Enter a VIN, pay, get a full report back in seconds.

- **Node + Express** API (~5,300 lines) with Helmet, CORS, compression and per-route rate limiting
- **Supabase** (Postgres + auth) for accounts, order history and report storage
- **Stripe** checkout and webhook handling for one-off purchases and memberships
- Transactional email via **Nodemailer**; admin dashboard and support chat
- A companion **Chrome extension** that picks up VINs from listing pages and links straight to a report
- **Tailwind** front end with ~20 SEO landing pages (per-make VIN decoders, a car payment calculator, free decoder tools)

### 🔧 CheapestCarFax — [cheapestcarfax.com](https://cheapestcarfax.com)
A second brand on the same product line, running as its own service and codebase (~4,000 lines) against shared Supabase and Stripe infrastructure. Building the second one taught me most of what I know about keeping two near-identical codebases from drifting apart.

### 📅 Khlin Automotive — [khlinautomotive.com](https://khlinautomotive.com)
Booking and payments site for an auto shop.

- Admin panel generates one-off **Stripe** pay links (CAD), persisted in Supabase
- **Twilio** SMS and **SendGrid** email notifications to customers
- Lean Express service — small, focused, and boring on purpose

### 🔎 jobhunt
A zero-dependency Node CLI that pulls ~14 job feeds, dedupes them, scores postings against a profile file, and drafts tailored cover letters, with a local dashboard.

Built with deliberate limits: it never clicks submit, never fills voluntary self-identification fields, never overwrites something you've already typed, and refuses to drive LinkedIn/Indeed/Glassdoor entirely. Those aren't missing features — automating them is how you get your account banned.

---

## How it all runs

All three sites live on a single **Hetzner VPS** behind **Caddy** (automatic TLS), each as its own `systemd` service on its own port. Deploys are push-to-`main` through **GitHub Actions** — no manual SSH, no "works on my machine."

---

## Stack

**Languages** JavaScript (ES modules), SQL, HTML/CSS, Bash
**Backend** Node.js, Express, REST APIs, webhooks, rate limiting
**Data** Supabase, PostgreSQL
**Payments** Stripe (Checkout, webhooks, subscriptions)
**Frontend** Tailwind CSS, vanilla JS, Chrome extensions (Manifest V3)
**Infra** Linux, systemd, Caddy, GitHub Actions CI/CD, Nginx-style reverse proxying
**Integrations** Twilio, SendGrid, Nodemailer

---

## Reach me

📧 [niyiollie@gmail.com](mailto:niyiollie@gmail.com)
📍 Edmonton area — open to remote or relocation across Canada
