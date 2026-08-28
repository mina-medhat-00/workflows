# BOA Funnel

https://cal.com/team/tile-health/board-of-advisors-demo
https://cal.com/team/tile-health/board-of-advisor

## 1. Contract Sent

1. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.
2. Add the agreement link (`{{opportunity.contract_link}}`) to all five touches (Email 1 (Day 0), Email 2 (Day 2), SMS (Day 5), Email 3 (Day 7), Email 4 (Day 9)). Today the link is in none of them.
3. Turn on click tracking on every email in the flow so the "opened but didn't sign" list becomes measurable.
4. Rewrite Email 1 (Day 0): subject "Contract - Tile Healthcare" to a real subject, add a `{{contact.first_name}}` greeting, drop the "Hey, Adam from Tile Healthcare here" opener, sign off as Adam. This is the best-engagement email and it has no name, no link, and a company sign-off.
5. Rewrite Email 2 (Day 2) with a first-name greeting and the agreement link (`{{opportunity.contract_link}}`).
6. Add a booking link to Email 3 (Day 7, "One thing I want to flag before you sign") — it offers a phone call with no way to book one.
7. Add the agreement link to Email 4 (Day 9, "should we close this out?"). Keep the takeaway-close subject; it works.
8. Add the agreement short link to the Day-5 SMS.
9. Add a Pipeline Stage trigger on the BOA "Contract Sent" stage; keep the manual `boa-contract-sent` tag as the backup path.

## 2. Missed Meeting

1. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.
2. Add the cal.com board-of-advisor link to Email 1 (Day 0) before the sign-off. It carries most of the volume (40 delivered, 23 opened) and is reply-only; the link currently appears only in Emails 2 and 3, after opens have decayed 23 → 6 → 1. _(~10 min)_
3. Add the same link (short version) to the +30 min SMS, which is also reply-only today. Keep the reply path as the alternative in both.
4. Replace the compensation paragraph in Email 3 (Day 5): "equity plus revenue share for warm introductions… you get compensated every time one signs on" is the volume-tied language the partner compliance cleanup purged. Rewrite to whatever framing the counsel-approved BOA agreement actually uses.
5. Re-check the 10 advisors parked in the wait after Email 2 once the Day-0 link ships — do they convert?

## 3. Signed & Nurture

1. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.
2. Make the nurture recurring. After the Day-12 SMS add: Wait 30 days → Send Email → Go To back to the wait step. One short email a month, forever, alternating a traction update with a direct intro ask, each ending in the same one-line ask. This is the funnel's biggest gap: Missed Meeting sends more touches to people who ghosted a demo than this flow sends to people who signed an agreement. _(~30 min)_
3. Add the cal.com booking link to Email 1 (Day 0, welcome). It asks "what time would it be good to set a biweekly 15Mins meeting?" with no calendar link, and no later step ever mentions the meeting again.
4. Rewrite Email 1's greeting/sign-off: `{{contact.first_name}}` greeting, sign off as Adam, not "Tile Healthcare".
5. Fix Email 2 (Day 7): correct "alot" in the first sentence, and replace "equity stake plus commission on anything that closes" with agreement-aligned comp language. Keep the update → ask → offer-materials structure; it's good.
6. Decide reply handling for the new loop: a reply should notify Adam and pause the month, not kill the workflow. If stop-on-reply would remove the advisor permanently, turn it off here and rely on a reply notification instead.

## 4. Cross-workflow / account-level fixes

1. Wire the pipeline stages. All 6 BOA stages currently trigger nothing; enrollment runs entirely on manually applied tags. Trigger Contract Sent from the "Contract Sent" stage, and apply `boa-contract-signed` automatically from the "Signed" stage or the document-signed event. One forgotten tag today causes a double failure: the signer keeps getting chased _and_ never enters the nurture.
2. Set From Name / From Email on all 9 emails (Adam). Every email currently rides on the account default sender.
3. Standardize on `{{contact.first_name}}` greetings and a human sign-off across the funnel — the least personal copy in the org is pointed at the most relationship-driven audience.
4. Align all compensation language with the counsel-approved BOA agreement (Missed Meeting Email 3, Nurture Email 2) and keep per-deal payment language out of automated copy.
5. Build a demo confirmation / reminder mini-flow on the "Demo Booked" stage (confirmation → −24h reminder → −1h SMS). Nothing fires on Demo Booked today, and this funnel just produced a 40-advisor no-show cohort.
6. Cover "Ready to Sign." No automation exists between verbal yes and contract out.
