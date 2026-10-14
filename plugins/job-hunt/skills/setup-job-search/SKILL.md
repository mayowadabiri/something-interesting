---
name: setup-job-search
description: One-time setup of the user's automated job hunt - finds their master CV in Google Drive, learns what jobs they want, creates their job tracker and Drive folders, and schedules a daily job search (with tailored CVs) and daily cold email drafts. Use when the user says "set up my job search", installs this plugin for the first time, or wants to change their job preferences or refresh their daily tasks.
---

# Set up my job search

Build this user's own job-hunt system, in their own accounts. Everything you
create belongs to the person in this conversation. Never reuse IDs, links,
names or preferences from anyone else, including any examples in this plugin.

Work through the steps in order. Ask questions in plain language and keep it
short: the user should be done in about 10 minutes.

## Step 0: Check requirements

Confirm these are available in this session before going further:

- **Google Drive** and **Google Docs** connectors, connected to the user's own
  Google account. If either is missing, tell the user to connect it (Settings →
  Connectors, or the connector suggestion card) and stop until they have.
- **Artifacts** (to publish the tracker page and store its data).
- **Scheduling tools** (to create scheduled tasks; look for a tool that creates
  scheduled tasks, loading it with tool search if needed). If none exists, do
  everything else and tell the user the daily tasks can't be created from this
  session; they can ask again from a session that supports scheduled tasks.

## Step 1: Check for an existing setup

- List the user's artifacts. If one titled **Job Application Tracker** exists,
  reuse it (read its `settings/profile` document if present).
- List the user's scheduled tasks. If tasks named **Morning job search** or
  **Daily cold email drafts** exist, this is a refresh: update those tasks in
  place (same IDs) instead of creating duplicates.
- Search Drive for folders titled exactly **Job Applications** and **Cold
  Outreach** owned by the user. Reuse them if they exist.

## Step 2: Find the master CV

Search Drive for the user's own Google Docs (`owner = 'me'`) with "CV",
"Resume" or "Master" in the title. Ignore files shared by other people.
Show the most likely one (or a short list) and ask the user to confirm which
is their master CV. Note its file ID and title.

If their CV is only a PDF or Word file, ask them to open it with Google Docs
(File → Save as Google Docs) and confirm the new Doc.

Read the confirmed CV in full (Google Docs read_doc; if the result is saved to
a file, extract all paragraph and table text from the JSON and read all of it).
Never edit the master CV, now or later.

## Step 3: Build the profile

