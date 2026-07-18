# Visual grounding, art, and honesty

Shared by the **parent tutor** and **subagent workers**. Wolfram evaluator rules and recipes: `references/wolfram-recipes.md`. Learner reply shape (parent only): `references/reply-format.md`.

## Visual choice matrix

| Intent | Prefer |
|--------|--------|
| World/continent geopolitics at a year | Continent or world historical map; list major polities |
| One empire/kingdom in a year | Historical country map (optionally after continent context) |
| War / battle | War map (`geoRange`: battles / actors / world) + actor graph summary; AI scene only as optional atmosphere |
| Conflict landscape at a year | Military-conflict communities + actors map or legend |
| Era + culture | Periods timeline; events (art, inventions); person roster; period art or AI reconstruction |
| One person | Person timeline; portrait via period art if possible, else careful AI reconstruction |
| Compare several threads | Combined timeline (minimal inputs) |
| Overlay empires + battles + modern places | Combined map (require year for historical countries) |
| Mood / daily life / material culture | Period art/coin first; else detailed AI image (not a map) |
| Same visual, larger image | `ImageSize -> 1152` or `ImageResize[…, Scaled[2]]` after successful `Large` render (see wolfram-recipes) |
| River highways / expansion corridors at continent scale | Natural Earth centerlines → thick `GeoPath` in Wolfram + relief (`references/rivers-natural-earth.md`) |
| Towns / forts on any geo plot | Custom `GeoMarker[…, Graphics[Disk[]]]` — never `GeoDisk`, never default triangle pins |

## Illustrating story elements

When a teaching turn includes a **story** element, it **MUST** be illustrated with a concrete image of that moment, person, or scene. This is separate from Wolfram maps/timelines.

**Who does the work:** finding or generating the image **MUST** be done by a **subagent** (see `SKILL.md` orchestration / `references/worker-brief.md`). The parent only briefs the scene and synthesizes the returned image into the learner reply — it does not run image search or AI image generation itself.

Worker paths:

1. **Find** a suitable image online when a good one exists — period fine art, museum photo, coin, sculpture, etc. Resolve a real URL; never invent Wikimedia or other CDN paths.
2. **Generate** with AI image generation when a fitting real image is unavailable or weak — the more common path for specific story moments.

Caption every image (what / when / how it ties to the story). Label AI outputs as **modern reconstruction**. **MUST NOT** use a story illustration instead of a Wolfram map or timeline for geography or chronology.

## Art and material culture

- Prefer **period-contemporary** fine art: painting, sculpture, architecture, illuminated work, **coins and medals** (excellent for rulers, propaganda, iconography).
- Search for images (web / Wolfram / available image tools) with queries that include the era, artist, or “coin of …”, “denarius”, “solidus”, etc.
- Caption what the object is, roughly when it was made, and how it relates to the topic.
- Prefer **real photographs / museum images** for standing buildings and period objects; never invent Wikimedia or other CDN paths (resolve a real URL first). Details for photo delivery: `references/reply-format.md` (parent) — workers return verified URLs or note that a photo search failed.

## AI image generation

Built-in AI image generator (e.g. `image_gen` / Hermes equivalent) for illustration on the fly. Default way to fulfill the story-illustration **MUST** when no good period/photo image exists.

### When to use

- **Story scenes** — the moment, people, or action in the narrative hook (primary use).
- Scenes of daily life, battles *as impression*, ceremonies, cityscapes, dress, ships, workshops.
- Portraits or group scenes when you lack a good contemporary artwork (label as modern reconstruction).
- Material culture when a coin/photo search fails.

### When not to use

- **Not in place of Wolfram maps** — borders, theaters of war, continent/world geopolitics, battle locations, and timelines stay computational.
- Not as fake “primary evidence” or unlabeled period photography.

### Prompt craft (historical accuracy first)

Write **detailed** prompts; aim for accuracy as far as the model allows:

- **Time and place** — exact century/year range, region, polity (e.g. “Rome, Trajanic period, c. 100 AD”).
- **People** — status, ethnicity as known, age band, hairstyle/beard conventions of the culture, rank insignia if relevant.
- **Dress and kit** — period clothing, armor types, weapons, textiles, dyes — avoid generic “medieval fantasy” or anachronistic plate/guns.
- **All artifacts and stuff in frame** — every object that appears (furniture, vessels, tools, ships, vehicles, writing media, standards, coins, foodware, harness, scaffolding, etc.) **MUST** be historically accurate for the stated time and place; no anachronistic props, no generic “old-looking” filler.
- **Architecture and landscape** — known building types, materials (mudbrick, opus caementicium, timber), climate, vegetation.
- **Style** — “historically informed reconstruction,” documentary or museum-diorama realism unless a specific art style was requested; avoid modern logos, neon, or steampunk.
- **Composition** — subject, action, lighting, viewpoint in a few clear sentences.
- State **what to avoid** only when needed (e.g. “no modern clothing, no fantasy dragons”).

Caption every generated image: **modern AI reconstruction**, intended date/place, and that details may be approximate. Prefer real period art/coins when you have them.

## Web encyclopedia lookup

When you need a secondary encyclopedia / background web page (beyond Wolfram):

- Search and cite **[Grokipedia](https://grokipedia.com)** (`site:grokipedia.com` …).
- **Do not** use Wikipedia (en.wikipedia.org or mirrors) for background or secondary synthesis.
- Primary sources, Wolfram, and museum/photo hosts remain fine; this rule is about encyclopedia search, not dated maps.

## Honesty for visuals and quotes

- Missing Wolfram polygon/entity → say so; offer nearby year, broader range, or text. Disclose what year/entity a map actually shows when it differs from the ask.
- Do **not** invent map image URLs, Wikimedia paths, or primary-source quotations from memory.
- Label myth vs record; note uncertainty on contested claims.
- AI illustrations are modern impressions — never substitutes for dated maps or unlabeled “period photos.”
