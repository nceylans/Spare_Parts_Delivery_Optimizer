# Spare Parts Delivery Optimizer

A decision-support tool built during my summer 2026 internship at Toyota's Spare Parts & Supply Chain department in Istanbul.

**[Live demo →](https://nceylans.github.io/Spare_Parts_Delivery_Optimizer/)**

## The problem

Spare parts are shipped from the warehouse to dealers in four daily waves. Vehicles sometimes leave with boxes far from full, which then forces an unnecessary extra vehicle later the same day. The goal of this project was to turn that judgment call into a transparent, data-driven decision.

Along the way, the scope evolved with real operational constraints discovered during the internship:

- Boxes come in a small number of fixed sizes, but whether a box can be stacked depends on what is inside it, not on the box type itself.
- Some parts (glass, tires, long items such as exhausts or shafts) are never boxed at all and are placed directly, taking up their own fixed space on the vehicle.
- Vehicle capacity is really about the footprint boxes occupy, not the volume of the parts inside them.

## What the tool does

1. **Part volume estimation** — 199 Toyota spare part categories, each with an estimated packed volume based on typical shape and a packing-efficiency factor (irregular or fragile items pack less densely than uniform ones).
2. **Box recommendation** — given a set of parts and quantities for one shipment, suggests the smallest suitable box size and its fill rate.
3. **Vehicle departure decision** — aggregates every box (or unboxed item) going out on a wave, compares total footprint against vehicle capacity, and recommends *ship now* / *hold for next wave* / *over capacity*, accounting for a configurable penalty for non-stackable loads.
4. **Route optimization** — clusters dealer stops across a chosen number of vehicles and orders each route with a nearest-neighbor heuristic, starting and ending at the depot (Toyota's Tuzla parts warehouse). Real driving distance and duration are fetched from OSRM where available, with an automatic fallback to straight-line distance.
5. **Session save/load and print** — export the current session as JSON to resume later, or print/export to PDF for reporting.

## Why this matters

The underlying data (part volumes, box capacities, dealer demand) was not accessible during the internship, so the tool is deliberately built to run on **editable estimates** rather than a live system connection. Every assumption is visible and adjustable in the interface, so the same logic can be plugged into real data (AS/400 exports, warehouse measurements) whenever access becomes available — the calculation logic does not need to change, only the inputs.

## Tech

Single self-contained HTML file — no build step, no backend, no API keys. Runs entirely in the browser.

- Vanilla JavaScript, HTML, CSS (no framework)
- [Leaflet](https://leafletjs.com/) + OpenStreetMap tiles for the map
- [OSRM](http://project-osrm.org/) for real driving distance/duration
- [Nominatim](https://nominatim.org/) for depot address geocoding

## Disclaimer

Part volumes, box capacities, and dealer coordinates are estimates for demonstration purposes and should be validated against real warehouse data before being used operationally.
