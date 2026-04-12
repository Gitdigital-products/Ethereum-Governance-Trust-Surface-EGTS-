=== FILE: codeowners.md ===
OWNER: GitDigital Products
AUTHOR: RickCreator87

Purpose

This document provides a human-readable explanation of the CODEOWNERS mechanism for the EGTS project, including how ownership is assigned and how to update it.

Definitions

· Code Owner: A person or entity responsible for reviewing and approving changes to specific code or documentation paths.
· Approval Gate: The requirement that at least one code owner must approve a pull request before merging.

Roles & Responsibilities

· GitDigital Products: Final owner of all repository assets; delegates path‑specific ownership as needed.
· RickCreator87: Maintains this document and ensures it reflects the current CODEOWNERS file.
· Contributors: Must check ownership before submitting pull requests.

Requirements

· This file must be updated whenever CODEOWNERS changes.
· All owners listed in CODEOWNERS must have a corresponding entry in AUTHORS.md.
· A change to ownership must be proposed via a pull request and reviewed by GitDigital Products.

Template for adding a new owner

```
Pattern: <path>
Owner: @GitDigitalProducts or @username
Reason: <business or technical justification>
```

Governance Alignment

Adheres to the review and approval rules in GOVERNANCE.md. Violations of ownership rules are treated as process breaches.
