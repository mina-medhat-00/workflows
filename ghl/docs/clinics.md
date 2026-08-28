# Clinic Drip Campaign — Optimized for Conversion (Jul 27, 2026)

Covers the four live clinic workflows: **Missed Meetings, Contract Sent, Post Signing, Monthly Check-in.**
Based on the Jul 27 HAR captures. Builder state is unchanged since Jul 20; the numbers below are what moved.

---

## Diagnostics: what the stats are telling you

1. **Missed Meetings just ran its first full cohort and converted nobody.** Six contacts entered under the new tag triggers, all six aged out of the 5-day chase: 0 replies, 0 rebooks, plus 1 hard bounce and 1 failed SMS. The chase now *fires* reliably (the trigger rebuild worked), but both follow-up emails still say literal "[Link]" where the calendar link should be, so the only way to rebook is to reply, and nobody did. This is the single biggest conversion leak in the funnel right now.
2. **Contract Sent is your working horse, but it converts by attrition, not by click.** Opens are strong (75–100%) and it generated the funnel's only reply. But there is no signing link or booking link in any touch, so every email asks the prospect to go find a different email. One unsubscribe in a tiny window is also worth respecting: Day 3 → Day 10 is five touches on someone who may have already signed elsewhere in the stack (the stage guard only runs once, at Day 3).
3. **Post Signing has one clinic in it, parked right before the "you're live" email.** That email fires on a blind Day-7 timer. The clinic it's about to hit is still in Waiting for Patient Data, so the automation is about to congratulate them on going live before they've sent a single record. The Day-3 email also still promises an attached guide that isn't attached, which undercuts the exact ask (send us your data) that gates all onboarding revenue.
4. **Monthly Check-in has still never fired.** The `status-live` tag is manual, has never been applied, and the email it would send still contains `[Month]` and `$[X]` placeholders. Live clinics, the people already paying you, get zero automated touches.
5. **SMS deliverability is shaky everywhere:** 1–2 undelivered per window in Contract Sent, 1 failed in Missed Meetings. Check A2P 10DLC registration status and that contacts actually have mobile numbers before leaning harder on SMS.
6. **No from-name / from-email on any of the 10 emails.** The Missed Meetings hard bounce is the first casualty. Set a real human sender on every email before scaling volume.

## Where the funnel leaks (process view)

```
Demo booked ──────────► Demo happens ─────► Ready to sign ───► Contract Sent ───► Signed ───► Onboarding ───► Live
     │                        │                   │                (works)                       │              │
     │ no confirmation        │ no follow-up      │ no nudge                                     │ blind timer  │ manual tag,
     │ no reminder            │ automation        │                                              │ "you're live"│ never applied
     ▼                        ▼                   ▼                                              ▼              ▼
  NO-SHOWS ──► Missed Meetings chase (0 conversions, broken links) ──► dead end          wrong message      silence
```

The theme: **you chase failure downstream instead of preventing it upstream, and every recovery path is missing its link.** Preventing one no-show is worth more than five chase emails; a working booking link is worth more than any subject line.

---

## Structural changes (do these in the builder)

Ordered by conversion impact per minute of work.

