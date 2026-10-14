---
name: cold-outreach
description: Find the person in charge of hiring at a company (founder, CTO, engineering manager or recruiter), find their publicly listed work email, and draft a short, natural cold email and follow-up for the user to send. Use when the user asks for cold emails, outreach to recruiters or founders, or who to contact at a company. Drafts only; never sends without explicit approval.
---

# Cold outreach

The user is whoever is in this conversation. Use only their own CV, profile
and tracker. Read their settings from the **Job Application Tracker**
artifact's database (`settings/profile`) if it exists; otherwise read their
master CV from Google Drive.

## Who to contact

- Startup (roughly under 50 people): a founder, CEO or CTO, ideally the
  technical co-founder.
- Mid-size: head of engineering or the relevant engineering manager, or the
  recruiter for the role.
- Large company: only the recruiter named for the role; otherwise suggest
  applying through the posting.
- A Hacker News "Who is hiring" poster who invites emails is always a good contact.

## Finding the email (public, work addresses only)

Look only at public professional sources: the job posting, HN post, the
company's team/about/careers/contact pages, the person's own site, GitHub
profile or professional bio.
- Use only work addresses on the company's domain, or an address the person
  published for hiring. Never use personal addresses, phone numbers, home
  addresses or private social accounts, and collect nothing beyond name, title,
  work email and the source link.
- Label each address **published** (seen exactly; give the URL) or
  **pattern-based** (format taken from another employee's public address at the
  same domain; give that URL). Never guess without a public example.
- If nothing is found, say so and suggest another route (careers email, a
  LinkedIn message, applying through the posting).
- Don't get around blocked sites or logins.

## Writing the email

- Subject: short and specific ("<Role> at <Company>").
- 100 to 150 words, plain, warm and natural, in the user's voice: contractions,
  short sentences, no buzzwords, no em dashes, no "I hope this finds you well".
- (1) One specific line about their company or role from what you read;
  (2) one or two lines tying the user's most relevant real experience to it;
  (3) one simple ask, like a 15-minute chat, with the CV attached;
  (4) sign-off with first name and links.
- Use only real facts from the CV. Be honest about location and time zone.
- Add a 2-3 sentence follow-up for 5 working days later.

## Sending

Never send, schedule or queue an email on your own. If the user has an email
connector and asks you to send, show each final email (recipient, subject,
body, attachment) and send only after they clearly approve that specific email.
Advise a few emails a day at most, and verifying pattern-based addresses first.

## Logging

If a tracker exists, log each contact in its `outreach` collection (company,
person, title, email, emailStatus, source, role, jobLink, status "drafted" or
"sent", date), and fill the matching job's `contact` field in `applications`.
