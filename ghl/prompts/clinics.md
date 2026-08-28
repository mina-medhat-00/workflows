# Clinics Funnel

https://cal.com/team/tile-health/board-of-advisors-demo
https://cal.com/team/tile-health/board-of-advisor

## 1. Contract Sent

1. Fix the grammar bug in Email 2 (Day 6): "Sections sometimes reads a little dense".
2. Swap `{{business.name}}` with `{{contact.company_name}}` in SMS 2 (Day 10). It currently reads "the APCM math for Tile Health" instead of the clinic's name.
3. Add the signing link (`{{opportunity.contract_link}}`) to Email 1 (Day 3) after "feel free to reach out".
4. Add the signing link (`{{opportunity.contract_link}}`) to SMS 1 (Day 4).
5. Add both a 10-minute booking link and the signing link to Email 2 (Day 6), which offers a walkthrough call with no way to book one.
6. Add the signing link to Email 3 (`{{opportunity.contract_link}}`) (Day 10), the scarcity close.
7. Add the booking link to SMS 2 (Day 10).
8. Add a second stage guard before the Day-10 block (Email 3 + SMS 2), duplicating the Day-3 guard. The existing guard runs once, so a deal that signs on Day 4 still receives the Day-10 scarcity email and SMS.
9. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all three emails. 2 of the last 5 SMS in this workflow went undelivered - check A2P registration too.

## 2. Missed Meetings

1. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.
2. Replace the literal text `[Link]` with the real booking link in Email 2 (Day 2). Recipients currently see the raw characters "[Link]" with nothing to click.
3. Replace the second literal `[Link]` in Email 3 (Day 5), at the end of the $300-400K APCM value pitch.
4. Add the booking link to Email 1 (Day 0), which is reply-only today ("just reply and I'll send a new invite over"), so the highest-attention touch carries the ask.
5. Add the short booking link to the +30 min SMS, also reply-only today.
6. Turn stop-on-reply ON. It is OFF, so a prospect who replies keeps getting chased by the rest of the sequence - and since re-entry is now ON, repeat no-shows compound it.
7. Add an Appointment Status trigger (No-show / Cancelled) if the demo calendar is connected in GHL, so the tags apply automatically instead of by hand. Keep both manual tags as the backup path.

## 3. Monthly Check-in

1. Automate the `status-live` tag. Build a small workflow on the "Live" stage that applies it, which turns this loop on automatically for every future clinic. A builder sticky note documents that the tag is applied by hand - documented-manual still means it has never happened.
2. Fix the placeholder email before the first clinic is ever tagged. `[Month]` and three `$[X]` figures are literal text; GHL has no billing data to merge, so the first live clinic would receive an email full of brackets. Replace the Send Email action with an Add Task action for Adam ("send {{contact.company_name}} their APCM summary"). A founder-written email with real numbers beats an automated one with brackets.
3. Turn stop-on-reply OFF for this workflow only. It is ON, so one reply ends the recurring loop forever. Use a reply notification to Adam instead.
4. Update or remove the builder sticky note once the tagging is automated.
5. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.

## 4. Post Signing

1. Move Email 3 (Day 7, "{{contact.company_name}} is live") out of this workflow. Put it in a new workflow triggered by the "Live" stage. The email itself is good; keep the copy unchanged.
2. Fix Email 2 (Day 3): it promises "I've attached a guide" and no attachment is configured on the step. Either attach the real patient-data guide or swap the sentence for a hosted guide link. This is the ask that gates the entire onboarding.
3. Replace `{{contact.name}}` with `{{contact.first_name}}` in the Day-1 SMS.
4. Reword the Day-1 task copy ("Call - Reference Touch 1 Email"), which still references "uncollected APCM revenue" - a sales prompt left in an onboarding flow. Cosmetic, internal-only, low priority.
5. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.

## 5. Cross-workflow / account-level fixes

1. Set From Name / From Email on all 10 emails. Every email rides on account fallback settings today; there is already 1 hard bounce and 2 undelivered SMS on tiny volume.
2. Create custom values for every link (clinic booking link, SMS short link, patient-data guide URL) so no email step ever carries a literal placeholder again.
3. Turn on click tracking and insert links as real hyperlinks, so "clicked but didn't book or sign" becomes a callable list.
4. Verify A2P 10DLC registration before scaling SMS: 2 of 5 recent Contract Sent texts undelivered, 1 failure in Missed Meetings.
5. Cover the silent stages. Four of the ten Clinics Pipeline stages trigger nothing:
   - Demo Booked - build a demo confirmation + reminder flow (confirmation at booking -> −24h email -> −1h SMS). It attacks no-shows at the source, which is cheaper than chasing them, and it feeds the tags Missed Meetings now depends on.
   - Demo Attended - nothing between the demo and a verbal yes.
   - Ready to Sign - nothing between verbal yes and contract out; add a single 2-day nudge gated on the deal still sitting in that stage.
   - Panel Review Scheduled - no reminder, no prep email.
