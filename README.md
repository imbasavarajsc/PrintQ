# PrintQ — Campus Print Queue & Rush-Hour Estimator (PS-24)

**Event:** OWASP NIE VibeSprint 1.0 | **Problem Card:** PS-24 (`PrintQ`)

## 🚀 Live Prototype & Architecture
- **Zero-Dependency Browser Prototype:** Built with semantic HTML5, Tailwind CSS, and Vanilla JavaScript (`localStorage` state persistence). No `.env`, external APIs, or hardware printer drivers required.
- **Split-View Interface:** Simultaneous **Customer Token View** (left pane) and **Shop Operator Queue** (right pane) for instant end-to-end judge verification.

## ✅ Core MVP & 3-Step Judge Verification Path
1. **Step 01 (Submit & Estimate):** Students enter fictional roll number, document name, page count, copy count, job type, and B&W/Colour choice to receive a unique token (`#PQ-104`) and dynamic wait-time breakdown.
2. **Step 02 (Shop Queue Controls):** Shop operators view the live queue, update statuses (`Queued` → `Printing` → `Ready` → `Completed`), and manually reorder jobs (`↑`/`↓`) with real-time wait recalculation.
3. **Step 03 (Urgent Override & Confirmation):** Operators can promote a job to Urgent priority via a dedicated confirmation modal.

## ⚡ Minute-40 Twist Implementation ("Rush-Hour Queue")
- **Page Count + Job Type + Colour Dynamic Estimation:** Calculates wait time using editable per-page speeds (B&W `4s/pg`, Colour `12s/pg`), job finishing types (`Loose Sheets +0s`, `Duplex +15s`, `Spiral Bound +60s`, `Hardbound Report +90s`), and a toggleable **1.25x Rush-Hour Multiplier**.
- **Single Urgent Job Constraint with Delay Confirmation:** The shop can mark **only one active job urgent at a time (1/1 slot)**. Promoting a job opens an impact modal listing every queued job that will be delayed, its exact added delay (`+Xm Ys`), and requires checking an explicit delay confirmation box before promotion. Delayed jobs display a visible `⚠️ Delayed +Xm Ys` badge in the queue.