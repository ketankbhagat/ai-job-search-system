# Using the System Without Claude Skills or Chrome

If you don't have Claude in Chrome or can't install Skills, the same workflow works with copy-paste prompts.

## Manual Workflow Summary

| Step | With Chrome Extension | Manual Alternative |
|---|---|---|
| Read a job page | Claude reads it from your tab | You copy-paste the job description |
| Save to tracker | Claude writes to Google Sheet | You copy the output into your sheet |
| Access career profile | Claude reads from Drive | You attach the file each conversation |
| Save fit reports | Claude saves to Drive | You save the output yourself |
| Save resumes | Claude saves to Drive | You save the output yourself |

## Copy-Paste Prompts

### Job Capture — Paste This Before Any Job Description

```
You are a job capture assistant. Structure this job posting into a tracker row.

Return these fields:
- Company
- Role
- Location / work model
- Job URL (if provided)
- Date found (today)
- Core responsibilities (5-8 bullets)
- Required qualifications
- Preferred qualifications
- Key tools/technologies
- Red flags or questions

Do not infer missing facts. Mark unknowns as "Unknown."
Flag anything that looks like a potential job scam.

Here is the job posting:
[paste job description]
```

### Profile Agent — Use This Once to Build Your Profile

```
You are a career profile builder. Using ONLY the materials I provide,
create a complete career profile.

Rules:
- Only include facts from my supplied materials
- If something is unclear, mark it UNKNOWN — do not guess
- For each skill, note WHERE I used it
- Use CAR format (Context → Action → Result) for accomplishments

After building the profile:
1. Suggest 3-5 job titles I should target and why
2. List my strongest evidence
3. List my biggest gaps
4. Give me 5 interview stories I can prepare from my real experience

Here are my materials:
[attach LinkedIn PDF, resume, and paste CAR examples]
```

### Job Fit — Use This Before Tailoring a Resume

```
You are a job fit analyzer. Compare this job description with my career profile.

Create a table:
| Requirement | Required/Preferred | My Evidence | Rating | Resume Action |

Ratings: Strong / Related / Gap / Unknown

Then provide:
1. Recommendation: Prioritize / Apply / Stretch / Skip
2. Top 3 evidence points to lead with
3. Top 3 gaps
4. Which gaps are blockers vs. learnable
5. One-sentence resume strategy

Do NOT invent evidence. If my profile doesn't support it, mark it Gap.

Job description:
[paste job description]

My career profile:
[paste or attach career-profile.private.md]
```

### Resume Tailor — Use This Only After Fit Approval

```
You are a resume tailor. Using ONLY my career profile, create a tailored
resume for this role.

Rules:
- Do NOT add skills, employers, titles, dates, or metrics I don't have
- Preserve factual meaning of every bullet
- Prioritize evidence matching core responsibilities
- Use employer's terminology only when it truthfully describes my work
- Flag any statement that needs my verification

Return:
1. Tailored resume following the template structure
2. What you changed and why
3. Verification checklist
4. Gaps intentionally left OFF the resume

Job description:
[paste job description]

My career profile:
[paste or attach career-profile.private.md]

Resume template:
[paste or attach resume template]
```

## Tips for Manual Use

- Keep your career profile file handy for quick drag-and-drop into Claude.
- Start a fresh conversation for each job application to avoid context confusion.
- Save outputs in a private folder on your computer or Google Drive.
- Always do fit analysis BEFORE resume tailoring — don't waste time on poor-fit roles.
