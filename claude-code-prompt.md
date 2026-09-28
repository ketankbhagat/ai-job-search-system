# Claude Code Prompt: Build the AI Job Search System

Copy this entire prompt into Claude Code (terminal) to scaffold the complete project.

---

## The Prompt

```
Create a GitHub-ready project called "ai-job-search-system" with this structure
and content. This is an open-source, AI-assisted job search workflow for new
graduates. It uses Claude Skills as "agents" with Claude in Chrome for reading
the user's LinkedIn Saved jobs and Google Drive for storage. Human stays in the loop at every step.

## Architecture

4 AI agents (Claude Skills) + Claude in Chrome + Google Drive + 1 human:

1. Job Capture Agent — opens the user's LinkedIn Job tracker (Saved tab) via
   Claude in Chrome, reads each saved job, creates one Google Doc per job
   description, and writes one row per job to the Google Sheet tracker
2. Profile Agent — builds a career profile from LinkedIn PDF + resume + CAR examples,
   suggests target job titles, saves profile to Google Drive
3. Job Fit Agent — reads the job's JD Doc + pulls profile from Drive,
   compares them, recommends Prioritize/Apply/Stretch/Skip, updates tracker
4. Resume Tailor Agent — reads job from Chrome + profile from Drive,
   tailors resume using only real facts, saves to Drive, updates tracker
5. Human — reviews every output, verifies claims, applies manually

The Chrome extension flow:
- User browses LinkedIn logged into their own account
- Clicks Save on jobs; they collect in LinkedIn's Job tracker (My Jobs → Saved)
- Asks Claude in Chrome to capture the saved jobs
- Claude reads only the user's saved jobs, read-only, at a human pace
- Saves JD docs + tracker rows to Google Drive / Google Sheet

Manual fallback for users without Chrome extension:
- Copy-paste job descriptions into Claude conversations
- Attach files manually instead of reading from Drive
- Copy outputs into their own spreadsheet

## Directory structure

ai-job-search-system/
├── README.md                    # Main guide with Chrome extension workflow
├── SETUP.md                     # Step-by-step: extension, Drive, Sheet, Skills
├── LICENSE                      # MIT
├── .gitignore                   # Block private files
├── docs/
│   ├── how-it-works.md          # Architecture with Chrome + Drive diagrams
│   ├── no-skills-fallback.md    # Manual prompts for users without Skills
│   └── privacy-guide.md         # What to keep private
├── templates/
│   ├── career-profile.template.md
│   ├── fit-report.template.md
│   └── resume-template.md
└── skills/
    ├── job-capture/SKILL.md
    ├── profile-agent/
    │   ├── SKILL.md
    │   └── references/career-profile.template.md
    ├── job-fit/
    │   ├── SKILL.md
    │   └── references/fit-report.template.md
    └── resume-tailor/
        ├── SKILL.md
        └── references/resume-template.md

## Key principles for ALL content:

1. Claude in Chrome reads only the user's own saved jobs — read-only, user-started
2. Google Drive stores everything — tracker, profile, resumes, fit reports
3. AI is copilot, human is pilot — every agent output needs human approval
4. Never invent experience — gaps are labeled "Gap" not filled with fiction
5. Privacy first — no personal data in the public repo
6. Simple enough for non-technical users — no coding, no API keys, no terminal
7. Manual fallback always available for users without Chrome/Drive

## README.md requirements:

- ASCII diagram showing Chrome extension → agents → Google Drive flow
- "What You Need" table (Claude + Chrome extension + Google account, all free)
- Quick Start in 5 steps: install extension, connect Drive, create Sheet,
  install Skills, build profile
- Daily workflow section showing the Chrome extension flow on LinkedIn
- "Two Ways to Use" comparison table (Chrome+Drive vs manual)
- Each agent explained with how it works with Chrome and Drive
- Important rules including "don't scrape LinkedIn" clarification
- Credits line for Ketan Bhagat

## SETUP.md requirements:

- Part 1: Install Claude in Chrome (with test step)
- Part 2: Connect Google Drive (with test step)
- Part 3: Create Google Sheet tracker (with test step)
- Part 4: Install 4 Claude Skills
- Part 5: Build career profile (one-time, 30 min)
- Part 6: Daily workflow with Chrome extension
- Troubleshooting section

## SKILL.md format for each agent:

Each skill needs YAML frontmatter with name and description that mentions
Chrome extension and Drive where applicable. Then markdown with:
- Purpose (one line)
- How Input Works (Chrome extension primary, manual fallback)
- Rules (what it must/must not do)
- Process (steps it follows)
- Output (exact format + where it saves in Drive)

Create all files with complete content. Make it scannable — a new grad should
be able to set up in 20 minutes. Keep language direct and jargon-free.
```
