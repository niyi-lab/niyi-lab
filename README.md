# Oyeniyi Oyetunji

**Full-Stack Software · Embedded Systems · Robotics · Industrial Automation**<br>
📍 Edmonton, Alberta · 🎓 Computer Engineering Technology, NAIT (graduating December 2026)

I'm a software and embedded developer with hands-on experience ranging from production web platforms to microcontroller firmware. I founded and operate **AutoVINReveal**, a vehicle-history platform with 1,500+ users and ~$30K in processed payments. I also design robotics and embedded systems on STM32 and Raspberry Pi, including an **AI-driven autonomous rover** that searches for and identifies objects.

**Seeking junior / entry-level roles across Canada** (remote or relocation). Available part-time now and full-time from January 2027.

| Project | Summary |
|---|---|
| 🚗 **AutoVINReveal** | Vehicle-history report platform I built and operate solo: 1,500+ users, ~$30K in processed payments |
| 🤖 **AI Autonomous Rover** | Rover driven entirely by AI that explores a room, searches for objects and identifies them, using LiDAR SLAM and an STM32 safety controller |
| 🎮 **USB Game Controller** | Custom gamepad built on an STM32 microcontroller, recognized by any PC as a standard USB controller |
| ⚙️ **Industrial Automation** | PLC programming, HMI design and VFD control with Allen-Bradley Studio 5000 and FactoryTalk |

---

## 🚗 AutoVINReveal · Founder & Sole Developer, 2025–present

