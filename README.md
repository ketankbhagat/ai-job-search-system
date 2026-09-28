# AI Job Search System for New Graduates

A free, open-source workflow that uses Claude AI to organize your job search — without inventing experience, scraping LinkedIn, or automating applications.

> **One rule:** AI is the copilot. You make every decision, verify every claim, and submit every application.

## How It Works — 5 Agents, 1 Human

```
You ──→ ① Capture ──→ ② Profile ──→ ③ Fit ──→ ④ Resume ──→ ⑤ Apply
          save jobs     build your    score     tailor for    review &
          to tracker    evidence      each job  the role      submit
              │              │            │          │            │
              ▼              ▼            ▼          ▼            ▼
          Google Sheet   Career      Fit Report  Draft Resume  You click
          (private)      Profile     (go/skip)   (for review)  "Apply"
```

Each agent is a Claude Skill — a reusable instruction set you install once. You stay in control at every step.

## What You Need

| Tool | Why | Cost |
|---|---|---|
| [Claude](https://claude.ai) | AI assistant that runs the agents | Free tier works; Pro is better |
| Google account | Sheets for tracker, Drive for resumes | Free |
| A browser | To view job postings | Free |

That's it. No coding. No API keys. No terminal.

## Quick Start (15 minutes)

### Step 1: Fork this repository

Click **Fork** on GitHub. This gives you your own copy.

### Step 2: Set up your private Google Sheet tracker

1. Open [Google Sheets](https://sheets.google.com) and create a new sheet.
2. Name it **Job Search Tracker**.
3. Add these column headers in Row 1:

```
Company | Role | Location | Job URL | Date Found | Fit Score | Status | Applied On | Resume Version | Notes
```

4. Keep this sheet private — never share it publicly.

### Step 3: Install the Claude Skills

1. Go to [claude.ai](https://claude.ai) → Settings → Skills.
2. Upload each `.zip` file from the `skills/` folder in this repo (one at a time):
   - `job-capture` — saves job details to your tracker
   - `profile-agent` — builds your career evidence file
   - `job-fit` — scores how well you match a job
   - `resume-tailor` — tailors your resume for a specific role
3. Enable all four skills.

> **Don't have Skills yet?** You can paste the prompts from each `SKILL.md` directly into your Claude conversation instead. See [docs/no-skills-fallback.md](docs/no-skills-fallback.md).

### Step 4: Build your career profile (one time, ~30 minutes)

Open Claude and say:

```
I want to build my career profile. Here's what I have:
- My LinkedIn profile [attach PDF export]
- My current resume [attach file]
- 3-5 examples of things I accomplished (use CAR format:
  Context → Action → Result)

Help me build a complete career profile and suggest
job titles I should target.
```

Claude will use the **Profile Agent** skill to create your private career profile. Save the output as `career-profile.private.md` — this is your source of truth.

### Step 5: Start applying

For each job you find interesting:

**A) Capture it:**
```
Add this job to my tracker: [paste job URL or description]
```

**B) Check fit:**
```
Analyze how well I fit this role. Use my career profile.
[attach career-profile.private.md and the job description]
```

**C) Tailor resume (only for jobs worth applying to):**
```
Create a tailored resume for this role.
Use only facts from my career profile.
Flag anything I need to verify.
[attach career-profile.private.md and resume template]
```

**D) Review, verify, and apply yourself.**

## The 5 Agents Explained

### Agent 1: Job Capture
**What it does:** Turns a job posting into a structured tracker row.
**You control:** Which jobs to save. Whether the job is legitimate.
**Output:** A row for your Google Sheet with company, role, requirements, and red flags.

### Agent 2: Profile Agent
**What it does:** Builds a factual career profile from your resume, LinkedIn, and real examples.
**You control:** What evidence to include. Which job titles to target.
**Output:** `career-profile.private.md` + suggested target job titles.
**Input it needs:** LinkedIn PDF, resume, and 3-5 CAR/PAR accomplishment examples.

### Agent 3: Job Fit Analyzer
**What it does:** Compares a job description against your career profile.
**You control:** Whether to apply or skip.
**Output:** Fit report with Strong/Related/Gap ratings + recommendation.

### Agent 4: Resume Tailor
**What it does:** Reorders and rephrases your real experience for a specific role.
**You control:** Final review of every line before submitting.
**Output:** Tailored resume draft + verification checklist.

### Agent 5: You (the human)
**What you do:** Verify every claim. Confirm the job is real. Click "Apply."
**Why this matters:** AI can write convincing lies. You are responsible for what you submit.

## Important Rules

- **Never invent experience.** If you can't defend it in an interview, delete it.
- **Never keyword-stuff.** Use the employer's words only when they truthfully describe your work.
- **Never automate applications.** Apply manually through legitimate channels.
- **Never trust a fit score blindly.** It helps prioritize — it doesn't predict hiring.
- **Never put private data in this public repo.** Keep resumes, tracker, and profile private.
- **Always verify the job is real.** Check the employer's official careers site. See [FTC job scam guidance](https://consumer.ftc.gov/articles/job-scams).

## Repository Structure

```
.
├── README.md                  ← You are here
├── SETUP.md                   ← Detailed setup with screenshots
├── docs/
│   ├── how-it-works.md        ← Architecture explanation
│   ├── no-skills-fallback.md  ← Use without Claude Skills
│   └── privacy-guide.md       ← What to keep private
├── templates/
│   ├── career-profile.template.md
│   ├── fit-report.template.md
│   └── resume-template.md
├── skills/
│   ├── job-capture/SKILL.md
│   ├── profile-agent/SKILL.md
│   ├── job-fit/SKILL.md
│   └── resume-tailor/SKILL.md
├── claude-code-prompt.md      ← Prompt to recreate this with Claude Code
├── linkedin-post.md
└── LICENSE
```

## For Mentors and Career Coaches

Fork this repo, customize the templates for your students, and share it. The skills and templates are designed to be adapted — swap the resume template for your program's preferred format, add industry-specific positioning rules, or create a shared tracker template.

## Credits

Built by [Ketan Bhagat](https://linkedin.com/in/ketanbhagat) — an engineering leader who built his own AI-assisted job search system and open-sourced the workflow so others don't start from scratch.

## License

MIT — use it, adapt it, improve it. Keep your personal data out of public forks.
