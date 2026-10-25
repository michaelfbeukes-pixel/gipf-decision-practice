# GIPF Decision Practice Studio

A frontend-only pilot for Option 1: AI-supported scenario and role-play learning. The application contains 30 fictional GIPF-relevant scenarios with GIPF-aligned colours, logo usage, typography and circular brand geometry.

## Curriculum

Five scenarios are provided in each of six learning areas:

- Fraud detection
- Risk culture
- Policy and compliance
- Business continuity
- Opportunity management
- Member service

The library supports keyword search, topic filters, completion status and best scores. Each scenario includes two consequential decisions, four alternatives per decision, a written role-play, scripted character responses, four response-rubric criteria, three learning objectives and three facilitator debrief questions. A facilitator view provides all 30 debriefs and a downloadable text guide.

## Prototype boundaries

- **No live AI model is connected.** Scenarios and compliance-critical choices are controlled content. Written-response feedback uses transparent keyword rules rather than inferred employee competence.
- Scores, attempts and completion are stored only in the current browser's `localStorage` under `gipf-practice-progress`. Written responses are not persisted.
- Results do not synchronise between learners, browsers or devices. Clearing browser data removes the local record. This is not an institutional learning record or LMS.
- All cases are fictional. GIPF policy owners must validate decision rules, escalation routes and scoring rubrics before operational use.
- The prototype is not intended for disciplinary decisions, employee ranking or automated monitoring.
- Central accounts, cross-device records, dashboards, LMS integration and live AI would require a properly governed backend and further privacy, security and AI review.

## Run locally

Use a supported Node.js LTS environment, then run:

```bash
npm ci
npm run dev
```

Open the local address printed by Vite.

## Create a production build

```bash
npm ci
npm run build
```

The deployable static site is generated in `dist/`. Test it locally with:

```bash
npm run preview
```

Do not deploy `src/`, `node_modules/` or the project root as the public website. Publish only the contents of `dist/` for this frontend-only version.

## Deployment option A: Kimi preview and manual publishing

1. Open the saved website version card in the Kimi conversation.
2. Use the preview to complete content, brand, accessibility and policy-owner acceptance checks.
3. If approved, click **Publish** in Kimi. Publishing is a manual user action; saving a version does not make the site public.
4. Share the resulting public address only with the intended pilot audience. This version has no user accounts, so a public link is reachable by anyone who has it unless the platform provides additional access controls.
5. Use Kimi's code export if GIPF wants its own team to maintain or independently host the source.

## Deployment option B: Personal GitHub Pages test site

The project includes `.github/workflows/deploy.yml`, which builds and publishes the application automatically. It uses relative asset paths, so it works at a repository address such as `https://<username>.github.io/gipf-decision-practice/` as well as at a domain root.

1. Export the complete project and unzip it on your computer.
2. In your personal GitHub account, create a **public** repository named `gipf-decision-practice`. GitHub Free supports Pages from public repositories. Do not initialise it with another README, licence or `.gitignore`.
3. Open the repository, select **Add file → Upload files**, and upload the **contents** of the extracted project folder. Include `src`, `public`, `.github`, `package.json`, `package-lock.json` and the other project files. Do not upload `node_modules`, `dist` or `.git`.
4. Commit the files to the `main` branch.
5. Open **Settings → Pages**. Under **Build and deployment → Source**, select **GitHub Actions**; do not select **Deploy from a branch**.
6. Open the **Actions** tab and wait for **Build and deploy to GitHub Pages** to finish. Both the `build` and `deploy` jobs should show green ticks.
7. Return to **Settings → Pages** and open the published address, normally `https://<username>.github.io/gipf-decision-practice/`.
8. If deployment did not start automatically, open **Actions → Build and deploy to GitHub Pages → Run workflow**, select `main`, and run it.

A GitHub Pages test site is publicly accessible. Use fictional information only. Hosting does not change the data model: attempts, scores and completion remain in each tester's browser, and written responses are not saved. There is no central learner report.

For future edits, upload the changed source files and commit them to `main`; the workflow will rebuild and republish automatically. Keep the repository and workflow when moving to an organisational account later.

## Deployment option C: GIPF-controlled static hosting

1. Export the project and place it in GIPF's approved source-control repository.
2. On the approved build runner, use a clean Node.js LTS environment and run `npm ci && npm run build`.
3. Copy the **contents** of `dist/` to the document root of GIPF's approved static web server or internal web application gateway.
4. Configure HTTPS, a suitable hostname, security headers, backups and rollback according to GIPF standards. If the pilot must be staff-only, enforce authentication at the hosting gateway; the application itself does not provide login.
5. Confirm that `/`, `/gipf-logo.png`, `/favicon.svg` and the generated `/assets/` files load over HTTPS. The current interface does not use client-side URL routes, so no SPA rewrite rule is needed.
6. Complete a production smoke test: open a scenario, make both decisions, submit a response, view feedback, retry, use library filters and download the facilitator guide.
7. Obtain brand-owner and policy-owner approval before wider rollout, and replace the reference logo with GIPF's approved master artwork if required.

A minimal Nginx document-root configuration can use:

```nginx
location / {
    root /path/to/gipf-decision-practice/dist;
    try_files $uri $uri/ /index.html;
}
```

Apply GIPF's own TLS, access-control, logging and security-header standards; do not treat this snippet as a complete production configuration.

## Release checklist

- Confirm all 30 scenarios and the facilitator guide reflect approved GIPF policy.
- Verify logo usage and brand approval with GIPF's brand owner.
- Review npm dependency findings and apply tested upgrades before production release.
- Test current desktop and mobile browsers, keyboard navigation and accessibility requirements.
- Decide whether browser-local demonstration progress is acceptable. If not, commission a backend rather than treating local progress as an official record.
- Pilot with a small, clearly informed group and communicate that no live AI assessment is taking place.
- Keep a versioned release and rollback package, and assign scenario content owners for future updates.

## Main source files

- `src/App.tsx` — application views, scenario flow, scoring and local progress.
- `src/scenarios.ts` — curriculum aggregator and category list.
- `src/core-scenarios.ts` — original three scenarios.
- `src/scenario-expansion-a.ts` and `src/scenario-expansion-b.ts` — additional scenario content.
- `src/scenario-factory.ts` and `src/scenario-types.ts` — typed scenario authoring helpers.
- `src/App.css` — responsive interface styles.
- `src/index.css` — GIPF-aligned design tokens, typography and global foundations.
- `public/gipf-logo.png` — reference logo asset; see `CREDITS.md`.

The interface uses locally bundled Montserrat and Public Sans fonts and Lucide icons. No external runtime font or image requests are required.
