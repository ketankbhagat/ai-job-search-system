# Setup Guide

## Prerequisites

You need three things (all free):

1. **A Claude account** — [claude.ai](https://claude.ai) (free or Pro)
2. **A Google account** — for Sheets (tracker) and Drive (resume storage)
3. **Your career materials** — resume, LinkedIn profile PDF, and a few accomplishment examples

## Step-by-Step Setup

### 1. Create Your Private Job Tracker (5 minutes)

1. Go to [Google Sheets](https://sheets.google.com).
2. Create a new blank spreadsheet.
3. Name it **Job Search Tracker** (or whatever you prefer).
4. In Row 1, type these headers across columns A through J:

| A | B | C | D | E | F | G | H | I | J |
|---|---|---|---|---|---|---|---|---|---|
| Company | Role | Location | Job URL | Date Found | Fit Score | Status | Applied On | Resume Version | Notes |

5. Bookmark this sheet — you'll use it throughout your search.

**Status values to use:** Saved → Analyzing → Tailoring → Applied → Interview → Offer → Closed

### 2. Export Your LinkedIn Profile (2 minutes)

1. Go to your LinkedIn profile page.
2. Click **More** → **Save to PDF**.
3. Save the PDF to your computer.
4. This is one of the inputs for your career profile — not something you publish.

### 3. Write 3–5 Accomplishment Examples (15 minutes)

These are your strongest stories. Use the CAR format:

```
Context: What was the situation?
Action: What did YOU specifically do?
Result: What changed because of your work?
```

**Example for a student:**
```
Context: Our senior capstone team needed a way to process 10,000 survey responses.
Action: I built a Python script that cleaned, validated, and summarized the data
        using pandas, replacing a manual Excel process.
Result: Reduced data prep from 6 hours to 15 minutes. The professor adopted the
        script for future semesters.
```

Write yours in a simple text file. Real numbers only — if you don't know the number, describe the scale instead ("large dataset" instead of inventing "50,000 rows").

### 4. Install Claude Skills (5 minutes)

Each skill is a `.zip` file in the `skills/` folder. To install:

1. Open [claude.ai](https://claude.ai).
2. Click your profile → **Settings** → **Skills**.
3. Click **Create skill** or **Upload**.
4. Upload the skill folder (or zip it first).
5. Repeat for all four skills.

The skills to install:

| Skill | What It Does |
|---|---|
| `job-capture` | Structures job postings into tracker rows |
| `profile-agent` | Builds your career profile and suggests target titles |
| `job-fit` | Scores how well you match a specific job |
| `resume-tailor` | Tailors your resume for a specific role |

### 5. Build Your Career Profile (30 minutes, one time)

This is the most important step. Open a new Claude conversation and say:

```
I want to build my career profile for job searching.

Here are my materials:
1. My LinkedIn profile PDF [drag and drop the file]
2. My current resume [drag and drop the file]
3. My accomplishment examples:

[Paste your 3-5 CAR examples here]

Please:
- Extract all my skills, experience, projects, and education
- Suggest 3-5 job titles I should be targeting
- Identify my strongest evidence and biggest gaps
- Create a complete career-profile.private.md file I can save
```

**Save the output.** Copy it into a file called `career-profile.private.md` and store it in a private folder (NOT in this GitHub repo).

### 6. Connect Google Drive to Claude (optional but recommended)

If you want Claude to read/write your Google Sheet tracker directly:

1. In Claude, click the **Connectors** icon (puzzle piece or similar).
2. Find **Google Drive** and click **Connect**.
3. Authorize Claude to access your Google account.
4. Now Claude can read your tracker and save fit reports to Drive.

This is optional — you can always copy/paste between Claude and your sheet manually.

## Daily Workflow

Once setup is complete, your daily job search looks like this:

**Morning (find jobs):**
1. Browse LinkedIn, company sites, campus portal.
2. For each interesting job, tell Claude: *"Add this job to my tracker: [paste URL or description]"*

**Evaluation (pick the best):**
1. Tell Claude: *"Analyze fit for [Company] [Role]. Here's my career profile and the job description."*
2. Read the fit report. Decide: apply or skip.

**Application (quality over quantity):**
1. For jobs worth applying to: *"Tailor my resume for this role. Use only facts from my profile."*
2. Review every line of the draft.
3. Apply through the employer's official channel.
4. Update your tracker with the date and resume version.

**Weekly review:**
1. Which roles are getting responses?
2. Which gaps keep appearing?
3. Adjust your targeting.

## Troubleshooting

**"Claude doesn't trigger the right skill"**
→ Be explicit: *"Use my job-fit skill to analyze this role."*

**"I don't have Claude Skills"**
→ See [docs/no-skills-fallback.md](docs/no-skills-fallback.md) — copy/paste the prompts manually.

**"Claude invented something on my resume"**
→ This is why Step 5 of every application is YOUR review. Delete anything you can't defend in an interview.

**"I don't know what CAR examples to write"**
→ Think about: class projects, internships, part-time jobs, volunteer work, hackathons, research, tutoring. Any situation where you solved a problem counts.
