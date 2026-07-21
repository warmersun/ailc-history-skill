# Wolfram Language recipes for AILC History

Sole home for **all** Wolfram / evaluator rules and recipes used by the parent tutor and by subagents. Visual choice, art, and honesty live in `references/grounding.md`.

When a plot is returned as a markdown image link, paste that link into the reply (parent) or worker summary.

**Context pass (typical order):** periods for year → continent/world polities → military-conflict communities (actor graph) → historical-event communities / notables → then focus map or person timeline.

---

## Evaluator rules

When using `wolfram__WolframLanguageEvaluator` (and related Wolfram tools as needed):

- Always resolve names with `\[FreeformPrompt]["…"]` or `Interpreter["HistoricalCountry" | "Person" | "MilitaryConflict" | "HistoricalPeriod"][…]`. **Never** hand-write `Entity["HistoricalCountry", "RomanEmpire"]` style canonical names.
- Prefer returning the plot expression itself (`GeoGraphics`, `GeoListPlot`, `TimelinePlot`, …) so results can surface as markdown image links.
- On entity failure: free-form re-resolve → `GeoIdentify` for polities on modern territory → retry with `CommonName` / resolved entity (see **Entity name failure loop** below).
- Raise `timeConstraint` (e.g. 90–120) for dense continent/world maps or multi-entity plots.
- **Image size:** default `ImageSize -> Large` (~576). On “2× larger”, use `ImageSize -> 1152`. If a dense geo plot times out at 1152, keep the **same** plot at `Large` and enlarge with `ImageResize[Rasterize[map, "Image"], Scaled[2]]` — same content, doubled pixels. Do not redesign the map when only size was requested. Details: **Image size** section.
- Year strings: `"1700"`, `"60 AD"`, `"-500"`, `"500 BC"`. Parse with `interpreterDate` (below). If exact-year polygons are missing, fall back and **disclose** what year the map actually shows.
- **Place markers:** always `GeoMarker` (prefer `GeoMarker[loc, Graphics[{… Disk[] …}], "Scale" -> …]`). **Never** `GeoDisk` for cities (that is a ground-area radius). **Never** leave default ugly yellow pin/triangles on finished maps. Details: **Place markers** section.
- **Rivers as the teaching argument** (empire expansion, Urals/Siberia corridors): basemap hydro is too thin on continent scale; Wolfram `River` entities lack drawable centerlines. Pull **Natural Earth** river centerlines, then draw thick `GeoPath`s **in Wolfram** (not a Python final plot). Details: **Rivers** section + `references/rivers-natural-earth.md`.

### Entity name failure loop

1. `\[FreeformPrompt]` / typed `Interpreter`.
2. `GeoIdentify[Dated["HistoricalCountry", year], modernCountry]` for “who was here then?”.
3. Wars via `MilitaryConflict` + `MainActors` for a polity and year.
4. Retry the visual with resolved names; disclose entity choice + year when it differs from the ask.

### Entity types

| Type | Use for |
|------|---------|
| `HistoricalCountry` | Kingdoms, empires, polities with dated borders |
| `MilitaryConflict` | Wars, battles, revolutions; actor graphs |
| `HistoricalPeriod` | Eras, dynasties, cultural ages |
| `Person` | Notable people, lifespans, linked events |
| `HistoricalEvent` | Events, inventions, cultural milestones |
| `Country` / `City` | Present-day places on combined maps |
| `GeographicRegion` | Continents: Europe, Asia, Africa, NorthAmerica, SouthAmerica, Australia |

---

## Shared helpers

Paste these once at the top of a multi-step evaluation when useful.

