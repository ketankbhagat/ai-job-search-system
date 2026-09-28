# How the Agent System Works

## Architecture: Chrome Extension + Drive + Human-in-the-Loop

```
┌─────────────────────────────────────────────────────────────────────┐
│                        YOUR JOB SEARCH                              │
│                                                                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │              CLAUDE IN CHROME (browser extension)            │   │
│  │                                                              │   │
│  │  You save jobs on    ──→ Claude reads your Saved list        │   │
│  │  LinkedIn (logged in)    and each saved job (read-only)      │   │
│  └──────────────┬───────────────────────────────────────────────┘   │
│                 │ job description text                               │
│                 ▼                                                    │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐        │
│  │ CAPTURE  │──▶│ PROFILE  │──▶│   FIT    │──▶│  RESUME  │        │
│  │  Agent   │   │  Agent   │   │  Agent   │   │  Agent   │        │
│  └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘        │
│       │              │              │              │               │
│       ▼              ▼              ▼              ▼               │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                 GOOGLE DRIVE (your private storage)          │   │
│  │                                                             │   │
│  │  📊 Job Tracker Sheet    📄 Career Profile                  │   │
│  │  📄 Job Description Docs 📝 Fit Reports                     │   │
│  │  📄 Tailored Resumes                                        │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                          │                                         │
│                     YOU DECIDE                                     │
│               (review → verify → apply)                            │
└────────────────────────────────────────────────────────────────────┘
```

## How Claude in Chrome Fits In

Claude in Chrome is a browser extension that lets Claude read and navigate pages in your own Chrome, using your logged-in sessions. Here's how the Job Capture agent uses it:

```
1. You browse LinkedIn and click Save on jobs you like
   → they collect in My Jobs → Saved (LinkedIn's Job tracker)
2. You click the Claude extension icon and ask it to capture your saved jobs
3. Claude opens your Saved list and collects the link to each saved job
4. Claude opens each saved job, expands the description, and reads it
   (company, title, location, posted, applicant count, full description)
5. Claude checks each company's size on its LinkedIn About page
6. Claude creates one Google Doc per job and one tracker row per job
7. Claude reads the sheet back to verify every row
```

**What it does:**
- Opens your own Saved list and the jobs on it, in your session, when you ask
- Clicks "See more" and page numbers so it can read the whole description and list

**What it does NOT do:**
- Log into LinkedIn as anyone
- Apply, message, save, un-save, or change anything on LinkedIn
- Search for or collect jobs you didn't save
- Run in the background or on a schedule

It is still browser automation on LinkedIn, so keep runs to a human pace and stop if LinkedIn shows a warning. See "Why Not Fully Automated?" below.

## The Three Connected Layers

### Layer 1: Claude in Chrome (input)
Reads the jobs you saved in LinkedIn's Job tracker. This is how job descriptions enter the system.

### Layer 2: Claude Skills (processing)
Four skills that each do one thing well:

| Skill | Reads | Produces |
|---|---|---|
| Job Capture | Your LinkedIn Saved jobs via Chrome | JD Doc per job + tracker rows in Google Sheet |
| Profile Agent | Your resume + LinkedIn PDF + CAR examples | Career profile in Google Drive |
| Job Fit | JD Doc + career profile from Drive | Fit report + Fit Score in tracker |
| Resume Tailor | JD Doc + profile + template from Drive | Tailored resume in Google Drive + Resume link in tracker |

### Layer 3: Google Drive (storage)
Your private storage for everything the system produces. Nothing goes to the public repo.

## Data Flow — With Chrome Extension

```
LinkedIn Saved jobs ──(Chrome ext reads)──▶ Capture Agent ──(Drive)──▶ JD Docs + Google Sheet
                                                                        │
Your LinkedIn PDF ─┐                                                    │
Your Resume ───────┼──▶ Profile Agent ──(Drive)──▶ career-profile.private.md
Your CAR Examples ─┘                                     │
                                                         │(Drive reads)
JD Doc (from Drive)─┐                                     │
                   ├──▶ Fit Agent ──▶ Fit Report ──(Drive)──▶ saved
Career Profile ────┘                      │
                                          │(if "go")
                                          ▼
Career Profile ────┐
Fit Report ────────┼──▶ Resume Agent ──(Drive)──▶ Tailored Resume
Resume Template ───┘                                  │
                                                      ▼
                                                 YOU REVIEW
                                                      │
                                                      ▼
                                                 YOU APPLY
```

## Data Flow — Manual Fallback (no extension)

```
LinkedIn Job Page ──(you copy-paste)──▶ Capture Agent ──(you copy)──▶ Google Sheet
                                                                        │
Your LinkedIn PDF ─┐                                                    │
Your Resume ───────┼──▶ Profile Agent ──▶ career-profile.private.md (saved locally)
Your CAR Examples ─┘                              │
                                                  │(you attach)
Job Description ───┐                              │
                   ├──▶ Fit Agent ──▶ Fit Report (saved locally)
Career Profile ────┘                      │
                                          │(if "go")
                                          ▼
Career Profile ────┐
Fit Report ────────┼──▶ Resume Agent ──▶ Tailored Resume (saved locally)
Resume Template ───┘                              │
                                                  ▼
                                             YOU REVIEW → YOU APPLY
```

Same agents, same quality. The Chrome extension + Drive connector just removes friction.

## Where Your Data Lives

| Data | Where | Public? |
|---|---|---|
| Skills (agent instructions) | This GitHub repo | Yes — no personal data |
| Templates (blank) | This GitHub repo | Yes — no personal data |
| Career profile | Your Google Drive (private) | **NO** |
| Job tracker | Your Google Sheet (private) | **NO** |
| Fit reports | Your Google Drive (private) | **NO** |
| Tailored resumes | Your Google Drive (private) | **NO** |

## Why Not Fully Automated?

1. **AI invents things.** It can write a convincing resume bullet that never happened. Only you know what's real.
2. **Platforms restrict automation.** LinkedIn's User Agreement restricts scraping and automated activity. That's why the capture step only reads jobs *you* saved, only when *you* start it, only reads, and goes at a human pace. Applying is always manual. Read LinkedIn's terms and decide for yourself.
3. **Quality beats quantity.** Five well-targeted applications beat fifty generic ones.

## Browser Safety

Claude in Chrome can read pages you're logged into, including sensitive ones. For job searching:

- Keep sensitive tabs (banking, email) closed when using the extension on other pages
- Do not paste passwords, API keys, or identity documents into Claude
- Verify important actions before submission
- The extension respects site permissions — some sites may block it
