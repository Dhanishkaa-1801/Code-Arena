# Code Arena

Code Arena is a college‑focused competitive programming platform that runs ICPC‑style contests with **stream‑wise separation**, **basic proctoring**, and a **permanent practice bank** of 100+ LeetCode‑style problems.

> Built solo as a hackathon project to solve how our own college runs coding contests.

---

## 🌟 Overview

### Why I built it

Existing online judges didn’t match how our college actually works:

- Different **streams/departments** need separate contests.
- We need a clear **leaderboard** that accounts for time and basic cheating signals.
- After contests, everyone wants a **practice mode** with the same problems.

Code Arena gives our college its own “arena” with:

- Stream‑based contests (Core, Electrical, CS/IT)
- ICPC‑style scoring with a tab‑switch penalty
- Proctoring warnings + disqualification
- A built‑in practice bank of 100+ problems

---

## ✅ Core Features

- **Stream‑based contests**
  - Streams are auto‑derived from `department`:
    - **Stream 1:** AERO, BME, CIVIL, MECH, R&A  
    - **Stream 2:** ECE, EEE, EIE  
    - **Stream 3:** CSE, IT, AIDS/AI&DS, M.Tech
  - Contests can target a specific stream or “All Streams”.
  - Wrong‑stream students can view problems & leaderboard but **cannot submit**.

- **ICPC‑style leaderboard with fairness**
  - Score = number of distinct problems solved (`Accepted`).
  - Total Time per user:
    \[
    \text{Total Time} = (\text{finish time} - \text{start time})
    \;+\; 20 \text{ minutes} \times \text{(tab switches)}
    \]
  - Leaderboard is sorted by **Score desc**, then **Total Time asc**.

- **Practice mode + pre‑loaded problems**
  - Once a contest ends, its problems move to a **Problem Bank**.
  - Practice has **no stream restriction**.
  - The app ships with **100+ pre‑loaded LeetCode‑style problems**.

- **Basic proctoring**
  - During contests, the editor watches for **tab switches**:
    - 1st + 2nd switch → warnings  
    - 3rd switch → **disqualified** (submissions blocked, enforced on server)
  - Admins see `tab_switches` and `run_count` on the leaderboard.
  - Students only see their **score and aggregated time**.

- **Admin dashboard**
  - `/admin/contests` shows **Scheduled**, **Live**, **Archived** contests.
  - Inside each status: **Stream All / 1 / 2 / 3** columns.
  - Quick actions: View leaderboard, manage problems, delete contest.

---

## 🛠 Tech Stack

- **Frontend:** Next.js 14 (App Router), TypeScript, Tailwind CSS  
- **UI:** shadcn/ui, Radix UI, Lucide Icons  
- **Backend / DB:** Supabase (PostgreSQL, Auth, Realtime, Row Level Security)  
- **Judge:** Judge0 CE (direct HTTP API)  
- **Deploy:** Vercel (frontend + API routes)

---

## 🚀 Getting Started (Local)

### 1. Prerequisites

- Node.js ≥ 18  
- npm / pnpm / yarn  
- A Supabase project  
- A Judge0 CE endpoint (public or self‑hosted)

---

### 2. Clone & Install

```bash
git clone https://github.com/<your-username>/Code-Arena.git
cd Code-Arena
npm install
