# Setup Guide — Claude in Chrome + Google Drive

## What You're Setting Up

```
Chrome + Claude Extension  ──→  Reads the jobs you saved on LinkedIn
        │
Claude Skills (4 agents)   ──→  Process jobs, profile, fit, resumes
        │
Google Drive Connector     ──→  Saves tracker, job docs + resumes to your Drive
```

Total time: ~20 minutes for setup, ~30 minutes to build your first career profile.

## Prerequisites

- A **Google account** (free — for Sheets and Drive)
- **Chrome browser** (free)
- A **Claude account** (free tier works; Pro recommended)
- Your **resume** (any format)
- Your **LinkedIn profile PDF** (exported from LinkedIn)

---

## Part 1: Install Claude in Chrome (3 minutes)

### 1.1 Install the Extension

1. Open Chrome.
2. Go to the [Claude in Chrome extension page](https://chromewebstore.google.com/detail/claude/danfohhgmbaghpjboimpdpfmfjnkjocbp).
3. Click **Add to Chrome** → **Add extension**.
4. Click the puzzle-piece icon in your Chrome toolbar → pin the Claude icon.

### 1.2 Sign In

1. Click the Claude icon in your toolbar.
2. Sign in with your Claude account (same as claude.ai).
3. You should see the Claude side panel open.

### 1.3 Test It

1. Open any webpage (e.g., a news article).
2. Click the Claude icon.
3. Type: "Summarize this page."
4. If Claude summarizes the page content, the extension is working.

**What Claude in Chrome does:** It lets Claude read and navigate pages in your own Chrome. You stay logged into your accounts (LinkedIn, etc.) — Claude works through the extension in your session, not by logging in as you.

**How this system uses it:** When you ask, the Job Capture agent opens your LinkedIn Saved jobs list and each job on it, and reads them. It never applies, messages, or un-saves anything, and it doesn't touch jobs you didn't save.

---

## Part 2: Connect Google Drive (3 minutes)

### 2.1 Connect the Google Drive Connector

1. Go to [claude.ai](https://claude.ai) and open any conversation.
2. Look for the **Connectors** or **Integrations** option (puzzle piece icon or similar).
3. Find **Google Drive** and click **Connect**.
4. Select your Google account and authorize access.
5. Claude can now read and write files in your Google Drive, including Google Sheets.

### 2.2 What This Enables

With Google Drive connected, Claude can:

- **Create** your Google Sheet tracker (from the jobs you saved on LinkedIn)
- **Create** one Google Doc per saved job, holding the full job description
- **Read** your tracker and job docs
- **Read** your career profile from Drive (so you don't have to attach it every time)
- **Save** fit reports and tailored resumes directly to your Drive

**Limit:** the connector cannot edit cells in an existing sheet. For an existing sheet, Claude types the rows in through Claude in Chrome instead. Don't edit the sheet yourself while a capture run is going.

### 2.3 Test It

In a Claude conversation, type:

```
List my recent files in Google Drive.
```

If it shows your files, the connection is working.

---

## Part 3: Create Your Job Tracker Sheet (5 minutes)

**Shortcut:** skip this part and let the Job Capture agent create the sheet on its first run (*"…create a new Job Search Tracker sheet"*).

### 3.1 Create the Sheet

1. Go to [Google Sheets](https://sheets.google.com).
2. Click **Blank spreadsheet**.
3. Name it **Job Search Tracker** (click "Untitled spreadsheet" at top-left).

### 3.2 Add Column Headers

In Row 1, add these headers across columns A through L:

| A | B | C | D | E | F | G | H | I | J | K | L |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Company | Employees | Job Link | Title | Location | Posted | Clicked Apply | JD Doc | Fit Score | Status | Applied On | Resume |

Columns A–H are filled by **Job Capture** from your LinkedIn saved jobs. **Fit Score** comes from **Job Fit**, and **Resume** from **Resume Tailor**.

### 3.3 Status Values

Use these in the Status column:

```
Saved → Analyzing → Tailoring → Applied → Interview → Offer → Closed
```

### 3.4 Test Claude's Access

In a Claude conversation, type:

```
Find my Google Sheet called "Job Search Tracker" and read the headers.
```

If Claude reads back your headers, the connection works end-to-end.

---

## Part 4: Install the 4 Claude Skills (5 minutes)

### 4.1 What Skills Are

A Skill is a reusable set of instructions that tells Claude how to do a specific task. Each of the four agents in this system is a Skill.

### 4.2 Install Each Skill

For each of the four skill folders in this repo (`skills/job-capture`, `skills/profile-agent`, `skills/job-fit`, `skills/resume-tailor`):

1. Go to [claude.ai](https://claude.ai) → **Settings** → **Skills**.
2. Click **Create skill** or **Upload**.
3. Upload the skill folder (or zip the folder first, then upload the zip).
4. Enable the skill.

| Skill | Trigger Phrases |
|---|---|
| `job-capture` | "capture this job," "add to tracker," "save this job" |
| `profile-agent` | "build my career profile," "update my profile," "suggest job titles" |
| `job-fit` | "analyze fit," "score this job," "should I apply" |
| `resume-tailor` | "tailor my resume," "create a resume for this role" |

### 4.3 No Skills? No Problem

If you can't install Skills, see [docs/no-skills-fallback.md](docs/no-skills-fallback.md). You'll copy-paste the same prompts manually — same results, just more typing.

---

## Part 5: Build Your Career Profile (30 minutes, one time)

This is the most important step. It creates the source of truth that all the other agents use.

### 5.1 Export Your LinkedIn Profile

1. Go to your LinkedIn profile page.
2. Click **More** → **Save to PDF**.
3. Save the PDF to your computer.

### 5.2 Write 3-5 Accomplishment Examples

Use the CAR format (Context → Action → Result):

```
Context: What was the situation?
Action: What did YOU specifically do?
Result: What changed because of your work?
```

**Example for a student:**
```
Context: Our senior capstone team needed to process 10,000 survey responses.
Action: I built a Python script using pandas that cleaned, validated,
        and summarized the data, replacing a manual Excel process.
Result: Reduced data prep from 6 hours to 15 minutes. The professor
        adopted the script for future semesters.
```

If you don't know a number, describe the scale — don't invent metrics.

### 5.3 Build the Profile

Open a Claude conversation and say:

```
I want to build my career profile for job searching.

Here are my materials:
1. My LinkedIn profile PDF [drag and drop the file]
2. My current resume [drag and drop the file]
3. My accomplishment examples:

[paste your 3-5 CAR examples]

Please:
- Extract all my skills, experience, projects, and education
- Suggest 3-5 job titles I should be targeting
- Identify my strongest evidence and biggest gaps
- Create a complete career-profile.private.md file
- Save it to my Google Drive
```

### 5.4 Review and Save

- Read every fact in the profile. Correct anything wrong.
- Make sure it saved to your Google Drive.
- This file is your source of truth — update it when you gain new experience.

---

## Part 6: Start Your Job Search

### The Chrome Extension Workflow

**Step 1 — Save jobs on LinkedIn (you, logged in):**
- Search for jobs normally on LinkedIn.
- Click **Save** on each job that interests you. Saved jobs appear under **My Jobs → Saved** (LinkedIn's Job tracker).

**Step 2 — Capture your saved jobs (Claude in Chrome):**
- Once you have a batch, click the Claude icon in Chrome.
- Say: *"Use my job-capture skill to capture my LinkedIn saved jobs into my Job Search Tracker sheet. One Google Doc per job in my Job Descriptions folder. Only new jobs."*
- Claude asks a few setup questions (tab, sheet, folder), then opens your Saved list and each saved job. It creates a doc per job, adds a row per job, and reads the sheet back to verify.
- Expect a few minutes per job. Don't edit the sheet while it runs.

**Step 3 — Analyze fit (Claude in Chrome or claude.ai):**
- Say: *"Analyze my fit for the [Company] – [Title] job in my tracker. Use its JD Doc and my career profile from Drive."*
- Claude reads the job doc + your profile and returns a fit report.

**Step 4 — Tailor resume (only for Apply/Prioritize jobs):**
- Say: *"Tailor my resume for this role. Save to Drive."*
- Claude drafts a resume using only your real evidence.

**Step 5 — You review and apply:**
- Read every line of the resume.
- Verify the job is real on the employer's careers site.
- Apply through the official channel.
- Set **Status** to `Applied` and fill **Applied On** in your tracker.

### Weekly Review

Once a week, ask Claude:

```
Read my Job Search Tracker from Google Drive.
Give me a summary: how many jobs saved, how many applied,
which roles are getting responses, and what gaps keep appearing.
```

---

## Troubleshooting

**"Claude in Chrome doesn't read the page"**
→ Make sure the extension is installed and you're signed in to both Claude and LinkedIn. Try refreshing the page.

**"A job came back with no description"**
→ LinkedIn loads slowly. Ask Claude to reload that job and wait longer. Don't accept an empty row.

**"All my values landed in one cell"**
→ Typed tab characters don't move between Sheets cells. Select the range → Data → **Split text to columns**. The skill's reference file tells Claude to press the Tab key instead.

**"LinkedIn showed a warning or CAPTCHA"**
→ Stop the run. Capture fewer jobs at a time and go slower.

**"Claude can't find my Google Sheet"**
→ Check that Google Drive is connected in Claude's connectors. Make sure the sheet name matches exactly.

**"Claude doesn't trigger the right skill"**
→ Be explicit: *"Use my job-fit skill to analyze this role."*

**"I don't have Claude Skills"**
→ See [docs/no-skills-fallback.md](docs/no-skills-fallback.md) — same prompts, just copy-paste.

**"Claude invented something on my resume"**
→ This is why YOU review every line. Delete anything you can't defend in an interview.

**"I'm not using Chrome"**
→ The manual workflow (copy-paste) works in any browser. See the fallback doc.
