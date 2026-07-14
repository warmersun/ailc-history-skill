# Wolfram Language recipes for AILC History

Evaluate with the Wolfram Language evaluator. Always resolve names with `\[FreeformPrompt]` or typed `Interpreter` — never invent `Entity[…]` canonical names by hand.

When a plot is returned as a markdown image link, paste that link into the user-facing reply.

**Context pass (typical order):** periods for year → continent/world polities → military-conflict communities (actor graph) → historical-event communities / notables → then focus map or person timeline.

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

## Maps

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
| Ambiguous free-form (many interpretations) | Pick the type you need (`HistoricalCountry` vs `HistoricalPeriod`); state choice to learner |
| Empty battles list | Map the conflict entity alone; use `geoRange = "actors"` |

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
