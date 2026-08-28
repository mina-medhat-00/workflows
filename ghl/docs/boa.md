# BOA Drip Campaign — Optimized for Conversion (Jul 27, 2026\)

Covers the three live Board of Advisors workflows: **Contract Sent, Missed Meeting, Signed & Nurture.** Based on the Jul 27 HAR captures. Companion to `boa-workflow-map-v2.html`.

---

## Diagnostics: what the stats are telling you

1. **BOAs reply. Nothing else is possible, so that's all they do.** Contract Sent delivered 68 emails in 30 days and got 12 replies and 0 clicks, because no touch contains the agreement link. That reply rate (7 on the first email alone) is the best engagement in the org. Give these people a link and the flow converts without a phone call.  
2. **A 40-advisor no-show cohort just went through Missed Meeting** and the Day-0 email, which carries most of the volume, has no calendar link. By the time the linked follow-ups arrive, opens have decayed from 23 to 6 to 1\. Ten advisors are parked mid-flow right now.  
3. **Signed & Nurture is a 12-day flow for a multi-year relationship.** Welcome, one update, one text, then permanent silence. The update email itself says advisor introductions are "one of our strongest channels," and the automation stops asking after Day 12\. Meanwhile the welcome asks for a biweekly meeting with no way to book one, and no later step mentions it again.  
4. **Everything runs on manual tags.** `boa-contract-sent` starts the chase, `boa-contract-signed` both exits it and starts the nurture. No stage or signing event applies either one. One forgotten tag means a signed advisor keeps getting reminders and never gets welcomed, at the same time.  
5. **Comp language needs a counsel pass.** "You get compensated every time one signs on" (Missed Meeting Email 3\) and "equity stake plus commission on anything that closes" (Nurture Email 2\) are the volume-tied class of language the partner cleanup purged. The BOA agreement may legitimately frame advisor compensation this way; the emails should quote that framing, not improvise it.  
6. **Copy hygiene:** most Contract Sent emails have no first name, open with "Hey, Adam from Tile Healthcare here," and sign off as "Tile Healthcare." No from-name or from-email on any of the 9 emails. "alot" in the nurture update.

## Structural changes (do these in the builder)

1. **Add `{{document.url}}` to all five Contract Sent touches** (four emails \+ SMS) and turn on click tracking. The tag-guard exit logic here is already the best in the org; keep it.  
2. **Loop the nurture:** after the Day-12 SMS, add Wait 30 days → Go To a monthly ask email (pattern below). One email a month, forever, until the advisor is removed.  
3. **Add the cal.com board-of-advisor link to Missed Meeting's Day-0 email and SMS.**  
4. **Automate the tags:** trigger Contract Sent from the "Contract Sent" stage (keep the tag as backup), and apply `boa-contract-signed` from the "Signed" stage or the document-signed event. One missed manual tag currently causes a double failure.  
5. **Set from-name/from-email (Adam) on all 9 emails**, standardize `{{contact.first_name}}` greetings and a human sign-off.  
6. **Build the demo confirmation/reminder mini-flow** on Demo Booked (same 3-step shape as the clinic recommendation). Forty no-shows in 30 days is the cost of not having it.  
7. **Cadences stay.** Day 0/2/5/7/9 (contract), 0/+30m/2/5 (missed), 0/7/12/monthly (nurture) all read fine.

---

## Rewritten copy

Merge fields are standard GHL. \[agreement link\] \= `{{document.url}}`. \[booking link\] \= the cal.com board-of-advisor link already live in Missed Meeting.

### Contract Sent

**Email 1 — Day 0\. Subject:** "{{contact.first\_name}}, your Tile Health agreement (quick sign)" · **Preview:** Takes about 3 minutes. Link inside.

> Hi {{contact.first\_name}},  
>   
> Your advisor agreement is ready. Signing takes about three minutes: \[agreement link\]  
>   
> Once it's in, I'll send over the welcome details and we'll get your first introductions moving.  
>   
> Anything in there you want to talk through first, just reply. I read these personally.  
>   
> Adam Tile Health

**Email 2 — Day 2\. Subject:** "quick check on the agreement" · **Preview:** Any questions, concerns, or conflicts, I'd rather hear them.

> Hi {{contact.first\_name}},  
>   
> Were you able to take a look at the agreement yet? Here it is again so you don't have to dig: \[agreement link\]  
>   
> If anything raises a question or a conflict I should know about, reply and tell me. That's normal and I'd rather sort it out than keep reminding you.  
>   
> Adam

**SMS — Day 5:**

> Hi {{contact.first\_name}}, it's Adam at Tile Health. Your advisor agreement is still waiting, takes about 3 minutes: \[short link\]. Any questions, just text back.

**Email 3 — Day 7\. Subject:** "One thing I want to flag before you sign" (keep) · **Preview:** Some sections read denser than they are. 10 minutes clears it up.

> Hi {{contact.first\_name}},  
>   
> One heads-up before you go through the agreement on your own: a couple of sections read denser than they actually are in practice.  
>   
> Happy to walk through any of it on a quick call: \[booking link\]. And the agreement is here whenever you're ready: \[agreement link\]  
>   
> Adam

**Email 4 — Day 9\. Subject:** "{{contact.first\_name}}, should we close this out?" (keep, add the link)

