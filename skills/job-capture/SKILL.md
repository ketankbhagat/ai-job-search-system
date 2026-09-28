---
name: job-capture
description: "Structure a job posting into a tracker row and save it to the user's Google Sheet. Works with Claude in Chrome (reads the job page directly) or with a pasted job description. Triggered when the user shares a job URL, pastes a job description, or asks to save/capture/track a job."
---

# Job Capture Agent

## Purpose

Turn a job posting into a clean, structured tracker row and save it to the user's Google Sheet.

## How Input Works

**With Claude in Chrome (preferred):**
The user is viewing a job page (LinkedIn, company careers site, etc.) and asks Claude to capture it. Read the job description directly from the current page.

**Manual fallback:**
The user pastes a job URL or job description text.

## Rules

1. Do not scrape, crawl, or automate LinkedIn or any platform. Reading the current page via Claude in Chrome is fine — it is human-initiated, one page at a time.
2. Do not infer missing values — mark them as "Unknown."
3. Do not publish job descriptions or recruiter messages publicly.
4. Prefer the employer's official careers page for verification.

## Output

Return a structured block AND (if Google Drive is connected) write it as a new row to the user's Google Sheet tracker:

```
Company: [name]
Role: [exact title]
Location: [location / remote / hybrid / onsite]
Job URL: [URL from the page or as provided]
Date Found: [today's date]
Core Responsibilities: [5-8 key items]
Required Skills: [list]
Preferred Skills: [list]
Key Technologies: [list]
Compensation: [only if explicitly stated, otherwise "Not listed"]
Deadline: [only if stated, otherwise "Not listed"]
Red Flags: [any scam indicators or concerns]
Questions: [ambiguities worth verifying]
```

**Google Sheet mapping:**
When writing to the tracker, map to columns: Company | Role | Location | Job URL | Date Found | Fit Score (leave blank) | Status ("Saved") | Applied On (blank) | Resume Version (blank) | Notes (key requirements summary).

## Scam Check

Flag (without declaring fraud) if you see:
- Payment required to get the job
- Check/equipment purchasing scheme
- Sensitive identity/banking request before normal hiring
- Look-alike employer domain
- Role can't be verified on an official site
