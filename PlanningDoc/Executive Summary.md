Executive Summary — Outlook–CRM Contact Enrichment System

This project delivers an automated, auditable workflow that enriches Outlook contact records with the correct business names sourced from a CRM system. The CRM stores accurate company associations, but Outlook does not—creating gaps that affect communication, reporting, and customer management. This solution resolves that issue through a controlled, reviewable enrichment pipeline built on Google Apps Script, Google Sheets, and a GitHub/Codex development workflow.

The system operates in two stages: a staging phase that extracts, normalizes, and deduplicates contacts, followed by a company-suggestion engine that matches contacts to CRM accounts using exact, email-based, domain-based, and fuzzy matching strategies. Users review results in a dedicated staging sheet, marking any rows to skip. Only rows that are safe, unambiguous, and not skipped are written back into Outlook.

A configuration sheet allows dynamic mapping of column headers from CRM and Outlook, ensuring resilience against export format changes. All critical operations produce structured logs and write audit entries to a metrics tab, supporting traceability and diagnostics. Ambiguous matches are never auto-applied, enforcing data integrity.

The project is developed using OpenAI’s Codex agentic workflow, which enables asynchronous, parallel task execution, PR-driven code delivery, and rigorous testing. GitHub Actions manage CI/CD, with clasp-based deployments ensuring that only validated code reaches the Apps Script environment.

In full, the system provides a robust, maintainable, and transparent method for restoring correct business context to Outlook contacts—improving data quality, reducing manual effort, and enabling more reliable downstream operations.
