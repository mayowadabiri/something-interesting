---
name: tailor-cv
description: Tailor the user's master CV (a Google Doc in their Drive) to a job posting by changing keywords only, keeping the layout identical, rate their chance of an interview invite, save the tailored CV in their Job Applications folder, give them a clean Word/PDF copy, and log the job in their tracker. Also handles "I applied to X" updates and writing application answers or cover messages. Use when the user shares a job link or description, asks to tailor or check a CV, or reports applying.
---

# Tailor CV to a job

The user is whoever is in this conversation. Use only their own CV, folders,
tracker and details; never anyone else's.

## Load the user's settings

1. List the user's artifacts and find **Job Application Tracker**. Read its
   database document `settings/profile` (collection `settings`, id `profile`).
   It holds their master CV ID, folder IDs, profile, home country, time zone and
   preferences.
2. If there's no tracker or profile, run the **setup-job-search** skill first,
   or at minimum confirm the master CV (their own Google Doc with "CV",
   "Resume" or "Master" in the title) and create a **Job Applications** folder.

Requires Google Drive and Google Docs connected to the user's own account.

## Hard rules

1. **Never edit the master CV.** Always work on a copy.
2. **Layout stays identical.** No new sections, reordering, added or removed
   bullets, or formatting changes. Only replace wording inside existing text,
   keeping replacements close in length so page breaks don't move.
3. **Keywords, not facts.** Use the posting's own terms where the CV already
   describes that thing. Never add a skill, tool, employer, title, date, number
   or achievement the master doesn't support. Report missing requirements as gaps.
4. **Never cross a formatting boundary** in a find string (e.g. a plain
   "Tools: " label followed by bold text). replaceAllText gives the whole
   replacement the first character's style, so the bold would be lost.
5. **No subfolders.** Save directly in Job Applications.
6. Ask before deleting, trashing or moving any Drive file.

## Steps

1. **Read the posting.** Fetch the link (or use pasted text). Extract title,
   company, location/remote rules, pay, responsibilities, requirements,
   nice-to-haves, exact tech terms and the company's mission in its own words.
   If the page can't be read, try the company's hiring system (Greenhouse,
   Lever, Ashby, Workable, Recruitee) or ask the user to paste it.
2. **Read the master CV** in full (Docs read_doc; if saved to a file, extract
   all paragraph and table text from the JSON).
3. **Plan 3 to 8 edits.** Targets: the summary's opening phrase (match the job
   title's wording); any summary sentence naming another company or mission
   (always rewrite it for this company); bullets the posting describes in other
   words; skill spellings that differ from the posting. Each find string must
   match exactly (curly apostrophes ’) and occur once.
4. **Copy and edit.** Drive copy_file the master into Job Applications as
   "<master title without role suffix>_<Company>" (append "_YYYY-MM-DD" if the
   title exists). Apply all edits in one Docs update_doc call with
   replaceAllText (matchCase true), plus one request replacing "♦" with "|".
   Every keyword edit must show occurrencesChanged 1; fix any that show 0.
5. **Check and deliver.** Export the Doc as Word (.docx) with Drive
   download_file_content (exportMimeType
   application/vnd.openxmlformats-officedocument.wordprocessingml.document).
   The result is usually saved to a file: decode its base64 `content` with a
   script and send the .docx to the user. Confirm the page count matches the
   master (export the PDF too if the user needs PDF). Recommend the Word file:
   Google's PDF export adds invisible zero-width characters after bullets and
   headings, which a few older ATS handle badly. Large files can't be uploaded
   back to Drive through the connector; the Doc in the folder is the source of
   truth.
6. **Rate the interview chance** using `references/interview-chance.md`.
7. **Log it.** In the tracker's `applications` collection, add or update the
   job (doc_id "<company>-<role>-<YYYY-MM-DD>"): company, role, location,
   source, link, status "saved", fit (the chance as a number), applied "",
   followUp "", contact "", notes ("Interview chance: N%. CV: <Doc link>. <top
   gaps>. <pay if listed>."), updated.

## Report back (short)

1. Each edit as "old wording → new wording".
2. Interview chance: the %, 2-4 reasons for and against, and what would raise it.
3. Where things are: the Doc in Job Applications, the Word file here, tracker updated.
4. At most two questions whose answers would let you strengthen the CV honestly.

## When the user reports applying

Update the job in the tracker: status "applied", applied = today,
followUp = 7 days later. If they say they were rejected or got an interview,
set status "rejected" or "interview".

## Application answers and cover messages

When asked to write a "Why us?" answer, a cover message or an email to a hiring
manager: use only real facts from their CV, reference the company's actual
product or mission from the posting, write plainly and warmly in first person
(contractions, short sentences, no buzzwords, no em dashes, no clichés like "I
hope this finds you well"), be honest about gaps, and keep to the length they
ask for. Mention their location and time-zone overlap when the role lists
working hours.
