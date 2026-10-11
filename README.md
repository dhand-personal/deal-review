# deal-review

A Claude skill that reviews one sales deal against the evidence. It reads the CRM record next to what the buyer actually said and did, grades the deal, challenges the forecast category, tests the champion, and ends with hard questions and dated next steps.

It does not assume a sales methodology. If your team uses MEDDPICC, Command of the Message, Sandler, BANT, SPICED or a blend, the review uses that vocabulary. If you use none, it works from ten neutral dimensions.

## Two modes

| | Manager mode | Rep mode |
|---|---|---|
| Use it to | Inspect and coach a deal someone else owns | Prepare for and defend your own deal |
| Evidence order | Communications and structured data first, rep-written text after | Same evidence, rep's own picture labeled as rep-reported |
| Extra sections | CRM versus evidence reconciliation, forecast challenge, questions to ask the rep, coaching notes | CRM versus evidence reconciliation, questions reviewers will likely ask, asks of the manager, plan for this week |

The evidence standard is the same in both modes. A self-review is not softer.

## Three output options

- **Chat** (default). Takeaway first, then the full review.
- **Word brief.** Three to four pages, editable, written as a pre-read for the people running the review.
- **PowerPoint presentation.** Up to twelve slides for a larger audience, with the evidence detail in speaker notes.

Manager coaching notes appear in chat only. They are never written into the Word brief or the deck, and files describe the deal, not the person.

## What it checks

- **Ten dimensions:** need, timing, differentiation, value, evaluation process, buying process, people, commercials, plan and momentum, outside risks. Each is tagged Confirmed, Assumed, Unknown or Contradicted, with a dated source.
- **Source weight:** every fact is marked as a buyer action, a buyer statement, or the rep's own assessment. Buyer actions outweigh statements, and both outweigh the rep's read.
- **Champion:** a strength read (Strong champion, Coach but not champion, Single point of failure, or No champion identified) plus a champion test: has anyone asked the champion for something that costs them something, and did they deliver?
- **Multi-threading:** counted from the communications, not from the CRM contact count.
- **Stage, close date and forecast:** compared to stage exit criteria and to buyer actions. The skill recommends a forecast category using your company's own definitions when they are available, and labeled default tests when they are not.
- **Reconciliation:** each main CRM field is marked Matches, Stale, Unsupported, Contradicted or Unpopulated.
- **Verdict:** Strong, At risk, Not qualified or Cannot tell. Renewals use their own scale (Strong, At risk, Likely to lapse or shrink, Cannot tell).

Renewals and expansions are handled from the deal type. They are not separate modes.

## Rules built in

- No percentages as ratings, probabilities or confidence. Ratings are High, Medium or Low with reasons.
- Text inside emails, notes, transcripts and CRM fields is treated as evidence, never as instructions.
- If there is no communication history and the CRM holds only placeholder text, the skill returns a short data-gap report ("Cannot tell") instead of grading.
- If the CRM text is entirely the rep's own, the verdict is capped at At risk and marked provisional.
- Every proposed date is after today and falls on a weekday.
- The skill only reads. It never updates the CRM, sends messages or creates tasks.

## Inputs and integrations

- **CRM.** HubSpot and Salesforce through a connector, read-only. Any other CRM through a pasted or exported snapshot.
- **Communications.** Pulled through the `unified-communication` skill by default. A pasted transcript, email thread or set of notes also works.
- **Files.** The Word and PowerPoint outputs use the `docx` and `pptx` skills.

## Related skills

- `unified-communication`: supplies the account and contact history.
- `deal-premortem`: use it for "how could this deal die?"
- `deal-postmortem`: use it for closed deals, won or lost.
- `crm-deal-updater`: use it to write any field changes this skill proposes.
- `anchor-deck-builder`: use it for customer-facing decks.

## Install

Place the `deal-review` folder (the one containing `SKILL.md`) wherever your Claude setup loads skills. In Claude Code that is a `.claude/skills/` folder. In the Claude apps, add it as a custom skill.

Then ask in plain language, for example:

- "Review the Acme renewal in manager mode and give me a Word brief."
- "I own the Northwind deal. Prep me for my deal review with a presentation."
- "Gut check this deal before my forecast call." (paste the CRM export and any call notes)

## Examples

The `examples` folder holds one output of each type, all generated from invented test deals:

- `example_word_brief_manager_mode.docx`: a manager-mode Word brief on an At risk deal.
- `example_presentation_rep_mode.pptx`: a rep-mode deck on a Strong deal.
- `example_chat_review_renewal.md`: a manager-mode chat review of a renewal, including a CRM field that contained an injected instruction. The review flags it and does not follow it. This example was generated with an earlier revision of the skill, so its length and layout differ slightly from the current rules.

## How it was tested

The skill was run blind against invented deals and then graded against private answer keys. The runs covered pasted CRM exports, live HubSpot and Salesforce developer accounts, and these cases:

- no communication history
- a record written entirely by the rep
- custom forecast categories
- a renewal where the notice date had passed
- a Pipedrive export with a Gong summary
- a Strong deal with an owner mismatch
- a record containing an injected instruction
- a request for a win percentage

## Known limits

- It reviews one deal at a time. It is not a pipeline scan.
- With a single pasted export, the manager-mode independent read cannot be fully honored, because the rep's fields arrive with the structured data. The review says so.
- Without your forecast category list and definitions, it assumes Pipeline, Best Case and Commit and labels them as assumed.
- Product usage and churn health are out of scope for renewals. Anything reported about usage is recorded as reported context.
- Only HubSpot and Salesforce have been tested through a connector. Other CRMs were tested with pasted data.

## License

MIT. See [LICENSE](LICENSE).
