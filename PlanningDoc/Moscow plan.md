MoSCoW Plan — Outlook–CRM Contact Enrichment System

Must Have (M)

These are essential for the system to function correctly and safely.

1. Config-driven field mapping

config tab defining logical fields → actual Outlook/CRM headers.



2. Two-stage enrichment workflow

prepareStaging() → extract/deduplicate Outlook contacts.

suggestCompanies() → generate company matches.



3. Match engine (baseline methods)

Exact email match

Exact normalized name match

Domain-based match (unique domain only)



4. Human-review staging sheet

stagingReview tab with skip checkbox to prevent application.



5. Safe application step

applySuggestions() writes only non-skipped, high-confidence matches.



6. Auditing & traceability

Structured logs (logStep) for all major actions.

Metrics tab with processed/updated/skipped/failed counts.

Ambiguous matches must never auto-fill.



7. CI/CD workflow

Lint → Tests → Clasp Deploy (no direct pushes to GAS).



8. Idempotency & stability

rowKey ensures consistent staging even after refreshes.

No destructive operations without validation.





---

Should Have (S)

Important for quality, but the system still functions without them.

1. Fuzzy matching module

Name similarity scoring with thresholds (e.g., Jaro-Winkler).



2. Email-domain heuristics

Boost confidence when multiple contacts share a domain.



3. Alias dictionary support

Allow William → Bill, Robert → Bob, company alias normalization.



4. Unmatched reporting

Dedicated unmatched tab with reasons and suggestions.



5. PR standards for Codex

Required scaffolds, logs, dryRun output, and docs per PR.



6. Enhanced error annotations

Inline status markers like Status = Error:<msg> where applicable.





---

Could Have (C)

Valuable enhancements but lower priority.

1. Interactive UI improvements

Color coding or conditional formatting in stagingReview.



2. Auto-detection of header variations

Suggest best mapping if a header changes.



3. Metrics visual dashboard

Charts for match distribution, daily runs, error rates.



4. Optional phone-number matching

Only as a confidence booster—not primary key.



5. “What changed?” diff report

Before/after summary for applied updates.





---

Won’t Have (W)

Explicitly out of scope for this version.

1. Direct Outlook or CRM API sync

System relies on exported CSV uploads.



2. Automated CRM or Outlook data cleaning

No rewriting or normalization of source data files.



3. Machine-learning models

Matching remains rule-based + fuzzy heuristics only.



4. Real-time enrichment

Workflow is batch-based, triggered manually.
