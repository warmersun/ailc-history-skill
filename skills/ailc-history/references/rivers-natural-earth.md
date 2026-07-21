# River centerlines for history maps (Natural Earth → Wolfram)

Use when rivers are the **argument** of the map (Muscovy/Siberia expansion, river trade spines, “how did they cross X”), not when a local StreetMap zoom already shows water clearly.

## Why this exists

| Approach | Verdict |
|----------|---------|
| `GeoBackground -> "StreetMap"` hydro | Too thin at continent scale |
| `Entity["River", …]` in `GeoGraphics` | Metadata only (length, source/mouth); usually **no** drawable path |
| Hand-made city-hopping `GeoPath` | Acceptable emergency only — inferior to real centerlines |
| **Natural Earth centerlines + thick Wolfram `GeoPath`** | **Preferred** for teaching emphasis |
| Python matplotlib as the final map | **Forbidden** once coords exist — Wolfram draws the deliverable |

## Data source

Public domain **Natural Earth — Rivers + lake centerlines**:

- 10m (detail): `https://naciscdn.org/naturalearth/10m/physical/ne_10m_rivers_lake_centerlines.zip`
- 50m (coarser): `https://naciscdn.org/naturalearth/50m/physical/ne_50m_rivers_lake_centerlines.zip`

Name field: prefer exact `name_en` matches (e.g. `Don` not substring matches like `Dong`).

## Extract steps (local shell; not the final render)

1. Download + unzip the shapefile.
2. Filter features by exact river names needed for the lesson (example set for Russian expansion: Volga, Kama, Ob, Irtysh, Yenisei/Yenisey, Angara, Lena, Amur, Don).
3. Merge multi-part lines; simplify for the map scale (e.g. ~0.08–0.25° depending on range).
4. Export JSON: each river → list of parts; each part → list of `{lat, lon}` (Wolfram `GeoPosition` order).
5. If Wolfram MCP cannot read the host filesystem, embed JSON via `ImportString[json, "RawJSON"]` in the evaluator call (keep simplified so the payload stays small).

## Wolfram draw (required final step)

```wl
rivers = ImportString[json, "RawJSON"];
toPath[part_] := GeoPath[GeoPosition /@ part];
core = {"Volga", "Kama", "Ob", "Irtysh", "Yenisei", "Angara", "Lena"};
coreStyle = Directive[RGBColor[0.05, 0.3, 0.85], AbsoluteThickness[5.5], Opacity[0.95]];
pin = Graphics[{EdgeForm[Black], FaceForm[RGBColor[1., 0.84, 0.]], Disk[]}];

GeoGraphics[
  {
    Flatten @ Table[
      {coreStyle, toPath /@ rivers[name]},
      {name, core}
    ],
    GeoMarker[#, pin, "Scale" -> 0.016] & /@ cities
  },
  GeoRange -> {{45, 73}, {28, 150}},
  GeoBackground -> "ReliefMap",
  ImageSize -> 1152,
  GeoScaleBar -> {"Imperial", "Metric"}
]
```

**Thickness is the pedagogy:** empire maps should make rivers look like **highways**, not hairlines.

## Pitfalls

- Substring name match → wrong rivers (Don/Dong). Use exact `name_en`.
- Blue on brown relief can vanish — use thick stroke and labels with white background.
- Plotting in Python “because we already have shapefiles” — extract OK; **final map always Wolfram**.
- Unsimplified huge paths + dense labels → timeouts; simplify first.
