<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Flask-2.x-000000?style=for-the-badge&logo=flask&logoColor=white" />
  <img src="https://img.shields.io/badge/Bootstrap-5-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white" />
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" />
</p>

<h1 align="center">🛡️ NetSentry</h1>
<h3 align="center">Web Scraper Honeypot &amp; Malicious Bot Detection System</h3>

<p align="center">
  A decoy e-commerce honeypot that simulates a realistic online store to lure, identify, and analyze malicious bot traffic — empowering security teams with actionable threat intelligence and automated alerting.
</p>

---

## 📌 Table of Contents

- [Problem Statement](#-problem-statement)
- [How It Works](#-how-it-works)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Usage](#-usage)
- [License](#-license)

---

## 🧩 Problem Statement

Modern e-commerce platforms face relentless automated threats — credential stuffing, price scraping, inventory hoarding, and reconnaissance bots probing for vulnerabilities. Traditional WAFs and rate limiters are often reactive and signature-dependent.

**NetSentry** takes an offensive-defense approach: it deploys a convincing fake storefront that acts as a trap. Any entity interacting with hidden endpoints or exhibiting non-human behavior is flagged, logged, and analyzed in real time.

---

## 🔄 How It Works

```
Attacker / Bot                 NetSentry Honeypot
     │                               │
     │──── HTTP Request ────────────▶│
     │                               ├─── Log IP, UA, Headers, Timestamp
     │                               ├─── Check against rate-limit rules
     │                               ├─── Evaluate CAPTCHA interaction
     │                               ├─── Detect hidden trap endpoint access
     │                               │
     │                               ├─── 🚨 Suspicious? ──▶ Flag + Email Alert
     │                               ├─── 📦 Store in DB (interactions, bot_flags)
     │◀── Fake Product Page ─────────┤
     │                               │
                              Scapy / Wireshark
                           (Packet-level analysis)
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python 3.10+, Flask |
| Database | SQLite (dev) / MySQL (prod) |
| Frontend | HTML5, Bootstrap 5, Montserrat font |
| CAPTCHA | Custom server-side image generation |
| Email Alerts | Gmail SMTP (App Passwords) |
| Packet Analysis | Scapy, Wireshark |
| Bot Simulation | Selenium WebDriver, Python `requests` |
| Containerization | Docker (optional) |

---

## 📂 Project Structure

```
Network-Security-Honeypot/
├── app.py                              # Main Flask application and route definitions
├── main.py                             # Application entry point
├── db.py                               # Database initialization and query helpers
├── countermeasures.py                   # Rate limiting, IP blacklisting, bot mitigation
├── analyze_logs.py                      # Log analysis and bot-flagging logic
├── simulate_bot.py                      # Selenium/requests-based bot simulation scripts
├── basic_bot.py                         # Lightweight bot simulator for quick testing
├── honeypot.db                          # SQLite database (auto-generated at runtime)
├── Animation - 1741553560744.json       # Lottie animation asset for the frontend
├── LICENSE                              # MIT License
├── README.md
│
├── static/                              # Static assets served by Flask
│   ├── css/                             # Stylesheets (Bootstrap overrides, custom styles)
│   ├── js/                              # Client-side JavaScript
│   └── images/                          # Product images and site assets
│
├── templates/                           # Jinja2 HTML templates
│   ├── index.html                       # Homepage with product listings
│   ├── product.html                     # Individual product detail page
│   ├── signup.html                      # Signup page with CAPTCHA
│   ├── login.html                       # Login page with CAPTCHA
│   └── cart.html                        # Shopping cart page
│
└── venv/                                # Python virtual environment (not tracked in prod)
```

---

## ⚡ Getting Started

### Prerequisites

- Python 3.10 or higher
- pip package manager
- MySQL (optional — SQLite is used by default)
- Wireshark (optional — for packet-level analysis)

### Installation

**1. Clone the repository**

```bash
git clone https://github.com/Rebantaa/Network-Security-Honeypot.git
cd Network-Security-Honeypot
```

**2. Create and activate a virtual environment**

```bash
python3 -m venv venv

# macOS / Linux
source venv/bin/activate

# Windows
venv\Scripts\activate
```

**3. Install dependencies**

```bash
pip install -r requirements.txt
```

**4. Set environment variables and launch**

```bash
export FLASK_APP=app.py
export FLASK_ENV=development
export SECRET_KEY="your_secret_key"

# SMTP settings for email alerts
export SMTP_USER="you@gmail.com"
export SMTP_PASS="app_password"

flask run
```

The honeypot will be live at `http://127.0.0.1:5000/`

---

## 🎯 Usage

### Run the Honeypot

```bash
flask run
```

Browse to `http://127.0.0.1:5000/` to see the fake storefront.

### Simulate Bot Traffic

```bash
# Basic lightweight bot simulation
python basic_bot.py

# Full scraper simulation using Selenium + requests
python simulate_bot.py
```

### Monitor Logs

Inspect captured interactions directly in the database:

```sql
-- View all logged requests
SELECT * FROM interactions ORDER BY timestamp DESC LIMIT 50;

-- View flagged bot sessions
SELECT * FROM bot_flags WHERE severity = 'HIGH';
```

### Analyze Captured Data

```bash
# Run the log analyzer to flag suspicious sessions
python analyze_logs.py
```

### Network-Level Analysis

```bash
# Capture packets on the Flask port using Scapy
sudo python utils/packet_monitor.py --port 5000

# Or open a live capture in Wireshark
wireshark -i lo -f "tcp port 5000"
```

### Email Alerts

When a high-risk pattern is detected (rapid-fire requests, trap endpoint access, missing headers), an alert is dispatched to the configured SMTP recipient.

---

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---

<p align="center">
  Built for security research and education.<br/>
  <strong>NetSentry</strong> — Lure. Detect. Defend.
</p>
