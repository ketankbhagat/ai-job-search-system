# How the Agent System Works

## Architecture: Human-in-the-Loop Agents

This system uses four AI "agents" — each one is a Claude Skill (a reusable set of instructions). The fifth agent is you.

```
┌─────────────────────────────────────────────────────────────┐
│                    YOUR JOB SEARCH                          │
│                                                             │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐ │
│  │ CAPTURE  │──▶│ PROFILE  │──▶│   FIT    │──▶│  RESUME  │ │
│  │  Agent   │   │  Agent   │   │  Agent   │   │  Agent   │ │
│  └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘ │
│       │              │              │              │        │
│       ▼              ▼              ▼              ▼        │
│  Google Sheet   Career Profile  Fit Report    Draft Resume  │
│  (tracker)      (your truth)    (go/skip?)    (for review)  │
│       │              │              │              │        │
│       └──────────────┴──────────────┴──────────────┘        │
│                          │                                  │
│                     YOU DECIDE                              │
│               (review → verify → apply)                     │
└─────────────────────────────────────────────────────────────┘
```

## What Makes This "Agentic"

Each agent has:
- **A clear job** — one task, well defined
- **Inputs it needs** — your data, not invented data
- **An output it produces** — structured, consistent, reusable
- **A human checkpoint** — you approve before moving to the next step

This is the opposite of "let AI do everything." Each agent is good at one thing, and you connect them by deciding what moves forward.

## Agent Details

### Agent 1: Job Capture
```
Input:  Job URL or pasted job description
Output: Structured row for your Google Sheet
Human:  Verify the job is real. Decide whether to save it.
```

### Agent 2: Profile Agent
```
Input:  LinkedIn PDF + resume + CAR/PAR examples
Output: career-profile.private.md + suggested job titles
Human:  Review every fact. Correct anything wrong. Add missing evidence.
```
You run this once, then update it when you gain new experience.

### Agent 3: Job Fit Analyzer
```
Input:  Job description + your career profile
Output: Evidence matrix + fit recommendation (Prioritize / Apply / Stretch / Skip)
Human:  Decide whether to spend time tailoring for this role.
```

### Agent 4: Resume Tailor
```
Input:  Job description + career profile + fit report + resume template
Output: Tailored resume draft + verification checklist
Human:  Read every line. Can you defend every claim in an interview?
```

## Why Not Fully Automated?

Three reasons:

1. **AI invents things.** It can write a convincing resume bullet that never happened. Only you know what's real.

2. **Platforms prohibit automation.** LinkedIn's User Agreement prohibits unauthorized scraping and automated activity. This system uses manual capture, not bots.

3. **Quality beats quantity.** Five well-targeted applications beat fifty generic ones. The human review step is where quality happens.

## Data Flow

```
LinkedIn/Job Board  ──(you copy)──▶  Capture Agent  ──▶  Google Sheet
                                                              │
Your LinkedIn PDF ─┐                                          │
Your Resume ───────┼──▶  Profile Agent  ──▶  career-profile.private.md
Your CAR Examples ─┘                              │
                                                  │
Google Sheet row ──┐                              │
Job Description ───┼──▶  Fit Agent  ──▶  Fit Report (go/skip)
Career Profile ────┘                        │
                                            │ (if "go")
                                            ▼
Career Profile ────┐
Fit Report ────────┼──▶  Resume Agent  ──▶  Draft Resume
Resume Template ───┘                            │
                                                ▼
                                          YOU REVIEW
                                                │
                                                ▼
                                          YOU APPLY
```

## Where Your Data Lives

| Data | Where | Public? |
|---|---|---|
| Skills (agent instructions) | This GitHub repo | Yes — no personal data |
| Templates (blank) | This GitHub repo | Yes — no personal data |
| Career profile | Your private folder | **NO** |
| Job tracker | Your Google Sheet | **NO** |
| Resumes | Your Google Drive | **NO** |
| Fit reports | Your private folder | **NO** |

The public repo contains only the **instructions** and **blank templates**. Your real data stays private.
