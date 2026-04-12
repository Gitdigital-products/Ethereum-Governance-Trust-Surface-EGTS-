=== FILE: support_wall.md ===
# Support Wall

## Purpose
This document describes the “Support Wall” — a public webpage and in‑repository file that acknowledges all individual supporters of ApexOS. The wall serves as a permanent thank‑you space.

## Definitions
- **Support Wall**: The canonical list of supporters, updated automatically from payment data every 24 hours.
- **Wall Position**: The order in which supporters appear (by tier then by join date).
- **Wall Archive**: A quarterly snapshot of the Support Wall stored in the repository.

## Tiers (if applicable)
Supporters are grouped by tier: Bronze, Silver, Gold.

## Benefits
- Having one’s name or handle permanently recorded in the project’s history (unless removal is requested).
- Supporters may optionally display a personal message (max 140 characters) next to their name on the wall.
- Top 10 lifetime supporters (by total contributed) receive a gold star icon.

## Requirements
- To appear on the Support Wall, the supporter must have made at least one monthly contribution in the past 90 days (active) or have a lifetime total over $100 (inactive but kept).
- Supporters must opt in to name display; default is GitHub username or “Anonymous”.
- No commercial logos are allowed on the Support Wall (only text names).

## Governance alignment
The Support Wall is maintained by the project’s automation team. Any dispute about removal or misrepresentation is handled by the Code of Conduct committee. The Core Team reserves the right to remove any entry that violates the project’s values.

## Branding rules
- The Support Wall may be referred to as “ApexOS Support Wall” only.
- No advertising or promotional text is permitted within supporter messages.
- The wall’s visual style must follow the project’s design system (colors, fonts).

## Public recognition rules
- The Support Wall is linked from the project’s main README under “Acknowledgments”.
- Each quarter, a static HTML export of the wall is committed to the `website/support-wall/` directory.
- Supporters who request anonymity appear as “A grateful supporter”.

## Templates (if applicable)
- Support Wall HTML template: `/templates/support_wall.html`
- CSV export schema for archive: defined in `SUPPORT_WALL_SCHEMA.md`
