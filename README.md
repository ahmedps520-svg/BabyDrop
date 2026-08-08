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

**2. Firebase Realtime Database** — already wired in. Project **Babydrop** (`babydrop-57614`),
Spark plan, database in Singapore (`asia-southeast1`). Claims sync live between everyone's
phones through it.

The `apiKey` in `index.html` is not a secret — a Firebase web API key identifies the project
and is designed to ship in client code. Access is governed entirely by the database rules:

```json
{ "rules": { "reservations": { ".read": true, ".write": true } } }
```

`DB_PATH` is therefore `reservations/claims-v1`. Move it outside `reservations` and every
claim is denied and silently fails to save.

Because those rules are open, anyone who finds the database URL can read or overwrite the
claims. That is a reasonable trade for a family registry with no sign-in, but it is why the
path is versioned — bump `claims-v1` to `claims-v2` to start over if the data is ever messed
with.

If live sync ever fails, the page still renders and falls back to this-device-only storage,
showing a small banner.

To re-fetch the config: Console -> gear -> **Project settings** -> **General** -> **Your apps**
-> Web app -> **SDK setup and configuration** -> **Config**.

## Adding gifts

Append to the `ITEMS` array (`2. PRODUCTS`). Each entry needs `id`, `name`, `subtitle`,
`price`, `link`, and `image` (a base64 data URI — retailer image URLs mostly block
hotlinking). Everything else wires up automatically.

## Link preview

`preview.png` is the image WhatsApp/iMessage show when the link is shared, wired up via the
Open Graph tags in `<head>`. It is just a 1200x630 screenshot of the page itself, so if the
design changes noticeably, regenerate it and re-commit.