```wl
(* Year string -> DateObject. Accepts "1700", "-500", "500 BC", "60 AD" *)
interpreterDate[d_String] := Module[{int},
  int = Interpreter["Integer"][d];
  If[IntegerQ[int],
    If[int < 0,
      Interpreter["Date"][IntegerString[Abs[int]] <> " BC"],
      Interpreter["Date"][IntegerString[Abs[int]] <> " AD"]
    ],
    Interpreter["Date"][d]
  ]
]

entityValueStartDate[e_Entity] :=
  If[MissingQ[EntityValue[e, "StartDate"]], InfinitePast, EntityValue[e, "StartDate"]]

entityValueEndDate[e_Entity] :=
  If[MissingQ[EntityValue[e, "EndDate"]], InfiniteFuture, EntityValue[e, "EndDate"]]

(* Historical country from free text *)
toHistoricalCountry[s_String] := Module[{hc},
  hc = Interpreter["HistoricalCountry"][s];
  If[FailureQ[hc], \[FreeformPrompt][s, Entity], hc]
]

(* Military conflict from free text *)
toMilitaryConflict[s_String] := Module[{mc, ff},
  mc = Quiet @ Interpreter["MilitaryConflict"][s];
  If[FailureQ[mc] || MissingQ[mc],
    ff = \[FreeformPrompt][s];
    If[Head[ff] === Entity && EntityTypeName[ff] === "MilitaryConflict", ff, Missing["Unknown"]],
    mc
  ]
]
```

---

## Lookups (grounding data)

### Historical countries on modern territory in a year

```wl
year = interpreterDate["200 BC"]; (* or DateObject[{-200},"Year"] *)
country = \[FreeformPrompt]["Romania", Entity];
CanonicalName /@
  (First /@ GeoIdentify[Dated["HistoricalCountry", DateObject[year, "Year"]], country])
```

### Historical events near a year (graph communities)

```wl
yearDate = interpreterDate["1545"];
yearsBack = 5;
events = Normal @ EntityValue[
  EntityClass["HistoricalEvent", {
    "StartDate" -> LessEqualThan[DateObject[yearDate, "Year"]],
    "EndDate" -> GreaterEqualThan[DatePlus[yearDate, Quantity[-yearsBack, "Years"]]]
  }],
  "CountriesInvolved", "NonMissingEntityAssociation"
];
FindGraphCommunities @ Graph @ Flatten @
  Table[Rule[#, Keys[events][[i]]] & /@ Values[events][[i]], {i, Length[events]}] /.
    Entity[type_, name_] :> EntityValue[Entity[type, name], "Name"]
```

### Wars / revolutions near a year (graph communities)

```wl
yearDate = interpreterDate["1917"];
yearsBack = 1;
filter[type_] := FilteredEntityClass[
  EntityClass["MilitaryConflict", {"Type", type}],
  EntityFunction[w,
    DateObject[w["StartDate"], "Year"] <= DateObject[yearDate, "Year"] &&
    DateObject[w["EndDate"], "Year"] >= DatePlus[yearDate, Quantity[-yearsBack, "Years"]]
  ]
];
wars = Normal @ Flatten /@ Join[
  EntityValue[filter["war"], "MainActors", "NonMissingEntityAssociation"],
  EntityValue[filter["revolution"], "MainActors", "NonMissingEntityAssociation"]
];
FindGraphCommunities @ Graph @ Flatten @
  Table[Rule[#, Keys[wars][[i]]] & /@ Values[wars][[i]], {i, Length[wars]}] /.
    Entity[type_, name_] :> EntityValue[Entity[type, name], "Name"]
```

### Wars of a historical country in a year

```wl
hc = toHistoricalCountry["Ottoman Empire"];
yearDate = interpreterDate["1526"];
Select[
  EntityValue[
    EntityClass["MilitaryConflict", {"StartDate" -> yearDate}],
    {"CanonicalName", "StartDate", "EndDate", "MainActors"},
    "PropertyAssociation"
  ],
  MemberQ[Flatten @ Lookup[#, "MainActors"], hc] &
]
```

### Person facts + related events

