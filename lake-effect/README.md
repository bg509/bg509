# Lake Effect: Cleveland flight deals

A flight deal tracker for Northeast Ohio. It shows round-trip fares from Cleveland Hopkins (CLE), plus drive-to airports (Akron-Canton, Columbus, Pittsburgh, Detroit), that are well below their usual price. Users set fare alerts for places they want to go.

**Status: clickable prototype. All fares are sample data.**

## What's in the prototype

- **Deals feed**: fares ranked by how far they are under the usual price, with filters (warm weather, city breaks, international, nonstop), a budget slider, and sorting.
- **Deal detail**: fare vs. typical, savings, a 60-day price chart, and a "Check on Google Flights" button that opens the real search for that route and dates.
- **Alerts**: pick a destination and a target price. The Alerts tab shows a badge when a fare is under a target.
- **Home airports**: CLE is always on. Turn on CAK/CMH/PIT/DTW to see deals worth the drive.
- Settings and alerts are saved on the device (localStorage).

Run it: open `index.html` in a browser. No build step and no install.

## The path to a real app (following the one-person-app playbook)

| Step | What | Service | You need to |
|---|---|---|---|
| 1 | Live fare data | A flight-price API (options: Duffel, Kiwi Tequila, SerpApi's Google Flights endpoint, Travelpayouts) | Sign up and get an API key; compare pricing and terms first |
| 2 | Deal engine | Small server that checks fares from CLE/CAK/CMH/PIT/DTW a few times a day, stores price history, and flags fares 25%+ below the route's 60-day median | Nothing; Claude builds it |
| 3 | Host the server + database | Railway (server, Postgres, scheduled job) | Create a Railway account |
| 4 | Email alerts | Resend | Create an account, verify a sending domain |
| 5 | Native iOS app | Expo (React Native), reusing this prototype's design | Apple Developer account ($99/yr) |
| 6 | Premium tier | RevenueCat (e.g. free: weekly digest; paid: instant alerts, more airports) | RevenueCat account, App Store Connect setup |
| 7 | TestFlight | EAS Build + Submit | A Mac isn't required with EAS cloud builds |
| 8 | Marketing | HTML brand guide once the design is locked, then landing page | Pick a landing-page tool |

### How "a deal" is defined

`savings % = 1 − (current fare ÷ typical fare)`, where *typical* is the route's median price over the last 60 days for similar trip lengths.

- **Steal**: 40% or more under typical
- **Great**: 25–39%
- **Good**: 15–24%
