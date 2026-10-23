# ELLORA AI V2 — Cloudflare PAGES source package

**IMPORTANT: This ZIP contains the SOURCE files. Do not upload it as one ZIP file to GitHub and expect deployment. Extract it first.**

## What works after configuring the backend
- Responsive teacher/student interface and sign-in with separate roles
- Teacher-created classes and invitation codes
- Teacher-uploaded readable text/PPTX materials, AI draft lessons with teacher approval
- Lessons, tasks, multiple-choice assessment, submissions, reflections, class information, progress views
- AI chat via server-side OpenAI API, when configured
- Browser text-to-speech and an animated **illustration** of the requested white/blue robot (not 3D or accurate lip sync)

## iPad deployment — follow exactly
1. Open Files, tap this ZIP, and let iPad extract the folder.
2. Open your GitHub repository. Upload the **contents of the extracted folder**, keeping `index.html`, `schema.sql`, `robot-reference.jpg`, `README.md` at the repository root and `functions/api/[[path]].js` inside its nested folders. Do NOT upload just the ZIP.
3. In Cloudflare Dashboard → Workers & Pages → Create application → **Continue to Pages** (not Workers) → Connect to Git → select the repository → use the **Pages** deployment workflow. Build command blank, output directory `/` or `.` as permitted by the dashboard. If a root-output setting is rejected, use a Pages-compatible static assets directory and move the static files there, keeping `functions/` at the project root.
4. Create a Cloudflare **D1** database. Run the contents of `schema.sql` in its Console (run only once on an empty database).
5. In the **Pages project** Settings → Bindings, add D1 database binding named **DB**. In Settings → Variables and Secrets, add `SESSION_SECRET` (long, random) and `OPENAI_API_KEY` as **encrypted secrets**. Never paste your key in GitHub or HTML. Optionally set `OPENAI_MODEL` to a supported model.
6. Redeploy Pages. Test `/api/me` (a 401 without login is expected, a 503 means missing binding/secret), then register a teacher and a student test account, create a class and join with its code. Use **fictional test data only**.
7. After verifying the app, you may retire the earlier Worker deployment to avoid confusion, but do not delete it until the Pages app is verified.

## Important limitations
This is an enhanced functional **prototype**, not a finished school production service. It does NOT provide real-time spoken AI conversation, accurate robot lip sync, scanned worksheet OCR/AI marking, full PPTX media rendering, comprehensive ELL placement tests, secure formal exams, school-verified teacher accounts, or device kiosk lock. Voice output uses device browser TTS. No secrets or D1 are bundled. Real student data requires school authorization, child privacy review, role provisioning, data retention rules, rate limits, audit logs, and secure authentication hardening. Do not use this prototype with real schoolchildren's private information.

### Debugging
- If you see the old 'Maya AI teacher preview' page, your deployment still serves the old `index.html`. Verify Git commit, Pages project and redeploy.
- If you see `No URLs enabled`, you are in a Worker; a Pages project has its own `*.pages.dev` URL.
- If login shows 503, check D1 binding `DB` and `SESSION_SECRET`, then redeploy.
- If AI fails, check `OPENAI_API_KEY`, account billing/quota, and server logs. Avoid sharing keys or tokens.
- GitHub cannot deploy a Pages Function from a ZIP kept as a single file in the repository.
