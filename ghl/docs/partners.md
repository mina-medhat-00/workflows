# Partner Drip Campaign — Optimized for Conversion (Jul 27, 2026\)

Covers the four live partner workflows: **Missed Meeting, Contract Sent, Booking Rejected, Partner Referral** (RCM / EHR / consultants funnel). Based on the Jul 27 HAR captures vs. the Jul 16 baseline. Companion to `partners-workflow-map-v2.html`.

---

## What the rebuild already fixed (credit where due)

The Jul 17–21 rebuild solved the two failures that made this funnel invisible: triggers and SMS. The broken Contact Tag trigger (2,650 attempts, 0 matches) and the one-off webhook are gone, replaced by tag \+ pipeline-stage triggers that actually fire. SMS went from 0% delivered to 21 of 21\. Contract Sent put 12 emails and 2 signing-link clicks on the board in 30 days with 4 partners in flight, and stop-on-reply is now ON in three of the four flows. The optimized Partner Referral copy shipped for Emails 1 and 2, word for word, with the 50+ Medicare threshold filled in.

## Diagnostics: what still leaks

1. **Contract Sent fires reliably and sends the wrong thing.** All the old corporate copy survived, including the "Revenue Share Terms" FAQ that (a) contradicts the flat-monthly-fee model, (b) resurrects the AKS-risk language the compliance cleanup purged, and (c) answers every question with "it's in the agreement." 2 clicks and 0 replies from 12 delivered emails says partners open, find nothing to act on, and leave. The full replacement copy already exists in `contract_signature_followup_optimized.md`.  
2. **Booking Rejected ends on an email with subject "test."** It's published, it's last in the sequence, and its body promises "you get a referral fee" (volume-tied payment language). It has never fired because the triggers are 6 days old. Fix it before the first decline hits the tag.  
3. **One tag, two workflows.** `partnership-intro-canceled` triggers both Missed Meeting and Booking Rejected. A canceled intro currently gets a "missed you on the call" chase and a "was it the offer?" chase at the same time, from the same sender.  
4. **Signature still doesn't stop the chase.** The signing link lives only in Email 1, and the `agreement-signed` check is a dead-end branch at the bottom that takes no action. A partner who signs on Day 1 receives all six touches anyway.  
5. **Partner Referral goes corporate at Day 7\.** The upgraded front half hands off to the old back half: a "please submit referrals" SMS with no way to submit, a Day-14 "update" containing no update, and a Day-21 branch that does nothing on either path.  
6. **Missed Meeting Email 3 reintroduces referral-fee language.** "We pay referral fees for every clinic that signs on" is exactly the sentence the compliance notes said never to write.  
7. **The EHR/consultants pipeline is unwired.** Every stage trigger points at the RCM pipeline; EHR and consultant deals only enter these flows if someone remembers a tag.

## Structural changes (do these in the builder)

1. **Remove `partnership-intro-canceled` from Booking Rejected's triggers.** One event, one workflow: cancellations belong to Missed Meeting, declines to Booking Rejected.  
2. **Add a Goal to Contract Sent:** on `agreement-signed` tag (or the document-signed event), end the workflow. Delete the decorative if/else at the bottom.  
3. **Put the signing link (`{{document.url}}`) in every Contract Sent touch,** both SMS included, and turn click tracking on. "Clicked but didn't sign" is the call list.  
4. **Set from-name and from-email on all 14 emails** and standardize on `{{contact.first_name}}`.  
5. **Wire the EHR/consultants pipeline:** duplicate the Contract Sent and Partner Referral stage triggers for that pipeline's equivalent stages.  
6. **Wire Partner Referral's Day-21 branch:** engaged (opened/clicked) → the 15-minute list-review ask; unengaged → monthly cadence. Both currently dead-end.  
7. **Keep cadences as they are.** Timing was never the problem in this funnel either.

---

## Rewritten copy

Only what's still broken; everything already shipped stays. No em dashes, first-name merge fields, one ask per message. Booking links: use the cal.com partnership-call link already live in Missed Meeting.

### Contract Sent

Use the rewrite in `contract_signature_followup_optimized.md` as written (Emails 1–4 \+ both SMS). Two updates to it:

**The FAQ email (Day 4), final version** (flat fee, real answers; replaces "Revenue Share Terms"):

> **Subject:** The 4 questions partners ask before signing  
>   
> Hi {{contact.first\_name}},  
>   
> Most partners have the same handful of questions before signing, so here are straight answers.  
>   
> How does compensation work? It's a flat monthly marketing fee of $\[X\], the same amount every month regardless of referral volume. Simple, predictable, and it keeps the arrangement clean from a compliance standpoint.  
>   
> When are payments made? \[Actual timing.\]  
>   
> What are we committing to? \[Term length, what a referral means operationally (one forwarded email), no quota.\]  
>   
> Can we exit? \[Actual termination terms.\]  
>   
> The full detail is in the agreement itself, here so you don't have to hunt for it: [Review and Sign Agreement](http://{{document.url}})  
>   
> Anything I didn't cover, reply and ask.  
>   
> {{user.full\_name}}

**Both SMS get the link:**

> Hi {{contact.first\_name}}, it's {{user.name}} from Tile Health. Your partnership agreement is ready, takes about 3 minutes: \[short link\]. Reply here if any part needs a look first.

### Booking Rejected

**Email 1 (Day 0\)** keep the diagnosis angle, tidy it up:

