
=== FILE: support_leaderboard.md ===

OWNER: GitDigital Products

AUTHOR: RickCreator87

Purpose

Ranks supporters by estimated value of their contributions (financial and in‑kind).

Definitions

· Supporter Leaderboard: A ranked list based on total contributed value.

Roles & Responsibilities

· GitDigital Products – Approves valuation method.
· RickCreator87 – Updates monthly.

Requirements

· In‑kind support is valued at fair market rates.
· Disputes resolved by GitDigital Products.

Support Leaderboard (2026-04-12)

Rank Supporter Total Value Type
1 GitDigital Products $60,000 Mixed

Governance Alignment

Leaderboard is for recognition only; does not affect governance rights.




=== FILE: support_leaderboard.md ===
# Support Leaderboard

## Purpose
This document defines the “Support Leaderboard” — a ranking of individual supporters (not sponsors) based on their total lifetime recurring contributions to ApexOS.

## Definitions
- **Support Leaderboard**: Public list of top 50 supporters by total amount donated (cumulative, not monthly).
- **Lifetime Support**: Sum of all payments made via recurring subscriptions (including canceled ones).
- **Update Frequency**: Every 24 hours via API from payment processors.

## Tiers (if applicable)
No formal tiers, but supporters are grouped by donation brackets: $0–$499, $500–$1999, $2000–$9999, $10000+.

## Benefits
- Supporters in the top 10 receive a “Top Supporter” role on Discord.
- #1 supporter each month receives a hand‑written thank‑you note from the Core Team.
- All supporters on the leaderboard get a special `@TopSupporter` mention in the project’s release notes.

## Requirements
- Only supporters who have opted into public recognition are included.
- Supporters must have made at least one payment in the last 365 days to remain on the active leaderboard (but lifetime totals are preserved in archive).
- Organizations are not eligible for the Support Leaderboard (use sponsorship leaderboard instead).

## Governance alignment
The Support Leaderboard is considered a community goodwill feature. The Core Team may delist any supporter who violates the Code of Conduct. No financial benefit (other than recognition) is tied to leaderboard position.

## Branding rules
- Leaderboard displays “ApexOS Support Leaderboard” heading with the project logo.
- Supporters’ names are plain text; no logos or images allowed.
- The leaderboard may be embedded in the project’s GitHub Sponsors profile.

## Public recognition rules
- A “Weekly Top Supporter” is highlighted in the `#announcements` channel every Monday.
- The leaderboard page includes a “Past Champions” section for supporters who held #1 in any previous month.
- Supporters who achieve 12 consecutive months in top 10 receive a “Loyal Supporter” badge.

## Templates (if applicable)
- Leaderboard API endpoint: `/api/v1/support-leaderboard` (JSON)
- Embeddable widget HTML: `/templates/leaderboard_widget.html`
