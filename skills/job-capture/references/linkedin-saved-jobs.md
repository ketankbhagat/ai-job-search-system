# LinkedIn Saved Jobs → Google Docs + Tracker Sheet

Browser playbook for the Job Capture agent's default mode. Requires:

- Claude in Chrome, with the user logged into LinkedIn
- Google Drive connector **connected and enabled for this chat**

## Step 1 — Collect the saved job links

Open `https://www.linkedin.com/my-items/saved-jobs/` (it redirects to the Job tracker). The header shows a count per tab; the list shows 10 jobs per page.

On each page, collect the job links:

```js
JSON.stringify([...document.querySelectorAll('a[href*="/jobs/view/"]')]
  .map(a => a.href.split('?')[0])
  .filter((v, i, s) => s.indexOf(v) === i))
```

Click "Page 2", "Page 3", … and repeat. Confirm the total matches the tab count. This list is also the full **Job Link** column.

On a re-run, read the existing sheet first and drop job IDs that are already there.

## Step 2 — Read each job

```
navigate https://www.linkedin.com/jobs/view/<ID>/
wait 8s              # LinkedIn renders placeholders first; 3s is not enough
scroll down
wait 4s
click the "…more" / "See more" button on the description
wait 3s
read the page text
```

Expand the description:

```js
const b = [...document.querySelectorAll('button')]
  .find(x => /^(see more|…more|more)$/i.test(x.innerText.trim()));
if (b) b.click(); 'ok'
```

**Read the description with the page-text tool, not JavaScript.** JavaScript return values are truncated at about 1,000 characters, and descriptions run 5–10k. The page text includes unrelated content; ignore everything after "Set alert for similar jobs".

From the header, take: Company, Title, Location, Posted, and "N clicked apply". Easy Apply jobs show an applicant count instead. Record it as `N applicants (Easy Apply)`; the two numbers are not comparable.

If only the header came back, the page had not loaded. Reload and wait longer. Never record a job as empty.

## Step 3 — Company size (never guess the URL)

**Do not build the company URL from the company name.** Company page slugs are often unguessable, and a wrong guess can land on a real, unrelated company that returns a believable but wrong size.

Take the company link from the job page itself:

```js
JSON.stringify([...new Set([...document.querySelectorAll('a[href*="/company/"]')]
  .map(x => x.href.split('?')[0].replace(/(life|insights|jobs)\/$/, '')))].slice(0, 1))
```

Open `<company url>about/` and read the size band:

```js
const l = [...document.querySelectorAll('*')].filter(e => e.children.length === 0 && e.innerText);
JSON.stringify({ t: document.title,
  h: [...new Set(l.map(e => e.innerText.trim())
     .filter(x => /employees$/i.test(x) && x.length < 40))].slice(0, 2) })
```

Take the first result (e.g. `1K-5K employees`), and check that `document.title` names the expected company. Bands can be ranges, so don't match only on digits.

## Step 4 — Create the job-description Doc

Write the doc as HTML and let Drive convert it:

```
create_file
  title: "<Company> - <Job Title>"
  contentMimeType: "text/html"
  parentId: "<folder id>"
  textContent: "<html><body>…</body></html>"
```

```html
<h1>Job Title</h1>
<p><i>Company | Location | Salary (if listed)</i></p>
<p>Source: <a href="<job link>">LinkedIn job posting</a> | Posted X | N clicked apply</p>
<h2>Section</h2> … <ul><li>…</li></ul>
```

Escape `&` as `&amp;`. Put the doc link in the **JD Doc** column.

After each doc is created, append the finished row to a local scratch file. If something fails partway, only that job is lost.

## Step 5 — Write the sheet

**New sheet:** `create_file` with `contentMimeType: "text/csv"` creates a Google Sheet. Quote any field that contains a comma.

**Existing sheet:** the connector cannot edit cells, so type through the browser. Watch for three traps:

1. **A tab character inside typed text does not move to the next cell.** The whole line lands in one cell, and it can look correct in a screenshot. Press the `Tab` **key** between values instead. Typing `value\n` moves **down**, so filling one column top to bottom is the easiest method. If tabs did get into cells, select the range → Data → **Split text to columns**.
2. **Screen coordinates go stale.** If the window was resized, clicks land in the wrong place and typing goes nowhere. Take a fresh screenshot at the start and locate the Name box again.
3. **Move with the Name box, not by clicking cells.** Click the Name box, type the address (e.g. `B17`), press Enter, then type the value.

## Step 6 — Verify (do not skip)

Read the sheet back with the Drive connector:

- Every row has the right number of separate columns. A row shown as one long cell with tab entities was tab-joined; fix that row.
- The row count matches the number of saved jobs.
- The docs folder has one doc per job, no duplicates, and no doc that is nearly empty.

Problems like these are often invisible in the spreadsheet UI; the read-back is what catches them.

## Step 7 — Mark jobs you have a resume for (optional)

If the user keeps tailored resumes in one Drive folder, match them to rows:

1. List the folder (`parentId = '<folder id>'`). The folder ID is the last part of its URL.
2. Normalize each filename: remove the user's name prefix, the extension, any ` (1)` suffix, and trailing spaces; replace `_` with spaces; lowercase.
3. Normalize each Company: drop `Inc.`, `LLC`, `Ltd`, and anything in parentheses; lowercase.
4. It's a match if either contains the other. Run a second pass with spaces removed, which catches one-word filenames for two-word company names.
5. If a company has two resumes, use the most recently modified one and tell the user which file was skipped.

For each match set **Status** = `Applied`, **Applied On** = the resume's modified date, and **Resume** = its Drive link. Leave unmatched rows alone. Tell the user that "Applied On" is when the resume was last edited, not necessarily when they applied.

## Troubleshooting

| Problem | Fix |
|---|---|
| Drive returns 403 | Disconnect and reconnect Google Drive in Claude → Settings → Connectors |
| Drive tools missing in chat | The connector must be enabled for that chat |
| `create_file` timeout / "Resource has been exhausted" | Drive rate limit. Search for the title first (a file may have been created anyway), wait ~45s, retry |
| LinkedIn shows a warning or CAPTCHA | Stop the run and tell the user |

## Cost and Time

Plan on a few minutes per job. A run of ~30 saved jobs, including company sizes, can take over an hour. Use the check after the first 2–3 jobs to confirm the output looks right before continuing.
