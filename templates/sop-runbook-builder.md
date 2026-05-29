---
title: SOP Runbook Builder
app_type: sop-runbook-builder
wallet: 0x434210b9eBFC183b6defb42815efeCED11C68764
---

Build a polished single-page React app called "Runbook Desk" for operators, founders, and small teams who need to turn messy process notes into clean standard operating procedures without signing into a SaaS tool.

The app should feel like a compact work surface rather than a marketing page. It must open directly into the tool with no landing-page hero. The first viewport should show a left-side editor and a right-side live runbook preview.

## Core job

The user writes a reusable SOP or incident runbook by filling structured fields. The app generates a clean preview, a copyable Markdown export, and a printable version.

Everything is local only. No backend, no accounts, no network calls, no analytics.

## Layout

Use a responsive two-pane layout:

- Desktop: left pane is the editor, right pane is the preview.
- Mobile: editor on top, preview below, with a sticky segmented control to jump between "Edit", "Preview", and "Export".
- Header bar: app name on the left, document status in the center, and icon-style buttons on the right for new, save, duplicate, export, and print.

The layout should be dense but calm. Avoid decorative cards inside cards. Use full-width panels separated by borders and clear spacing.

## Data model

Maintain the runbook in React state with this shape:

- title
- owner
- version
- lastReviewed
- purpose
- trigger
- prerequisites
- roles
- steps
- checks
- rollback
- escalation
- notes

Use arrays for prerequisites, roles, steps, checks, and escalation contacts. Each array item must have a stable local id. If crypto.randomUUID is not available, fall back to Date.now plus Math.random.

## Editor pane

The editor is a vertical form with clear sections.

Section 1: Document basics
- Title input, required
- Owner input
- Version input, default "1.0"
- Last reviewed date input, default to today's local date

Section 2: Scope
- Purpose textarea
- Trigger textarea

Section 3: Prerequisites
- Repeating list with text input and remove button
- Add prerequisite button
- Empty state text: "No prerequisites yet"

Section 4: Roles
- Repeating list with role name and responsibility inputs
- Add role button

Section 5: Procedure steps
- Repeating list of steps
- Each step has: title, instruction textarea, expected result, risk level select (Low, Medium, High)
- Buttons to move step up, move step down, duplicate step, and remove step
- Step numbers update automatically

Section 6: Validation checks
- Repeating list with checkbox label and pass/fail hint

Section 7: Rollback and escalation
- Rollback textarea
- Escalation contacts list with name, channel, and when-to-contact fields

Section 8: Notes
- Freeform textarea

Inputs should be comfortable for repeated editing. Use labels above fields, small helper text only where it prevents confusion, and compact section headers. Do not include instructional marketing copy.

## Preview pane

Render a professional runbook document preview that updates live:

- Title line with version and owner metadata
- Purpose and trigger blocks
- Prerequisites as a checklist
- Roles as a two-column table
- Procedure as numbered steps
- Risk level chip per step: Low, Medium, or High
- Expected result shown under each step in smaller text
- Validation checks as checkboxes
- Rollback block
- Escalation table
- Notes block only when notes are present

If a section is empty, show a subtle "Not specified" placeholder in the preview instead of leaving a blank hole.

## Export pane

Provide a Markdown export generated from the current runbook. It should include:

- H1 title
- Metadata table
- H2 sections for purpose, trigger, prerequisites, roles, procedure, validation checks, rollback, escalation, and notes
- Procedure steps as numbered Markdown items
- Risk level text for each step

Include a "Copy Markdown" button. When clicked:

- Use navigator.clipboard.writeText if available
- Fallback to selecting a hidden textarea and document.execCommand("copy")
- Show a small "Copied" status for 1.2 seconds

Include a "Download .md" button that creates a Blob and downloads a file named from the app title in kebab-case. Example: `database-failover-runbook.md`.

## Local persistence

Persist the current runbook to localStorage under `runbook-desk.current.v1` with a 300ms debounce.

On first load, if no saved data exists, seed this example:

