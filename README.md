# AI Job Search System for New Graduates

A free, open-source workflow that uses Claude AI + Claude in Chrome to organize your job search — without inventing experience, scraping LinkedIn, or automating applications.

> **One rule:** AI is the copilot. You make every decision, verify every claim, and submit every application.

## How It Works — 5 Agents, 1 Human

```
You save jobs on LinkedIn ──→ Claude in Chrome reads your Saved jobs list
                                   │
                                   ▼
You ──→ ① Capture ──→ ② Profile ──→ ③ Fit ──→ ④ Resume ──→ ⑤ Apply
          save to       build your    score     tailor for    review &
          Google Sheet   evidence      each job  the role      submit
              │              │            │          │            │
              ▼              ▼            ▼          ▼            ▼
          Google Sheet   Career      Fit Report  Draft Resume  You click
          via Drive      Profile     (go/skip)   saved to      "Apply"
          connector      (private)               Google Drive
```

Each agent is a Claude Skill — a reusable instruction set you install once. You click **Save** on jobs as you browse LinkedIn; they collect in LinkedIn's Job tracker (**My Jobs → Saved**). Then you ask Claude in Chrome to capture them, and it pulls each saved job's details into your Google Sheet tracker, with one Google Doc per job description. The Google Drive connector lets Claude save everything to your Drive.

## What You Need

