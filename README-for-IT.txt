ASSET SCANNER - internal web app (for IT)
=========================================

What it is
  A small static web app that turns an iPhone or Android phone into a live
  Code39 barcode scanner for the asset relabelling project. No server code,
  no database, no external calls: everything runs on the phone, and scans are
  stored only on that phone until the user sends or clears them.

Files (host all of them together in one folder)
  index.html             the app
  zxing.min.js           barcode decoding library (ZXing, Apache-2.0), bundled locally
  sw.js                  service worker, caches the app so it works offline
  manifest.webmanifest   lets users add it to the home screen as an app
  icon-180.png, icon-192.png, icon-512.png

Hosting requirements
  1. Must be served over HTTPS. Phones only allow camera access on https://
     pages. A self-signed certificate will not work on iPhone unless the
     certificate is trusted on the device (MDM-pushed internal CA is fine).
  2. Any static web server works (IIS, nginx, Apache, SharePoint static hosting, etc.).
  3. Serve .webmanifest as  application/manifest+json  (IIS: add the MIME type).
  4. No special headers, cookies, or authentication are required. Putting it
     behind the intranet / VPN / SSO is fine.

Install on a phone
  1. Open the https:// address in Safari (iPhone) or Chrome (Android).
  2. iPhone: Share > Add to Home Screen. Android: menu > Install app.
  3. Open it from the home screen, tap Start scanning, allow camera access.

How scans reach the tracker
  Send (share sheet: Mail / Teams / Files) or Copy, then on the laptop:
  Asset_Label_Printer.html > Scan & verify > Import scans > choose file or paste.

Live link to the laptop (added)
  The phone app can be linked to the laptop label page by scanning a QR code
  the label page shows. Each scan's asset number is then posted to a relay and
  the label page receives it instantly.
  - Relay used: https://ntfy.sh (free public pub/sub). Only the asset number is
    sent, on a random, unguessable link code. Nothing else leaves the phone.
  - If ntfy.sh is blocked or not approved, IT can self-host ntfy (open source)
    and change the RELAY constant at the top of the link section in both
    index.html (phone app) and Asset_Label_Printer.html (laptop page).
