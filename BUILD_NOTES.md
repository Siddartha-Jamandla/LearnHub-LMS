# LearnHub — new separate build (9 Nov 2026)

Brand name is **LearnHub** — one word, no space. Owned by **CareerPlus Co-Private Limited**.

This is a NEW, separate copy. The live LearnHub at
https://learnhubapp.pythonanywhere.com/ was NOT touched.

## What it is

Full-stack LMS, Python standard library only + SQLite + vanilla JS
(no Django / Flask / React — same stack as the live site, so it runs
free on PythonAnywhere with no card).

- `backend/app.py` — ~4,967 lines, 93 routes
- Frontend — 30+ pages, dark/light theme, one violet accent
- `wsgi.py` — PythonAnywhere WSGI adapter (stdlib only)

## Spec checklist

- [x] Public exploration: course catalog, syllabi, intro/preview lessons
      open without sign-in; advanced lessons, assignments, videos and
      the tutor need an account.
- [x] OTP auth: register at `/api/register`, then
      `POST /api/auth/request-otp` + `POST /api/auth/verify-otp`.
      Demo mode — the OTP is shown on screen, no SMS provider
      (free, no-card constraint). Only the SHA256 hash is stored.
- [x] AI tutor: `POST /api/tutor` — step-by-step, from the current
      lesson's own material, with micro-tasks. Closes on outside
      click, Escape, X button, and after 60s idle.
- [x] Personal notes: `GET/POST/PUT/DELETE /api/me/notes`,
      per course/lesson, tied to the signed-in user (`notes.html`).
- [x] Dashboard/profile: `/api/me/dashboard` — enrolled courses,
      progress %, purchase history, XP/streak/badges, certificates.
- [x] Proctored assignments: `exam.html` + `/api/assignments/:id/*`
      — copy/paste and right-click disabled, tab-switch / window-blur
      counted as flags. 5 seeded assignments, including
      "Reverse an Array in C" (course 13).
- [x] Help desk / Q&A: floating tutor assistant + lesson discussions
      (`/api/discussions`, moderated in admin).
- [x] Payments: test-mode only (`razorpay-test-mock`), no real
      charges. Verify-then-enrol, revenue syncs to admin stats.
- [x] Security shield: blocks SQL-injection / traversal patterns
      (tested: `?q=' OR 1=1--` → 403 "blocked by the LearnHub
      security shield"), rate-limits OTP + logins, logs blocked
      attempts to the security log.
- [x] Super Admin Security & Error Log Center: `/api/admin/security`,
      `/api/admin/audit-logs`, `/api/admin/security-stats`,
      `/api/errors` — security alerts, audit log, error self-fix log.
- [x] Admin user management: `/api/admin/users` — profiles, login
      timestamps, roles, status, role changes.
- [x] Hidden super admin: `super-admin.html` returns 404 when
      signed out, and only the admin Gmail account can unlock it.
      No public link to it anywhere.
- [x] Real-time course CRUD: `POST/PUT/DELETE /api/admin/courses/:id`
      and lessons, quizzes, tasks — instant, no caching layer.

## Courses (16, seeded)

Flagships are **C + DSA, Python, Java, Web Development** — NOT C++.
There is no C++ course in this build (that was a past mistake,
corrected 9 Nov 2026).

1. Complete Web Development Bootcamp — ₹999
2. JavaScript: From Beginner to Advanced — ₹799
3. Python Programming Masterclass — ₹899
4. Machine Learning with Python — ₹1,499
5. Deep Learning & Neural Networks — ₹1,799
6. Data Science with SQL & Python — ₹1,199
7. UI/UX Design Fundamentals — ₹899
8. React - Build Modern Web Apps — ₹1,299
9. Digital Marketing Masterclass — ₹699
10. Django Full-Stack Web Development — ₹1,399
11. TypeScript: Static Typing for JavaScript — ₹1,099
12. Git & GitHub: Version Control Mastery — ₹599
13. C Programming from Zero to Hero — ₹899
14. Data Structures & Algorithms — ₹1,299
15. Java Programming Masterclass — ₹999
16. SQL Essentials: Queries to Databases — ₹799

## Local test results (9 Nov 2026, ~21:15 IST, port 8101)

- `GET /api/health` → 200 `{"ok": true, "service": "learnhub"}`
- `GET /api/courses` → 16 courses, 0 with "C++" in the title
- Homepage title → "LearnHub — coding courses for B.Tech students"
- Register → 201, OTP request → 200 (demo OTP shown),
  OTP verify → 200, token issued
- Tutor → 200, step-by-step reply from lesson material
- Notes create + list → 201 / 200
- C assignments → 200, "Reverse an Array in C" present
- Non-enrolled lesson → 403 "You must be enrolled…"
- SQLi probe → 403 blocked by the security shield
- `/api/admin/security` signed out → 403
- `super-admin.html` signed out → 404 (hidden, as designed)
- No spaced "Learn Hub" left in code/pages (only in the old
  TEST_RESULTS.md history file, left untouched on purpose)

## To make this one live (needs Siddartha)

1. Pick a new GitHub repo name (suggestion: `LearnHub-LMS-Website-v2`
   or a name of his choice) — this copy has no git history yet.
2. Pick a new PythonAnywhere account / subdomain — the free tier
   allows one web app per account, and `learnhubapp` is already used
   by the live site. Free, no card needed.
3. Push (one approval tap from him), then deploy with the same
   `wsgi.py` flow as the live site, pull + reload, and verify live.
4. First thing after going live: register with
   siddarthajamandla@gmail.com (via OTP) to claim admin — until then
   that Gmail is the open admin window, same as the live site.
