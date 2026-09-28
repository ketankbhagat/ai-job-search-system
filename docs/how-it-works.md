# How the Agent System Works

## Architecture: Chrome Extension + Drive + Human-in-the-Loop

```
┌─────────────────────────────────────────────────────────────────────┐
│                        YOUR JOB SEARCH                              │
│                                                                     │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │              CLAUDE IN CHROME (browser extension)            │   │
│  │                                                              │   │
│  │  You browse LinkedIn ──→ Claude reads the job page           │   │
│  │  (logged in as you)      (no scraping, no automation)        │   │
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
│  │  📝 Fit Reports          📄 Tailored Resumes                │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                          │                                         │
│                     YOU DECIDE                                     │
│               (review → verify → apply)                            │
└────────────────────────────────────────────────────────────────────┘
```

## How Claude in Chrome Fits In

Claude in Chrome is a browser extension that lets Claude read the webpage you're currently viewing. Here's what that means for job searching:

**What happens when you click the Claude icon on a LinkedIn job page:**

```
1. You open a job on LinkedIn (you're logged in)
2. You click the Claude extension icon
3. Claude reads the page content (job title, company, description, requirements)
4. Claude processes it through the appropriate skill (capture, fit, or resume)
5. Claude saves the output to your Google Drive
```

**What Claude in Chrome does NOT do:**
- It does not log into LinkedIn as you
- It does not click buttons or submit forms
- It does not scrape multiple pages or crawl
- It does not automate any LinkedIn activity
- It reads one page at a time, only when you ask

This is the same as you reading the page and copying the text into Claude — the extension just removes the copy-paste step.

## The Three Connected Layers

### Layer 1: Claude in Chrome (input)
Reads job pages you're viewing in your browser. This is how job descriptions enter the system.

### Layer 2: Claude Skills (processing)
Four skills that each do one thing well:

| Skill | Reads | Produces |
|---|---|---|
| Job Capture | Job page via Chrome | Tracker row in Google Sheet |
| Profile Agent | Your resume + LinkedIn PDF + CAR examples | Career profile in Google Drive |
| Job Fit | Job page + career profile from Drive | Fit report |
| Resume Tailor | Job page + profile + template from Drive | Tailored resume in Google Drive |

### Layer 3: Google Drive (storage)
Your private storage for everything the system produces. Nothing goes to the public repo.

## Data Flow — With Chrome Extension

```
LinkedIn Job Page ──(Chrome ext reads)──▶ Capture Agent ──(Drive)──▶ Google Sheet
                                                                        │
Your LinkedIn PDF ─┐                                                    │
Your Resume ───────┼──▶ Profile Agent ──(Drive)──▶ career-profile.private.md
Your CAR Examples ─┘                                     │
                                                         │(Drive reads)
LinkedIn Job Page ─┐                                     │
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
2. **Platforms prohibit automation.** LinkedIn prohibits unauthorized scraping and automated activity. Claude in Chrome reads one page at a time as you browse — that's human-initiated reading, not automation.
3. **Quality beats quantity.** Five well-targeted applications beat fifty generic ones.

## Browser Safety

Claude in Chrome can read pages you're logged into, including sensitive ones. For job searching:

- Keep sensitive tabs (banking, email) closed when using the extension on other pages
- Do not paste passwords, API keys, or identity documents into Claude
- Verify important actions before submission
- The extension respects site permissions — some sites may block it