Title: "API Incident Triage"
Owner: "Platform Team"
Version: "1.0"
Purpose: "Restore customer-facing API availability while preserving evidence for a later postmortem."
Trigger: "Alert fires for elevated 5xx errors, p95 latency over threshold, or failed health checks across two regions."
Prerequisites:
- Access to production dashboard
- Incident channel created
- Current deploy SHA known
Roles:
- Incident lead: coordinates decisions and timeline
- Operator: runs checks and remediation steps
- Scribe: records timestamps and customer impact
Steps:
1. Confirm the alert is real by checking traffic, error rate, and synthetic probes. Expected result: alert is reproducible outside a single monitor. Risk: Low.
2. Freeze deploys and announce the incident in the incident channel. Expected result: no new changes are released during triage. Risk: Low.
3. Compare current deploy SHA against the last known healthy deploy. Expected result: suspect change is identified or ruled out. Risk: Medium.
4. If rollback is indicated, roll back one version and watch error rate for five minutes. Expected result: error rate decreases and latency stabilizes. Risk: High.
Checks:
- Error rate below threshold for 15 minutes
- p95 latency returned to normal range
- Support notified with customer-facing status
Rollback: "If rollback fails, revert traffic to the secondary region and page database on-call."
Escalation:
- Engineering manager, Slack, after 15 minutes without clear owner
- Database on-call, PagerDuty, if database saturation or lock contention is observed

If localStorage is unavailable, keep state in memory and show a small warning in the header: "Local save unavailable".

## Interactions

- New: confirm before clearing current data, then reset to an empty runbook with today's date.
- Save: manually writes to localStorage and flashes "Saved".
- Duplicate: copies the current runbook into a new draft with title suffixed "copy" and version reset to "1.0".
- Print: calls window.print. Print mode should hide the editor and export controls, rendering only the preview as a clean document.
- Keyboard: Ctrl+S or Cmd+S triggers save and prevents browser save dialog.
- Reordering steps should preserve focus when possible.

## Validation and status

Show a compact status pill:

- "Ready" when title, purpose, and at least one step are filled.
- "Needs title" if title is empty.
- "Needs purpose" if purpose is empty.
- "Needs steps" if no procedure steps exist.

This status is informational only; never block editing or export.

## Visual design

Use Tailwind CSS and TypeScript.

Style direction:

- Background: #F5F7FA
- Editor surface: #FFFFFF
- Preview paper: #FFFFFF
- Borders: #D7DEE8
- Primary text: #172033
- Secondary text: #5F6B7A
- Accent: #2563EB
- Success: #16835D
- Warning: #B7791F
- Danger: #C2410C

Use a restrained system font stack. Use tabular numbers for version/date metadata. Buttons should be compact with clear icons drawn as inline SVG. Do not import icon libraries. Use 8px radius or less on panels and controls.

Risk chips:

- Low: green text on pale green
- Medium: amber text on pale amber
- High: red text on pale red

The app must avoid overlapping text on small screens. Use responsive grids and wrapping labels. No font size should scale directly with viewport width.

## Print styles

Add print CSS:

- Hide header buttons, editor pane, export pane, and any mobile tabs
- Show only the preview document
- Use white background
- Remove shadows
- Set page margin to 0.5in
- Preserve table borders and risk chip colors

## Files the generated app should include

- package.json
- index.html
- src/main.tsx
- src/App.tsx
- src/components/Header.tsx
- src/components/EditorPane.tsx
- src/components/PreviewPane.tsx
- src/components/ExportPane.tsx
- src/components/RepeatingList.tsx
- src/lib/runbook.ts
- src/lib/storage.ts
- src/lib/markdown.ts
- src/lib/download.ts
- src/styles.css
- tailwind.config.ts
- README.md

Keep the implementation dependency-light:

- React 18
- TypeScript
- Vite
- Tailwind CSS
- No external UI libraries
- No icon libraries
- No date libraries
- No backend

## Acceptance criteria

The app is successful if a user can:

1. Open it and immediately see a complete example runbook.
2. Add, duplicate, remove, and reorder procedure steps.
3. Refresh the page and see their current draft restored.
4. Copy Markdown and download a `.md` file.
5. Print a clean runbook without editor controls.
6. Use it comfortably on a phone without text overlap.

If any of these flows requires manual code edits after generation, the template has failed.
