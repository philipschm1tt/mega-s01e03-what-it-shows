# mega-s01e03 · what it shows

Deliverables for the MEGA.dev course Challenge **S01E03 — what it shows**: the support desk
merged 412 reports about dates into one ticket, **#4471**, and marked it known as a single
bug. The kb export says otherwise: #4471 holds **two distinct problems**. This repo files
one ticket per problem and carries the kb page about them.

## The two problems in #4471

| cause | reports | what it is | who it hits |
|---|---|---|---|
| `parser` | **379** of 412 | the shared parser reads every date month first; every tenant on file writes the day first, so their dates come out flipped. The known one — this is what the queued fix covers. | all 11 tenants, every channel except booking-v3's form (2026-04-01 → 2026-10-04) |
| `hint` | **33** of 412 | the booking-v3 form prints `MM/DD/YYYY` under its date field; people type month-first as told, and the form reads it day-first per the tenant's locale. A second flip, in the form's own field, before the shared parser is reached. **No ticket of its own — the queued fix does not cover it.** | the 3 booking-v3 tenants (Alder Physio 12, Copper Studio 11, Pale Harbour 10), form channel only (2026-04-17 → 2026-09-22) |

Counts come from the kb export dated 2026-10-04: 427 export rows, **412 unique reports**
(15 ids appear twice — overlapping export pages; one id, one report), grouped by cause with
the desk's own logic in `causes.ts`: each tenant's locale and booking form, never intake's
`meta.intakeTag` guess, which disagrees with the real cause for 38 of the 412 reports.
5 reports the desk read as a one-day move stay counted where they were filed, footnoted
under `parser` (`desk-notes.json`), per the desk's convention.

## Tickets (GitHub issues)

One issue per problem, each with the count of reports it covers:

- [Issue 1 — `parser`: the shared parser reads day-first dates month-first (379 reports)](https://github.com/philipschm1tt/mega-s01e03-what-it-shows/issues/1)
- [Issue 2 — `hint`: the booking-v3 date hint contradicts its own form (33 reports)](https://github.com/philipschm1tt/mega-s01e03-what-it-shows/issues/2)

## Files

- [`src/routes/kb/incidents/4471-causes/+page.svx`](src/routes/kb/incidents/4471-causes/+page.svx) —
  the page added to the kb: signed (`signature` in frontmatter, `<RenderStamp />` at the end),
  per the kb's conventions. It computes its numbers at render time through the desk's own
  `<CauseTable ticket={4471} />` rather than typing them.
- [`dist/4471-causes.html`](dist/4471-causes.html) — the page as the desk will see it:
  rendered through the kb's `runbook` layout from the same export, including the signed
  desk ref.

The kb page also lives in the local copy of the kb export at
`support-kb/src/routes/kb/incidents/4471-causes/+page.svx`, where it slots straight into
`src/routes/kb/incidents/`.

—

Filed by OpenCode, Philip Schmitt's harness, for the MEGA.dev course, Challenge S01E03.
Public by the Challenge's rules; delete after the course accepts.
