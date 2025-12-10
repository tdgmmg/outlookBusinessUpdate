Project Summary — Outlook–CRM Contact Enrichment System

1. Purpose

The project builds an automated, auditable workflow—implemented in Google Apps Script, integrated with Google Sheets, and deployable through GitHub + Codex agents—that enriches Outlook contact records with the correct business (company) names coming from a CRM export.

The CRM correctly stores the business associations, but Outlook does not.
This system fills that gap reliably and safely.


---

2. Core Problem

Outlook contact exports include first name, last name, email, but often no business name.

The CRM contains the correct company → contact relationships, but its Outlook sync does not populate company names.

Many businesses have multiple contacts, so the system must support 1:N relationships.

Human oversight is required—matches cannot be blindly applied.



---

3. Solution Overview

The system revolves around a two-stage enrichment pipeline:

Stage 1 — Prepare Staging

Reads Outlook export from outlookContacts.

Extracts distinct (firstName, lastName, email) tuples.

Generates a normalized and deduplicated staging table (stagingReview).

Adds a persistent rowKey (hash) so data can be tracked across runs.


Stage 2 — Suggest Companies

Loads CRM data from crmAccounts.

Builds three indexes:

Name key

Email key

Email domain key


Attempts to match each staging row using:

1. Exact email match


2. Exact name match


3. Domain-based inference


4. Fuzzy name similarity



Writes:

suggestedCompany

matchType

confidence

Notes for ambiguous cases


Writes all failures/unknowns to an unmatched tab.


Human Review

User reviews each staging row and checks skip for anything incorrect.

Ambiguous matches always appear blank by design.


Apply Suggestions

Applies only:

Non-skipped rows

With confident exact/email matches


Writes the company value back into Outlook’s companyOutlook column.

Outputs metrics for every run.



---

4. How Configuration Works

A separate config tab allows mapping logical fields to actual header names in Outlook and CRM.

Example:

logicalField	outlookHeader	crmHeader

firstName	Given Name	FirstName
lastName	Surname	LastName
email	E-mail Address	Email
companyOutlook	Company	—
accountNameCrm	—	AccountName


This ensures resiliency even when CRM or Outlook exports change column headers.


---

5. Technology & Architecture

Technical Stack

Google Apps Script (pipeline logic + menu)

Google Sheets (data store + review UI)

GitHub + Codex (agentic coding and PR automation)

Clasp (continuous deployment from GitHub → Apps Script)


Files (logical modules)

menu.js — sheet menu and entrypoints

logging.js — structured logging utilities

constants.js — sheet names and thresholds

sheets.js — IO utilities, config loader, metrics writer

utils.js — normalization, hashing, helpers

matching.js — match engine

pipeline.js — prepare/suggest/apply workflow

tests.js — test harness



---

6. Agent Workflow (Codex)

Codex performs development using:

PR-based workflows

Asynchronous, parallelizable tasks

Strict coding scaffolds with zone markers

Tests and dryRun expectations

Logging and metrics consistency rules


Codex does not directly deploy—CI does.


---

7. CI/CD

The CI pipeline performs:

1. Lint


2. Unit + integration tests (runAllTests)


3. Clasp deploy on merge to main



Deployment requires stored secrets:

CLASP_CLIENT_ID

CLASP_CLIENT_SECRET

CLASP_REFRESH_TOKEN

SCRIPT_ID


Only CI can push code to GAS (Apps Script).


---

8. Quality & Safety Rules

skip prevents unintended application of suggestions

Ambiguous results never auto-fill

Fuzzy matches require minimum threshold

All errors logged with structured payloads

All updates tracked in metrics

Idempotent runs using stable rowKey



---

9. Status & Deliverables

Codex is expected to deliver:

Fully functional pipeline code

Tests that validate every stage

Clear documentation (docs/architecture.md, FunctionIndex, test index)

PRs with dryRun evidence and metrics

Deployment through CI


You will receive a system that is:

Deterministic

Testable

Observable

Reviewable

Codex-automatable


