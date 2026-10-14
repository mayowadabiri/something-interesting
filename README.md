# Job Hunt for Claude

A Claude plugin that runs your job hunt for you, using your own Google Drive
and your own CV:

- **Every morning:** finds jobs first posted in the last 3 days that fit you,
  rates your chance of an interview for each, tailors a copy of your CV for
  each good one, and adds them to your personal job tracker.
- **Every morning, a bit later:** finds the person hiring at matching companies,
  their public work email, and drafts a short cold email for you to send.
- **Any time:** share a job link and Claude tailors your CV, rates your chances
  and writes application answers.

Your master CV is never edited. Tailored CVs keep the exact same layout; only
keywords change, and nothing is invented. Nothing is ever sent or applied for
on your behalf.

## What you need

- A paid Claude plan (Pro, Max, Team or Enterprise).
- The **Google Drive** and **Google Docs** connectors, connected to your own
  Google account.
- Your CV as a Google Doc in your Drive (if it's a PDF or Word file, open it
  with Google Docs and choose File → Save as Google Docs).

## Install (once)

1. In Claude, open **Customize**, then the **Plugins** tab.
2. Choose **Add**, then **Add marketplace**, then **Add from a repository**.
3. Enter this repository: `mayowadabiri/something-interesting`.
4. Install the **job-hunt** plugin.

In Claude Code instead, run: `/plugin install job-hunt --marketplace mayowadabiri/something-interesting`

## Set up (once, about 10 minutes)

Start a new chat and type:

> Set up my job search

Claude will:
1. Ask you to confirm which Google Doc is your master CV.
2. Read it and ask what you're looking for: job titles, technologies, remote
   or relocation, countries to include or avoid.
3. Create your **Job Application Tracker**, and **Job Applications** and
   **Cold Outreach** folders in your Drive.
4. Schedule your two daily tasks (results arrive as push notifications).

## Everyday use

- **Morning:** check the notification, open the tracker, review the tailored CVs.
- **Applying:** open the CV Doc, File → Download → Microsoft Word (.docx),
  which is the cleanest format for applicant tracking systems.
- **After applying:** tell Claude "I applied to <company>" and it updates the
  tracker and sets a follow-up date.
- **Cold emails:** open today's doc in Cold Outreach, check each email, verify
  any "pattern-based" address (e.g. with Hunter.io), and send a few a day from
  your own email.
- **Change preferences:** type "set up my job search" again.

## Updates

The plugin has no fixed version, so it follows the latest commit here. The
on-demand skills (CV tailoring, cold outreach) pick up changes when your Claude
refreshes the plugin. Your two daily tasks keep the instructions they were
created with: after a big update, type "set up my job search" again to refresh
them (it updates the existing tasks rather than making new ones).

## For the maintainer

- `plugins/job-hunt/skills/setup-job-search/`: setup flow, plus the templates
  for the two daily tasks in `references/`.
- `plugins/job-hunt/skills/tailor-cv/`: CV tailoring rules and the
  interview-chance method.
- `plugins/job-hunt/skills/cold-outreach/`: on-demand outreach rules.
- Keep this repository free of anyone's personal details, CVs or IDs.