```wl
p = \[FreeformPrompt]["Cleopatra"];
events = EntityList[EntityClass["HistoricalEvent", "PeopleInvolved" -> p]];
{
  CommonName[p],
  EntityValue[p, {"BirthDate", "DeathDate", "NotableFacts"}, "NonMissingPropertyAssociation"],
  EntityValue[#, {"Name", "Date", "StartDate", "EndDate"}, "NonMissingPropertyAssociation"] & /@ events
}
```

---

## Image size (legibility)

| Request | Setting |
|---------|---------|
| Default | `ImageSize -> Large` (~576) |
| “2× larger” / hard to read | `ImageSize -> 1152` (exactly 2× Large) |
| Dense continent map **times out** at 1152 | Same plot at `Large`, then `ImageResize[Rasterize[map, "Image"], Scaled[2]]` — identical content, doubled pixels |
| Learner said “exact same visual, just larger” | Change **only** size (or resize); do not redesign |

---

## Maps

### Place markers (do this, not GeoDisk)

`GeoDisk` is a **ground-area** primitive (radius on Earth), **not** a city pin.

Use **`GeoMarker`**:

```wl
(* default pin *)
GeoMarker[Entity["City", {"Moscow", "Moscow", "Russia"}]]

(* preferred: custom marker graphics — clean disk, not default triangle pin *)
pin = Graphics[{EdgeForm[Black], FaceForm[RGBColor[1., 0.84, 0.]], Disk[]}];
GeoMarker[Entity["City", {"Moscow", "Moscow", "Russia"}], pin, "Scale" -> 0.02]
```

Also fine: `Point[GeoPosition[city]]` with `PointSize`, or `GeoListPlot` + `Labeled` / `GeoLabels`.  
Do **not** use `GeoDisk[city, Quantity[…]]` to mark towns.

### Rivers on history maps (Natural Earth → Wolfram)

**Problem:** At continent scale, basemap hydrography is too thin to teach with. Wolfram `Entity["River", …]` is **metadata only** (length, source, mouth) — **no drawable centerline**, so stroking a River entity in `GeoGraphics` fails or is useless for emphasis.

**Solution (proven):** download external river **centerlines**, simplify to lat/lon JSON, then **draw and style in Wolfram** with thick `GeoPath`. Keep extraction local; **final map must be Wolfram** (not matplotlib) for AILC sessions.

Full checklist: `references/rivers-natural-earth.md`.

#### 1) Download Natural Earth (public domain)

```bash
curl -fsSL -o ne_10m_rivers.zip \
  "https://naciscdn.org/naturalearth/10m/physical/ne_10m_rivers_lake_centerlines.zip"
unzip -o ne_10m_rivers.zip -d ne10m
# → ne10m/ne_10m_rivers_lake_centerlines.shp
```

Use field **`name_en`** (fallback `name`). Prefer **exact** name matches (`Volga`, `Ob`, `Yenisey`/`Yenisei`, `Lena`, `Kama`, `Irtysh`, …) to avoid false hits (e.g. Don vs Dong).

#### 2) Extract + simplify (local pyshp + shapely)

- Merge MultiLineString parts per river (`unary_union` / `linemerge`).
- `simplify` ~0.08–0.25° for continent frames.
- Export each part as `[[lat, lon], …]` (Wolfram `GeoPosition` order).
- Keep payload small enough to `ImportString[…, "RawJSON"]` in the MCP evaluator (~few KB for major arteries).

#### 3) Draw in Wolfram MCP

