# ELLORA — ELL AI Classroom (Cloudflare Pages + D1)

A responsive full-stack foundation for teacher-controlled ELL classes, Years 6–13. Includes teacher/student email-password login, invitation codes, PPTX text extraction, material storage, OpenAI-generated draft lessons, teacher review/publish, quizzes/homework/exams, submissions, scores, reflections, class scheduling, avatar animation, browser speech and AI chat.

## Important: deployment steps
1. Create a Cloudflare Pages project with this folder as the deployment source. For Pages Functions to work, use a Git-connected Pages project or deploy using Wrangler; simply dragging the ZIP to Pages may not deploy Functions.
2. Create a D1 database in Cloudflare, and run `schema.sql` against it (Cloudflare dashboard D1 console or Wrangler `wrangler d1 execute <database-name> --file=schema.sql --remote`).
3. In Pages project **Settings → Bindings**, add a D1 binding named **DB** pointing to that database.
4. In Pages project **Settings → Variables and Secrets**, add **SESSION_SECRET** as a long random secret and **OPENAI_API_KEY** as a secret. Optionally set **OPENAI_MODEL** (defaults to `gpt-4o-mini`; check current API model availability).
5. Redeploy, open your HTTPS Pages URL, create a teacher account, create a class, join using a separate student account and class code.
6. Upload a PPTX with selectable slide text or paste source material, generate AI draft, inspect/edit and publish, then create a homework/quiz/exam from it.

## Limitations — do not claim these work yet
- Animated teacher is a **CSS 2D avatar** with approximate mouth motion and browser text-to-speech, not a realistic 3D Memoji or accurate audio lip sync. Voice recognition is browser-dependent and may not work on iPad Safari.
- PPTX extraction handles selectable text, not scanned slides, embedded images, videos, PDF text, or `.ppt` legacy files. For PDFs, paste text manually.
- Teacher accounts are currently self-registered. **School production requires verified teacher provisioning, admin approval, consent, school data protection, rate limits, account recovery, and audit logs.** Do not upload real children's records to this prototype.
- The homework generator creates teacher-triggered tasks from AI lesson questions, not an automatic new unique 30-minute assignment every calendar day. Add Cloudflare Cron Triggers/Queues with approval policies for this.
- No automatic 30–40 minute placement test bank, writing grading, real-time speech API, worksheet handwriting recognition, full curriculum catalog, or monthly feedback surveys yet.
- Exam release scheduling is stored as a deadline but the UI currently shows all assigned tasks; implement release gating before high-stakes use. Score reporting covers auto-marked multiple-choice questions only. Do not use as a secure formal examination system.
- Session schedule is recorded, but student attendance check-in is API-only (not yet wired in the UI); no iPad lock/kiosk mode. iPad locking requires school-managed device settings.
- No cross-device sync without Cloudflare D1. Opening `index.html` locally displays the landing page, but login and live features require deployment.

## Security
Never put API keys into `index.html`. Secrets must be configured server-side. Cloudflare Pages Functions route `/api/*` to the server. The app stores a signed session token in localStorage; for a production school app prefer secure HttpOnly cookies and CSRF defenses. Set a strict CORS and CSP policy before production. Avoid uploading student-identifiable information until the school has approved the privacy architecture.

## Development
Use Wrangler Pages dev with a local D1 binding and configured secrets. Validate backend and API access after deployment. No live production account has been created or deployed by this package.
