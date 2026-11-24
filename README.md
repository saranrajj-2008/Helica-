# Helica: On-Demand Helicopter Transport

An animated, single-page web app for a helicopter ride-booking service (think Uber/Rapido, but for helicopters). It combines a cinematic landing page with a working booking and dispatch system, all in one self-contained HTML file.

## Features

**Landing page**
- Hero with spinning rotor blades, floating helicopter, drifting clouds and a perspective grid floor
- Scroll-linked flight path: a route line is drawn down the page and a helicopter marker travels along it as you scroll
- Sections: How it works, Fleet, Pricing, Safety, Booking, My bookings
- Scroll-reveal animations, responsive layout, respects `prefers-reduced-motion`
- In-page navigation only (smooth scrolling, no redirects or page reloads)

**Fare computer**
- Distance slider (1-150 km) and 1 or 2 passengers
- Animated fare readout, recalculated live
- Tiered rate per km (shorter hops cost more per km):

| Distance | Rate |
|---|---|
| up to 15 km | Rs 5,000 / km |
| up to 40 km | Rs 3,500 / km |
| up to 80 km | Rs 2,600 / km |
| beyond 80 km | Rs 2,000 / km |

- Fare is doubled for 2 passengers

**Booking system**
- Form: from, to, distance, date, time, passengers
- Checks fleet availability for the requested slot (2 aircraft, 60-minute slot per booking)
- Assigns a free helicopter and its pilot, or marks the booking as waitlisted if both are taken
- Fare uses the same logic as the fare computer
- "My flights" lists the bookings you made

**Dispatch admin**
- Opens as an overlay panel (no page change) from the nav bar
- Shows every booking with route, time, fare, aircraft and status
- Dispatcher can mark a booking Completed or Cancelled

## Tech stack

- HTML, CSS, vanilla JavaScript (no frameworks, no build step)
- Fonts: Space Grotesk, Inter, JetBrains Mono (Google Fonts)
- Persistence: the artifact `window.storage` key-value API

## Project structure

```
helica.html     the entire app (markup, styles, scripts)
README.md       this file
```

## How it works

1. **Detection of availability:** `findAvailableAircraft()` loads all stored bookings and checks each helicopter for any non-cancelled booking within 60 minutes of the requested time.
2. **Storage layout:** `bookings-index` (list of booking ids), `booking:<id>` (one record per booking), `my-booking-ids` (ids created by the current user).
3. **Booking and dispatch data is stored as shared data** so fleet availability works across users. Anyone using the same artifact can see route, time and fare details.

## Running it

- **Inside Claude:** open the artifact. Booking, "My flights" and dispatch work and persist across visits.
- **Opened as a plain file or hosted elsewhere (e.g. Vercel):** the landing page, animations and fare computer work fully, but `window.storage` does not exist there, so bookings will not save. To deploy it as a real site, replace the storage functions (`getIndex`, `saveBooking`, `getMyIds` and related) with calls to a real backend or database such as Node + PostgreSQL, Firebase or Supabase.

## Possible next steps

- Real backend with authentication (OTP login) and payments (Razorpay/Stripe)
- Distance from a maps API instead of manual entry
- Live flight status and SMS/email confirmations
- Role-based dispatcher login instead of an open admin panel

## Notes

- The company name, fleet details and pricing are demo values for the concept.
- This is a prototype and not a licensed aviation operator.
