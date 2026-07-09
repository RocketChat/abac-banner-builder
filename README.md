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

- **Live preview** pinned at the top — driven by the configuration and the simulated room values, using the same engine semantics as Rocket.Chat (`@rocket.chat/abac` classification-banners engine): segment order, delimiters, label prefixes, alphabetical sorting, group-collapse threshold, most-restrictive color resolution, contrast-computed foreground, fallback banner.
- **Attribute editor**: add/remove/reorder attributes, per-value labels and colors, exactly one color-driving attribute.
- **Cross-field lint** (warn-only): one `drivesColor`, unique ids/sources, `multipleLabel` with `groupThreshold`, `bannerLabel` with `showLabel`.
- **Import / copy JSON**, validated against the published v1 schema contract (`https://rocket.chat/schemas/classification-banners/v1.json`).

The configuration document is described by the schema in the Rocket.Chat repository: `ee/packages/abac/docs/classification-banners.schema.json`.
