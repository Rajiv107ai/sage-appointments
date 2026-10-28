# AI Development Chat Log: Sage Wellness Appointments

This document contains the unvarnished log of prompts, queries, clarifications, and iterative corrections used with AI during the architecture, design, and implementation of the **Kayro AI Product Intern Assignment**.

---

## Session 1: Problem Decomposition & Strategy for Part 1

**User Prompt:**
> I'm working on an assignment for a wellness clinic. A nutritionist runs consultations using WhatsApp and Excel, but 1 in 4 clients (25%) simply don't show up. She only finds out when sitting there waiting. How should I solve this for her? Give me practical steps that don't require heavy engineering or immediate payment friction, and explain why each step works. No code.

**AI Output:**
> Here is a practical, 5-step operational framework to tackle client no-shows:
> 1. Automated WhatsApp Reminders (24h before & 2h before).
> 2. Active 1-tap confirmation ("I'll be there" or reply YES).
> 3. Frictionless self-service rescheduling link in every notification.
> 4. Unconfirmed client triage list for the nutritionist.
> 5. Deposit policy for repeat offenders.
> 
> *Rollout recommendation:* Start with steps 1–4 because they cost nothing and leverage existing client habits on WhatsApp. Only introduce deposits if no-shows stay above 8% after 30 days.

**User Clarification:**
> I want to tie this strategy directly into Part 2 (the prototype build). What features should the prototype have to reflect this strategy?

**AI Output:**
> To reflect your Part 1 answer directly in the Part 2 build, you should add:
> 1. An "I'll be there" confirmation button on the client app's next visit card.
> 2. A "✓ Client confirmed" status tag on the nutritionist dashboard when the client taps it.
> 3. A 1-click "Remind on WhatsApp" link with a pre-filled message (`wa.me`) for upcoming visits.
> 4. A live "No-show rate" metric card on the dashboard so she can track performance.

---

## Session 2: Prototype Architecture & Initial Code Generation

**User Prompt:**
> Build a single-file prototype (`index.html`) with two views that share the same data:
> Side A: Client app (mobile-first, calm wellness look, shows next consultation, booking slot picker with no double booking, upcoming and past visits, reschedule/cancel).
> Side B: Nutritionist dashboard (table of appointments with name, phone, date, time, and colored status for Booked, Done, No-show that she can change inline; manual add form; "Today" filter).
> Load it with the sample data:
> - Riya Shah | 98200 11111 | 24/10/2026 | 10:00 | Booked
> - Arjun Mehta | 98200 22222 | 2026-10-24 | 11:30am | Booked
> - Riya Shah | 9820011111 | 25-10-2026 | 4 pm | Booked
> - Meera Iyer | (no phone) | 23/10/2026 | 09:00 | Done
> - Kabir Rao | 98200 33333 | 22/10/2026 | 14:00 | No-show
> 
> Include the Part 1 no-show mitigations.

**AI Output:**
> [Generated first draft of `index.html` with vanilla JS, CSS custom properties, and localStorage].

---

## Session 3: Testing, Bug Catching & Iterations

**User Correction 1 (Sample Data Normalization):**
> Look at the sample data: dates come in 3 different formats (`24/10/2026`, `2026-10-24`, `25-10-2026`), times come in 4 notations (`10:00`, `11:30am`, `4 pm`, `14:00`), and phones have spaces or missing values. Your current code broke on "4 pm" and failed to match Riya's two visits. Fix the regex parsers for dates, times, and phone numbers.

**AI Output:**
> Updated normalization logic:
> - `nDate`: handles `YYYY-MM-DD`, `DD/MM/YYYY`, and `DD-MM-YYYY` using regex extraction.
> - `nTime`: parses both 12h (am/pm with optional minutes) and 24h formats, normalizing to `HH:mm`.
> - `nPhone`: strips non-digits, trims leading `91` country code, and validates 10 digits.
> - Phone formatting helper displays 10 digits as `98200 11111`.

**User Correction 2 (Rescheduling Bug for Phone-less Clients):**
> When testing Meera Iyer's appointment, if I try to reschedule her appointment, `go()` throws an error: "Enter a valid 10-digit phone number". But in reschedule mode (`r = true`), the phone input field is not rendered! Why is it throwing this error, and how do we fix it?

**AI Output:**
> *Root cause:* In `go()`, the phone validation checked `!ph`, where `ph = ex ? ex.phone : nPhone(S.ph)`. Since Meera Iyer has no phone number, `ex.phone` is empty string, which evaluated to falsy and triggered the mandatory phone validation.
> *Fix:* Updated `go()` so that if an existing appointment is being rescheduled (`ex` exists), it retains `ex.phone` without forcing 10-digit validation on an invisible input field.

**User Correction 3 (The Evaluator Date Time-Warp Issue):**
> When I open `index.html` in my browser today, the client home screen says "No visit planned" and the nutritionist dashboard "Today" filter shows 0 appointments. Why is that?
> Ah, wait: the sample data is pinned to October 2026! Because your code checks `new Date()`, any browser running on a date outside October 2026 considers all appointments to be either in the past or far in the future.
> An evaluator opening the hosted demo will immediately see an empty screen. How can we fix this so the evaluator gets the intended experience immediately?

**AI Output:**
> *Solution:* Implement a **Demo Date Context / Simulator**.
> By default, anchor the app's relative reference clock to `2026-10-24T08:30:00` (the date of the sample data).
> - Riya's appointment on 24 Oct 2026 at 10:00 AM immediately appears in the hero card with the "I'll be there" button.
> - The Nutritionist "Today" filter immediately displays today's 2 appointments (Riya Shah and Arjun Mehta).
> - The "Remind on WhatsApp" buttons are active.
> - Add a prominent top banner: `"📅 Viewing as Demo Date: 24 Oct 2026 (matches sample data)"` with a 1-click toggle to switch to the user's live device clock.

**User Correction 4 (Client Profile Switching UX):**
> Right now, switching clients on the Client side requires clicking "Not you?" and typing a full 10-digit phone number. Evaluators won't remember Arjun's or Kabir's numbers. Add quick-switch chips for sample clients.

**AI Output:**
> Added client quick-switcher chips (`[Riya Shah] [Arjun Mehta] [Kabir Rao] [Other...]`) directly below the consultation card. Clicking any chip updates `S.me` instantly and re-renders the home screen, visits, and bookings for that profile.

**User Correction 5 (Cross-Tab Synchronization):**
> When I open the client app in one tab and the nutritionist dashboard in another tab, and book a consultation, the other tab doesn't update until manually refreshed. Make it sync automatically.

**AI Output:**
> Added `window.addEventListener('storage', (e) => { if (e.key === K) { A = load() || A; render(); } })`. Now when `localStorage` is updated in one tab, the storage event triggers an instant re-render across all open tabs.

---

## Session 4: Final Review & Polish

**User Prompt:**
> Check if there are any remaining edge cases. Can an evaluator restore the sample data if they accidentally delete all rows?

**AI Output:**
> Added a `↺ Reset Sample Data` button in both the filter bar and footer. When clicked, it restores `A` back to the initial 5 normalized rows and resets the view state.
