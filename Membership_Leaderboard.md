=== FILE: membership_Leaderboard.md ===
# Membership Leaderboard

## Purpose
This document defines the “Membership Leaderboard” — a gamified recognition system that ranks members based on cumulative contribution (financial and engagement) within the membership program.

## Definitions
- **Leaderboard**: A public ranking of members sorted by “Membership Points”.
- **Membership Point**: Earned by paying $1 = 1 point, attending a member event = 50 points, referring a new member = 100 points.
- **Leaderboard Reset**: Points accumulate lifetime; leaderboard shows all‑time top 100.

## Tiers (if applicable)
Not applicable; leaderboard ranks individual members irrespective of tier, but tier badges are shown next to names.

## Benefits
- Top 10 members receive a “Leader” badge on their profile.
- #1 member each quarter receives a $200 credit toward any ApexOS merchandise or conference ticket.
- Members in top 20 are invited to an exclusive “Leaderboard Council” (non‑voting advisory group).

## Requirements
- Only active members (paid within last 30 days) appear on the leaderboard.
- Points from referrals are only awarded if the referred member remains active for 3 months.
- Abuse of point system (e.g., fake referrals) leads to removal from leaderboard and possible membership termination.

## Governance alignment
The leaderboard is maintained by an automated script that pulls data from the membership database and event attendance logs. The Core Team may audit any member’s points upon request. Changes to point valuation require a pull request approved by two Core Team members.

## Branding rules
- The leaderboard is styled as a “Hall of Fame” with ApexOS branding.
- No advertisements or external links are permitted on the leaderboard page.
- Member names appear as chosen (real name or pseudonym), but must be unique.

## Public recognition rules
- Leaderboard is updated daily at 12:00 UTC.
- A “Leader of the Month” social media post features the current #1 member.
- Archived leaderboards are stored monthly in `/public/leaderboards/`.

## Templates (if applicable)
- Leaderboard data schema: `/schemas/leaderboard.json`
- Monthly leaderboard announcement email template: `/templates/leaderboard_email.md`
