# Fill Easy — Individual KYC by email

An interactive, single-file HTML demo of the individual KYC (background screening) flow:
fill the workbook, encrypt it, email it to the agent, get verification results back — and
keep talking to the agent in plain English on the same thread.

Built from four supplied sources:

- **`KYC_____1.xlsx`** — the `Background_Screening_Orders` template. All 18 bilingual column
  headers, the row-2 sample data, the mandatory-field rules and the instruction text are
  reproduced verbatim.
- **`Fill_Easy_HK_Agency_Onboarding_Data_Map_2.xlsx`** — the checks. Taken from the
  due-diligence map rather than invented, and ordered by what actually stops an application:
  **IA register** (licence and appointment history, whether the previous appointment is
  terminated, public enforcement actions) and **legal search** (bankruptcy & IVA at the ORO,
  litigation and court search, Companies Registry directorships) are the critical sections;
  identity and global screening sits behind them.
- **`Register_of_Licensed_Insurance_Intermediaries.pdf`** — the IA register layout. The licence
  particulars block in the summary report mirrors it field for field: Licence No., Licence Type,
  Licence Status, Licence Period, Line(s) of Business, Current Appointing Principal(s), Conditions
  of the Licence, Public Enforcement Actions in the last 5 years, and the previous-appointments
  table with appointment and termination dates.
- **`Fill_Easy_HK_Company_Search_Guide_v2.docx`** — the agent's reply format. The
  `Fill Easy` / `Automated Notification` bar, the `ORDER SUCCESS` / `PROCESSING (n)` /
  `NEED CLARIFICATION (n)` sections, the `Request ID | Request | Status` tables, the yellow
  *Your reply* column and the `Disclaimer.txt` attachment all follow the real emails.

## The flow

| # | Step | What happens |
|---|---|---|
| 1 | Fill the template | Fill rows 3–5, one per candidate. Mandatory: 中文名, the 18-digit 身份證號碼 and the English name. Columns S–U are added to the supplied template — the IA register, ORO and court searches key off the English name, HKID and licence number, not the Chinese name. |
| 2 | Encrypt the file | Set a password. The workbook is locked with AES-256 before it goes near email. |
| 3 | Attach and send | One email to `agent@ses.fill-easy.com` covering the whole batch, written in plain language. |
| 4 | Processed | The acknowledgement returns automatically: one request ID per check per candidate, plus a clarification for the blank academic-credential columns. |
| 5 | Results returned | `ORDER SUCCESS` as an encrypted workbook, plus the summary report — critical findings first, then IA register, legal search, and identity screening. |
| 6 | Reply in plain English | A follow-up chases the same-name writ on 王建国. No re-upload, same thread. |

One email covers three candidates and 22 checks. 王建国 is the case study: still appointed by
AIA with no termination date published (which blocks the IIC submission), a public enforcement
action from 2023, a writ matching his name, and an undisclosed directorship. The same-name writ
is what the step-6 follow-up resolves — ID-anchored matching separates a real hit from a false
positive, and the declaration stays with the recruit.

## Interactions

- **Step 1** — the grid opens on 7 key columns with **Show all 18 columns** to expand. Row 5's
  name and ID cells are editable; validation updates live per row, including the 18-digit
  mainland ID rule. **Fill row 5** completes it for you.
- **Step 2** — set a password (mismatch is rejected), or press **Use desk password**.
- **Step 3** — **Send** advances the demo.
- **Step 5** — the returned workbook opens only against the password you set in step 2.
  **View summary report** renders the report inline; **Download HTML** (print it to PDF),
  **Download CSV** and **Copy as CSV** export it. The guide documents the Summary Report as a
  real Fill Easy feature, requested with *"Please also generate a summary report"* — which is
  what the demo email says.

  Sandboxed embeds (including the published artifact) block file downloads at the browser
  level, so the inline view is the primary path and the page says so when it detects it is
  framed. The downloads work when `index.html` is opened directly or served from Pages.
- **Auto-play** runs the whole walkthrough on a timer, filling in each step's input as it goes.
  The **‹ ›** controls, the step pills, or the **← →** arrow keys navigate manually. **◐** switches
  light and dark; the page follows the OS setting on load.

## Viewing it

Open `index.html` in a browser — no build step, no dependencies, no network calls.

To serve it on GitHub Pages: **Settings → Pages → Source: "Deploy from a branch" → Branch:
`main` → Folder: `/ (root)` → Save.** It then publishes at
`https://zjlee8888.github.io/email-bot-demo-kyc-china/`, usually within a minute or two.

`index.html` is at the root so it serves directly, and `.nojekyll` stops Pages running the
files through Jekyll. No build step, so there is nothing to configure beyond that one setting.

There is deliberately no Actions workflow: `actions/configure-pages` cannot create the Pages
site from this repository (the workflow token gets *"Resource not accessible by integration"*),
so enabling Pages is a manual one-off either way and the branch source needs no workflow.

## Known gaps

- **`KYC1` / `KYC2`** appear as the codes the template uses. Their definitions weren't in the
  supplied documents, so the demo passes them through rather than inventing what they expand to.
- **Styling reference.** The layout follows the component idiom of the myPrudential Mainland China
  Verification demo: centered hero, a scrollable row of step pills with the active one filled,
  white cards floating on a tinted ground with soft shadows rather than hairline borders, fully
  rounded controls, and a single right-hand explanation card carrying an eyebrow pill, a segmented
  progress bar, and tinted `WHAT YOU DO` / `BEHIND THE SCENES` blocks. Colours are deliberately
  different from Prudential red: petrol `#0B5C63` takes the active/primary role, amber `#8A6108`
  is reserved for encryption state, green `#2C6E49` for verified. The site itself could not be
  reached from the build environment, so this was matched from a screenshot — the source CSS would
  let it be trued up exactly.
- **The agent notification block is not restyled.** The black `Fill Easy` / `Automated Notification`
  bar and its tables reproduce the real product email, so they deliberately sit outside the palette.
- Subject data (李慧敏, IDs, bank and certificate numbers) is fabricated. The `.example`
  sender domain is reserved for documentation. There is no mail server, no data source and no
  encryption actually running behind the UI.
