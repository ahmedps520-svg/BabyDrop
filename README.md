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
var PANIC_PHONE = '+966500000000';
```

**2. Firebase Realtime Database** (free Spark plan) — this is what makes claims sync live
between everyone's phones instead of staying in one browser.

```js
var FIREBASE_CONFIG = { apiKey: 'PASTE_API_KEY', ... };
```

Get it from Firebase Console → Project settings → Your apps → Web app → SDK setup.
Then create a Realtime Database and set its rules to allow public read/write on the
registry path (guests are not signed in):

```json
{
  "rules": {
    "registry": { ".read": true, ".write": true }
  }
}
```

Until the config is filled in the page still works perfectly — it just falls back to
this-device-only storage and shows a small banner saying live sync is off.

## Adding gifts

Append to the `ITEMS` array (`2. PRODUCTS`). Each entry needs `id`, `name`, `subtitle`,
`price`, `link`, and `image` (a base64 data URI — retailer image URLs mostly block
hotlinking). Everything else wires up automatically.
