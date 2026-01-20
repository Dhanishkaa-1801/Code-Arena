# Code Arena

Code Arena is a college‑focused competitive programming platform that runs ICPC‑style contests with **stream‑wise separation**, **basic proctoring**, and a **permanent practice bank** of 100+ LeetCode‑style problems.

> Built solo as a hackathon project to solve how my own college runs coding contests.

---

## 🌟 Inspiration

I built Code Arena out of my own experience struggling to run and join fair coding contests in college.

Most existing judges felt either too generic or too rigid for how our college actually works. Streams (departments), time windows, cheating concerns, and practice after contests weren’t handled in a way that made sense on campus. I wanted a single place where:

- contests could be stream‑wise and fair,
- practice could be open to everyone afterwards, and  
- admins could actually see what was going on.

Code Arena is my attempt to give my college its own competitive programming arena instead of bending other platforms to fit us.

---

## ✅ What it does

Code Arena is a college‑focused competitive programming platform that:

### 🏟 Stream‑based contests

Students are auto‑grouped into 3 streams based on department. Live contests can be restricted to a stream or opened to all.

- **Stream 1 – Core / General**  
  AERO, BME, CIVIL, MECH, R&A
- **Stream 2 – Electrical**  
  ECE, EEE, EIE
- **Stream 3 – CS / IT**  
  CSE, IT, AIDS / AI&DS, M.Tech

Wrong‑stream students can still **view** problems and the leaderboard, but the editor and submit button are locked with a clear message.

### 🧮 ICPC‑style scoring with a fairness tweak

Each student’s total time is based on when they start working on the contest and when they finish, plus a penalty for tab switching:

\[
\text{Total Time} = (\text{finish time} - \text{start time})
\;+\; 20 \text{ minutes} \times \text{(tab switches)}
\]

- **Score** = number of distinct problems solved (`Accepted`).
- **Leaderboard ordering**:
  1. Higher score
  2. Lower total time (including penalty)

### 📚 Practice mode + permanent problem bank

- After a contest ends, its problems appear in a **global Problem Bank**.
- Practice mode has **no stream restriction** and does not affect past rankings.
- The platform ships with **100+ pre‑loaded LeetCode‑style problems**, so students always have something to solve even when no contest is live.

### 🔐 Basic proctoring

- During contests, the code editor watches for **tab switches**:
  - 1st switch → warning (1/3)  
  - 2nd switch → warning (2/3)  
  - 3rd switch → **disqualified**
- Disqualified users cannot submit for that contest — this is enforced on the **server**, not only in the UI.
- Admins can see:
  - `tab_switches`  
  - `run_count` (how many times “Run Code” was pressed)
- Students only see their **score and total time**.

### 🧑‍💻 Admin dashboard

- `/admin/contests` shows **Scheduled**, **Live**, and **Archived** contests.
- Within each status, contests are grouped into:
  - Stream All  
  - Stream 1 – AERO / BME / CIVIL / MECH / R&A  
  - Stream 2 – ECE / EEE / EIE  
  - Stream 3 – CSE / IT / AI&DS / M.Tech
- Quick actions:
  - View contest leaderboard  
  - Manage contest problems  
  - Delete contest (with protection for the practice collection)

---

## 🛠 Tech Stack

- **Frontend:** Next.js 14 (App Router), TypeScript, Tailwind CSS  
- **UI:** shadcn/ui, Radix UI, Lucide Icons  
- **Backend & DB:** Supabase (PostgreSQL, Auth, Realtime, Row Level Security)  
- **Judge:** Judge0 CE (direct HTTP API)  
- **Deploy:** Vercel (frontend + API routes)

---

## 🚀 Getting Started

### 1. Prerequisites

- Node.js ≥ 18  
- npm / pnpm / yarn  
- A **Supabase** project  
- A **Judge0 CE** endpoint (public or self‑hosted)

---

### 2. Clone & Install

~~~bash
git clone https://github.com/<your-username>/Code-Arena.git
npm install
~~~

---

### 3. Environment Variables

Create a `.env.local` file in the project root:

~~~bash
touch .env.local
~~~

Add the following (replace with your own values):

~~~env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=<your-supabase-url>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<your-supabase-anon-key>

# Optional service role (only if some utilities need it)
SUPABASE_SERVICE_ROLE_KEY=<optional-service-role-key>

# Judge0
JUDGE0_API_URL=https://ce.judge0.com   # or your own Judge0 CE URL

# App
NEXT_PUBLIC_SITE_URL=http://localhost:3000
~~~

---

### 4. Supabase Setup (short version)

In your Supabase project:

1. Create the core tables used by the app:
   - `profiles` – user info (`id`, `full_name`, `roll_no`, `department`, `year`, `role`, `profile_complete`, …)
   - `contests` – (`id`, `name`, `description`, `start_time`, `end_time`, `stream`)
   - `contest_problems` – problems per contest
   - `problem_test_cases` – hidden input/output pairs
   - `submissions` – submissions with verdicts, time, memory
   - `contest_monitoring` – `user_id`, `contest_id`, `tab_switches`, `run_count`, `first_opened_at`, `last_warning_at`

2. Enable **Row Level Security (RLS)** on these tables.  
3. Add policies so that:
   - Each user can read/update **their own** profile and submissions.
   - Admins can read all submissions.
   - Any authenticated user can `SELECT` from `contest_monitoring`, but only the owner (matching `auth.uid()`) can `INSERT`/`UPDATE`.

> The exact SQL can be adapted from the code in `app/actions/submissions.ts` and `app/actions/proctor.ts`.

---

### 5. Run the app locally

**Development:**

~~~bash
npm run dev
# visit http://localhost:3000
~~~

**Production build:**

~~~bash
npm run build
npm start
~~~

---

## 🧠 What I learned

Through building Code Arena, I learned:

- **Backend rules matter more than UI**

  Stream checks, contest windows, and disqualifications all **must** live on the server. The UI is just a hint; security and fairness live in the backend.

- **Row Level Security is powerful and easy to get wrong**

  Whenever data “disappeared,” the first place to check was RLS. I got used to writing and debugging policies and thinking carefully about who can select vs update vs insert.

- **AI is great for speed, but product decisions are human**

  AI tools helped me scaffold and refactor quickly, but they can’t decide contest rules or academic fairness. Those had to come from my own experience and manual testing.

- **Building for my own environment helps**

  Because I’m designing this for my own college, decisions about streams, penalties, and practice mode were grounded in real issues I’ve seen, not abstract ideas.

---

## 📄 License

This is a personal / academic project.  