```wl
rivers = ImportString[jsonString, "RawJSON"]; (* name -> list of parts *)
toPath[part_] := GeoPath[GeoPosition /@ part];
pin = Graphics[{EdgeForm[Black], FaceForm[RGBColor[1., 0.84, 0.]], Disk[]}];

core = {"Volga", "Kama", "Ob", "Irtysh", "Yenisei", "Angara", "Lena"};
corePrims = Flatten @ Table[
  {Directive[RGBColor[0.05, 0.3, 0.85], AbsoluteThickness[5.5], Opacity[0.95]],
   toPath /@ rivers[name]},
  {name, core}
];

hc = \[FreeformPrompt]["Tsardom of Russia", Entity];
poly = EntityValue[hc, Dated["Polygon", DateObject[{1650}, "Year"]]];

GeoGraphics[
  {
    If[! MissingQ[poly],
      {FaceForm[Directive[RGBColor[0.85, 0.15, 0.12], Opacity[0.14]]],
       EdgeForm[Directive[RGBColor[0.5, 0.08, 0.08], Thickness[0.003]]], poly},
      Nothing],
    corePrims,
    GeoMarker[#, pin, "Scale" -> 0.016] & /@ cities
  },
  GeoRange -> {{45, 73}, {28, 150}},
  GeoBackground -> "ReliefMap",
  ImageSize -> 1152,
  GeoScaleBar -> {"Imperial", "Metric"}
]
```

| Need | Setting |
|------|---------|
| Emphasize arteries | `AbsoluteThickness` ~5–7 (core); thinner secondary |
| Continent legibility | NE centerlines + thick stroke (not basemap alone) |
| City pins | `GeoMarker[loc, Graphics[Disk[…]]]` |
| Polity underlay | dated `HistoricalCountry` polygon, low opacity |
| Relief + rivers | `GeoBackground -> "ReliefMap"` + thick `GeoPath` |

**Anti-patterns:** `GeoDisk` as city pin; default ugly pin without custom `Graphics`; StreetMap hydro alone for “show the Volga” at Eurasia scale; final map in Python when session is Wolfram/AILC; inventing source→mouth straight lines when centerline data exists.

### Historical country in a year

```wl
hc = toHistoricalCountry["Roman Empire"]; (* or \[FreeformPrompt]["Roman Empire"] *)
yearDate = DateObject[{100}, "Year"];
poly = EntityValue[hc, Dated["Polygon", yearDate]];
If[MissingQ[poly],
  (* fallback: union area neighbors for that year *)
  With[{range = GeoBoundsRegion[GeoVariant[hc, "UnionArea"]]},
    GeoListPlot[
      Partition[
        Labeled[Dated[First[#], yearDate], CommonName[First[#]]] & /@
          GeoEntities[range, Dated["HistoricalCountry", yearDate]],
        1
      ],
      GeoBackground -> "CountryBorders", ImageSize -> Large,
      PlotLabel -> "Area of " <> CommonName[hc] <> " around " <> DateString[yearDate, "Year"]
    ]
  ],
  GeoGraphics[
    {FaceForm[Red], EdgeForm[{Red, Thick}], poly},
    GeoBackground -> {"Coastlines", "CountryBorders"},
    PlotLabel -> "Historical map of " <> CommonName[hc] <> " in " <> DateString[yearDate, "Year"],
    ImageSize -> Large, GeoScaleBar -> {"Imperial", "Metric"}
  ]
]
```

`geoRange` variants: set `GeoRange -> Automatic` (tight), continent(s) from `EntityValue[hc["CurrentCountries"], "Continent"]`, or `"World"`.

### Continent in a year

```wl
yearDate = interpreterDate["1230"];
continent = Entity["GeographicRegion", "Europe"]; (* Asia, Africa, NorthAmerica, SouthAmerica, Australia *)
hcs = EntityValue[
  GeoEntities[GeoBoundsRegion[continent], Dated["HistoricalCountry", yearDate]],
  {"Name", Dated["Polygon", yearDate]}, "PropertyAssociation"
];
GeoListPlot[
  Partition[
    Labeled[Lookup[#, Dated["Polygon", yearDate]], Lookup[#, "Name"]] & /@ hcs,
    1
  ],
  GeoRange -> continent,
  GeoBackground -> Dated["CountryBorders", yearDate],
  GeoLabels -> True, LabelStyle -> Tiny, ImageSize -> Large,
  GeoScaleBar -> {"Imperial", "Metric"}
]
```

### War / battle map