A live automotive-data platform: **[autovinreveal.com](https://www.autovinreveal.com)** · source: **[niyi-lab/autovinreveal](https://github.com/niyi-lab/autovinreveal)**

**1,500+ active users · ~$30K in processed customer transactions**

Responsible for the entire product: architecture, development, payments, infrastructure, security and customer support.

- **Payments.** Stripe, PayPal and cryptocurrency (NOWPayments) with webhook signature verification, idempotent processing, transaction-state tracking and partial-payment handling, so a retried webhook never double-charges a customer or issues a duplicate report.
- **Authentication & data.** Supabase Auth and Google OAuth with protected routes and multi-role access. PostgreSQL schemas and row-level security policies for users, orders, transactions, reports and service state.
- **AI customer support.** A Claude-powered support assistant with access to the signed-in customer's account. It can re-send a purchased report after a verification step confirms a paid order matching the customer's email and VIN. It also accepts screenshots, uses prompt caching, and allows a human to take over the conversation.
- **Data integrations.** Multiple vehicle-data providers plus NHTSA, with automatic provider switching and fallback when an upstream service changes or fails. Browser-emulated scraping where no API exists, maintained as target sites change.
- **Infrastructure.** Migrated both brands from Render to a self-managed Linux VPS: systemd services behind Caddy, a least-privilege deploy user, and scripted and GitHub Actions deployments with health checks.
- **Security.** Audits and hardening across authentication, authorization, sensitive routes, webhook processing and data exposure. Caching layers that reduce external API traffic.
- **Frontend.** Vanilla JS, Tailwind, jQuery/AJAX, Chart.js and custom SVG data visualization.
- **Automation.** Python workers built on Requests, BeautifulSoup, feedparser and IMAP, including a self-recovering Reddit monitor that detects buying-intent posts and forwards them to Discord with deduplication and rate-limit backoff.
- **Growth.** SEO, Google Ads conversion tracking, free VIN tool pages and a VIN-checker browser extension that bring users to the paid product.

**Related work:** [cheapestcarfax.com](https://cheapestcarfax.com), a second brand running on the same backend · [khlinautomotive.com](https://khlinautomotive.com), online booking and Stripe payment links for an auto shop, with Twilio SMS and SendGrid notifications.

---

## 🔌 Embedded Systems & Hardware

### 🤖 AI-Driven Autonomous Rover · Personal Project, 2026

A four-wheel rover driven entirely by AI. Given an object to find, the AI decides where to go, drives the rover through the room, and identifies objects from its camera. All the rover's movement is controlled by the AI, while dedicated safety layers keep it from colliding with obstacles or running away if the software fails.

**System overview**
- **AI control.** An AI model plans the search, issues the driving commands and identifies objects from the camera feed.
- **Mapping and localization.** ROS 2 Jazzy and SLAM Toolbox on a Raspberry Pi 5 build a map of the room from an RPLIDAR C1 and track the rover's position in it.
- **Obstacle avoidance.** Every drive command is checked against the live LiDAR scan and slowed or stopped before the rover can hit something.
- **Safety controller.** STM32 firmware written in C sits between the Pi and the motor driver and stops the motors if commands stop arriving or the software hangs.
- **Monitoring dashboard.** A web dashboard shows the live LiDAR scan, the map and the rover's status from a phone or laptop, and includes manual override and an emergency stop.

<details>
<summary><b>Engineering details</b>: firmware, safety, calibration, fault diagnosis, PCB design</summary>

<br>

- **Safety controller firmware (C, STM32 NUCLEO-G0B1RE).** Session-based serial protocol with sequence numbers. The controller stops and disarms the motors if heartbeats stop for 500 ms or drive commands stop for 300 ms, and a hardware watchdog resets a hung microcontroller. It also supports encoder-controlled distance moves. I resolved UART receive overruns under heavy two-way traffic by raising the clock to 64 MHz and enabling the UART FIFO.
- **Dashboard backend.** Python (aiohttp + WebSockets) handles arming, driving and emergency stop, and stops the rover automatically if the connection drops. The scan and map views are rendered with JavaScript on canvas.
- **ROS 2 integration.** Wheel odometry and TF from encoder data, LiDAR driver setup, SLAM Toolbox tuning for a small, slow-moving robot, and a map-publishing node for the dashboard.
- **Obstacle avoidance algorithm.** Projects the rover's 24 × 15 cm footprint along the exact path each command would take (straight, curve or rotation) and reduces speed so the rover can always stop in the available space. Stopping distances are based on measured braking data.
- **LiDAR-based calibration.** Fitting wall lines in the LiDAR data (RANSAC) revealed that the sensor's mounting position was recorded 22 cm off. Correcting it fixed both obstacle distances and map accuracy. The same method reduced wheel-encoder distance error from 4.3% to about 1.3%.
- **Intermittent stop failure.** The rover occasionally kept moving for up to 1.3 s after being told to stop. A byte-level trace proved the stop command was sent on time and the motor driver was ignoring it. A revised stop sequence (active brake, then release, repeated) eliminated the problem: 0 failures in 100 trials.
- **Hardware fault diagnosis.** When the STM32 board stopped responding, I traced a short circuit to the microcontroller's 3.3 V supply using resistance measurements and jumper isolation. To keep development moving, I ported the safety controller's C code to a Linux service on the Pi, supervised by a systemd watchdog and covered by 17 simulated-hardware tests.
- **Custom PCB design (KiCad).** A 2-layer STM32G071 safety controller board (54 × 42 mm), ERC/DRC clean, with Gerbers, BOM and placement files ready for JLCPCB assembly. I also designed a 4-layer version with a 3S battery input, a 5 V / 5 A power supply with USB-C output, and a latching motor cutoff. The earlier breadboard prototype used a TL431 voltage reference, LM393 comparators with hysteresis for low-battery and supply faults, a MAX1232 watchdog and a 74HC74 stop/re-arm latch.
- **Testing and documentation.** 100+ automated tests across the dashboard, ROS nodes and safety controller. Every issue is documented from symptom and evidence through the fix and its verification.

</details>

### 🎮 USB Game Controller · Personal Project, 2026

A two-joystick game controller I designed and built on an STM32F411 "Black Pill" microcontroller. It connects over USB and is recognized by the PC as a standard game controller, with no driver installation needed.

- **Custom USB HID implementation.** I wrote the HID report descriptor by hand: 4 analog axes and 16 buttons in a 6-byte report, sent about 200 times per second.
- **Joystick input with ADC + DMA.** The ADC continuously samples all four joystick axes and DMA transfers the results to memory, so the main loop never waits on a conversion. Readings are scaled from 12 to 8 bits and the Y axes are inverted to match the physical sticks.
- **14 buttons.** A/B/X/Y, D-pad, L1/L2/R1/R2 and both joystick clicks, all active-low with internal pull-up resistors.
- **Clock configuration.** A 25 MHz crystal and PLL produce a 96 MHz CPU clock and the exact 48 MHz clock USB requires.
- **Iterative development.** It progressed from an STM32F401 ADC test to a NUCLEO-G0 prototype with serial debugging, and then to the final USB build. The schematic was drawn in KiCad.

### Coursework & Lab Work

C/C++ microcontroller programming for GPIO, ADC, PWM, timers, interrupts, sensors, actuators and motor control. Breadboard prototyping, plus hardware/software debugging with an oscilloscope and multimeter. C# desktop GUIs with GDI+.

---

## ⚙️ Industrial Automation & Controls

Rockwell / Allen-Bradley experience from academic and lab projects:

**Studio 5000** (Ladder Logic + Structured Text) · **FactoryTalk View** HMI screens · **PowerFlex** VFDs · **Micro800** controllers · sequencing and state-based machine logic with safety interlocks · high-speed counters and pulse-based instrumentation · meter K-factor proving · state diagrams for equipment modelling and troubleshooting.

---

## 🧰 Skills

| Area | Skills |
|---|---|
| **Languages** | C, C++, Python, JavaScript, C#, SQL / T-SQL, PHP, Bash, HTML/CSS |
| **Embedded firmware** | STM32 (F4, G0) · STM32CubeIDE / CubeMX / HAL · GPIO · ADC + DMA · PWM · timers & interrupts · UART · USB device (HID) · watchdogs · encoders · motor control · serial protocol design |
| **Robotics & embedded Linux** | Raspberry Pi 5 · Ubuntu 24.04 · ROS 2 Jazzy · SLAM Toolbox · LiDAR · odometry & TF · AI-driven control · systemd services · udev rules |
| **Electronics & PCB** | KiCad schematic capture & PCB layout (2- and 4-layer) · JLCPCB fab & assembly files (Gerbers, BOM, CPL) · voltage references, comparators, watchdog ICs, logic latches · breadboard prototyping · oscilloscope & multimeter · board-level fault diagnosis |
| **Industrial automation** | Studio 5000 (Ladder Logic, Structured Text) · FactoryTalk View · PowerFlex VFDs · Micro800 · high-speed counters · K-factor proving · safety interlocks · state diagrams |
| **Backend** | Node.js · Express · Python aiohttp · ASP.NET Core Minimal APIs · REST · WebSockets · webhooks · caching · authentication & authorization · OAuth |
| **Data** | PostgreSQL · Supabase (row-level security) · MySQL · SQL Server · SQLite · schema design · stored procedures |
| **Payments** | Stripe · PayPal · NOWPayments · webhooks · idempotency · transaction state |
| **AI** | Claude API (tool use, prompt caching, vision) · OpenAI API · AI-assisted development with Claude Code |
| **Frontend** | Vanilla JS · Tailwind · jQuery/AJAX · Chart.js · SVG & canvas visualization · C# desktop GUIs (GDI+) |
| **Automation** | Python Requests · BeautifulSoup · feedparser · IMAP · Discord webhooks · browser-emulated scraping |
| **DevOps** | Git/GitHub · GitHub Actions · Linux · SSH · systemd · Caddy · VPS hosting · DNS/TLS · Tailscale |
| **Testing** | Python and Node unit & integration tests · NUnit · simulated-hardware tests · on-device verification |
| **Also used** | React · TypeScript · Docker · AWS |

---

## Strengths

- **End-to-end ownership:** from circuit design and firmware to payments, deployment and production support.
- **Methodical debugging:** I diagnose problems with measurements and traces, and verify each fix with repeatable tests.
- **Safety-focused hardware design:** systems default to a safe state when communication or software fails.
- **Adaptability:** I keep systems running when third-party APIs, websites or hardware change unexpectedly.

## Contact

📧 [niyiollie@gmail.com](mailto:niyiollie@gmail.com) · 📍 Edmonton, AB · Open to remote work or relocation anywhere in Canada
