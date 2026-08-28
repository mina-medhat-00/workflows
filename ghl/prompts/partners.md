# Partners Funnel

https://cal.com/team/tile-health/board-of-advisors-demo
https://cal.com/team/tile-health/board-of-advisor

## 1. Booking Rejected

1. Fix Email 4 (Day 7) before anything enters this workflow. Its subject is literally "test", it is published, and it is last in the sequence. The body also promises "you get a referral fee", the volume-tied payment language the compliance cleanup ruled out. Replace both: reframe the win as client-relationship value (their practices pick up recurring Medicare revenue, the partner is the one who found it for them) with a "not now" reply exit and a booking link.
2. Remove `partnership-intro-canceled` from this workflow's triggers, keeping `meeting-declined (third parties)` only. That tag also triggers Missed Meeting, so a canceled intro currently gets a "missed you" chase and a "was it the offer?" chase at the same time, from the same sender. One event, one workflow: cancellations belong to Missed Meeting, declines to Booking Rejected.
3. Fix the dangling `{{business.name}}` in Email 3 (Day 5). It sits alone mid-paragraph and renders as "…the practices you work with. Tile Health" with no sentence around it.
4. Add a booking link to Email 2 (Day 3), the CMS rule-change email. It is the strongest email in the flow and has no way to act on it — one line before the sign-off is enough.
5. Tidy Email 1 (Day 0) and add a sign-off; it has none today. Keep the diagnosis angle ("was it the timing, or did something about the offer not land?").
6. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.

## 2. Contract Sent

1. Delete the body of Email 2 (Day 4, "Understanding Our Revenue Share Terms - FAQs") and replace it entirely. It is wrong on two counts and still live: the model is a flat monthly fee, so revenue-share language is an AKS compliance risk, and every answer is circular ("any applicable fees are clearly detailed in our agreement"), it tells a partner you won't tell them things. Replace with a flat-fee FAQ giving real answers to the four questions that block signature: how compensation works, when payments are made, what they're committing to, how to exit. Fill in the actual fee amount, payment timing, term, and termination before publishing.
2. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.
3. Add the signing link (`{{opportunity.contract_link}}`) to every touch that lacks one: SMS 1 (Day 2), Email 3 (Day 7), SMS 2 (Day 10), and Email 4 (Day 14, which already has the founder calendar link). Today the link exists only in Email 1, so every reminder asks the partner to go find the first email.
4. Confirm the `agreement-signed` tag is actually applied on signature in Documents & Contracts next goal depends on it.
5. Convert the decorative If/Else into a real exit. The `agreement-signed` tag check sits at the very bottom of the flow and neither branch does anything, so a partner who signs on Day 1 still receives all six touches. Replace it with a workflow Goal on the `agreement-signed` tag (or the Document Signed event) set to end the workflow.
6. Ship the full rewrite of Emails 1, 3, and 4: one ask per message, signing link (`{{opportunity.contract_link}}`) on every touch, `{{contact.first_name}}` greetings. Email 1 currently opens "Hello {{contact.name}}", which renders as "Hello Memorial Hermann RCM Services".

## 3. Missed Meeting

1. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.
2. Replace the compliance-risk sentence in Email 3 (Day 5): "We pay referral fees for every clinic that signs on, and there's no ongoing work on your end." The model is a flat monthly marketing fee, and the compliance cleanup ruled out tying dollars to signed clinics anywhere in partner copy. Reframe as introductions plus a monthly update on every practice they've connected.
3. Add a booking link to Email 1 (Day 0) and the +30 min SMS, both reply-only today, so the highest-volume touches carry the ask. The cal.com link already lives in Emails 2 and 3.
4. Keep as is: the five-tag trigger coverage (`partnership-intro-no-show`, `partnership-intro-canceled`, `partnership-demo-no-show`, `partnership-demo-canceled`, `partnership-contract-no-show`) that replaced the old single tag + webhook, the real cal.com links in Emails 2 and 3, stop-on-reply ON, re-entry OFF.
5. `partnership-intro-canceled` also triggers Booking Rejected. Cancellations should stay with Missed Meeting.

## 4. Partner Referral

1. Replace the Day-7 SMS ("We appreciate you partnering with us! Please take a moment to submit any referrals you may have.") with the reply-based ask, there is no way to "submit" anything. Ask them to reply with a clinic name while they're going through their client list.
2. Replace Email 3 (Day 14, "Partner Program Update"), which promises an update and then contains none: "we wanted to check in and provide you with a brief update" followed by nothing. Use "The math on one referral" - a clinic with 500 traditional Medicare patients enrolling 40% into APCM nets roughly $6,900/mo, about $83K/yr, at 2026 CMS national rates. Also fixes its `{{contact.name}}` greeting.
3. Wire the Day-21 If/Else block. The call task fires, then a `referral-sent` tag check nested with an opportunity-won check, and every branch dead-ends with no action. Build the engaged/unengaged split from the optimization doc:
   - ENGAGED (opened or clicked anything): send the 15-minute list-review ask: grab 15 minutes and flag 3 to 5 fits from their client list together.
   - UNENGAGED: stop the drip and move to a monthly cadence: one email per month, a clinic result or an APCM reimbursement note, same one-line ask at the end.
4. Set From Name / From Email to Adam (adam@tilehealthcare.com) on all emails.

## 5. Cross-workflow / account-level fixes

1. Scrub referral-fee language everywhere. Missed Meeting Email 3 and Booking Rejected Email 4 both tie payment to signed clinics. Reframe both as introductions plus a flat monthly marketing fee, per the compliance cleanup.
2. Wire the Third Parties (EHR & Consultants) pipeline. All 8 stages are unwired: every stage trigger points at the RCM pipeline only, so EHR and consultant deals enter these flows only if someone remembers a tag. Duplicate the stage triggers for the equivalent EHR stages on Contract Sent and Partner Referral.
3. Set From Name / From Email on all 14 emails (Adam) and standardize on `{{contact.first_name}}` — `{{contact.name}}` and `{{contact.first_name}}` are mixed today, which is what produces "Hello Memorial Hermann RCM Services".
4. Turn on click tracking across the funnel. The 12 delivered / 2 clicks / 0 replies pattern in Contract Sent is the signature of copy with no ask; you need the click data to tell the difference after the rewrite.
5. Cover the silent stages in the RCM pipeline:
   - Demo Booked: no confirmation or reminder; the five no-show/cancel tags that feed Missed Meeting only get applied after the damage. Build a demo confirmation mini-flow, same shape as the clinic recommendation.
   - Demo Attended: nothing between the demo and "Ready to sign".
   - Ready to Sign: nothing between verbal yes and contract out.
   - RCM Kick-off: no welcome or kickoff sequence for a partner who just signed.
