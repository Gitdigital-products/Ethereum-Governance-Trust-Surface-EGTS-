=== FILE: docs/governance-dashboard-spec.md ===

Governance Dashboard Specification – EGTS

1. Overview

The EGTS Governance Dashboard provides a real‑time, verifiable view of governance trust surfaces across Ethereum. It aggregates data from the Contributor Registry, Badge Authority, and cross‑chain adapters, presenting health metrics and risk indicators.

2. Functional Requirements

2.1 Data Sources

· Contributor Registry (on‑chain)
· Badge Authority (on‑chain)
· Cross‑chain adapters (verified events)
· Validator set registry
· Integrity proof store

2.2 User Roles

· Viewer – Read‑only access to aggregated metrics.
· Analyst – Can download raw data and proofs.
· Admin – Configures alert thresholds and data source endpoints.

2.3 Core Views

2.3.1 Trust Surface Map

· Geographic / network visualization of governance nodes.
· Color‑coded by health score (green/yellow/red).
· Filters by chain, badge type, contributor scope.

2.3.2 Health Metrics Dashboard

Displays the trust surface health metrics defined in trust-surface-health-metrics.md:

· Badge distribution Gini coefficient.
· Validator participation rate.
· Cross‑chain adapter latency.
· Integrity proof age.

2.3.3 Contributor Explorer

· List of all active contributors with badges.
· Drill‑down to see contribution claims, metadata, and scope.
· Search by public key, ENS name, or metadata keywords.

2.3.4 Integrity Verification Panel

· For any displayed data point, show the corresponding integrity proof.
· Allow user to re‑verify proof locally (download proof + verification key).

3. Non‑Functional Requirements

· Latency – Dashboard renders initial view within 2 seconds.
· Verifiability – Every displayed metric must be traceable to an on‑chain proof.
· Privacy – Metadata URIs may be encrypted; dashboard prompts for decryption key.
· Availability – Hosted on IPFS + fallback to L2 archive node.

4. API Endpoints (REST)

All endpoints return JSON with an embedded proof field (Merkle inclusion or zk‑SNARK).

Endpoint Description
GET /v1/contributors/{id} Contributor details + badge list
GET /v1/metrics/health Current health metrics
GET /v1/proofs/latest Latest integrity proof root
POST /v1/verify Submit a proof for local verification

5. Security & Authentication

· No authentication required for read‑only views.
· Admin endpoints use SIWE (Sign In with Ethereum) and require a badge with DASHBOARD_ADMIN role.
· All API responses are signed by the dashboard backend; clients can verify the signature against a known public key rotated every 30 days.

6. Frontend Implementation

· React / TypeScript frontend.
· Uses ethers.js and lightweight proof verification library (WebAssembly).
· Caches data in IndexedDB with invalidation based on proof root changes.
· Dark mode / high contrast accessibility compliant.

7. Deployment & Updates

Dashboard is deployed as a static site on IPFS with a DNSLink record. Updates are proposed via EGTS governance; after approval, a new IPFS hash is published. Users receive update notifications via wallet popup or RSS feed.
