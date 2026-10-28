# Sage: appointments app

- **Live Link**: https://rajiv107ai.github.io/sage-appointments/
- **GitHub**: https://github.com/Rajiv107ai/sage-appointments

A small appointments app for a nutritionist. It has two views that use the same data: a client app (made for phones) and a nutritionist dashboard.

## How to run

Open `index.html` in any browser. Nothing to install.

To put it online: push this folder to GitHub, then import the repo in Vercel and choose the "Other" preset. It is a plain static page.

## What is in it

Client app
- Home screen with the next appointment and an "I will be there" button
- Book a visit: pick a date and a free time slot, enter name and phone. A taken slot cannot be booked again
- Upcoming and past visits, with reschedule and cancel for upcoming ones

Nutritionist dashboard
- List of appointments with name, phone, date, time and status
- Status is Booked, Done or No-show, each in a different colour, and she can change it
- Add an appointment by hand, and a Today filter
- Anything booked in the client app shows here, and changes here show in the client app
- Shows when a client has confirmed, a WhatsApp reminder button, and the no-show rate

## Sample data

The five sample rows are cleaned when the app loads. Dates become YYYY-MM-DD, times become 24 hour (for example 4 pm becomes 16:00), and phones become 10 digits. The two Riya Shah rows have the same phone, so they are one client. Meera Iyer has no phone and shows "No phone". The "Reset sample data" button brings back the original five rows.

The sample dates are in October 2026, so the app starts on 24 Oct 2026 (a banner shows this). One tap switches to the real date.

## Notes

- Data is saved in the browser (local storage). The two views stay in sync in one browser, also across tabs. Two different devices do not sync. A real product would need a database.
- A client is found by phone number. There is no login.
- A cancelled visit is deleted so the slot is free again.