```wl
mc = toMilitaryConflict["Battle of Waterloo"]; (* or \[FreeformPrompt]["World War I"] *)
data = EntityValue[mc, {"Battles", "Subconflicts", "MainActors"}, "PropertyAssociation"];
battles = data["Battles"];
mainActors = Cases[Flatten @ data["MainActors"], _Entity];
geoRange = "actors"; (* "battles" | "actors" | "world" *)
Show[
  GeoListPlot[
    If[geoRange === "battles", {}, Partition[mainActors, 1]],
    GeoBackground -> Switch[geoRange,
      "battles", {"ReliefMap", "StreetMapLabelsOnly"},
      "actors", "Coastlines",
      "world", "Coastlines"
    ],
    GeoLabels -> True, PlotLegends -> CommonName /@ mainActors, ImageSize -> Large
  ],
  GeoListPlot[
    If[MissingQ[battles], {mc}, battles],
    GeoBackground -> None,
    PlotMarkers -> GeoMarker,
    PlotLabel -> "Map of " <> CommonName[mc],
    GeoScaleBar -> {"Imperial", "Metric"}
  ],
  GeoRange -> If[geoRange === "world", "World", Automatic]
]
```

### Combined map (minimal inputs)

Require `year` whenever historical countries are listed.

```wl
yearDate = interpreterDate["200 AD"];
hcs = DeleteMissing[toHistoricalCountry /@ {"Roman Empire"}];
wars = DeleteMissing[toMilitaryConflict /@ {"Battle of Waterloo"}];
places = Join[
  \[FreeformPrompt] /@ {"Hungary", "Romania"},
  \[FreeformPrompt] /@ {"Budapest"}
];
hcPrims = Labeled[
  Replace[EntityValue[#, Dated["Polygon", yearDate]], _Missing :> EntityValue[#, "Polygon"]],
  EntityValue[#, "Name"]
] & /@ hcs;
battlePrims = Labeled[GeoMarker[#], EntityValue[#, "Name"]] & /@
  Select[Flatten @ Join[wars, DeleteMissing[EntityValue[#, "Battles"] & /@ wars]],
    ! MissingQ[EntityValue[#, "Position"]] &];
GeoListPlot[
  {hcPrims, battlePrims, Labeled[#, CommonName[#]] & /@ places},
  LabelStyle -> Tiny, ImageSize -> Large, GeoBackground -> "CountryBorders",
  PlotLegends -> {"Historical countries", "Military conflicts", "Places"},
  GeoScaleBar -> {"Imperial", "Metric"}
]
```

---

## Timelines

### Periods active in a year (+ optional people list)

```wl
yearDate = interpreterDate["1700"];
periods = EntityList @ EntityClass["HistoricalPeriod", {
  "StartDate" -> LessEqualThan[DateObject[yearDate, "Year"]],
  "EndDate" -> GreaterEqualThan[DateObject[yearDate, "Year"]]
}];
TimelinePlot[
  {periods, {Labeled[DateObject[yearDate, "Year"], DateString[yearDate, "Year"]]}},
  PlotTheme -> "Web",
  PlotLabel -> "Historical periods for " <> DateString[yearDate, "Year"],
  ImageSize -> Large
]
```

### Person timeline

```wl
p = \[FreeformPrompt]["Lenin"];
events = EntityList[EntityClass["HistoricalEvent", "PeopleInvolved" -> p]];
periods = EntityList[EntityClass["HistoricalPeriod", "PeopleInvolved" -> p]];
bdate = EntityValue[p, "BirthDate"]; ddate = EntityValue[p, "DeathDate"];
TimelinePlot[
  {
    events,
    periods,
    {Labeled[DateInterval[{bdate, ddate}],
      DateString[bdate, {"YearUnsigned", " ", "ADBC"}] <> " – " <>
      DateString[ddate, {"YearUnsigned", " ", "ADBC"}]]}
  },
  PlotTheme -> "Web", PlotLayout -> "Packed",
  PlotLabel -> CommonName[p] <> " timeline",
  ImageSize -> Large
]
```

