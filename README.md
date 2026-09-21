# Oyeniyi Oyetunji

**Full-Stack Software · Embedded Systems · Industrial Automation**
📍 Edmonton, Alberta

Computer Engineering Technology student at **NAIT**, graduating **December 2026**. I work at both ends of the stack — production web apps that take real money from real customers, and the firmware and control logic running on the metal underneath.

**Open to junior / entry-level roles across Canada** — remote or relocating.

---

## 🚗 AutoVINReveal — founder & sole developer, 2025–present

A live automotive-data platform: **[autovinreveal.com](https://www.autovinreveal.com)** · source: **[niyi-lab/autovinreveal](https://github.com/niyi-lab/autovinreveal)**

**1,500+ active users · ~$30K in processed customer transactions**

I own the whole thing — architecture, code, payments, infrastructure, security, support, and every 2 a.m. production incident.

- **Payments.** Stripe, PayPal and crypto (NOWPayments) side by side, with webhook signature verification, idempotent processing, transaction-state tracking and partial-payment handling — so a retried webhook can't double-charge a customer or issue a second report.
- **Auth & data.** Supabase Auth and Google OAuth with protected routes and multi-role access. PostgreSQL schemas and row-level security policies covering users, orders, transactions, reports and service state.
- **Integrations that keep moving.** Multiple vehicle-data providers plus NHTSA, with provider-switching and fallback logic for when an upstream service changes or goes down. Where no API existed, browser-emulated scraping — re-adapted each time the target site changed.
- **Hardening.** Security audits across authentication, authorization, sensitive routes, webhook processing and data exposure. Caching layers to cut unnecessary external API traffic.
- **Frontend.** Vanilla JS, Tailwind, jQuery/AJAX, Chart.js and hand-built SVG data visualization.
- **Automation.** Python workers using Requests, BeautifulSoup, feedparser and IMAP — including an auto-healing Reddit keyword monitor that spots buying-intent posts and pushes them to Discord with deduplication and rate-limit backoff.
- **Growth.** SEO, Google Ads conversion tracking, and free VIN tools built as an acquisition funnel into the paid product.

**Also running:** [cheapestcarfax.com](https://cheapestcarfax.com) — a second brand on shared backend infrastructure · [khlinautomotive.com](https://khlinautomotive.com) — booking and Stripe pay-links for an auto shop, with Twilio SMS and SendGrid notifications.

---

## 🤖 Vehicle Inventory Auditor Rover — NAIT Capstone, 2026

An autonomous rover that identifies, locates and tracks vehicles across a dealership lot.

My scope: the **Raspberry Pi 5 control and AI software**, the **motor-command interface** bridging high-level control to embedded functions, and **STM32-based supervision and safety circuitry**. I designed the AI control architecture and prepared the Pi environment for tool-based commands — live AI rover control is still in development, not yet fully implemented or tested.

Planned functions: movement decision-making, camera-based scene understanding, obstacle-aware navigation, vehicle and plate recognition, and writing scan results to the database. Team scope covers chassis, camera/LiDAR, the web dashboard and full-system testing.

---

## ⚙️ Industrial Automation & Controls

Rockwell / Allen-Bradley work from academic and lab projects:

**Studio 5000** (Ladder Logic + Structured Text) · **FactoryTalk View** HMI screens · **PowerFlex** VFDs · **Micro800** controllers · sequencing and state-based machine logic with safety interlocks · high-speed counters and pulse-based instrumentation · meter K-factor proving · state diagrams for equipment modelling and troubleshooting.

## 🔌 Embedded & Hardware

C/C++ on **STM32** and microcontrollers — GPIO, ADC, PWM, timers, interrupts, sensors, actuators, motor control. Breadboard prototyping, debugging at the hardware/software boundary with an oscilloscope and multimeter. **KiCad** schematic capture and custom PCB layout. C# desktop GUIs with GDI+.

---

## Stack

| | |
|---|---|
| **Languages** | JavaScript, Python, C, C++, C#, PHP, SQL/T-SQL, HTML/CSS |
| **Backend** | Node.js, Express, ASP.NET Core Minimal APIs, REST, webhooks, caching, authn/authz |
| **Data** | PostgreSQL, Supabase, MySQL, schema design, stored procedures, row-level security |
| **Payments** | Stripe, PayPal, NOWPayments — webhooks, idempotency, transaction state |
| **Frontend** | Vanilla JS, Tailwind, jQuery/AJAX, Chart.js, SVG viz |
| **Embedded** | STM32, Raspberry Pi 5, GPIO/ADC/PWM, interrupts, motor control, KiCad, TCP/IP |
| **Automation** | Studio 5000, Ladder Logic, Structured Text, FactoryTalk View, PowerFlex, Micro800 |
| **Testing** | NUnit, generic methods, edge-case and production runtime testing |
| **DevOps** | Git, Linux CLI, SSH, systemd, Caddy, GitHub Actions, Render, DNS/TLS |

---

## What I'm actually good at

End-to-end product ownership · root-cause troubleshooting under production pressure · hardware/software integration · adapting fast when a third-party API changes out from under you.

## Reach me

📧 [niyiollie@gmail.com](mailto:niyiollie@gmail.com) · 📍 Edmonton, AB — open to remote or relocation across Canada
