---
name: job-capture
description: "Pull the jobs the user saved in LinkedIn's Job tracker (My Jobs → Saved) into a Google Sheet job tracker, with one Google Doc per job holding the full job description. Runs in Claude in Chrome with the user logged into LinkedIn, and uses the Google Drive connector. Also captures a single job from the current page or a pasted description. Use when the user asks to capture, import, sync, back up, or track their saved LinkedIn jobs, or to save/capture one job."
---

# Job Capture Agent

## Purpose

Turn the jobs you saved on LinkedIn into a structured Google Sheet tracker, plus one Google Doc per job with the full description, so the Fit and Resume agents have something to work from.

## How It Works

1. **You** browse LinkedIn normally and click **Save** on jobs that interest you. They collect in LinkedIn's Job tracker (**My Jobs → Saved**).
2. **You** open Claude in Chrome and ask it to capture your saved jobs.
3. **Claude** opens your Saved list in your own logged-in session, visits each saved job, and reads its details.
4. **Claude** creates one Google Doc per job (the full "About the job" text) and writes one tracker row per job.
5. **You** review the sheet.

Step-by-step browser instructions are in `references/linkedin-saved-jobs.md`. Follow them.

## Modes

| Mode | Trigger | Input |
|---|---|---|
| **Saved jobs (default)** | "Capture my saved jobs", "sync my LinkedIn saved jobs to my tracker" | LinkedIn Job tracker → Saved tab |
| Single job | "Capture this job" | The job page open in Chrome, a job URL, or pasted text |

## Ask First (skip anything the user already specified)

1. Which Job tracker tab: **Saved** (default), In Progress, or Applied.
2. New sheet or existing sheet (see "Writing to the sheet" below).
3. Which Drive folder the job-description docs go in. A dedicated folder is better than My Drive root.
4. All saved jobs, or only those not already in the sheet (on a re-run, default to new only).
5. Whether to stop for a check after the first 2–3 jobs.
6. Ask the user **not to edit the sheet while the run is in progress**.

## Tracker Columns

Confirm the order with the user; they may already have a layout.

| Column | Source |
|---|---|
| Company | Job page header (the saved list abbreviates names) |
| Employees | Company's LinkedIn About page size band, e.g. `1K-5K employees` |
| Job Link | `https://www.linkedin.com/jobs/view/<ID>/` |
| Title | Job page header |
| Location | Job page header |
| Posted | Job page header, e.g. `2 weeks ago` |
| Clicked Apply | Job page header; for Easy Apply write e.g. `164 applicants (Easy Apply)` |
| JD Doc | Link to the Google Doc created for this job |
| Fit Score | Blank — filled by the Job Fit agent |
| Status | `Saved` — updated later by you or the Resume agent |
| Applied On | Blank |
| Resume | Blank — filled by the Resume Tailor agent |

## Writing to the Sheet

The Google Drive connector can **create** a sheet but **cannot edit cells in an existing sheet**.

- **New sheet:** create it from CSV (`text/csv` converts to a Google Sheet). This is the most reliable path.
- **Existing sheet:** type into it through Claude in Chrome, following the browser rules in the reference file.

Either way, **read the sheet back afterwards** and confirm every row has the right number of columns.

## Rules

1. Only open the user's own saved jobs, and only when the user asks. Do not search, crawl, or collect jobs the user did not save.
2. Never apply, message, un-save, or change anything on LinkedIn. Read only.
3. Do not infer missing values; write `Unknown`.
4. Never guess a company page URL from its name. Take the link from the job page (see reference).
5. Do not publish job descriptions or recruiter messages publicly. The docs stay in the user's private Drive.
6. Go at a human pace, with waits between pages. If LinkedIn shows a warning, CAPTCHA, or rate-limit page, stop and tell the user.

## Single-Job Output

For single-job mode, return this block, then add the row as above:

```
Company: [name]
Title: [exact title]
Location: [location / remote / hybrid / onsite]
Job Link: [URL]
Posted: [as shown]
Core Responsibilities: [5-8 key items]
Required Skills: [list]
Preferred Skills: [list]
Compensation: [only if explicitly stated, otherwise "Not listed"]
Red Flags: [any scam indicators or concerns]
Questions: [ambiguities worth verifying]
```

## Scam Check

Flag (without declaring fraud) if you see:
- Payment required to get the job
- Check/equipment purchasing scheme
- Sensitive identity/banking request before normal hiring
- Look-alike employer domain
- Role can't be verified on an official site

## Final Report

Tell the user: how many jobs were captured, any that failed or were skipped (and why), any Easy Apply substitutions, and the sheet and folder links.
