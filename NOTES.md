# Observing HTTP Requests in DevTools

**Site:** Google Chrome New Tab page (chrome://new-tab-page)
**Browser:** Chrome
**Date:** 2026-11-10

I opened DevTools (Network tab) on my Chrome New Tab page and refreshed it. The page made 89 requests in total, with 376 kB transferred. DOMContentLoaded happened at 752 ms and the full load finished at 761 ms.

## Request 1

- **Method:** GET
- **URL:** chrome://new-tab-page/cr_iconset.js
- **Status code:** 200
- **Type:** script
- **Initiator:** icons.html.js:4
- **Size / Time:** 2.0 kB / 14 ms
- **Purpose:** The browser requested a JavaScript file that defines the icon set used by the page. It was triggered by another script (icons.html.js), so it is a dependency loaded after the first files. The 200 status means it loaded successfully.

## Request 2

- **Method:** GET
- **URL:** chrome://new-tab-page/static_types.js
- **Status code:** 200
- **Type:** script
- **Initiator:** icons.html.js:4
- **Size / Time:** 1.1 kB / 2 ms
- **Purpose:** This is another script file loaded by the same initiator. It is a small support file the page needs before it can show the shortcuts and icons. It was very fast (2 ms) because it is small and local.

## Request 3

- **Method:** GET
- **URL:** chrome://favicon2/?size=24&scaleFactor=1x&showFallback...
- **Status code:** 200
- **Type:** png
- **Initiator:** lit.rollup.js:16
- **Size / Time:** 0.8 kB to 1.7 kB / 2 to 4 ms
- **Purpose:** The page requested small 24px icons (favicons) of my most visited sites, such as GitHub, Gmail, Instagram, and X. Each shortcut tile has its own request, which is why there are many similar rows. The parameter size=24 tells the server which icon size to return.

## What I learned

- One page load creates many requests (89 here), not only one. Most of them are small files like scripts and images.
- All the requests I looked at used GET, because the browser was only asking for files and not sending data.
- Status 200 means the request was successful.
- The Initiator column shows which file triggered a request, so I can see the chain: one script loads another script, which loads images.
- Some rows (like the data:image/gif one) were served from memory and took 0 ms, so not every request goes over the network.
- The Network tab shows the size and time of each request, which helps to understand page speed.
