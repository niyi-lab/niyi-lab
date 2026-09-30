# Oyeniyi Oyetunji

**Full-Stack Software · Embedded Systems · Robotics · Industrial Automation**
📍 Edmonton, Alberta · 🎓 Computer Engineering Technology, NAIT (graduating December 2026)

I build at both ends of the stack. On one end, production web apps that take real money from real customers. On the other, the firmware, circuits and control logic running on the metal underneath.

**Open to junior / entry-level roles across Canada**, remote or relocating. Available part-time now and full-time from January 2027.

| | |
|---|---|
| 🚗 **AutoVINReveal** | A live web platform I built and run alone: 1,500+ users, ~$30K in customer payments |
| 🤖 **Autonomous rover** | STM32 safety firmware plus ROS 2 and LiDAR SLAM on a Raspberry Pi 5. It maps a room and stops for obstacles |
| 🎮 **USB game controller** | A plug-and-play gamepad built from scratch on an STM32, with a custom USB HID descriptor and ADC + DMA |
| ⚙️ **Industrial automation** | Allen-Bradley PLCs, HMIs and VFDs in Studio 5000 and FactoryTalk |

---

## 🚗 AutoVINReveal · founder & sole developer, 2025–present

A live automotive-data platform: **[autovinreveal.com](https://www.autovinreveal.com)** · source: **[niyi-lab/autovinreveal](https://github.com/niyi-lab/autovinreveal)**

**1,500+ active users · ~$30K in processed customer transactions**

I own all of it: architecture, code, payments, infrastructure, security, support, and every 2 a.m. production incident.

- **Payments.** Stripe, PayPal and crypto (NOWPayments) side by side, with webhook signature verification, idempotent processing, transaction-state tracking and partial-payment handling. A retried webhook can't double-charge a customer or issue a second report.
- **Auth & data.** Supabase Auth and Google OAuth with protected routes and multi-role access. PostgreSQL schemas and row-level security policies covering users, orders, transactions, reports and service state.
- **AI support agent.** A Claude-powered support chat that knows the signed-in customer's account and can re-send a purchased report. It only does so after a tool call confirms a completed paid order matching the email and VIN. It also handles screenshot uploads, uses prompt caching, and has a human-takeover mode.
- **Integrations that keep moving.** Multiple vehicle-data providers plus NHTSA, with provider-switching and fallback logic for when an upstream service changes or goes down. Where no API existed, browser-emulated scraping, re-adapted each time the target site changed.
- **Infrastructure.** Moved both brands off Render onto a self-managed Linux VPS: systemd services behind Caddy, a least-privilege deploy user, and scripted and GitHub Actions deploys with health checks.
- **Hardening.** Security audits across authentication, authorization, sensitive routes, webhook processing and data exposure. Caching layers to cut unnecessary external API traffic.
- **Frontend.** Vanilla JS, Tailwind, jQuery/AJAX, Chart.js and hand-built SVG data visualization.
- **Automation.** Python workers using Requests, BeautifulSoup, feedparser and IMAP. One of them is an auto-healing Reddit keyword monitor that spots buying-intent posts and pushes them to Discord, with deduplication and rate-limit backoff.
- **Growth.** SEO, Google Ads conversion tracking, free VIN tool pages and a VIN-checker browser extension, all built as an acquisition funnel into the paid product.

**Also running:** [cheapestcarfax.com](https://cheapestcarfax.com), a second brand on the same backend · [khlinautomotive.com](https://khlinautomotive.com), booking and Stripe pay-links for an auto shop, with Twilio SMS and SendGrid notifications.

---

## 🔌 Embedded Systems & Hardware

### 🤖 Autonomous Object-Finding Rover · personal project, 2026

An indoor rover I'm building to find and identify objects on its own. I'm working on every layer, from the circuit board up:

```
camera + AI  →  mission logic  →  ROS 2 / SLAM on Raspberry Pi 5  →  STM32 safety bridge  →  motor controller  →  4 encoder motors
```

The AI will only make high-level decisions. It never drives the motors directly: every motor command passes through a safety layer that stops the rover the moment the commands stop arriving.

**Working today**
- Driven from a phone or laptop through a web control panel over private remote access (Tailscale), with live LiDAR and map views.
- Maps a room with SLAM Toolbox and an RPLIDAR C1. The first maps cover about 5 × 5 m.
- Stops or slows for obstacles by itself using the LiDAR.
- Measures distance with wheel encoders to within about 1.3% after calibration.

**Next:** camera-based object recognition, autonomous navigation with Nav2, and the AI mission layer.

<details>
<summary><b>Engineering details</b>: firmware, safety, calibration, debugging, PCB</summary>

<br>

- **Safety bridge firmware (C, STM32 NUCLEO-G0B1RE).** A session-based serial protocol with sequence numbers. The rover stops and disarms if heartbeats go quiet for 500 ms or motor commands for 300 ms, and an independent hardware watchdog catches a hung MCU. It also runs encoder-controlled distance moves. I fixed receive overruns under heavy two-way UART traffic by raising the clock to 64 MHz and using the UART FIFO.
- **Web control panel.** Python (aiohttp + WebSockets) with arm/stop, hold-to-drive and an automatic stop when input is lost. The live scan and SLAM map are drawn with vanilla JS on canvas.
- **ROS 2 Jazzy on Ubuntu 24.04.** Wheel odometry and TF from encoder telemetry, LiDAR driver bring-up, SLAM Toolbox tuned for a small, slow rover, and a map-viewer node.
- **Obstacle stop.** Sweeps the rover's 24 × 15 cm footprint along the exact arc each command would drive (straight, curve or spin), then scales the speed so the rover can stop in the space it has. The stopping model comes from measured coasting distances.
- **LiDAR as ground truth.** A RANSAC wall fit showed the LiDAR's mount position was recorded 22 cm off. Fixing it corrected both obstacle distances and the map. The same method calibrated the encoders (distance error from 4.3% down to about 1.3%) and measured how far the rover coasts after a stop.
- **Hunting an intermittent bug.** The rover sometimes kept moving for up to 1.3 s after release. A byte-level trace showed the Pi sent STOP on time and the motor controller ignored it. A new stop sequence (brake, then release, repeated) brought overruns to 0 in 100 trials.
- **Board-level fault isolation.** When the STM32 board went silent, I used resistance measurements and jumper isolation to trace a short to the MCU's 3.3 V supply branch. To keep working, I ported the same bridge C code to a Linux service on the Pi with a systemd watchdog and 17 simulated-hardware tests.
- **Custom PCB (KiCad).** A 2-layer STM32G071 supervisor and UART bridge board (54 × 42 mm), ERC/DRC clean, with Gerbers, BOM and placement files ready for JLCPCB assembly. Also a fuller 4-layer revision with a 3S battery input, a 5 V / 5 A buck with USB-C output, and a latched motor cutoff. The first breadboard supervisor used a TL431 reference, LM393 comparators with hysteresis for low-battery and 3.3 V rail faults, a MAX1232 external watchdog, and a 74HC74 stop/re-arm latch.
- **Tested and documented.** 100+ automated tests across the web app, ROS nodes and bridge. Every problem is logged as symptom → evidence → fix → verification.

</details>

### 🎮 USB Game Controller · personal project, 2026

A two-stick game controller I built from scratch on an STM32F411 "Black Pill". It plugs in as a standard USB gamepad, with no driver needed on the PC.

- **Custom USB HID.** I wrote the HID report descriptor by hand: 4 analog axes and 16 buttons packed into a 6-byte report, sent about 200 times a second.
- **Thumbsticks via ADC + DMA.** The ADC scans all four stick axes continuously and DMA writes them to memory, so the main loop never waits on a conversion. 12-bit readings are scaled to 8 bits, and the Y axes are inverted to match the physical sticks.
- **14 buttons.** A/B/X/Y, the D-pad, L1/L2/R1/R2 and both stick clicks. All are active-low with internal pull-ups.
- **Clock tree set by hand.** A 25 MHz crystal feeds the PLL to give a 96 MHz core and the exact 48 MHz clock that USB requires.
- **Iterated in hardware.** It went from an STM32F401 ADC test, to a NUCLEO-G0 prototype with serial debug output, to the final USB build. I drew the schematic in KiCad.

### Coursework & lab work

C/C++ on microcontrollers: GPIO, ADC, PWM, timers, interrupts, sensors, actuators and motor control. Breadboard prototyping, and debugging at the hardware/software boundary with an oscilloscope and multimeter. C# desktop GUIs with GDI+.

---

## ⚙️ Industrial Automation & Controls

Rockwell / Allen-Bradley work from academic and lab projects:

**Studio 5000** (Ladder Logic + Structured Text) · **FactoryTalk View** HMI screens · **PowerFlex** VFDs · **Micro800** controllers · sequencing and state-based machine logic with safety interlocks · high-speed counters and pulse-based instrumentation · meter K-factor proving · state diagrams for equipment modelling and troubleshooting.

---

## 🧰 Skills

| | |
|---|---|
| **Languages** | C, C++, Python, JavaScript, C#, SQL / T-SQL, PHP, Bash, HTML/CSS |
| **Embedded firmware** | STM32 (F4, G0) · STM32CubeIDE / CubeMX / HAL · GPIO · ADC + DMA · PWM · timers & interrupts · UART · USB device (HID) · watchdogs · encoders · motor control · serial protocol design |
| **Robotics & embedded Linux** | Raspberry Pi 5 · Ubuntu 24.04 · ROS 2 Jazzy · SLAM Toolbox · LiDAR · odometry & TF · systemd services · udev rules |
| **Electronics & PCB** | KiCad schematic capture & PCB layout (2- and 4-layer) · JLCPCB fab & assembly files (Gerbers, BOM, CPL) · voltage references, comparators, watchdog ICs, logic latches · breadboard prototyping · oscilloscope & multimeter · board-level fault isolation |
| **Industrial automation** | Studio 5000 (Ladder Logic, Structured Text) · FactoryTalk View · PowerFlex VFDs · Micro800 · high-speed counters · K-factor proving · safety interlocks · state diagrams |
| **Backend** | Node.js · Express · Python aiohttp · ASP.NET Core Minimal APIs · REST · WebSockets · webhooks · caching · authn/authz · OAuth |
| **Data** | PostgreSQL · Supabase (row-level security) · MySQL · SQL Server · SQLite · schema design · stored procedures |
| **Payments** | Stripe · PayPal · NOWPayments: webhooks, idempotency, transaction state |
| **AI** | Claude API (tool use, prompt caching, vision) · OpenAI API · AI-assisted development with Claude Code |
| **Frontend** | Vanilla JS · Tailwind · jQuery/AJAX · Chart.js · SVG & canvas visualization · C# desktop GUIs (GDI+) |
| **Automation** | Python Requests · BeautifulSoup · feedparser · IMAP · Discord webhooks · browser-emulated scraping |
| **DevOps** | Git/GitHub · GitHub Actions · Linux · SSH · systemd · Caddy · VPS hosting · DNS/TLS · Tailscale |
| **Testing** | Python and Node unit & integration tests · NUnit · simulated-hardware tests · on-device verification |
| **Also used** | React · TypeScript · Docker · AWS |

---

## How I work

- **End-to-end ownership:** from schematic and firmware to payments and production support.
- **Evidence-driven debugging:** measure, trace, fix, then prove the fix, like the 0-in-100 stop test above.
- **Hardware fails safe:** if the link goes quiet, the motors stop.
- **Fast adaptation:** I keep things working when a third-party API, a website or a circuit board changes under me.

## Reach me

📧 [niyiollie@gmail.com](mailto:niyiollie@gmail.com) · 📍 Edmonton, AB. Open to remote work or relocation anywhere in Canada.