### Combined timeline

```wl
periods = DeleteCases[\[FreeformPrompt] /@ {"Victorian era", "Industrial Revolution"}, _?FailureQ];
hcs = DeleteMissing[toHistoricalCountry /@ {"British Empire"}];
wars = DeleteMissing[toMilitaryConflict /@ {"Crimean War"}];
people = DeleteCases[\[FreeformPrompt] /@ {"Queen Victoria", "Charles Darwin"}, _?FailureQ];
hcBars = Labeled[
  DateInterval[{
    EntityValue[#, "StartDate"],
    If[MissingQ[EntityValue[#, "EndDate"]], Now, EntityValue[#, "EndDate"]]
  }],
  EntityValue[#, "Name"]
] & /@ hcs;
personBars = Labeled[
  DateInterval[{
    EntityValue[#, "BirthDate"],
    If[MissingQ[EntityValue[#, "DeathDate"]], Now, EntityValue[#, "DeathDate"]]
  }],
  EntityValue[#, "Name"]
] & /@ people;
TimelinePlot[
  {periods, hcBars, wars, personBars},
  PlotTheme -> "Web", PlotLayout -> "Stacked", Filling -> Axis,
  PlotLegends -> {"Periods", "Historical countries", "Military conflicts", "Persons"},
  ImageSize -> Large
]
```

---

## Quick Alpha checks

Use `wolfram__WolframAlpha` for one-shot questions, e.g.:

- `Roman Empire start and end dates`
- `Battle of Mohács participants`
- `notable people born in 1700`

Prefer the evaluator when you need an image or multi-entity computation.

---

## Failure patterns

| Symptom | Fix |
|---------|-----|
| `Missing["UnknownEntity"]` / invalid name | Re-resolve with `\[FreeformPrompt]`; try alternate spellings; use `GeoIdentify` |
| `Missing` polygon for year | Fall back to union-area neighbors; disclose the fallback |
| Timeout | Narrow entities; lower image complexity; raise `timeConstraint` |
| Timeout on dense continent / huge `ImageSize` | Drop labels; simpler single-polity `GeoGraphics`; raise `timeConstraint`; or `Large` then `ImageResize[…, Scaled[2]]` |
| Ambiguous free-form (many interpretations) | Pick the type you need (`HistoricalCountry` vs `HistoricalPeriod`); state choice to learner |
| Empty battles list | Map the conflict entity alone; use `geoRange = "actors"` |
| River entity draws nothing / `missloc` | River entities lack path geometry — Natural Earth centerlines → thick `GeoPath` (rivers section) |
| Default yellow triangle markers | `GeoMarker[loc, Graphics[{EdgeForm[Black], FaceForm[…], Disk[]}], "Scale" -> …]` |
| Used `GeoDisk` for a city pin | Wrong primitive — switch to `GeoMarker` |
| Final river map plotted in Python | Extract SHP/JSON externally OK; **always render final map in Wolfram MCP** |
| Continent map with invisible rivers | Thick `AbsoluteThickness` on `GeoPath`; do not rely on basemap hydro alone |

---

## Context-pass bundle (sketch)

Run as separate evaluations if one cell is too heavy:

```wl
yearDate = interpreterDate["100"]; (* pin the year *)

(* 1. Era *)
EntityValue[
  EntityList @ EntityClass["HistoricalPeriod", {
    "StartDate" -> LessEqualThan[DateObject[yearDate, "Year"]],
    "EndDate" -> GreaterEqualThan[DateObject[yearDate, "Year"]]
  }],
  "Name"
]

(* 2. Continent polities — then map with continent recipe *)
(* 3. Conflict communities — military conflicts recipe above *)
(* 4. Event communities — historical events recipe above *)
(* 5. Optional: notable people from periods' PeopleInvolved filtered to year *)
```
