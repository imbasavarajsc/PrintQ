# 🖨️ PrintQ — Smart Campus Print Queue & Rush-Hour Estimator (PS-24)

**Event:** OWASP NIE VibeSprint 1.0 | **Problem Statement:** PS-24 (`PrintQ`)  
**Live Deployed URL:** https://imbasavarajsc.github.io/PrintQ/  
**GitHub Repository:** https://github.com/imbasavarajsc/PrintQ  

---

## 🟨 DOC-01: README & Quickstart

### Project Purpose
**PrintQ** eliminates physical crowding at campus print shops by letting students submit print job metadata (document name, page count, copies, job type, and colour preference) to receive an instant **Token ID** and transparent **Estimated Pickup Time**, while giving the shop operator a live **Rush-Hour Queue** with status controls, manual reordering, and a strict single-job Urgent priority override.

### Key Features
- **Split & Dedicated Views:** Toggle between **Split View (Judge Demo)**, **Customer Token View**, **Shop Operator Queue**, and **6 Doc Cards Dossier**.
- **Visible Estimation Formula:** Editable assumptions (`B&W = 4s/pg`, `Colour = 12s/pg`, `Base Handling = 25s`) + **Job Type Finishing** (`Loose Sheets +0s`, `Duplex +15s`, `Spiral Bound +60s`, `Hardbound +90s`) + **1.25x Rush-Hour Multiplier**.
- **Minute-40 Twist (Single Urgent Override + Delay Impact Modal):** Shop operators can mark **only 1 active job urgent at a time (1/1 slot)** and must explicitly confirm the exact delay impact (`+Xm Ys`) on other queued jobs before promotion.

### No-Secret Setup Steps (30 Seconds)
1. **Zero Dependencies / No `.env`:** No API keys, logins, databases, or Node/npm installs required.
2. **Run Locally:** Clone `https://github.com/imbasavarajsc/PrintQ.git` and double-click `index.html` in any modern browser.
3. **What to Click Within 30 Seconds:**
   - Click **"✨ Auto-Fill Sample"** → **"🖨️ Submit Print Job & Generate Token"** (Left Panel).
   - Click **`Printing` / `Ready`** or **`↑` / `↓`** on any job card (Right Panel).
   - Click **"⚡ Mark Urgent"** on any queued job → Check the delay confirmation box → Click **"⚡ Confirm Delay & Mark Urgent"**.

### Demo Data & Known Limitations
- Pre-populated with 4 fictional NIE student print jobs (`#PQ-100` to `#PQ-103`).
- Metadata-only simulation (no physical PDF binary upload or hardware printer spooler driver).

---

## 🟪 DOC-02: Judge Runbook (Numbered 60-Second Demo Script)

- **Starting State:** App loads in **Split View** with **Rush-Hour Mode ON (1.25x)** and 4 sample jobs (`#PQ-101` Printing, `#PQ-102` & `#PQ-103` Queued, `#PQ-100` Ready). Urgent Slot shows `0 / 1`.
- **Action 1 (0–15s — Step 01 Submit & Estimate):** In the Left Panel, click **"✨ Auto-Fill Sample"** (or enter `12` pages, `2` copies, `Spiral Bound`, `Full Colour`) and click **"🖨️ Submit Print Job & Generate Token"**.
  - *Expected Output:* Token **`#PQ-105`** is generated in the Live Customer Token Receipt with exact math breakdown (`Backlog + Job Time × 1.25x Rush`) and added to the shop queue.
- **Action 2 (15–35s — Step 02 Queue Status & Reorder):** In the Right Panel (Shop Queue), click **`↑`** on `#PQ-105` to move it up, and click **`Ready`** on `#PQ-101`.
  - *Expected Output:* Queue reorders instantly and every student's wait time and pickup timestamp recalculate dynamically.
- **Action 3 (35–60s — Step 03 & Minute-40 Twist Appears Here):** Click **"⚡ Mark Urgent"** on `#PQ-105`.
  - *Expected Output (TWIST):* The **Urgent Delay Impact Confirmation Modal** opens, listing every queued job ahead (`#PQ-102`, `#PQ-103`) and their exact delay penalty (`+Xm Ys`). Check **"I confirm that other jobs in the queue may be delayed"** and click **"⚡ Confirm Delay & Mark Urgent"**. `#PQ-105` moves to `#1` waiting slot with an `⚡ ACTIVE URGENT (1/1)` badge, delayed jobs show `⚠️ Delayed +Xm Ys`, and other jobs show `🔒 1/1 Urgent Active`.
- **Simulated Features:** Document upload is simulated via filename metadata; printer hardware speed is simulated via mathematical formula.

---

## 🟥 DOC-03: Five-Test Checklist

