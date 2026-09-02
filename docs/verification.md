# Verification boundary

## What was checked

The private implementation was structurally validated and its deterministic test
suites were rerun on 2026-09-02. The public README records the fresh pass counts.

The suites cover shared-number matching, booking hours, date and text handling,
rebooking conflicts, explicit confirmation, post-booking status, simplified workflow
branches, and deterministic canvas layout.

## What this does not prove

- This repository has no executable workflow.
- It does not prove a current production deployment or live provider connection.
- It does not publish the original conversation transcripts used during development.
- It does not claim clinical correctness beyond the documented non-diagnostic guard.
- It does not claim a conversion rate, cost reduction, response time, or uptime.

The status is therefore `CASE_STUDY_ONLY` with supporting offline verification, not
open source and not a live demo.
