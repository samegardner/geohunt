# GeoHunt

A GPS scavenger hunt you build in a browser and share with one link. The player reads a clue, walks to the spot, and the next clue unlocks when they arrive. No app install, no account, no server.

**Live:** https://samegardner.github.io/geohunt/

## Who does what
- **You / the host** open the live link and build the hunt (title, clues, coordinates, a finish message), then tap **Copy share link**.
- **The player** opens that link on their phone and plays. They never see the builder.

## Building a hunt
1. Open the live link on a computer.
2. Give the hunt a title and a welcome message.
3. For each stop, write a clue and paste the target **latitude / longitude**.
   - To get coordinates: open **Google Maps**, right-click the exact spot, and click the lat/long numbers to copy them. Paste the first number into Latitude, the second into Longitude.
   - Or, if you're standing at the spot on your phone, tap **Fill from my current location**.
4. Set the **arrival radius** (how close counts as "arrived"). Default is 30 m (~33 yards).
5. Optionally add a short "you made it!" message for each stop and a finish/reward message.
6. Tap **Preview & test** to walk through it from your couch (no GPS needed).
7. When it's ready, tap **Copy short link** for a tiny link to send (it uses TinyURL). Prefer no shortener? **Copy full link** gives the long, fully self-contained version.

To revise a hunt later, open the builder and use **Edit an existing hunt** to paste its link back in, or open an `#edit=` link directly.

## Playing
Open the share link on a phone, tap **Start**, and allow location when asked. The screen shows how far you are from the next spot (in yards) and a hot/cold indicator. Get within the radius and the next clue unlocks automatically. An **I'm here** button is always there as a manual override if GPS is being stubborn. Close the tab and reopen the same link and you'll pick up right where you left off.

## A note on accuracy
Phone GPS is typically only accurate to about 5-20 yards, and worse next to tall buildings or water. A very tight radius (under ~15 yards) will often fail to trigger even when the player is right there. Keep radii generous and lean on the "I'm here" button for tricky spots.

## How it works (technical)
- One self-contained `index.html`. Plain HTML/CSS/JavaScript, no dependencies, no build step.
- The whole hunt is encoded into the share link's URL fragment (`#play=...`), so there's no backend and the data stays on the device. Hunts and progress are also cached in `localStorage`.
- Distance uses the haversine formula; arrival watches GPS via `navigator.geolocation.watchPosition`. The screen is kept awake during play with the Screen Wake Lock API.
- Requires HTTPS for the browser to grant location access — which is why it's hosted on GitHub Pages.

## Run locally
Geolocation needs a secure context, and `localhost` counts:
```
python3 -m http.server 8000
```
Then open http://localhost:8000. Use **Preview & test** or the **I'm here** button to step through without real GPS.
