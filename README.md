# Fill Easy — Individual KYC by email

An interactive, single-file HTML demo of the individual KYC (background screening) flow:
fill the workbook, encrypt it, email it to the agent, get verification results back — and
keep talking to the agent in plain English on the same thread.

Built from two supplied sources:

- **`KYC_____1.xlsx`** — the `Background_Screening_Orders` template. All 18 bilingual column
  headers, the row-2 sample data, the mandatory-field rules and the instruction text are
  reproduced verbatim.
- **`Fill_Easy_HK_Company_Search_Guide_v2.docx`** — the agent's reply format. The
  `Fill Easy` / `Automated Notification` bar, the `ORDER SUCCESS` / `PROCESSING (n)` /
  `NEED CLARIFICATION (n)` sections, the `Request ID | Request | Status` tables, the yellow
  *Your reply* column and the `Disclaimer.txt` attachment all follow the real emails.

## The flow

| # | Step | What happens |
|---|---|---|
| 1 | Fill the template | Complete row 3 of `Background_Screening_Orders`. Mandatory: 中文名 and the 18-digit 身份證號碼. Column Q says which data fields you want. |
| 2 | Encrypt the file | Set a password. The workbook is locked with AES-256 before it goes near email. |
| 3 | Attach and send | One email to `agent@ses.fill-easy.com` with the encrypted workbook attached, written in plain language. |
| 4 | Agent reads it | The agent decrypts the attachment, reads row 3, and highlights the request in the email body. |
| 5 | Processing reply | Standard Fill Easy notification: one request ID per data field, plus a clarification for the blank academic-credential columns. |
| 6 | Results returned | `ORDER SUCCESS` with verification outcomes, delivered as an encrypted workbook — same password. |
| 7 | Reply in plain English | A follow-up reply adds the driving-licence check. No re-upload; the agent carries the subject from the thread. |

The point of the demo is the split between the two inputs: **the workbook carries the
structured identifiers, the email body carries the intent**, and the agent merges them. The
two extra checks in step 4 (professional qualification, bank account) were asked for in
prose and never appeared in column Q.

## Interactions

- **Step 1** — four cells in row 3 are editable. Type into them and validation updates live,
  including the 18-digit mainland ID rule. **Fill sample row** completes it for you.
- **Step 2** — set a password (mismatch is rejected), or press **Use desk password**.
- **Step 3** — **Send** advances the demo.
- **Step 6** — the returned workbook opens only against the password you set in step 2.
- **Next / Back**, the numbered stepper, or the **← →** arrow keys navigate. **◐** switches
  light and dark; the page follows the OS setting on load.

## Viewing it

Open `index.html` in a browser — no build step, no dependencies, no network calls.

Hosted on GitHub Pages: **Settings → Pages → Source: "Deploy from a branch"**, then this
branch with folder `/ (root)`. `index.html` is at the root, so it serves directly.
`.nojekyll` stops Pages running the files through Jekyll.

## Known gaps

- **`KYC1` / `KYC2`** appear as the codes the template uses. Their definitions weren't in the
  supplied documents, so the demo passes them through rather than inventing what they expand to.
- **Styling reference.** The brief was to mirror the component style of
  `fill-easy-limited.github.io/prudential-demo-china-login`. That site was unreachable from
  the build environment (network policy blocks all outbound browsing), so the chrome is built
  in a form-first idiom — card panels, labelled inputs, numbered stepper, prominent primary
  button — with a deliberately different palette: petrol `#0B5C63` primary, amber `#8A6108`
  reserved for encryption state, green `#2C6E49` for verified. The agent notification block is
  *not* restyled; it reproduces the real template. Point me at that CSS and the chrome can be
  trued up to it.
- Subject data (李慧敏, IDs, bank and certificate numbers) is fabricated. The `.example`
  sender domain is reserved for documentation. There is no mail server, no data source and no
  encryption actually running behind the UI.
