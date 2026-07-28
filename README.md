# Fill Easy — Individual KYC by email

An interactive, single-file HTML demo of the individual KYC (background screening) flow:
fill the workbook, encrypt it, email it to the agent, get verification results back — and
keep talking to the agent in plain English on the same thread.

Built from three supplied sources:

- **`KYC_____1.xlsx`** — the `Background_Screening_Orders` template. All 18 bilingual column
  headers, the row-2 sample data, the mandatory-field rules and the instruction text are
  reproduced verbatim.
- **`Fill_Easy_HK_Agency_Onboarding_Data_Map_2.xlsx`** — the data points. The demo uses the
  key checks from the due-diligence map rather than invented ones: Mainland ID verification,
  Global PEP / terrorist screening, Global sanctions & adverse media, the Mainland court and
  enforcement layer (失信 / 限高), the consented criminal / police record, and academic
  credential verification through CHESICC / 學信網. The last three are the
  Mainland-background (MCV) layer — for HK-local recruits the map records those channels as
  N/A or non-existent, and the demo says so.
- **`Fill_Easy_HK_Company_Search_Guide_v2.docx`** — the agent's reply format. The
  `Fill Easy` / `Automated Notification` bar, the `ORDER SUCCESS` / `PROCESSING (n)` /
  `NEED CLARIFICATION (n)` sections, the `Request ID | Request | Status` tables, the yellow
  *Your reply* column and the `Disclaimer.txt` attachment all follow the real emails.

## The flow

| # | Step | What happens |
|---|---|---|
| 1 | Fill the template | Fill rows 3–5 of `Background_Screening_Orders`, one per subject. Mandatory: 中文名 and the 18-digit 身份證號碼. Column Q sets the scope per person. |
| 2 | Encrypt the file | Set a password. The workbook is locked with AES-256 before it goes near email. |
| 3 | Attach and send | One email to `agent@ses.fill-easy.com` covering the whole batch, written in plain language. |
| 4 | Processed | The acknowledgement returns automatically: one request ID per data field per subject, plus a clarification for the blank academic-credential columns. |
| 5 | Results returned | `ORDER SUCCESS` with verification outcomes as an encrypted workbook, plus a downloadable summary report. |
| 6 | Reply in plain English | A follow-up reply adds the driving-licence check. No re-upload, same thread. |

One email covers three subjects, and the results come back per data field per person — so a
single flagged bank account shows on its own line instead of sinking the batch.

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

Hosted on GitHub Pages: **Settings → Pages → Source: "Deploy from a branch"**, then this
branch with folder `/ (root)`. `index.html` is at the root, so it serves directly.
`.nojekyll` stops Pages running the files through Jekyll.

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
