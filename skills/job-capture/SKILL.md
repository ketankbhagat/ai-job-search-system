---
name: job-capture
description: "Structure a job posting into a tracker row for the user's private Google Sheet. Triggered when the user shares a job URL, pastes a job description, or asks to save/capture/track a job."
---

# Job Capture Agent

## Purpose

Turn a job posting into a clean, structured tracker row.

## Rules

1. Do not scrape, crawl, or automate LinkedIn or any platform.
2. Do not infer missing values — mark them as "Unknown."
3. Do not publish job descriptions or recruiter messages publicly.
4. Prefer the employer's official careers page for verification.

## Input

The user provides one of:
- A job URL
- Pasted job description text
- A job page open in the browser (Claude in Chrome)

## Output

Return a structured block suitable for a Google Sheet row:

```
Company: [name]
Role: [exact title]
Location: [location / remote / hybrid / onsite]
Job URL: [URL if provided]
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

## Scam Check

Flag (without declaring fraud) if you see:
- Payment required to get the job
- Check/equipment purchasing scheme
- Sensitive identity/banking request before normal hiring
- Look-alike employer domain
- Role can't be verified on an official site
