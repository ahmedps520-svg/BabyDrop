# BabyDrop — Asma's Baby Boy gift registry

A single self-contained `index.html` baby shower gift registry. No backend, no build step —
drop it on GitHub Pages and it works.

## Deploy

Settings → Pages → Source: *Deploy from a branch* → pick the branch, folder `/ (root)`.
The site is served from `index.html`.

## Two values you need to fill in

Both live at the top of the `<script>` block in `index.html`, under `1. CONFIG`.

**1. Panic button phone number**

```js
var PANIC_PHONE = '0568005010';
```

**2. Firebase Realtime Database** (project: **Babydrop**, Spark plan) — this is what makes
claims sync live between everyone's phones instead of staying in one browser.

To get the config again: Firebase Console -> gear icon -> **Project settings** -> **General**
tab -> scroll to **Your apps** -> pick the Web app -> **SDK setup and configuration** ->
select **Config**. Copy the whole `firebaseConfig` object into `index.html`. If no Web app is
listed there yet, click **Add app** and choose the Web (`</>`) icon first — a Realtime
Database on its own does not create one.

```js
var FIREBASE_CONFIG = { apiKey: 'PASTE_API_KEY', ... };
```

Two gotchas specific to this project:

- **The database is in Singapore (asia-southeast1)**, so `databaseURL` is the regional form
  `https://<project>-default-rtdb.asia-southeast1.firebasedatabase.app` — *not* the default
  `...-default-rtdb.firebaseio.com`. The config panel usually shows the right one, but if in
  doubt copy it from Realtime Database -> **Data**, where it is printed above the tree.
- **The rules only allow the `reservations` path:**

  ```json
  { "rules": { "reservations": { ".read": true, ".write": true } } }
  ```

  So `DB_PATH` is set to `reservations/claims-v1`. If you move it outside `reservations`,
  every claim is denied and nothing saves.

Until the config is filled in the page still works perfectly — it just falls back to
this-device-only storage and shows a small banner saying live sync is off.

## Adding gifts

Append to the `ITEMS` array (`2. PRODUCTS`). Each entry needs `id`, `name`, `subtitle`,
`price`, `link`, and `image` (a base64 data URI — retailer image URLs mostly block
hotlinking). Everything else wires up automatically.
