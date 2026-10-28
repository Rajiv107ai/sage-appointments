# Chat log: Sage appointments app

This is the log of my prompts and chats with Claude during this assignment, from start to finish.

---

### Session 1: Part 1 puzzle (no-shows)

**Me:**
> hey, got this assignment for an ai product intern role at kayro. attaching pdf.
> basically 2 parts: part 1 is a puzzle about fixing no-shows for a nutritionist, part 2 is building a small web app with client + nutritionist view.
> lets start with part 1. 1 in 4 clients dont turn up, and she only finds out when sitting and waiting. she only uses whatsapp and excel. how do we solve this? no code, just practical steps.

**Claude:**
> Analyzed why clients skip: mostly simple forgetting or feeling awkward to message and cancel. Suggested 5 practical steps:
> 1. WhatsApp reminder the day before and a few hours before.
> 2. Ask client to confirm ("I will be there" or reply YES).
> 3. Easy reschedule link in every text so moving a visit isn't awkward.
> 4. Unconfirmed list for tomorrow so nutritionist knows early.
> 5. Deposit for repeat no-shows.
> Recommended starting with 1 to 4 first because they cost nothing and have no payment friction.

**Me:**
> cool. i agree with starting with 1 to 4 first coz no payment gateway needed. we can track for 1 month then see if step 5 is needed.
> how do we connect this to part 2? what should i put in the app so it actually shows this no-show fix?

**Claude:**
> Suggested adding to Part 2:
> - Client app: an "I will be there" confirmation button on the next visit card.
> - Dashboard: a "✓ Client confirmed" tag, a one-tap WhatsApp reminder button with pre-filled text, and a live no-show rate card.

---

### Session 2: Building Part 2 (client app + dashboard)

**Me:**
> ok lets build the app for part 2 now.
> need a simple single page app (just 1 index.html file with vanilla js + css, no react or frameworks).
> two views sharing same data:
> 1) client app - phone view in middle, home screen showing next visit, book visit (date + 30 min slots 9am to 5:30pm, no double booking), upcoming and past visits, reschedule and cancel.
> 2) nutritionist dashboard - list of appts with name, phone, date, time, status (Booked, Done, No-show with diff colors, she can edit status), manual add form, and today filter.
> both views must stay in sync using localstorage.
> here is the sample data from brief:
> Riya Shah | 98200 11111 | 24/10/2026 | 10:00 | Booked
> Arjun Mehta | 98200 22222 | 2026-10-24 | 11:30am | Booked
> Riya Shah | 9820011111 | 25-10-2026 | 4 pm | Booked
> Meera Iyer | | 23/10/2026 | 09:00 | Done
> Kabir Rao | 98200 33333 | 22/10/2026 | 14:00 | No-show
> gimme full code.

**Claude:**
> Built the initial `index.html` containing:
> - Dual-view UI with top segmented toggle (`Client app` / `Nutritionist`).
> - LocalStorage persistence and sample data loading.
> - Booking slot picker with 14 days and double-booking checks.
> - Dashboard with colored status dropdowns and Today filter.

---

### Session 3: Testing in browser & fixing bugs

**Me:**
> opened index.html in browser and tested it. found a few bugs:
> 1) the sample data has messy formats like 24/10/2026, 2026-10-24, 4 pm, 11:30am. your parser broke on 4 pm.
> 2) why did u mark riyas first visit as conf: true in sample data? in the assignment table its just Booked, it shouldn't be confirmed already. make all sample rows unconfirmed by default.

**Claude:**
> Fixed date and time regex parsers to handle mixed styles (`DD/MM/YYYY`, `YYYY-MM-DD`, `DD-MM-YYYY`, and 12h/24h times). Set `conf: false` for all sample appointments on initial load.

**Me:**
> ok another issue. when i open the page today, client home screen says "No visit planned" and Today filter in dashboard shows 0 appointments!
> coz sample dates are in oct 2026, so new Date() thinks everything is in the past! if the reviewer opens the link today, they will see a blank screen and think app is broken lol.
> how do we fix this so they actually see riyas appointment on load?

**Claude:**
> Suggested anchoring default view to a Demo Date (24 Oct 2026), with a top banner: `"Viewing as Demo Date: 24 Oct 2026"` and a quick toggle to switch to the device's real date.

**Me:**
> yes that banner works. now it shows 2 appts today and riyas next visit.
> found another bug while testing reschedule: meera iyer has no phone in sample data. when i clicked reschedule on her, it gave error "Enter a valid 10-digit phone number". but during reschedule phone input is hidden so user is completely stuck! pls fix go() so it doesnt block existing visits that have no phone.

**Claude:**
> Updated `go()` so existing appointments keep their existing phone value and bypass the mandatory phone check during reschedule.

**Me:**
> tested everything in browser:
> booked a slot, tried same slot again (blocked). rescheduled, cancelled (slot freed up). changed status in dashboard, reloaded (saved). two tabs sync works too.
> can u add two small things:
> 1) quick buttons on client side to switch between riya, arjun, kabir so reviewer doesnt have to type 10 digit numbers
> 2) a reset sample data button in dashboard in case someone deletes all rows while testing

**Claude:**
> Added client profile quick-switch pills (`[Riya Shah] [Arjun Mehta] [Kabir Rao] [Other...]`) and a `↺ Reset Sample Data` button in the dashboard and footer.

---

### Session 4: Documentation & README

**Me:**
> now need documentation pdf and readme.
> for documentation, brief says use these exact headings:
> - What we were asked
> - Your approach
> - Decisions and assumptions (include sample data handling)
> - What you added
> - What the AI got wrong (and how you caught and fixed it)
> - How to run it
> in what ai got wrong, mention the 3 things:
> 1) testing in real browser without crashes
> 2) sample dates being in past causing empty screen and fixing with demo banner
> 3) you marking riya confirmed by mistake
> keep english simple and direct.

**Claude:**
> Drafted `Documentation.pdf`, `Part1-Answer.pdf`, and `README.md` matching the required headings, technical decisions, and testing findings.
