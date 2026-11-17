# biocharSuite

Field web apps for the TF biochar project. Static pages on GitHub Pages; data saved to a Google Sheet via Apps Script.

## appAssessBiomass.html

Biomass assessment recorder: tea pruning, maize cobs, invasives (3 m x 3 m plots) and areca waste (per 10 trees).

URL: https://forestsoil.github.io/biocharSuite/appAssessBiomass.html

**Use**
1. Pick biomass type, fill site and plot fields, press Save. Dry mass, moisture and t DM/ha are calculated live.
2. Records are stored on the device first and sync to the sheet when online (Records tab > Sync all).
3. After oven-drying (105 °C, 24 h), open the record under Records > Edit, add dry weights, Save. The sheet row updates, no duplicate.
4. Records > Export CSV gives a local backup.

**Setup (once)**
1. Create the Apps Script project under tfcarbondata@tomorrowfoundation.in, paste `appAssessBiomass_Code.gs`, set `SHEET_ID`.
2. Deploy > Web app > Execute as: Me > Access: Anyone. After any script edit: Manage deployments > New version.
3. Put the `/exec` URL in `DEFAULT_GAS_URL` in the HTML, or set it per device under ⚙.
4. Check: `<exec URL>?ping=1` returns `{"ok":true,...}`.

**Auto-captured per record**
- GPS fix (required; fills automatically, 📍 Refresh to retake)
- Submitter's Google account (signed in once at the launcher; name-only fallback until `GOOGLE_CLIENT_ID` is set)
- Photos from camera or gallery, stamped bottom right with submitter, date-time and lat/lon; uploaded to a Drive folder, links saved in the sheet

**Google sign-in setup (once)**
1. console.cloud.google.com as tfcarbondata: new project > APIs & Services > OAuth consent screen (Internal if all users are @tomorrowfoundation.in, else External + test users).
2. Credentials > Create credentials > OAuth client ID > Web application > Authorized JavaScript origins: `https://forestsoil.github.io`.
3. Paste the Client ID into `GOOGLE_CLIENT_ID` in `index.html` (the launcher is the single sign-in point; applets read the saved identity and redirect there if missing).
4. In Apps Script: paste the new `.gs`, run `authorize` once, then Manage deployments > New version.

**Files**
- `index.html`: launcher and sign-in
- `appAssessBiomass.html`: the app (single file, no dependencies)
- `appAssessBiomass_Code.gs`: Apps Script receiver (one sheet tab per biomass type, upsert by record ID)

**Notes**
- The endpoint has no password; anyone with the URL can post rows.
- Open the page once with data before going offline; entries made offline stay queued.
