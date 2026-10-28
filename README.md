# 🌿 Sage: Wellness Practice Appointments App

> A lightweight, modern two-sided appointment system designed for a wellness practice. Built with a mobile-first **Client App** and an actionable **Nutritionist Dashboard** that share synchronized data in real-time.

![License](https://img.shields.io/badge/license-MIT-green)
![Stack](https://img.shields.io/badge/stack-HTML5%20%7C%20CSS3%20%7C%20Vanilla%20JS-teal)
![Framework](https://img.shields.io/badge/framework-Zero%20Dependencies-blue)
![Deployment](https://img.shields.io/badge/deploy-Vercel%20Ready-black)

---

## 📌 Project Overview

Built as part of the **Kayro AI Product Intern Assignment**, **Sage** solves the real-world operational challenge of client no-shows in a wellness clinic while replacing informal WhatsApp/Excel workflows with an intuitive consumer experience.

### Key Highlights
- **Two Sides, One Store**: Seamlessly switch between the **Client App** (calm, consumer wellness feel) and the **Nutritionist Dashboard** (high-density clinical schedule).
- **Zero Double-Booking Guarantee**: Dynamic slot availability calculation prevents overlapping consultations across both client bookings and manual entries.
- **Built-in No-Show Mitigations**: Implements Part 1 strategy directly into the product via 1-tap client confirmations, one-click WhatsApp reminder links, and clinical show-rate analytics.
- **Cross-Tab Real-Time Sync**: Uses browser `localStorage` with reactive `storage` listeners for instant synchronization across multiple windows and tabs without needing a backend server.
- **Demo Date Controller**: Defaults to the assignment's sample dataset date (`24 Oct 2026`) so evaluators immediately experience the app in full fidelity, with a 1-tap toggle to live device time.

---

## 🚀 Live Demo & Deployment

- **Hosted URL**: Deployable in 60 seconds on [Vercel](https://vercel.com) (Preset: *Other / Static HTML*).
- **Local Run**: Double-click `index.html` or open in any web browser. No `npm install`, build step, or server configuration required.

```bash
# Optional: run with any static file server
python -m http.server 8080
# Open http://localhost:8080
```

---

## 📱 Product Features

### Side A: Client App (Mobile-First Consumer Experience)
1. **Friendly Home Screen**:
   - Greets client by first name with a calming, reassuring visual design.
   - Prominently showcases their next upcoming consultation (date, time, status).
   - **"I'll be there" confirmation button**: Allows clients to lock in their attendance with a single tap.
   - Quick reschedule and cancellation triggers.
2. **Consultation Booking Flow**:
   - 14-day interactive horizontal date picker.
   - 30-minute time slots (9:00 AM – 5:30 PM).
   - Blocked and strike-through styling for already-booked or past time slots.
   - Real-time client name and 10-digit mobile number validation.
3. **Visits Management**:
   - **Upcoming**: Full details of upcoming visits with 1-tap reschedule and 2-step safe cancellation.
   - **Past**: Historical log of completed visits with color-coded status badges.
4. **Quick Client Profile Switcher**:
   - Easily alternate between sample clients (`Riya Shah`, `Arjun Mehta`, `Kabir Rao`, or any custom phone number) without manual login friction.

### Side B: Nutritionist Dashboard
1. **Comprehensive Clinic Schedule**:
   - Complete list of consultations showing client name, direct `tel:` link, formatted date, time, and status.
2. **Color-Coded Status Management**:
   - Distinct, accessible status pills: `Booked` (Blue), `Done` (Green), `No-show` (Coral Red).
   - Instant inline status changes that immediately update clinic KPIs and reflect back in the client view.
3. **Manual Entry & Today Filter**:
   - Collapsible form to manually book walk-ins or phone calls with active double-booking checks.
   - **"Today" Filter**: 1-click filtering showing consultations scheduled for the current clinic day.
4. **Practice Analytics & No-Show Radar**:
   - **Today's count**: Live tally of visits scheduled today.
   - **Total Booked**: Active forward pipeline.
   - **No-Show Rate**: Clinical attendance metric calculated as `No-shows / (Done + No-shows)`.
5. **One-Tap WhatsApp Reminders**:
   - Pre-fills a personalized confirmation/reschedule message via `https://wa.me/` for upcoming clients.
6. **"Past: Please Update" Alert Tag**:
   - Flags consultations that have elapsed without an updated status so practice records remain clean.
7. **↺ Reset Sample Data Utility**:
   - One-click restore button allowing evaluators to reset the application back to the 5 sample records at any time.

---

## 🧠 Sample Data Normalization

The assignment dataset includes varied formatting styles:
- **Dates**: `24/10/2026` (DD/MM/YYYY), `2026-10-24` (ISO), `25-10-2026` (DD-MM-YYYY).
- **Times**: `10:00`, `11:30am`, `4 pm`, `14:00`.
- **Phones**: Spaced (`98200 11111`), unspaced (`9820011111`), and empty (`Meera Iyer`).

### How It Was Handled:
- Dates are converted to ISO `YYYY-MM-DD` using regex parsers (interpreting `24/10/2026` as day/month since there is no month 24).
- Times are normalized to standard 24-hour `HH:mm` format.
- Phone numbers are stripped of non-digits, normalized to 10 digits, and formatted as `98200 11111`.
- Riya Shah's two entries share the same phone and are correctly unified under one client profile with two separate visits.
- Meera Iyer has no phone number and is cleanly rendered with a "No phone" label without inventing dummy data.

---

## 📁 Repository Structure

```
├── index.html                   # Complete single-file application (HTML, CSS, JS)
├── Documentation.pdf            # 6-section technical documentation & design notes
├── Part1-Answer.pdf             # Strategy & workflow for solving the 25% no-show problem
├── chat-log.md                  # Raw AI prompts, questions, bug catches & iterations
├── chat-log.pdf                 # Printable PDF version of the development chat log
├── Kayro-Intern-Assignment.pdf  # Original assignment specification
└── README.md                    # Project documentation & evaluator guide
```

---

## 🛠️ Design & Tech Stack

- **Design System**: Tailored wellness palette (Sage `#1f6f6b`, Mint `#e6f3f1`, Peach `#f8cfb8`, Figtree typography, dark mode support).
- **Core Engine**: Vanilla JavaScript (ES6+), zero frameworks, zero external runtime dependencies.
- **Persistence**: Browser `localStorage` with reactive `window.storage` event synchronization.
