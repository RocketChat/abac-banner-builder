# ABAC banner generator

Static builder for Rocket.Chat **classification banners** configurations. Compose attributes, values and colors, watch the banner render live (exactly as the room banner engine renders it), then copy the JSON into the `ABAC_Classification_Banners_Config` setting (**Admin → ABAC → Settings → Classification banners**).

Single self-contained `index.html` — no build step, no dependencies, works offline.

## Run

Open `index.html` in a browser, or serve statically:

```sh
npx serve .
```

## Publish to GitHub Pages

1. Push this folder to a GitHub repository.
2. Repository **Settings → Pages → Source**: deploy from branch, root (`/`).
3. The app is served at `https://<user>.github.io/<repo>/`.

## What it does

- **Rocket.Chat version toggle**: **8.8 / 8.9** (banners in ABAC rooms only) or **9.0+** (adds the optional `nonAbacBanner` list for every other room). Both use schema `version: 1`; 9.0 only adds a property, but 8.8 and 8.9 reject documents that carry it, so the builder leaves it out of the JSON in 8.8 / 8.9 mode. Importing a document with `nonAbacBanner` switches to 9.0+.
- **Live preview** pinned at the top — driven by the configuration and the simulated room, using the same engine semantics as Rocket.Chat (`@rocket.chat/abac` classification-banner engine): segment order, delimiters, label prefixes, alphabetical sorting, group-collapse threshold, most-restrictive color resolution, contrast-computed foreground, fallback banner, non-ABAC room matching, and no banner in Omnichannel rooms.
- **Attribute editor**: add/remove/reorder attributes, per-value labels and colors, exactly one color-driving attribute.
- **Non-ABAC banner editor** (9.0+): ordered entries with text, color, enabled and an optional room match on type, federated, discussion and team. The first enabled entry without a match is the default.
- **Room simulator**: an ABAC room with picked attribute values, or a public channel, private group, DM or Omnichannel room with federated, discussion and team flags.
- **Cross-field lint** (warn-only): one `drivesColor`, unique ids/sources, `multipleLabel` with `groupThreshold`, `bannerLabel` with `showLabel`, and in 9.0+ an enabled default non-ABAC entry and non-empty room matches.
- **Import / copy JSON**, validated against the published v1 schema contract (`https://rocket.chat/schemas/classification-banners/v1.json`):
  - A document the builder cannot display (wrong JSON types, missing required fields, a version other than 1) stays in the import dialog with a list of problems, so you can fix it there and import again.
  - Any other schema violation (bad colors, empty text, unknown keys, ...) loads as written and is listed under **Fix before saving**; unknown keys get a **Remove** button.
  - The schema allows only the `classic` banner style; imported documents with another style are switched to it.
  - A saved configuration that can no longer be loaded is kept, and a notice offers to open it in the import dialog.

The configuration document is described by the schema in the Rocket.Chat repository: `ee/packages/abac/docs/classification-banners.schema.json`. `index.html` embeds a verbatim copy of it (`SCHEMA`) and validates with a small built-in validator, so the page still works offline. When the schema changes in Rocket.Chat, paste the new file into `SCHEMA`; the validator throws on any JSON Schema keyword it does not support yet.
