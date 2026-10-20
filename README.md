# Arc.tech: Interactive Computing Lab

A static website. There is no build step, no backend and no API key. Everything is in `index.html`.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole site: styles, pages, catalogue data, 3D models, simulations and quiz |
| `tests/smoke-test.js` | Node smoke test (see "Checks" below) |
| `README.md` | This file |

## Run it locally

Open `index.html` in a browser, or serve the folder:

```
python3 -m http.server 8080
```

then visit http://localhost:8080.

## Deploy (any static host)

Upload the folder (or just `index.html`) to one of these:

- **Netlify**: drag the folder onto https://app.netlify.com/drop
- **Cloudflare Pages / Vercel / Render**: create a static project, no build command, output directory `.`
- **GitHub Pages**: put `index.html` in a repository, then Settings, Pages, deploy from the main branch
- **Any web server**: copy `index.html` into the web root

## What the site needs

- A modern browser with WebGL for the 3D models.
- Internet access for two pinned scripts from jsDelivr (Three.js 0.128.0 and its OrbitControls). If they cannot load, the library, detail pages, simulations and quiz still work, but the 3D viewer shows a message instead.
- To self-host those two scripts, download them and change the two `<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/...">` lines near the end of `index.html` to your own copies:
  - `https://cdn.jsdelivr.net/npm/three@0.128.0/build/three.min.js`
  - `https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js`
- Progress and theme are stored in the browser (`localStorage`) and do not sync between devices.

## Where things are in `index.html`

The page script is split into several `<script>` blocks so an error in one does not stop the others. There are 11 blocks, in this order (the cache simulation and the quiz are two separate blocks listed together as item 2):

1. Core: theme, 3D viewer, assembly controls, RAM and CPU models, home page
2. Cache hit/miss simulation, then the practice quiz
3. CPU instruction-cycle simulation
4. Catalogue: category list, source list, entries (`CAT`), search index, library and detail views
5. Virtual-machine simulation, relationship map, library health
6. Model registry, first set of 3D models, lab detail pages, audit table
7. Keyboard and mouse models, more entries, filters, keyboard scan simulation
8. Hardware and security-tool models
9. Category taxonomy, concept entries, number converter, category introductions
10. SSD and fan models, status tags, and a small helper that scrolls to the top on view change

### Add a catalogue entry

Add a call like this to the `NEW` array in block 7 or to the `[...]` list in block 9, using an unused `id` and an existing category letter from `CATS`:

```js
E2('id','Name','alias one|alias two','D','Type','What it is.','How it works.','related ids',{f:'Troubleshooting.',s:'sourceKey'})
```

Related ids must exist. Add a source to `SRC` only if you have a real URL.

### Add a 3D model

Create a model with the same `id` as its entry. Each part is `B(id,name,kind,colour,[w,h,d],[x,y,z],explodeOffset,simpleText,technicalText,connections,options)`. Use `0` as the explode offset for the base part. Options include `n2`/`g` for repeated parts, `cy` for cylinders, `ring`/`rr` for parts around a circle and `shell:1` for translucent enclosures.

## Checks

```
node tests/smoke-test.js index.html
```

This loads every script block against a stubbed page and checks searches, entry pages, categories, the simulations, the quiz, the converter and the 12 main 3D model definitions. It is not a browser test, so it cannot check how models look or how touch controls feel. Check those in a real browser.

## Content notes

- Catalogue entries and 3D models are educational approximations written for this project. Entries are unreviewed and links go to publishers' main or documentation sites, not to page-level citations.
- Entries with no 3D model or simulation yet are marked "Planned" in the Library.

A product by beest.silo.
