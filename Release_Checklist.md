=== FILE: release_checklist.md ===
OWNER: GitDigital Products
AUTHOR: RickCreator87

Purpose

Provides a mandatory checklist that must be completed before any EGTS release.

Definitions

· Release: A tagged version of the repository intended for production use.

Roles & Responsibilities

· GitDigital Products – Approves final release.
· RickCreator87 – Verifies each checklist item.

Requirements

· All items must be marked [x] before a release tag is created.
· A release cannot be rolled back without a new checklist.

Pre‑Release Checklist

· All governance files generated and validated
· No placeholder content or TODOs
· OWNER and AUTHOR metadata present in every file
· CODEOWNERS, OWNERS, MAINTAINERS defined
· Audit status is pass for governance
· Quality assurance metrics all green
· Completeness score >= 95%
· Release notes drafted
· Sign‑off from GitDigital Products
· Sign‑off from RickCreator87

Governance Alignment

The checklist enforces the release policy defined in GOVERNANCE.md. Skipping any item invalidates the release.
