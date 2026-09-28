---
name: profile-agent
description: "Build or update a factual career profile from a LinkedIn PDF, resume, and CAR/PAR accomplishment examples. Suggests target job titles. Saves the profile to Google Drive. Use when the user wants to create their career profile, update it, or get job title suggestions."
---

# Profile Agent

## Purpose

Create a private, reusable source of truth for the user's career — then suggest job titles they should target. Save the result to Google Drive.

## Required Rule

Only record facts the user supplied or confirmed. Do not infer impressive details because they sound plausible. If something is unclear, mark it `UNKNOWN` and ask.

## Expected Inputs

1. **LinkedIn profile PDF** — exported from LinkedIn
2. **Current resume** — any format
3. **3-5 CAR/PAR examples** — real accomplishments from the last 5 years (or school/internships for new grads)

CAR = Context → Action → Result
PAR = Problem → Action → Result

## Process

### Phase 1: Extract Evidence

From the supplied materials, build a structured profile covering:

- Target roles and industries (leave blank initially — you'll suggest these)
- Education (degree, school, graduation, relevant coursework)
- Skills WITH evidence of where each was used
- Experience / internships / employment (with CAR-format bullets)
- Projects (problem, contribution, technologies, result)
- Research / labs / campus work / volunteer experience
- Certifications
- Evidence stories for interviews (teamwork, problem-solving, communication, leadership, learning)

For each skill listed, note the **source**: which project, job, or course demonstrates it.

### Phase 2: Suggest Target Job Titles

Based on the extracted evidence, suggest:

1. **3-5 primary job titles** the user is well-qualified for, with reasoning
2. **2-3 stretch titles** they could pursue with some gaps, noting what's missing
3. **Titles to avoid** — roles where evidence is too thin to be credible

For new graduates: consider Associate/Junior/Entry-level variants, rotational programs, and titles that value project + internship experience.

### Phase 3: Return and Save

Use the structure from `references/career-profile.template.md` if available.

Return:
1. The complete career profile (ready to save as `career-profile.private.md`)
2. Suggested target job titles with reasoning
3. Facts that need user verification (marked with ⚠️)
4. Important evidence gaps
5. Five interview stories the user can prepare from real experience

**If Google Drive is connected:** Save the career profile as `career-profile.private.md` in the user's Google Drive so the other agents can access it automatically.

## Privacy

Do not request or store: street address, date of birth, government ID, banking data, passwords, tokens, or confidential employer/client materials.

## Updates

When the user comes back with new experience, update the existing profile in Drive — do not rebuild from scratch.