From the CV, draft these fields, then show them to the user and ask them to
correct or fill in anything (ask only for what the CV doesn't answer):

| Field | Example of what to capture |
|---|---|
| name | Full name as on the CV |
| email, links | Email, LinkedIn, GitHub/portfolio from the CV |
| headline | e.g. "Senior Fullstack Engineer, frontend-focused, 6 years" |
| experience | 3 to 8 lines: each recent role, company, location, dates, 1-2 real achievements |
| skills | Main stack and secondary skills, from the CV |
| notOnCv | Common requirements in their field the CV does NOT show (for honest gap reporting) |
| education | Degree and school |
| targetRoles | Job titles and seniority they want, and roles to skip |
| mustHaveTech | Technologies a job must centre on (e.g. "React or TypeScript or Node.js") |
| homeCountry, timezone | Where they live and their UTC offset |
| workSetup | Any of: remote worldwide; remote in certain regions; relocation/visa sponsorship to certain countries; local jobs. Also any countries or regions to exclude (e.g. "no jobs in my home country") |
| hiresFromCheck | Whether to check that companies hire people from their country (yes/no) |
| excludedCompanies | Companies to never show (starts empty) |
| maxCvsPerDay | Default 15 |
| outreachPerDay | Default 5 to 8 (max 8) |
| searchTime, outreachTime | Default 07:58 and 09:15 in their local time zone |

Also note the CV's structure for tailoring: the exact first phrase of the
summary (e.g. "Product-focused Senior Fullstack Engineer with 5+ years of
experience"), and any sentence in the summary that names a specific company
or mission (it must be rewritten for every job).

## Step 4: Create folders and the tracker

1. In Drive, create (or reuse) folders **Job Applications** and **Cold
   Outreach** at the root of My Drive. Note both folder IDs.
2. Tracker: if none exists, publish `assets/job-tracker.html` (in this skill's
   folder) as a new artifact, title "Job Application Tracker", icon "briefcase",
   with capabilities `{"db": {}, "user": {}}`. Note its URL.
3. Save the profile in the tracker's database: collection `settings`, document
   `profile`, containing every field from Step 3 plus `masterCvId`,
   `masterCvTitle`, `jobFolderId`, `outreachFolderId`, `trackerUrl`,
   `summaryOpening`, `summaryCompanySentence` (or empty), and `updated`.
   The other skills in this plugin read their settings from here.

## Step 5: Create the two daily tasks

Fill in the templates in `references/daily-job-search-prompt.md` and
`references/cold-outreach-prompt.md`: replace every `{{PLACEHOLDER}}` with the
user's values from the profile. Write the profile sections as plain text, not
JSON. Check no `{{` remains before creating anything.

How to fill each placeholder:

| Placeholder | Fill with |
|---|---|
| NAME | Full name |
| PROFILE | Bullet list: headline; location and time zone; each recent role with company, location, dates and 1-2 real achievements; main stack; secondary skills; testing/tools; strengths; education; "Not on the CV: <notOnCv>"; email and links |
| MASTER_CV_ID, MASTER_CV_TITLE | The confirmed master Doc's ID and title |
| CV_TITLE_PREFIX | Master title without any role suffix, e.g. "Jane Doe_Resume" |
| TARGET_ROLES | One sentence: wanted titles and seniority, and roles to skip |
| MUST_HAVE_TECH | One sentence: the technologies a job must centre on |
| WORK_SETUP | Sentences listing the allowed setups (remote worldwide / remote in named regions or time zones / relocation or visa sponsorship to named countries / local) and any excluded countries or regions, e.g. "Exclude jobs located in <home country>." |
| HOME_COUNTRY, TIMEZONE | e.g. "Kenya", "UTC+3" |
| HIRES_FROM_CHECK | If yes: "For each job, look for public evidence the company employs people in <home country> or <region> (postings listing the country, employee profiles or team pages, EOR/talent-platform hiring). Spend at most a couple of searches per company. Label confirmed / likely / unknown with a few words of evidence. Prefer confirmed and likely; include unknown only if the posting is clearly open worldwide, ranked lower." If no: "Skip this check and write 'n.a.'." |
| EXCLUDED_COMPANIES | Comma-separated list, or "none" |
| TRACKER_URL, JOB_FOLDER_ID, OUTREACH_FOLDER_ID | From Step 4 |
| MAX_CVS | maxCvsPerDay |
| OUTREACH_PER_DAY | e.g. "5 to 8" |
| SUMMARY_OPENING | The exact first phrase of the CV summary |
| SUMMARY_COMPANY_RULE | If the summary names a specific company or mission: "Always replace this sentence in the summary, which names another company: \"<exact sentence>\". Write one sentence for this company using its mission or product in the posting's words." Otherwise: "Optionally add the company's name to the summary's final sentence only if it already expresses interest in a type of company." |

Create (or, on a refresh, update in place) two scheduled tasks in the user's
local time zone:

- **Morning job search**: daily at `searchTime` (cron like
  `CRON_TZ=<IANA zone> 58 7 * * *`), prompt = filled daily search template.
- **Daily cold email drafts**: daily at `outreachTime`, at least an hour after
  the search, prompt = filled outreach template.

Turn on push notifications only, unless the user asks for email too. Before
choosing email, tell them which address it goes to.

## Step 6: Confirm

Tell the user, briefly:
- Which Doc is their master CV (never edited).
- Their folders and tracker are ready (the tracker card shows the link).
- When the two daily tasks run and what each does: the search finds fresh jobs,
  rates the interview chance, tailors a CV per job and adds it to the tracker;
  the outreach task drafts cold emails into the Cold Outreach folder and never
  sends anything.
- Tips: download CVs as Word (.docx) when applying for the cleanest ATS
  result; check every cold email before sending; send only a few a day.
- To change preferences later, say "set up my job search" again.

Offer a test run of the job search now.
