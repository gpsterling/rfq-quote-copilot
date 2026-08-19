# RFQ Quote Copilot

**Turn a messy customer email into a quote-ready job ticket in one second.**

Every manufacturer runs on RFQs — and most of them arrive as rambling emails: "hey, need 500 brackets, 6061 aluminum, same as the 2023 run, by Oct 3." A human reads it, re-types it into a spreadsheet, misses the tolerance callout, quotes wrong, eats the margin. Copilot does the reading.

**Try it:** open `index.html` in any browser. Click a sample RFQ (or paste your own), hit **Extract job ticket**. No install, no account, nothing leaves your machine.

## What it does

1. **Extracts the job ticket** — part, quantity, material, deadline, tolerance, finish, drawings — from unstructured email text.
2. **Flags what's missing — with the business reason.** This is the part that saves money. A blank field isn't just blank:
   - *"No tolerances called out — quoting tight-tolerance work at loose-tolerance prices kills margin."*
   - *"Material not specified — biggest cost driver in the quote."*
3. **Runs the quote math** — type a unit price and setup fee, the total updates live off the extracted quantity.
4. **Copies a clean summary** for pasting straight into the reply.

The three built-in samples demonstrate the range: a complete RFQ (everything extracts green), a vague one (the ask-before-quoting checklist does the work), and a rush job with a STEP file.

## Why this exists

The cost of a bad quote isn't the quoting time — it's the re-quote, the margin eaten by an unpriced tolerance, or the job lost because a competitor answered in an hour. The fastest, most accurate quote usually wins. This tool attacks the slowest, most error-prone step: translating a customer's email into structured job data.

## How it works

A deterministic extraction engine: ordered pattern rules per field, specific patterns before loose keywords (so "6061 aluminum" wins over "aluminum"). Missing fields aren't failures — they're the product. Each `null` becomes a line on the ask-before-quoting checklist.

Two bugs caught and fixed during testing, both documented in the code: quantities leaking into part names ("50 flange"), and "d**raw**ings" false-matching the finish "raw" — the reason finish matching is whole-word only.

## The AI-first version

The extraction sits behind one function, deliberately. In the production architecture, that function routes to **Claude** for genuinely unstructured emails — multi-part RFQs, revision references ("same as the 2023 run"), attachments — with these rules as the deterministic fallback and audit baseline. The UI, checklist, and quote math never know which engine answered.

Rules where behavior must be predictable and auditable; AI where language gets ambiguous. Same architecture as my other projects, aimed at revenue instead of chores.

## What I'd build next

- Price-break table (100 / 250 / 500 / 1,000 quantities) auto-generated per quote
- History: "you quoted these brackets at $4.10 in 2023" from past quote data
- Email-inbox mode: RFQs auto-ticketed on arrival, quoter starts from a ticket, not an email

## Stack

Single HTML file. Vanilla JavaScript, no frameworks, no build step. Deliberately boring plumbing — the extraction and the checklist are the product.

---

*Built AI-first: developed with Claude, directed and tested by me against the sample set until extraction was clean.*