1. **Paste the real booking link over both "[Link]" placeholders** in Missed Meetings Emails 2 and 3. Fifteen minutes, and the recovery flow finally has a way to recover. Put the same link in the Day-0 email and the SMS too; every touch should carry the ask.
2. **Turn Missed Meetings stop-on-reply ON and add a reply notification.** A no-show who replies is a live deal; today the sequence keeps nagging them. Add an internal notification (or task for Adam) on any reply, in every workflow.
3. **Set from-name and from-email on all 10 emails** (Ali, or whoever owns the clinic pipeline). Turn click tracking on everywhere; "clicked the booking link but didn't book" is your best call list.
4. **Build the missing 3-step Demo Confirmation flow on Demo Booked** (new, ~1 hour): instant confirmation with calendar link, reminder 24h out, reminder 1h out with a one-tap reschedule link. This attacks no-shows at the source; it's also what feeds Missed Meetings its tags. If your calendar is connected in GHL, trigger the no-show/cancel tags from **Appointment Status** (no-show / cancelled) instead of applying them by hand, and keep the manual tags as a backup path.
5. **Gate the "you're live" email on the Live stage.** Move Post Signing Email 3 into a tiny workflow triggered by stage = Live (that same workflow can auto-apply `status-live`, killing the manual tag problem and turning Monthly Check-in on for every future clinic in one move). Attach the patient-data guide to Email 2, or link to it, so the copy stops promising something the email doesn't contain.
6. **Re-check the stage mid-sequence in Contract Sent.** Duplicate the existing stage guard after Email 2 (Day 6) so a deal that signed or died on Day 4 doesn't get the scarcity email and the last-note SMS. Fix the two copy bugs while you're in there: "Sections sometimes reads a little dense" and `{{business.name}}` → `{{contact.company_name}}` in SMS 2.
7. **Add a Ready-to-Sign nudge** (new, small): one email at +2 days if the deal hasn't moved to Contract Sent, subject "want me to just send the agreement over?" This stage is silent today and it's the warmest silence in the pipeline.

**Cadences stay as they are.** Missed Meetings Day 0/0/+30min/2/2/5, Contract Sent Day 3/4/4/6/8/10/10, Post Signing Day 0/1/1/3/5/7. The timing was never the problem; the links and gates were.

---

## Sequence overview (after changes)

| Flow | # | Touch | Timing | Primary CTA | Condition |
|---|---|---|---|---|---|
| Demo Confirmation (new) | 1 | Confirmation email | at booking | Add to calendar | all |
| | 2 | Reminder email | −24h | Confirm / reschedule link | all |
| | 3 | Reminder SMS | −1h | "See you at [time]" | has mobile |
| Missed Meetings | 1 | Email: missed you | Day 0 | Booking link | tag added |
| | 2 | SMS nudge | +30 min | Booking link | has mobile |
| | 3 | Task: call → Adam | Day 2 | — | no booking yet |
| | 4 | Email: soft follow-up | Day 2 | Booking link | no booking yet |
| | 5 | Email: value + breakup | Day 5 | Booking link | no booking yet |
| Contract Sent | 1 | Email: delivery check | Day 3 | Signing link | still in stage |
| | 2 | Task + SMS | Day 4 | Signing link | still in stage |
| | 3 | Email: walkthrough offer | Day 6 | 10-min call link | re-check stage |
| | 4 | Task: call → Adam | Day 8 | — | still in stage |
| | 5 | Email: onboarding spot + SMS | Day 10 | Signing link | still in stage |
| Post Signing | 1 | Email: welcome + roadmap | Day 0 | Reply with data timeline | stage guard |
| | 2 | Task + SMS: data nudge | Day 1 | — | data not received |
| | 3 | Email: data instructions | Day 3 | Data guide link | data not received |
| | 4 | Task: call → Adam | Day 5 | — | data not received |
| Go-Live (new, split out) | 1 | Email: you're live | stage = Live | — | auto-adds `status-live` |
| Monthly Check-in | 1 | Email: monthly summary | Day 0, loops 30d | Reply | `status-live` |

Exit rules, all flows: booking made / document signed / stage moved / reply received → exit and notify. Suppress any contact who is active in another clinic sequence.

---

## Rewritten copy

Merge fields are standard GHL. Set a real from-name and from-email on every email. No touch goes out without its link.

### New: Demo Confirmation (on Demo Booked)

**Email 1 — at booking. Subject options:** "You're booked: Tile Health demo on {{appointment.start_date}}" / "Confirmed, {{contact.first_name}}: your APCM walkthrough"
**Preview:** Takes 20 minutes. Here's what we'll cover.