| Tool | Why | Cost |
|---|---|---|
| [Claude](https://claude.ai) | AI assistant that runs the agents | Free tier works; Pro is better |
| [Claude in Chrome](https://chromewebstore.google.com/detail/claude/danfohhgmbaghpjboimpdpfmfjnkjocbp) | Lets Claude read your saved jobs in your logged-in LinkedIn session | Free extension |
| Google account | Sheets for tracker, Drive for resume storage | Free |
| Chrome browser | For Claude in Chrome extension | Free |

No coding. No API keys. No terminal.

## Quick Start (20 minutes)

### Step 1: Install Claude in Chrome

1. Open Chrome and go to the [Claude in Chrome extension](https://chromewebstore.google.com/detail/claude/danfohhgmbaghpjboimpdpfmfjnkjocbp).
2. Click **Add to Chrome**.
3. Pin the Claude icon to your toolbar for easy access.
4. Sign in with your Claude account.

### Step 2: Connect Google Drive

1. In Claude, open a conversation.
2. Click the **Connectors** icon (integrations/puzzle piece).
3. Find **Google Drive** and click **Connect**.
4. Authorize Claude to access your Google account.
5. Now Claude can create your tracker sheet, save job-description docs, and read files in your Drive.

> The connector can **create** a sheet but can't edit cells in an existing one. Claude either creates a fresh tracker sheet for you, or types into your existing sheet through Claude in Chrome.

### Step 3: Set up your Google Sheet tracker

Easiest: let the Job Capture agent create it on the first run. To make it yourself:

1. Open [Google Sheets](https://sheets.google.com) and create a new sheet.
2. Name it **Job Search Tracker**.
3. Add these column headers in Row 1:

```
Company | Employees | Job Link | Title | Location | Posted | Clicked Apply | JD Doc | Fit Score | Status | Applied On | Resume
```

### Step 4: Install the Claude Skills

1. Go to [claude.ai](https://claude.ai) → Settings → Skills.
2. Upload each skill folder from the `skills/` directory (one at a time):
   - `job-capture` — pulls your LinkedIn saved jobs into your tracker sheet
   - `profile-agent` — builds your career evidence file
   - `job-fit` — scores how well you match a job
   - `resume-tailor` — tailors your resume for a specific role
3. Enable all four skills.

> **Can't install Skills?** Use the copy-paste prompts in [docs/no-skills-fallback.md](docs/no-skills-fallback.md).

### Step 5: Build your career profile (one time, ~30 minutes)

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

Save the output as `career-profile.private.md` in your Google Drive. This is your source of truth.

## Daily Workflow with Claude in Chrome

This is where the Chrome extension shines. Here's how a typical session works:

### Saving and Capturing Jobs

1. **Open LinkedIn** in Chrome (logged into your account).
2. **Browse jobs** normally. Click **Save** on any job worth a closer look. Saved jobs collect in **My Jobs → Saved** (LinkedIn's Job tracker).
3. When you have a batch, click the Claude in Chrome icon in your toolbar.
4. **Tell Claude:**

```
Use my job-capture skill to capture my LinkedIn saved jobs
into my Job Search Tracker Google Sheet.
Create one Google Doc per job in my "Job Descriptions" Drive folder.
Only add jobs that aren't already in the sheet.
```

Claude opens your Saved list, visits each saved job, and reads the company, title, location, posted date, applicant count, company size and full description. It creates one Google Doc per job and writes one row per job to your sheet. At the end it reads the sheet back to check its work. Plan on a few minutes per job.

> Just want one job? Open it and say *"Capture this job to my tracker."*

### Analyzing Fit

Pick a row from your tracker:

```
Analyze how well I fit the [Company] – [Title] job in my tracker.
Use its JD Doc and my career profile from Google Drive
(career-profile.private.md). Give me a fit report.
```

Claude reads the saved job description and your profile from Drive and returns a fit report.

### Tailoring a Resume

After reviewing the fit report and deciding to apply:

```
Create a tailored resume for this role.
Use its JD Doc and my career profile from Drive.
Use only facts from my profile. Flag anything I need to verify.
Save the tailored resume to my Google Drive.
```

### A Typical Week

1. Save jobs on LinkedIn during the week (seconds each).
2. Once or twice a week, run Job Capture to pull the new saved jobs into your sheet.
3. Run Job Fit on the new rows and sort by recommendation.
4. Tailor resumes only for **Prioritize** / **Apply** jobs.

## The 5 Agents Explained

### Agent 1: Job Capture
**What it does:** Pulls the jobs you saved in LinkedIn's Job tracker into your Google Sheet, one row per job plus a Google Doc with the full description.
**How it works with Chrome:** Claude in Chrome opens your Saved list and each saved job in your own logged-in session. It is read-only: it never applies, messages, or un-saves.
**How it works with Drive:** Creates the job-description docs and the tracker sheet.
**You control:** Which jobs you save on LinkedIn, and when to run a capture. Whether each job is legitimate.

### Agent 2: Profile Agent
**What it does:** Builds a factual career profile from your resume, LinkedIn, and real examples. Suggests target job titles.
**Input it needs:** LinkedIn PDF, resume, and 3-5 CAR/PAR accomplishment examples.
**Output:** `career-profile.private.md` saved to your Google Drive + suggested target job titles.
**You control:** What evidence to include. Which job titles to target.

### Agent 3: Job Fit Analyzer
**What it does:** Compares a job description against your career profile.
**How it works with Drive:** Reads the job's JD Doc and your career profile from Drive, and records the result in the Fit Score column.
**You control:** Whether to apply or skip.
**Output:** Fit report with Strong/Related/Gap ratings + Prioritize/Apply/Stretch/Skip recommendation.

### Agent 4: Resume Tailor
**What it does:** Reorders and rephrases your real experience for a specific role.
**How it works with Drive:** Reads your profile and template from Drive, saves the tailored resume back to Drive.
**You control:** Final review of every line before submitting.
**Output:** Tailored resume draft + verification checklist, saved to Google Drive.

### Agent 5: You (the Human)
**What you do:** Verify every claim. Confirm the job is real. Click "Apply."
**Why this matters:** AI can write convincing lies. You are responsible for what you submit.

## Two Ways to Use the System

| | With Claude in Chrome (recommended) | Manual fallback |
|---|---|---|
| **Collecting jobs** | Save on LinkedIn; Claude pulls your whole Saved list | You copy-paste each job description |
| **Saving to tracker** | Claude creates/fills your Google Sheet | You copy Claude's output into your sheet |
| **Career profile** | Stored in Google Drive, Claude pulls it | You attach the file each time |
| **Fit reports** | Saved to Drive automatically | You save them yourself |
| **Tailored resumes** | Saved to Drive automatically | You save them yourself |
| **Works on** | Chrome desktop only | Any device, any browser |

Both paths use the same Skills and produce the same quality output. The Chrome extension removes the copy-paste work.

## Important Rules

- **Never invent experience.** If you can't defend it in an interview, delete it.
- **Never keyword-stuff.** Use the employer's words only when they truthfully describe your work.
- **Never automate applications.** Apply manually through legitimate channels.
- **Only your own saved jobs.** Job Capture reads the jobs *you* saved, in *your* logged-in session, when *you* ask. Don't point it at search results or other people's data, and keep runs to a human pace. It still drives a browser on LinkedIn, and LinkedIn's User Agreement restricts automated access, so read it and decide for yourself. If LinkedIn shows a warning, stop.
- **Never trust a fit score blindly.** It helps prioritize — it doesn't predict hiring.
- **Never put private data in this public repo.** Keep resumes, tracker, and profile in your private Google Drive.
- **Always verify the job is real.** Check the employer's official careers site. See [FTC job scam guidance](https://consumer.ftc.gov/articles/job-scams).

## Repository Structure

```
.
├── README.md                  ← You are here
├── SETUP.md                   ← Step-by-step setup
├── docs/
│   ├── how-it-works.md        ← Architecture + Chrome extension flow
│   ├── no-skills-fallback.md  ← Use without Skills (copy-paste prompts)
│   └── privacy-guide.md       ← What to keep private
├── templates/
│   ├── career-profile.template.md
│   ├── fit-report.template.md
│   └── resume-template.md
├── skills/
│   ├── job-capture/SKILL.md   ← + references/linkedin-saved-jobs.md
│   ├── profile-agent/SKILL.md
│   ├── job-fit/SKILL.md
│   └── resume-tailor/SKILL.md
├── claude-code-prompt.md      ← Prompt to recreate this with Claude Code
├── linkedin-post.md
└── LICENSE
```

## For Mentors and Career Coaches

Fork this repo, customize the templates for your students, and share it. The skills and templates are designed to be adapted.

## Credits

Built by [Ketan Bhagat](https://linkedin.com/in/ketanbhagat) — an engineering leader who built his own AI-assisted job search system and open-sourced the workflow so new graduates don't start from scratch.

## License

MIT — use it, adapt it, improve it. Keep your personal data out of public forks.
