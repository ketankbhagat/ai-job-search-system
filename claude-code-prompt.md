# Claude Code Prompt: Build the AI Job Search System

Copy this entire prompt into Claude Code (terminal) to scaffold the complete project. It creates the repo structure, all skills, templates, docs, and the README.

---

## The Prompt

```
Create a GitHub-ready project called "ai-job-search-system" with this structure
and content. This is an open-source, AI-assisted job search workflow for new
graduates. It uses Claude Skills as "agents" with a human-in-the-loop at every step.

## Architecture

4 AI agents (Claude Skills) + 1 human agent:

1. Job Capture Agent — structures job postings into Google Sheet tracker rows
2. Profile Agent — builds a career profile from LinkedIn PDF + resume + CAR examples,
   suggests target job titles
3. Job Fit Agent — compares job descriptions to the career profile, recommends
   Prioritize/Apply/Stretch/Skip
4. Resume Tailor Agent — tailors resume using only facts from the career profile
5. Human — reviews every output, verifies claims, applies manually

## Directory structure

ai-job-search-system/
├── README.md                    # Main guide with Quick Start
├── SETUP.md                     # Detailed step-by-step setup
├── LICENSE                      # MIT
├── .gitignore                   # Block private files
├── docs/
│   ├── how-it-works.md          # Architecture diagram and agent details
│   ├── no-skills-fallback.md    # Manual prompts for users without Skills
│   └── privacy-guide.md         # What to keep private
├── templates/
│   ├── career-profile.template.md   # Blank career profile structure
│   ├── fit-report.template.md       # Fit report output format
│   └── resume-template.md           # Blank resume structure
└── skills/
    ├── job-capture/
    │   └── SKILL.md             # Job capture agent instructions
    ├── profile-agent/
    │   ├── SKILL.md             # Profile builder + title suggestions
    │   └── references/
    │       └── career-profile.template.md
    ├── job-fit/
    │   ├── SKILL.md             # Fit analysis agent
    │   └── references/
    │       └── fit-report.template.md
    └── resume-tailor/
        ├── SKILL.md             # Resume tailoring agent
        └── references/
            └── resume-template.md

## Key principles for ALL content:

1. AI is copilot, human is pilot — every agent output needs human approval
2. Never invent experience — gaps are labeled "Gap" not filled with fiction
3. No LinkedIn scraping or automation — manual capture only
4. Privacy first — no personal data in the public repo
5. Evidence-based — every skill must cite where it was used
6. Simple enough for non-technical users — no coding, no API keys, no terminal

## README.md requirements:

- Start with the one-rule principle
- Show the 5-agent flow as ASCII art
- "What You Need" table (Claude + Google account + browser, all free)
- Quick Start in 5 numbered steps (fork, create sheet, install skills,
  build profile, start applying)
- Each agent explained: what it does, what you control, what it outputs
- Important Rules section (never invent, never keyword-stuff, never automate,
  verify jobs are real)
- Repository structure tree
- Credits line for Ketan Bhagat with LinkedIn link
- MIT license note

## SKILL.md format for each agent:

Each skill needs YAML frontmatter with name and description (the description
is what Claude uses to trigger the skill). Then markdown with:
- Purpose (one line)
- Rules (what it must/must not do)
- Inputs (what it needs from the user)
- Process (steps it follows)
- Output (exact format it returns)

## Templates:

- career-profile.template.md: sections for Target Roles, Education, Skills
  (with evidence source for each), Experience (CAR format), Projects,
  Research, Leadership, Certifications, Evidence Stories, Positioning Rules,
  Truth Rules
- fit-report.template.md: Job info, Recommendation, Evidence Matrix table,
  Top Evidence, Gaps table, Supported Keywords, Resume Strategy,
  Verification Checklist
- resume-template.md: Name/contact, Summary, Education, Skills, Experience,
  Projects, Additional — with "Template Rules for AI" section

## .gitignore:

Block: *.private.md, career-profile.*, job-tracker.*, *.env, node_modules,
.DS_Store, resumes/, applications/, fit-reports/

Create all files with complete content. Make the README engaging and
scannable — a new grad should be able to start in 15 minutes. Use clear
ASCII diagrams instead of complex graphics. Keep language direct and
jargon-free.
```

---

## After Running the Prompt

1. Review the generated files
2. Test each skill by installing it in Claude
3. Create a test career profile using the template
4. Try the workflow with a real job posting
5. Push to GitHub when satisfied
