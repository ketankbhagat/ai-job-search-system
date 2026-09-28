---
name: job-fit
description: "Compare a job description with the user's career profile and produce a fit report with a go/skip recommendation. Works with Claude in Chrome (reads job from the current page) or pasted job descriptions. Pulls career profile from Google Drive automatically. Use when the user asks to analyze fit, score a job, check if they should apply, or evaluate a role."
---

# Job Fit Analyzer

## Purpose

Help the user decide whether a role is worth applying to — using evidence, not guesswork.

## How Input Works

**With Claude in Chrome (preferred):**
Read the job description from the page the user is currently viewing.

**Manual fallback:**
The user pastes or attaches the job description.

**Career profile:**
If Google Drive is connected, pull `career-profile.private.md` from the user's Drive automatically. Otherwise, ask the user to attach it.

## Inputs

1. The job description (from Chrome tab, pasted, or attached)
2. The user's `career-profile.private.md` (from Drive or attached)
3. Optional: the user's Google Sheet tracker row for this job

## Source of Truth

The career profile outranks everything. Never invent evidence.

## Analysis Steps

1. Separate the job into: core responsibilities, required qualifications, preferred qualifications, tools/technologies, logistics, and ambiguous items.

2. For every important requirement, map the user's evidence:

| Requirement | Required/Preferred | User's Evidence | Rating | Resume Action |
|---|---|---|---|---|
| [item] | R or P | [evidence from profile] | Strong/Related/Gap/Unknown | [what to do] |

3. Recommend one of:
   - **Prioritize** — strong match, worth significant effort
   - **Apply** — good match, worth tailoring
   - **Stretch** — notable gaps but worth trying if interested
   - **Skip** — too many blockers or misalignment

## Output

Use `references/fit-report.template.md` when available.

Return:
1. Recommendation with reasoning
2. Top 3 evidence points to lead with
3. Top 3 gaps
4. Which gaps are true blockers vs. learnable/preferred
5. Truthful keywords the user can use on their resume
6. One-sentence resume strategy
7. Items needing verification

**If Google Drive is connected:** Update the Fit Score column in the user's Google Sheet tracker for this job.

## Rules

- Do NOT write or change a resume during fit analysis.
- A "Gap" is honest — do not upgrade it to "Related" without real adjacent evidence.
- Do not treat a fit score as a hiring prediction. It helps prioritize effort.
- Do not encourage self-rejection for missing preferred qualifications.