> **Subject:** no worries, quick question  
>   
> Hi {{contact.first\_name}},  
>   
> No problem at all. One quick question so I don't guess wrong: was it the timing, or did something about the offer not land right?  
>   
> If it's timing, I'll check back in a few weeks and leave you alone until then. If something didn't make sense, it usually takes two minutes to clear up and might change the picture.  
>   
> Either way, a one-line reply helps.  
>   
> {{user.full\_name}}

**Email 2 (Day 3\)** is strong; add one line before the sign-off:

> Happy to show you what it looks like on a 10-minute call: \[booking link\]

**Email 3 (Day 5\)** fix the dangling merge field:

> Hi {{contact.first\_name}},  
>   
> I still think this could be a fit for the practices you work with, so one more note from me.  
>   
> Worth a second look? Happy to do a faster version, just the parts relevant to your clients, 10 minutes max: \[booking link\]  
>   
> {{user.full\_name}}

**Email 4 (Day 7\)** replace subject "test" and the referral-fee line:

> **Subject:** last one from me, {{contact.first\_name}}  
>   
> Hi {{contact.first\_name}},  
>   
> Wanted to come at this from a different angle before I close the file.  
>   
> For a lot of consultants, the win here is what it does for their client relationships: their practices pick up recurring Medicare revenue they weren't billing, and we do all the operational work. You're the one who found it for them.  
>   
> If that's not relevant to what you're doing right now, totally fine, reply "not now" and I'll stop following up. If it is, happy to send over the details or walk through it in 10 minutes: \[booking link\]  
>   
> {{user.full\_name}}

### Missed Meeting

One sentence to fix, in Email 3\. Replace:

> ~~"We pay referral fees for every clinic that signs on, and there's no ongoing work on your end."~~

with:

> "Partners who introduce us to their clinics get a monthly update on every practice they've connected, and there's no ongoing work on your end."

Everything else in this flow is in good shape; it's the healthiest workflow in the funnel.

### Partner Referral (back half)

**SMS (Day 7\)**, replaces "please submit any referrals":

> Hi {{contact.first\_name}}, it's {{user.name}} at Tile Health. As you're going through your client list this week, any primary care clinic with a decent Medicare panel jump out? Reply with the name and I'll run the intro.

**Email 3 (Day 14\)**, replaces the empty "Partner Program Update" (this is "The math on one referral" from the earlier doc, ready to paste):

> **Subject:** The math on one referral  
>   
> Hi {{contact.first\_name}},  
>   
> Here's what one forwarded email turns into.  
>   
> A clinic with 500 traditional Medicare patients that enrolls 40% into APCM nets roughly $6,900 a month in recurring revenue, about $83K a year, with zero staff added on their side since we run the program. (2026 CMS national rates; happy to run the math for a specific clinic if you name one.)  
>   
> On your side: that clinic now associates {{contact.company\_name}} with found money, and you get a monthly report from us on everyone you've referred.  
>   
> If a name surfaced while reading this, reply with it. Still just one email.  
>   
> {{user.full\_name}}

**Day 21 branch:**

Engaged (opened or clicked anything):

> **Subject:** 15 minutes to go through your client list together?  
>   
> Hi {{contact.first\_name}},  
>   
> Rather than waiting for names to surface on their own, want to grab 15 minutes and flag 3 to 5 fits from your client list together? I'll bring the filter, you bring the list: \[booking link\]  
>   
> {{user.full\_name}}

Unengaged: exit to a monthly cadence (one email per month, a clinic result or APCM reimbursement note, same one-line ask at the end). Stop the drip.

---

## Sequence flow (after changes)

Intro/demo/contract no-show or cancel (tags) ──► Missed Meeting ──► rebook via cal.com ──► \[EXIT on booking/reply\]

Meeting declined (tag, exclusive) ──► Booking Rejected ──► diagnose → CMS proof → second look → last note

RCM stage: Contract Sent (+ tag) ──► Contract Sent ──► link in every touch ──► \[GOAL: agreement-signed → EXIT\]

RCM stage: Waiting for Referrals (+ tag) ──► Partner Referral ──► process → filter → math → Day-21 split

                                                                       engaged ──► list-review call

                                                                       quiet ────► monthly cadence

## Benchmarks and what to watch

Partner audiences run cooler than clinic prospects: opens 40–60%, clicks 5–10% on signing/booking links, signature within 14 days as the north star for Contract Sent, first referral within 30 days for Partner Referral. The current 12-delivered / 2-clicked / 0-replied pattern in Contract Sent is the signature of copy with no ask; expect replies to appear within one cohort of shipping the rewrite. Review weekly: signing-link clicks, agreement-signed exits (should be \> 0 once the Goal exists), referral replies, and whether `partnership-intro-canceled` contacts appear in exactly one workflow.

## A/B tests worth running

1. **Contract Sent Email 1 subject:** "Your Agreement Link is Ready" vs. "{{contact.first\_name}}, your Tile Health agreement (3 minutes to sign)". Measure signing-link clicks.  
2. **Booking Rejected Email 1:** question-first ("timing or fit?") vs. value-first (CMS rule change as the opener). Measure replies.  
3. **Partner Referral Day-14:** the $83K math email vs. a named-clinic case study. Measure referral replies.

## Placeholders to fill

$\[X\] flat monthly fee \+ payment timing/term/termination in the FAQ email · \[short link\] for SMS · \[booking link\] \= cal.com partnership-call · confirm the `agreement-signed` tag is actually applied on signature (the Goal depends on it).  
