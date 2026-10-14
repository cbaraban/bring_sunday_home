# Bring Sunday Home: how to update this site

This repo is the whole website at https://www.bringsundayhome.com. It's a single static page about Sunday, a dog missing in Overland Park, KS since Friday, October 9, 2026, after she got out of Pawz at Play. GitHub Pages serves it from `main` (root), and every push goes live in about a minute.

**Status: Sunday is home.** She walked through her own front door at 1:50 AM on Wednesday, October 14, 2026. The site is now a celebration and thank-you page. The top of the page has the "Sunday is home!" section (confetti on load), then the thank-you message from the Pawz at Play Team, then photos. After the thank-you is "Sunday's thank-you party", a sign-up button that opens the owner's Google Form (responses go to their Google account, never to this repo). The search map ("How the search unfolded") and the escape journey stay as the record. The contact number, reward, call buttons, "What happens next" and the lost-dog flyer have been removed. Don't add them back.

Note: this repo is public, and this file can be read at bringsundayhome.com/CLAUDE.md. Keep private details out of it.

## Before you change anything

1. Run `git pull` first. The site may also be edited from another computer.
2. Run `python3 -m http.server 8000` in this folder and open http://localhost:8000 to preview your changes. Check both desktop and phone widths (375px).
3. Commit, push to `main`, then confirm the change is live:
   `curl -s "https://www.bringsundayhome.com/?v=$RANDOM" | grep -c "<some new text>"`

## Files

| File | What it is |
|---|---|
| `index.html` | The entire site: HTML, CSS and JavaScript in one file |
| `sunday-photos/` | Web-sized photos (rotated upright, metadata stripped) and `share.jpg`, the 1200×630 link-preview image |
| `sunday-flyer.pdf` | Printable flyer (made in Canva by the AgencyOf team) |
| `CNAME` | Custom domain. Don't remove it |
| `.nojekyll` | Tells GitHub Pages to serve files as-is |
| `.gitignore` | **Whitelist:** everything is ignored except the files listed. Add new files to it or they won't be published |

## Where things are in index.html

- **`CONFIG`**: phone number, reward, the top update card (`latestUpdate`), `earlierUpdates`, `searchUpdated` (search map date) and `lastUpdated` (footer date). Most updates start here.
- **`PHOTOS`**: the gallery. The standing photo is an illustration and is labelled that way.
- **Escape journey map** (Friday, October 9): `P` (places), `SEGMENTS` (route lines), `STEPS` (cards), `HOW` (the "Click to Learn More" floor plan).
- **Search map** (inside the "Where the search stands" block):
  - `AREAS`: shaded polygons by color
  - `LEGEND`: map key chips
  - `SSTEPS`: the cards
  - Constants for scent routes and sightings (`SCENT`, `SCENT2`, `SIGHT`, `PINE`, `PINE_AREA` and so on)
  - Markers and layers, added in `smap.on("load", ...)`
- Coordinates are `[longitude, latitude]`. Street geometry has come from OpenStreetMap through the Overpass API (the `maps.mail.ru` mirror has been the most reliable). Hand-drawn areas were traced from the search team's screenshots.

## Rules the site owner has set

1. **Newest first, everywhere.**
   - The top card is always the latest update. The previous one moves to the top of `earlierUpdates`.
   - On the search map, the newest dated update is step 1, then earlier updates (newest to oldest), then the overview, then the reference areas.
2. **Real dates only.** Write "Monday, October 12", never "today", "earlier today" or "yesterday". The page stays up for days.
3. **Sentence case** for headings and buttons, matching the rest of the site.
4. **Page order:** sticky header, latest update card, "Have you seen Sunday?" banner (with reward), photos and flyer button, Where the search stands, What happens next, Sunday's escape journey, footer.
5. Keep the reminder that **searched areas don't mean Sunday isn't there**.
6. **Unconfirmed reports stay off the site** until the owner confirms them. Sightings are called "possible sightings."
7. **Map colors mean:**

   | Color | Meaning |
   |---|---|
   | Rust/orange, pulsing | Area of last reported sighting |
   | Purple | Where Sunday may be |
   | Pink line | Scent-dog routes |
   | Orange dashed | Indian Creek |
   | Green | Searched on foot, by drone and scent dog |
   | Yellow | Searched thoroughly |
   | Blue dashed | Thermal drone |

## Adding a new update (checklist)

1. Move the current `CONFIG.latestUpdate` into `CONFIG.earlierUpdates` as the first item, with its real date and `mapStep`.
2. Write the new `latestUpdate`: `date` (day, date and time if known), `title`, `body` paragraphs and `mapStep`.
3. If it has a location, add a step at the top of `SSTEPS` with a new `key`. Then add its bounds in `boundsOf()`, its markers and lines in the load handler, and a `LEGEND` chip if it's a new kind of area.
4. Update `searchUpdated` and `lastUpdated`.
5. Update "What happens next" if the plan changed.
6. Preview on desktop and phone, push, and check the live site.
7. If `share.jpg` changed, re-scrape the link in Facebook's Sharing Debugger.

## When Sunday is found

These changes switch the page to a "Sunday is home" version:

1. Set `CONFIG.showSearchBanner` to `false`. That hides the banner, the floating call button and the "Seen Sunday?" call-outs.
2. Set `CONFIG.reward` to `""`. That hides every mention of the reward.
3. Remove the header's "Report a sighting" button (`#headCall`) and the footer's "Report a sighting" line. Both follow `sightingsPhone`, not `showSearchBanner`.
4. Replace `latestUpdate` with the good news.
