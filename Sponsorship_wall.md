=== FILE: sponsorship_wall.md ===
# Sponsorship Wall

## Purpose
This document specifies the combined recognition wall that includes both sponsors and major supporters, called the “Sponsorship Wall”. It serves as a unified view of all entities contributing above a certain threshold.

## Definitions
- **Sponsorship Wall**: A single page listing both sponsors (organizational) and high‑tier supporters (individuals contributing ≥ $200/month).
- **Dual Recognition**: Sponsors appear with logos; Gold Supporters appear with names and a “Supporter” badge.
- **Threshold**: Minimum $200 monthly or $2,400 annual to appear.

## Tiers (if applicable)
| Entity Type | Tier on Wall           |
|-------------|------------------------|
| Sponsor     | Bronze, Silver, Gold, Platinum |
| Supporter   | Gold Supporter only    |

## Benefits
- Unified visibility to all visitors.
- Sponsors may request to be listed only on the Sponsorship Wall and not on separate sponsor wall (consolidation).
- Gold Supporters receive a small star icon next to their name.

## Requirements
- To appear on the Sponsorship Wall, entities must have an active contribution at the required threshold and must not have opted out of recognition.
- Sponsors must provide a logo; Gold Supporters provide only a name and optionally a link to their GitHub profile.
- The wall is regenerated weekly.

## Governance alignment
The Sponsorship Wall is considered an official project asset. Any changes to its content or structure require a pull request reviewed by two Core Team members. No entity may pay to modify their position except by moving to a higher tier.

## Branding rules
- The wall uses a masonry grid layout. Sponsors occupy the left column (wider), supporters the right column.
- Sponsorship Wall logo must be the ApexOS logo with the text “Sponsorship Wall”.
- No third‑party branding allowed on the wall page itself.

## Public recognition rules
- The wall includes a “Join them!” button linking to the sponsorship/support signup page.
- Every release blog post includes a screenshot of the top section of the Sponsorship Wall.
- Sponsors and supporters are listed together but visually separated by a “Sponsors” and “Gold Supporters” heading.

## Templates (if applicable)
- Sponsorship Wall React component: `/src/components/SponsorshipWall.jsx`
- Data fetching script: `/scripts/fetch_sponsorship_data.py`