> Hi {{contact.first_name}},
>
> You're confirmed for {{appointment.start_date}} at {{appointment.start_time}}. Here's the calendar invite: [calendar link]
>
> We'll spend about 20 minutes on two things: what your panel is likely worth under APCM, and exactly what your staff would (and wouldn't) have to do.
>
> If you can, have a rough count of your traditional Medicare patients handy. That's the number that drives everything.
>
> Looking forward to it,
> {{user.full_name}}

**Email 2 — 24h before. Subject:** "Tomorrow: your Tile Health demo" · **Preview:** Need a different time? One click fixes it.

> Hi {{contact.first_name}},
>
> Quick reminder that we're on for tomorrow at {{appointment.start_time}}.
>
> If the day got away from you, no problem. Grab a better slot here instead of missing it: [booking link]
>
> See you then,
> {{user.full_name}}

**SMS — 1h before:**
> Hi {{contact.first_name}}, it's {{user.name}} at Tile Health. See you at {{appointment.start_time}}. If something came up, rebook in 10 seconds here: [short link]

### Missed Meetings (repaired)

**Email 1 — Day 0.** Keep the current copy, it's good. Add the link so the ask has legs. Replace the last line with:

> Want to grab another slot this week? Here's my calendar so you don't have to wait on me: [booking link]. Or just reply and I'll send times.

**SMS — +30 min:**
> Hey {{contact.first_name}}, looks like we missed each other. No worries. Rebook here whenever suits: [short link]

**Email 2 — Day 2. Subject:** "Re: missed you on the call" (keep) · **Preview:** Grab any slot that works, or tell me the timing's off.

> Hi {{contact.first_name}},
>
> Circling back on this. Totally understand if ten things hit at once and the call got buried.
>
> If the timing is genuinely off right now, just say the word and I'll check back next month, no pressure at all. Otherwise here's my calendar, grab whatever works: [booking link]
>
> {{user.full_name}}

**Email 3 — Day 5. Subject options:** "{{contact.company_name}}, still worth 20 minutes?" (keep) / "Before I close this out" · **Preview:** Most practices are leaving $300K+ in APCM revenue uncollected.

> Hi {{contact.first_name}},
>
> Last nudge from me, I promise.
>
> Most practices we work with were leaving $300K to $400K a year in uncollected APCM revenue before we came in. That's not a typo. Medicare pays up to $117.53 per patient per month for dual-eligible patients and $53.91 for standard chronic care. It adds up fast.
>
> We run the whole billing program and your practice keeps 60 to 70% of everything collected, with no upfront cost.
>
> If that's worth 20 minutes, my calendar's here: [booking link]. If not, reply "not now" and I'll leave you be until next quarter.
>
> {{user.full_name}}