> Hi {{contact.first\_name}},  
>   
> I'm going to assume the timing isn't right and close out the open agreement on my end. I don't want to leave it hanging in limbo.  
>   
> If that's wrong, two ways to fix it: sign here (\[agreement link\]) or reply and tell me what's in the way.  
>   
> Either way, no hard feelings.  
>   
> Adam

### Missed Meeting

**Email 1 — Day 0:** keep the current copy, add before the sign-off:

> Or grab a slot directly so we skip the back-and-forth: \[booking link\]

**SMS — \+30 min:**

> Hey {{contact.first\_name}}, looks like we missed each other. No worries. Rebook in 10 seconds here: \[short link\]

**Email 3 — Day 5:** replace the compensation paragraph with agreement-aligned framing (adjust to whatever the BOA agreement says):

> Just a reminder of what's on the table: an advisor role with equity, compensated per the advisor agreement, in exchange for warm introductions to primary care practices. No heavy lifting on your end, and the practices you introduce pick up a revenue stream they're currently leaving on the table.

Everything else in this flow stays.

### Signed & Nurture

**Email 1 — Day 0\. Subject:** "Welcome aboard, {{contact.first\_name}}" · **Preview:** One click to set our first check-in.

> Hi {{contact.first\_name}},  
>   
> Welcome to Tile Health. Really glad to have you with us.  
>   
> Two quick things to get going:  
>   
> First, grab a slot for our first 15-minute check-in here: \[booking link\]. We'll do these biweekly to start, and I'll keep them short.  
>   
> Second, I'll send you the one-pager and talking points this week so making an introduction never takes more than a forward.  
>   
> Looking forward to it, Adam

**Email 2 — Day 7\. Subject:** "Quick update from Tile Health" (fix "alot", align comp language):

> Hi {{contact.first\_name}},  
>   
> Quick update: we're seeing a lot of interest from RCMs, clinics, and ACOs right now, and advisor introductions continue to be our strongest channel.  
>   
> If anyone comes to mind this month, just send me their name and I'll take it from there. Your role and compensation work exactly as laid out in your advisor agreement.  
>   
> Need a fresh one-pager or talking points? Reply and they're yours.  
>   
> Best, Adam

**SMS — Day 12:** keep as is. It's good.

**NEW: Monthly loop (Wait 30 days → Go To this email → repeat).** Alternate months between these two, or just use A:

**A — the ask:**

> **Subject:** {{contact.first\_name}}, anyone come to mind this month?  
>   
> Hi {{contact.first\_name}},  
>   
> Monthly nudge, as promised when you signed on. Any primary care practices in your network worth an introduction? One name in a reply is all it takes, I run everything from there.  
>   
> If it's a quiet month, no reply needed. I'll check in again next month.  
>   
> Adam

**B — the proof (use when there's real news):**

> **Subject:** What your last intro turned into  
>   
> Hi {{contact.first\_name}},  
>   
> A quick one this month: \[one concrete result, e.g. a clinic that went live, a milestone, a new state\]. That's the kind of thing your introductions turn into.  
>   
> Anyone else come to mind? A name in a reply is all it takes.  
>   
> Adam

*(Both depend on stop-on-reply behavior: a reply should route to Adam and pause the loop for the month, not kill the workflow. If stop-on-reply would remove them permanently, handle replies with the notification \+ manual re-add, or turn stop-on-reply off here like the clinic Monthly Check-in recommendation.)*

---

## Sequence flow (after changes)

\[Demo booked\] ──► (new) confirmation ──► −24h reminder ──► −1h SMS

                                                             │ no-show/cancel (boa-\* tags)

                                                             ▼

                                              Missed Meeting (link at Day 0 now)

                                                             │ books ──► \[EXIT\]

\[Contract Sent stage / tag\] ──► Contract Sent (link in every touch, tag guards intact)

                                                             │ signs ──► boa-contract-signed (auto from Signed stage)

                                                             ▼

                                              Signed & Nurture ──► welcome (booking link) ──► Day 7 update ──► Day 12 SMS

                                                             ▼

                                              Monthly loop: ask / proof, forever

## Benchmarks and what to watch

This audience opens at 80%+ and replies at 10–25% per email, which is exceptional. Targets after the fixes: agreement-link clicks 15–25% (from zero), signature within 9 days for half the cohort, no-show rebook rate 20–30% once the Day-0 link ships, and at least one introduction per active advisor per quarter from the monthly loop. Watch: clicks vs replies per email (BOAs may keep preferring reply, which is fine), how many contacts exit via the signed tag vs age out, and whether the 10 parked no-shows convert once the links move up.

## A/B tests worth running

1. **Contract Sent Email 1 subject:** "{{contact.first\_name}}, your Tile Health agreement (quick sign)" vs. the current "Contract \- Tile Healthcare". Measure link clicks \+ replies combined.  
2. **Monthly loop:** ask-only (A) vs. alternating ask/proof (A/B). Measure introductions per advisor per quarter.  
3. **Missed Meeting Day-0:** link-in-email vs. link-in-SMS-only. Measure rebooks within 48h.

## Placeholders to fill

\[agreement link\] \= `{{document.url}}` merge · \[short link\] for SMS · \[booking link\] \= cal.com board-of-advisor · comp-language wording to match the counsel-approved BOA agreement · confirm what applies `boa-contract-signed` once the Signed-stage automation exists.  
