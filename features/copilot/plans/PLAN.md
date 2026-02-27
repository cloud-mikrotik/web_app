# Copilot Feature Plan — CollectorApp

## Summary
Add a "Copilot" feature set to improve developer productivity and/or in-app assistance for collector users. This document defines scope, goals, architecture options, acceptance criteria, milestones, and tasks.

## Goals
- Provide contextual code/help suggestions for developers working on CollectorApp (developer Copilot).
- Optionally provide an in-app assistant to help field collectors (UX assistant) with tips, automated form-filling, or guided workflows.
- Integrate securely with existing backend and CI/CD processes.
- Keep changes incremental and testable.

## Scope
Included:
- Design and implement a developer-facing Copilot integration (VS Code / CI hints / PR suggestions).
- Design an optional lightweight in-app assistant UI (help panel, suggested actions).
- Backend API endpoints if needed for telemetry/feature toggles.
- Authentication and privacy guardrails (no leaking customer data).
Excluded:
- Large LLM model hosting (use external API / provider).
- Major UX redesign.

## User Stories
- As a developer, I want inline suggestions for repetitive code patterns (DAO, repository), so I can implement features faster.
- As a QA engineer, I want suggested test stubs for new activities/fragments, so coverage improves.
- As a collector, I want a help overlay that suggests next steps in CollectionsActivity, so I reduce errors in the field.

## Acceptance Criteria
- Developer suggestions appear in local editor or as CI comments for new PRs.
- In-app assistant (if enabled) shows context-aware tips in one or two screens (CollectionsActivity, AssignmentDetailActivity).
- Feature toggle exists in SettingsActivity to enable/disable assistant.
- No sensitive customer data is sent to third-party services without explicit consent and masking.
- Unit and integration tests for new services and privacy masking.

## Architecture Options
- Developer Copilot:
  - Option A: Use GitHub Copilot / GitHub Actions + PR bot for suggestions (lower maintenance).
  - Option B: Integrate an LLM API (OpenAI, Anthropic) with internal middleware to control prompts and masking.
- In-App Assistant:
  - Client-side lightweight rule-based suggestions + server-side enrichment for complex queries.
  - Or client calls LLM via backend proxy that performs data redaction.

## Components to Modify / Add
- app/src/main/java/com/ispbilling/collector/
  - services/AssistantService.java (new)
  - ui/help/AssistantOverlayFragment.java (new)
  - viewmodels/AssistantViewModel.java (new)
  - settings entry in SettingsActivity.java and SharedPreferencesManager
- backend (if present): new API endpoints for assistant prompt handling and telemetry

## Privacy & Security
- Implement strict redaction rules in EncryptionUtils or a new RedactionUtils.
- Require opt-in for any data sharing.
- Log only anonymized telemetry.

## Tasks & Rough Estimates
- Discovery & design (1–2 days)
- Prototype developer Copilot workflow (PR bot or Action) (2–3 days)
- Implement AssistantService + UI skeleton (3–5 days)
- Backend proxy + redaction (3–4 days)
- Tests & QA (2–3 days)
- Rollout & monitoring (1–2 days)

## Milestones
1. Design approved (UX + privacy)  
2. Developer Copilot prototype in CI (Action + PR comments)  
3. In-app assistant MVP (overlay + toggle)  
4. Production rollout and monitoring

## Risks
- Exposure of PII if redaction is incomplete.
- Cost and latency from LLM API usage.
- Acceptance by end users and developers.

## Metrics
- Developer time saved (qualitative initially)
- Number of PR suggestions accepted
- Collector error rate before/after assistant
- API call volume / cost

## Next Steps
1. Confirm scope (developer-only vs developer + in-app).
2. Choose provider and hosting model for LLM features.
3. Create issues/epics and assign owners.
4. Build prototype for first milestone.