*(The "not now" reply line only works once stop-on-reply is ON. That's the point: a reply, any reply, should reach a human.)*

### Contract Sent (keep the spine, add the link and one gate)

Emails 1 and 3 keep their current copy with two additions each: the **signing link in every touch**, and the fixed sentence in Email 2. Only the changed parts shown:

**Email 1 — Day 3, add after "feel free to reach out":**
> Here's the agreement again so it's one click away, not somewhere in your inbox: [signing link]

**Email 2 — Day 6, fixed sentence + link:**
> One part of the agreement reads denser than it really is, so I like to walk people through it on a quick call. It's simpler than it looks on paper.
>
> Takes about 10 minutes: [booking link]. And the agreement itself is here whenever you're ready: [signing link]

**SMS 2 — Day 10, fixed merge field:**
> {{contact.first_name}}, last note from me. Happy to show you the APCM math for {{contact.company_name}} if you're curious: [booking link]. No pressure either way.

**New guard:** duplicate the stage check before Email 3 (Day 10 block) so signed deals never get the scarcity email.

### Post Signing (make the promise true, move the finale)

**Email 2 — Day 3:** attach the actual patient-data guide, or swap the sentence to "Here's the guide that walks through exactly what we need: [guide link]". Nothing else needs to change; the copy is good once it's honest.

**Email 3 ("{{contact.company_name}} is live")** moves out of this workflow entirely, into:

### New: Go-Live (trigger: stage = Live)

1. Action 1: add tag `status-live` (this is what finally turns Monthly Check-in on, automatically, forever)
2. Action 2: send the existing "you're live" email unchanged. It's a great email; it was just firing at the wrong moment.

### Monthly Check-in (only send what you can fill)

GoHighLevel can't merge billing numbers, so stop pretending it can. Two options, in order of preference:

**Option A (recommended):** convert the loop into a monthly **internal task for Adam**: "Send {{contact.company_name}} their APCM summary (patients billed, collected, their share)." A founder-written monthly email with real numbers beats an automated one with brackets.

**Option B:** if it must stay automated, strip the numbers:

> **Subject:** {{contact.company_name}}, quick monthly check-in
>
> Hi {{contact.first_name}},
>
> Quick note to say everything's running as expected on our end this month. Your detailed billing summary is on its way separately.
>
> If anything looks off or your team has questions, just reply and I'll jump on it.
>
> {{user.full_name}}, Tile Health

Also: stop-on-reply is ON here, which currently means one reply ends the recurring loop forever. For a recurring relationship email, turn it OFF in this one workflow and rely on the reply notification instead.

---

## Sequence flow (after changes)

```
[Demo Booked] ──► Confirmation ──► −24h reminder ──► −1h SMS
                                                        │
                              attended ─────────────────┼──── no-show/cancel (auto tag)
                                  │                     ▼
                        [Demo Attended] ──► Missed Meetings (links fixed)
                                  │                     │ books again ──► [EXIT → Demo Booked]
                                  ▼                     ▼ no response ──► monthly cadence, not weekly
                        [Ready to Sign] ──► +2d nudge ──► [Contract Sent stage]
                                                              │
                                              Contract Sent flow (guarded at Day 3 AND Day 10)
                                                              │ signs ──► [EXIT]
                                                              ▼
                                            [Waiting for Patient Data] ──► Post Signing (data chase only)
                                                              │ data received ──► panel review ──► training
                                                              ▼
                                                        [Live stage] ──► Go-Live flow ──► adds status-live
                                                                                              │
                                                                                              ▼
                                                                                    Monthly Check-in loop
```

## Benchmarks and what to watch

For this audience (clinic owners/office managers, warm lifecycle stages), realistic targets: opens 50–70% (you're already there), **click-through 8–15% on booking/signing links (currently unmeasurable, this is the metric the whole rebuild unlocks)**, no-show recovery 15–25% once links work, contract-stage conversion within 14 days as the north star. Unsubscribes above 1% in Contract Sent means the Day-10 scarcity email is landing on already-signed deals; that's your signal the second stage guard is missing or broken.

Review weekly for the first month: booking-link clicks in Missed Meetings, signing-link clicks in Contract Sent, days-in-stage for Waiting for Patient Data, and whether `status-live` is being applied automatically.

## A/B tests worth running (once links are live)

1. **Missed Meetings Email 1 subject:** "{{contact.first_name}}, missed you on the call" vs. "want to grab a new time?". Measure rebooks, not opens.
2. **Contract Sent Email 3 angle:** scarcity ("holding your onboarding spot") vs. math ("what waiting costs {{contact.company_name}} per month", using the ~$6,900/mo figure from the calculator). Measure signing-link clicks.
3. **Demo reminder SMS timing:** 1h vs. 3h before. Measure show rate.

## Placeholders to fill

[booking link] and [short link] (SMS) · [signing link] merge field · [calendar link] in confirmations · patient-data [guide link] or attachment · from-name/from-email (Ali) · A2P 10DLC status check before scaling SMS.
