# Android Notification Inbox - Project Scope

Build a private Android inbox that turns captured notifications into items users can search and mark handled. The first release must prove repeat triage value beyond occasional recovery of an accidentally dismissed alert.

## Users and problem

Busy Android users lose context when notifications disappear or are dismissed and cannot easily track which alerts still need action. Focus on people who receive many work, delivery, or messaging alerts and already struggle to manage the system notification shade.

## MVP workflow

1. Explain notification access and let the user enable it in Android settings.
2. Choose which applications may be captured and select a retention period.
3. Search captured alerts and filter by app, date, unread, or handled state.
4. Mark an item handled, pin it, or set an inbox reminder for later review.
5. Delete captured history or export an explicitly selected encrypted archive.

## Included in the MVP

- NotificationListenerService capture while access is enabled, with visible status and an app allowlist.
- Room-backed local history, full-text search, app/date filters, and explicit capture limitations.
- Separate read and handled states, pinning, and app/conversation grouping where metadata permits.
- Update deduplication using notification identity without multiplying repeated updates into unrelated items.
- User-controlled retention, deletion, and rules that hide chosen items from this inbox.
- Local reminder/snooze within this app, without claiming to restore or control the original notification.
- Conservative extraction of tracking numbers and dates from supported non-sensitive text, with source context and user confirmation.
- Encrypted selected-history export/import with exclusion rules for sensitive content.

## Deferred features

Cross-device synchronization, advanced analytics, AI classifiers, automatic task/calendar actions, cross-app reply automation, and changes to system notification delivery are outside the MVP. No iOS version is planned initially. Do not promise access to past notifications never captured, OS-redacted text, reliable OTP capture, or unavailable app actions.

## Proposed architecture and data

Use Kotlin, Jetpack Compose, NotificationListenerService, Room/SQLite full-text search, and WorkManager for deferrable cleanup and reminders. Separate listener ingestion, normalization/deduplication, retention, indexing, triage state, and optional extraction. Reminder timing must acknowledge platform scheduling and battery limits.

Persist NotificationRecord, NotificationRevision, AppPolicy, TriageState, Reminder, and ExtractionSuggestion entities. Keep source keys and timestamps distinct from local workflow state so notification updates do not reset handled decisions. Store sender/conversation metadata only when supplied and permitted.

## Privacy, permissions, and cost

Default to user-selected apps, omit financial and authentication content until specifically enabled, and avoid persisting recognized OTPs or sensitive notification categories. OS-redacted content stays redacted. Explain that custom messages may still contain sensitive text and give users per-app exclusions. Protect local storage with platform controls and avoid notification bodies in logs or telemetry.

Revoking notification access must stop new capture. Local deletion and retention must remove indexed text too. Encrypted archives require a separate user-controlled key and must identify their sensitivity. The core inbox must operate offline without a hosted account, paid inference, or developer-operated storage.

## Acceptance criteria

- Enabled allowed apps produce searchable local records; excluded apps produce no stored content.
- Notification updates are deduplicated and preserve the user's handled and pinned state.
- Search, mark handled, pinning, and history viewing work offline after notifications are captured.
- Access revocation, restart, and process termination produce accurate capture status and preserve saved history.
- Sensitive-content and redaction fixtures verify that excluded payloads do not enter indexes, exports, or logs.
- Retention expiry and explicit deletion remove records and search results consistently.
- Reminders do not claim exact delivery or recreate original system actions; scheduling limitations are visible.
- Wrong-password or corrupt imports leave existing history intact; successful imports retain triage state.

## Delivery and validation

| Phase | Deliverable | Exit condition |
|---|---|---|
| 1 | Access controls, local capture, and search | Users understand access and find captured alerts reliably. |
| 2 | Handled state, pinning, and reminders | Users return to triage alerts throughout a week. |
| 3 | Retention, export, and platform testing | Data lifecycle and permission changes behave predictably. |

Budget 4-7 solo-developer weeks as a planning estimate. Validate local history, search, and mark-handled first; measure daily triage sessions, items handled, and one-week repeat use. Native Android history, device-specific background limits, policy changes, and permission distrust are the main risks. If use is limited to accidental dismissal recovery, keep the product a small utility. A future one-time purchase for advanced local rules or analytics remains an unvalidated option.

## Scope basis

This scope is derived from the supplied report, "Recent Verified User Pain Points Suitable for Zero-Backend SaaS, Mobile Apps, and Browser Extensions" (research window ending November 3, 2026). Market demand, estimates, and monetization are hypotheses to validate; the report's internal citation markers are not independently verified references. This repository starts with this scope document only; implementation has not started.