| Test # | Category | Exact Steps to Execute | Expected Result |
| :--- | :--- | :--- | :--- |
| **Test 1** | **Invalid Input Test** | Clear the `Document Name` field or set `Pages per Copy` to `0` (or `-5`), then click **Submit Print Job**. | Submission is blocked; a red validation banner appears: `⚠️ Validation Error: Please enter a valid Student Name, Document Name, Pages (1–500), and Copies (1–100).` |
| **Test 2** | **Happy Path Test** | Enter `Student: Riya`, `Doc: Lab.pdf`, `Pages: 10`, `Copies: 1`, `Job Type: Loose Sheets`, `B&W`, then click **Submit**. | Generates Token `#PQ-105`, updates Customer Token Receipt with exact wait math, and appends job to Shop Queue with status `Queued`. |
| **Test 3** | **Twist Check (Urgent + Delay Lock)** | Click **⚡ Mark Urgent** on `#PQ-103` → Check confirmation box → Confirm. Then try clicking **🔒 1/1 Urgent Active** on `#PQ-102`. | `#PQ-103` moves to front of waiting queue; `#PQ-102` shows `⚠️ Delayed +2m 5s`; attempting a 2nd urgent job is blocked with a toast alert enforcing the 1-urgent-job limit. |
| **Test 4** | **Reset / Refresh Check** | Submit a job and mark it urgent, refresh the browser (`F5`), then click **↺ Reset** in the top navbar. | On `F5`, `localStorage` preserves the custom state. Clicking **↺ Reset** restores the exact 4 default demo jobs and clears urgent locks. |
| **Test 5** | **Boundary Case Test** | Set `Pages per Copy` to `500` and `Copies` to `100` (max boundary `50,000` pages) with `Colour` and `Hardbound Report`. | App calculates large duration (`~12500m`) and cost (`₹5,06,000`) cleanly without UI overflow or NaN crashes. |

---

## 🟨 DOC-04: User Quickstart (Four-Step First-Time Guide)

1. **What the App Does:** PrintQ lets students book a print queue slot from their browser and gives print-shop operators a live priority queue with automated wait-time math.
2. **What Input to Provide:** Enter your Name/USN, Document Filename (`.pdf`/`.docx`), Page Count (`1–500`), Copies (`1–100`), Finishing Type (`Loose`, `Duplex`, `Spiral`, `Hardbound`), and `B&W` or `Colour`.
3. **What Result to Expect:** You immediately receive a numbered Token (`#PQ-105`), estimated wait time in minutes/seconds, exact pickup clock time, and total cost in `₹`.
4. **How to Recover from a Mistake:**
   - *Wrong input before submitting:* Edit any field or click **✨ Auto-Fill Sample**.
   - *Accidental Urgent promotion:* Click **`✓ Urgent Active (Release Slot)`** on the job card to remove urgent priority and clear delays.
   - *Corrupted demo queue:* Click **`↺ Reset`** in the top-right header to restore factory sample data.

---

## 🟪 DOC-05: Data & Privacy Note

- **Fields Stored Locally:** `id`, `token`, `student` (fictional name/roll number), `docName` (filename string only), `pages`, `copies`, `color`, `jobType`, `status`, `urgent`, and `delayedBySec`.
- **Storage Mechanism:** Uses browser `window.localStorage` under key `PRINTQ_PS24_FINAL_V3` so state persists across page refreshes during judging.
- **Demo / Fake Data:** All pre-loaded student names (`Aditya Nair`, `Sneha Kulkarni`, `Karthik Rao`, `Meera Joshi`) and roll numbers (`NIE-CS-088`, etc.) are 100% fictional.
- **Zero External Transmission & Zero Secrets:** The application makes **zero network POST/GET requests** to external servers, databases, or third-party APIs. No `.env` files, API keys, passwords, or real personal data are included or transmitted anywhere.

---

## 🟥 DOC-06: Handoff & Limitations

- **Main Files & Components:**
  - `index.html`: Single-file zero-build application containing semantic HTML structure, Tailwind CSS styling, and modular Vanilla JS logic.
  - `README.md`: Complete 6-card documentation dossier (`DOC-01` through `DOC-06`).
- **Where Core Logic Lives (`<script>` inside `index.html`):**
  - `calcJobDurationSec()` & `computeQueueMetrics()`: Dynamic wait-time engine combining page count, colour speed, job-type finishing, and the 1.25x Rush-Hour multiplier.
  - `handleJobSubmit()`: Form validation and token generator.
  - `openUrgentModal()` & `confirmMarkUrgent()`: Enforces the Minute-40 Twist (calculates affected jobs, enforces mandatory delay confirmation checkbox, and locks the 1/1 urgent slot).
- **How Demo Data is Reset:** Clicking **`↺ Reset`** invokes `resetDemoData()`, which overwrites `localStorage` with a deep clone of `DEFAULT_JOBS` and triggers `recalculateAll()`.
- **What is Deliberately Mocked:** PDF file parsing (page count is user-entered rather than extracted from binary PDF uploads), printer hardware spooling, and UPI payment gateway settlement.
- **Top 3 Improvements for Real Production Deployment:**
  1. **WebSocket / Firebase Real-Time Sync:** Synchronize student mobile browsers with the shop operator dashboard across separate devices in real time.
  2. **Client-Side `pdf.js` Page Counter:** Automatically read page count and colour density when a student selects a local PDF file.
  3. **UPI QR Payment & WhatsApp/WebPush Alerts:** Generate dynamic UPI QR codes for exact print cost and notify students when their token status changes to `Ready`.