# GeoSketch

GeoSketch is a single-file browser tool for quick OSINT map sketching, simple GIS-style visualisation, and lightweight map annotation. It is built around `index.html` and is intended to run without installing a desktop GIS package or setting up a project server.

Open the HTML file in a modern browser, choose a background map, add objects, save the project as GeoJSON, and reload it later from the same file format.

Live version: [https://jmkorhonen.github.io/geosketch/](https://jmkorhonen.github.io/geosketch/)

![GeoSketch screenshot](samples/GeoSketch_v1_25_screenshot.png)


## Current Status

GeoSketch is a lightweight browser tool for field-style sketching and visual analysis. It should be treated as a planning and OSINT visualisation aid rather than a survey-grade GIS package.

Current release version: `v1.2`

Current development build: `v1.2.19`

## Requirements

- A modern desktop browser, preferably current Firefox, Chromium, Chrome, or Edge.
- Internet access for map tiles, geocoding search, and the CDN-hosted JavaScript libraries used by the standalone HTML file.
- No local install step is required.

The app stores autosave data in browser `localStorage`, so autosaves are local to the browser and profile where the file is opened.

## Quick Start

1. Open `index.html` in your browser.
2. Set a map title at the top-left. The title is also used as the default export filename. Add map notes for author, source, or caveat details if needed.
3. Use **Map Search** to find a place, address, or coordinate.
4. Use **Add Object** to create points, units, lines, polygons, circles, or text boxes. Use **Files > Define export area** when you want a PNG crop guide.
5. Select an object on the map or in the Layers list to edit its details in the right pane.
6. Save the project with **Save project as GeoJSON**.
7. Use **Load project from GeoJSON** to continue later.

The **Load Sample** button creates a small example map that demonstrates common object types.

The same demo is also available as `samples/geosketch-sample.geojson` for testing project import/export without using the built-in button.

If no map title is set, GeoSketch uses `untitled_geosketch` as the default export filename and asks for a filename when saving project-level exports.

## Main Concepts

### Background Map

GeoSketch supports selectable background layers with opacity and stack-order controls. Layers higher in the list draw above layers below them, so a street or topographic layer can be made translucent over satellite imagery.

- OpenStreetMap/CARTO raster tiles
- Vector overlays (OpenFreeMap) through MapLibre GL
- OpenTopoMap
- Esri satellite imagery

Tile availability depends on the provider, network conditions, browser restrictions, and the provider's access policies.

### Filter Vector Overlays

GeoSketch can filter OpenFreeMap vector map detail into vector overlay groups. This is most useful when **Vector overlays (OpenFreeMap)** is enabled and placed as a translucent overlay above satellite imagery or a raster background map.

Initial reference groups include:

- base land detail
- water
- roads
- rail
- places
- borders
- land chokepoints such as bridges, tunnels, crossings, and fords
- airports and ports
- other map detail

The controls include quick presets for the full vector map, roads and places, transport-focused overlays, and clearing all overlay selections. Filter rows show a compact legend for the current render style, are saved in GeoJSON projects, and are carried into interactive HTML exports.

Filter Vector Overlays also includes optional highlighting for railways, land chokepoints, airports, and ports, plus map backdrop colour and vector land-opacity controls to make vector detail more legible over satellite imagery or dark map canvases. Vector overlays are live/HTML-export only and do not render in PNG exports.

### Layers

Objects live inside drawing layers. Layers can be:

- selected as the active drawing layer
- renamed directly from the layer name field
- shown or hidden
- collapsed
- reordered
- deleted with confirmation

Use layers to separate hypotheses, source types, time periods, units, or alternative interpretations.

### Objects

GeoSketch supports:

- **Point**: simple point markers with configurable marker shape, colour, opacity, and size.
- **Unit**: APP-6 style military symbols generated from SIDC fields using `milsymbol`.
- **Line**: routes, axes, boundaries, and other linear features.
- **Polygon**: areas of interest, zones, and bounded regions.
- **Circle**: range circles and circular areas.
- **Text box**: free map annotations.
- **Export area**: rectangular PNG crop guides with editable coordinates, labels, notes, and line/fill styling.
- **Image**: imported image overlays stored as layer objects, with editable bounds and an initial 3-point affine georeferencing workflow for digitising maps or imagery.

Objects can have labels, notes, measurements, styles, optional object shadows, buffers, visibility toggles, and layer assignment. New line, polygon, and circle drawing can snap to existing object points. Locked objects can still be copied or duplicated, but are protected from accidental editing.

### Map Title and Notes

The map title appears at the top-left and is used as the default export filename. It can also be shown as a styled title overlay on the map.

Project-level map notes sit under the map title. Use them for author details, sources, caveats, or links. They are saved with the project and included in interactive HTML exports; plain web and email addresses in those notes are made clickable in the exported map.

## Coordinates

GeoSketch accepts and displays multiple coordinate styles:

- decimal degrees
- DMS-style latitude/longitude
- MGRS

Coordinate handling is controlled in the Settings panel. Search, Create from coordinates, and Edit coordinates are designed to accept any recognised coordinate format. Export areas show two opposite corners in the coordinate editor; applying those coordinates always produces a rectangle.

The bottom status bar shows live cursor coordinates. `Shift+C` copies the cursor position in the currently selected coordinate format.

Distance labels and measurements use the Settings panel distance unit. The default auto-metric mode switches between metres and kilometres; metres, kilometres, miles, and nautical miles can also be forced.

Settings can also show horizontal and vertical scale bars. Each scale bar has its own position, X/Y offsets, and division count, and the bars follow the selected display distance unit while remaining planning-grade rather than survey-grade.

The reset control in Settings restores display/export settings and new-object drawing defaults without deleting existing map objects or layers.

## Drawing and Editing

Use the Add Object buttons or keyboard shortcuts:

- `1`: point
- `2`: line
- `3`: polygon
- `4`: circle
- `5`: text box
- `6`: unit
- `S`: focus search
- `E`: edit points/shapes on selected line, polygon, circle, export area, or image
- `M`: move selected object
- `Enter`: finish the current drawing
- `Esc`: cancel coordinate editing, coordinate/search input, control-point picking, drawing, edit, or move modes
- `Delete`: delete selected object, with confirmation
- `Ctrl+Z`: undo
- `Ctrl+S`: save project as GeoJSON
- `Ctrl+C`: copy selected object
- `Ctrl+V`: paste object at cursor
- `C`: copy selected object coordinates
- `Shift+C`: copy cursor coordinates

When drawing lines and polygons, dynamic measurements are shown while drawing. Polygon measurements include area, perimeter, and individual segment lengths.

Shape styling separates line color, fill color, opacity, line width, line dash, and optional fill patterns. Fill opacity `0` is treated as no fill, and newly drawn lines default to no fill. Buffer styling uses the same line/fill pattern controls, but remains attached to the parent object.

Edit points mode lets you adjust line, polygon, circle, export-area, and image geometry. For lines and polygons, click a point to select it, click a segment to add a point, use plus handles or Add Start/End Point to extend lines, and use Delete Point, Del, or Backspace to delete the selected point. For image objects, drag the corner handles to resize the image bounds, the centre handle to move the image, or the rotate handle to rotate it; image resizing can preserve the original proportions.

Export areas are rectangular guides for PNG crops. They are managed from the Files section, then edited like other shapes after creation. A format selector can constrain newly drawn export areas to common ratios such as 1:1, 4:3, 16:9, or A4 portrait/landscape.

Image objects are imported from local image files and stored inside GeoSketch project data as data URLs. This keeps the single-file/no-install workflow simple, but large images can make GeoJSON project files and browser autosaves large. The current image workflow supports rectangular placement plus 3-point affine georeferencing: add three control point rows, set the image and map sides in any order, then apply the affine warp. Image edit mode also supports proportional resizing, mouse rotation, numeric rotation, and small numeric warp adjustments for scale, skew, and projected offset. This is intended for practical digitising and visual alignment, not survey-grade rectification.

For best results with 3-point affine georeferencing:

- Use three control points that are spread across the image, preferably near stable features such as road junctions, corners, bridges, shorelines, or grid intersections.
- Avoid placing all three control points on one line or tightly clustered in one part of the image.
- Use rectangular resize and move handles to get the image roughly into place before collecting control points.
- Image control point inputs use original image pixels, measured from the top-left corner of the source image. Map control point inputs accept the same decimal degrees, DMS, and MGRS formats as other coordinate fields. On-map control points are shown as crosshairs labelled `P1`, `P2`, `P3` for image points and `M1`, `M2`, `M3` for map points.
- Keep the original image file reasonably small when possible; the image is embedded in the saved GeoJSON project.
- Treat the result as a visual alignment aid. Affine georeferencing can translate, rotate, scale, and skew an image, but it cannot rubber-sheet a distorted paper map or oblique photograph.

## Buffers and Measurements

Objects can show measurements on the map and in the object pane. Lines and polygons can have buffers; point and circle-style range objects can be used for distance visualisation.

Buffers are attached to the object that created them. If the object is edited or moved, the buffer updates with it. Buffer style can be edited separately from the source object using the same line and fill controls as drawn shapes.

GeoSketch uses WGS84 coordinates and geodesic calculations from Leaflet and Turf.js where practical. Range circles and buffers are generated as geographic shapes rather than simple screen-pixel overlays. This is suitable for OSINT sketching and planning, not legal boundary work, engineering survey, fire-control computation, or navigation safety.

## Units and APP-6 Symbols

Unit objects use APP-6 style symbols generated by [`milsymbol`](https://github.com/spatialillusions/milsymbol), created and maintained by Måns Beckman ([Spatial Illusions](http://www.spatialillusions.com/)). The unit picker builds numeric SIDCs from fields such as:

- identity
- symbol set
- main icon
- echelon/amplifier
- entity type and subtype
- headquarters/task force/feint modifiers
- reinforced/reduced modifier
- additional mobility and capability modifiers

GeoSketch stores the generated SIDC in the saved project data so exported and reloaded units retain their symbol configuration.

Unit fields include:

- Unique designation
- Name
- Higher formation
- Notes

The Layers view and selected-object pane display units as `unique designation/higher formation (name)` when those fields are available. If only a name exists, the unit is shown by name alone.

## Import and Export

### Save Project as GeoJSON

Use this for normal project save/load. GeoSketch stores object geometry plus app-specific styling and metadata inside GeoJSON feature properties.

Distance-bearing saved data uses SI units: buffer distances and circle radii are stored in metres. The Settings distance unit controls how measurements are displayed after load.

### Save Selected Layers as GeoJSON

Exports visible layers separately, using safe filenames derived from layer names.

### Load Project from GeoJSON

Loads a saved GeoSketch project.

For a quick import test, load `samples/geosketch-sample.geojson`. It contains the same small Helsinki-area demonstration project as **Load Sample**, including a point, APP-6 unit, route with buffer, range circle, polygon, labels, measurements, and text box.

### Load GeoJSON as Layer/Template

Loads external GeoJSON data as map layers/templates. This is useful for background overlays, reusable object sets, or prebuilt working maps.

### Export Excel

Exports an object table for review and reporting.

Buffer distances in Excel exports are reported in metres for consistency with saved GeoJSON.

### PNG Export

Use **Save visible PNG** to export the current map viewport. Use **Define export area** and **Export area format** in the Files section to draw a rectangular crop guide, then use **Save [area name] (width x height px) as PNG** to export it. The displayed pixel size is the expected PNG output size after GeoSketch automatically fits the map to that export area. If more than one export area exists, select the area from the map, Layers list, or the Files section export-area drawer first; GeoSketch keeps the button disabled until the intended crop is explicit. Export area guide rectangles are not burned into the cropped PNG. Settings can add a "Created with GeoSketch" credit and export date to PNGs, and can hide PNG map attribution when a particular output needs a cleaner frame.

PNG export can be used with raster background maps, or with no background map for an object-only export against the map backdrop. Raster layer opacity is preserved. Vector overlays such as OpenFreeMap are available for live viewing and interactive HTML export, but are not included in PNG export because browser-side WebGL capture is not reliable enough for release-quality map output.

Browser security rules and tile provider CORS settings can affect PNG output.

### HTML Map

Exports an interactive HTML map containing the current project state, including map title, map notes, links, layer controls, attribution, and projection notes. The exported map uses a collapsed slide-out information pane so the map itself remains unobstructed by default.

The exported map still depends on external libraries and map tile providers unless those resources are separately mirrored or bundled.

## Autosave and Recovery

GeoSketch autosaves to browser `localStorage`. Use **Restore Autosave** to recover the current browser's saved work.

Autosave is not a substitute for saving project GeoJSON files. Save important work explicitly.

## Built-In Checks

The **Run Checks** button performs lightweight browser-side diagnostics:

- GeoJSON export parse check
- project save-state round-trip check
- per-layer GeoJSON export check
- geometry export check
- measurement and buffer generation checks

This is a smoke test, not a full QA suite.

## Manual QA Checklist

Use this checklist before publishing a development build with image/georeferencing changes:

- Load the built-in sample map and confirm existing point, unit, line, polygon, circle, text, buffer, measurement, and export-area objects still render.
- Add a PNG, JPG, and WebP image object if test files are available.
- Select an image, move it with the centre handle, resize it with proportional resizing on, then repeat with proportional resizing off.
- Add three image control point rows, set image and map points in mixed order by both typing and picking, apply the affine warp, and confirm the image visibly moves/rotates/skews toward the selected map features.
- Delete one control point row and confirm the remaining rows renumber and still apply once three complete pairs exist.
- Save the project as GeoJSON, reload it, and confirm the image source, opacity, control points, and affine warp survive the round trip.
- Refresh the browser and restore autosave to confirm the same image state survives autosave.
- Export an interactive HTML map and confirm image objects appear there, including affine-warped images.
- Export visible-map PNG and export-area PNG with raster background maps enabled, then check object placement and attribution/date/credit settings.
- Confirm PNG export remains blocked or clearly warned when only vector-background layers are available.
- Run **Run Checks** after the manual pass.

## Privacy and Operational Notes

GeoSketch runs locally in the browser, but some features contact external services:

- background maps request tiles from the selected tile provider
- map search uses OpenStreetMap/Nominatim-style search
- JavaScript libraries are loaded from public CDNs

Avoid entering sensitive operational information into external search services or tile requests if that would create risk. For sensitive use, consider hosting libraries, geocoding, and map tiles in a controlled environment.

## Known Limitations

- The app is a single HTML file, so very large projects may become slow.
- Browser PNG export can be affected by CORS restrictions on map tiles.
- Vector overlays use MapLibre GL through a Leaflet bridge; they add WebGL/CDN requirements and are intentionally excluded from PNG export.
- Image objects are embedded as data URLs, so large images can make GeoJSON saves, autosaves, and HTML exports large.
- Image georeferencing is currently 3-point affine only. It does not support many-point rubber-sheeting, local residual/error display, or survey-grade rectification.
- Affine-warped images should be manually checked in HTML and PNG exports before publication, especially when large source images are used.
- GeoJSON exports include GeoSketch-specific styling metadata that other GIS tools may ignore.
- APP-6 symbol support is intentionally practical and UI-driven, not a full doctrinal validation engine.
- Measurements are appropriate for sketching and planning, not survey-grade work.
- Interactive HTML exports are portable as files, but still need internet access for external dependencies and tiles.

## Acknowledgements

GeoSketch is possible because of excellent open-source mapping and web tooling:

- [Leaflet](https://leafletjs.com/) for browser map rendering.
- [MapLibre GL JS](https://maplibre.org/) and [MapLibre GL Leaflet](https://github.com/maplibre/maplibre-gl-leaflet) for optional vector-tile background rendering.
- [Leaflet-Geoman](https://geoman.io/leaflet-geoman) for geometry editing interactions.
- [Turf.js](https://turfjs.org/) for geospatial calculations, buffers, distances, areas, and GeoJSON helpers.
- [milsymbol](https://github.com/spatialillusions/milsymbol), created by Måns Beckman / Spatial Illusions, for generating APP-6 / MIL-STD-2525 style unit symbols in the browser.
- [SheetJS](https://sheetjs.com/) for Excel export.
- [html2canvas](https://html2canvas.hertzen.com/) and [dom-to-image-more](https://github.com/1904labs/dom-to-image-more) for browser-side image export support.
- [OpenStreetMap](https://www.openstreetmap.org/) contributors for map data and the wider open mapping ecosystem.
- [OpenTopoMap](https://opentopomap.org/) and SRTM contributors for topographic map context.
- [Esri World Imagery](https://www.esri.com/) for satellite imagery tiles where available.
- [CARTO](https://carto.com/) for raster tile styling used by the OpenStreetMap background option.
- [OpenFreeMap](https://openfreemap.org/) and [OpenMapTiles](https://openmaptiles.org/) for the optional OpenStreetMap-based vector background.

Please respect the licenses, attribution requirements, tile usage policies, and rate limits of all upstream data and library providers.

## License

GeoSketch is released under the MIT License. See `LICENSE`.

## File Layout

- `index.html`: the GeoSketch application.
- `README.md`: this guide.
- `LICENSE`: the MIT License for this repository.
- `samples/geosketch-sample.geojson`: sample project for import/export testing and format inspection.

Other files in this workspace may belong to earlier experiments or adjacent tooling and are not required for basic GeoSketch use.

## Changes Since v1.0

- Added project-level map notes, including clickable web/email links in HTML exports.
- Added Export area objects and separate visible-map vs area PNG export.
- Added default distance-unit settings, including miles and nautical miles display, while saved distance data remains SI/metres.
- Improved PNG export rendering for cropped raster maps and tile seams.
- Refined interactive HTML exports with a collapsed slide-out information pane and project/repository links.
- Added clearer save/load state, save confirmations, safer unsaved-change prompts, and GeoJSON load conflict handling.
- Added fractional zoom support and a bottom-bar zoom slider.
- Added background map stack ordering.
- Cleaned selected-object titles and unit display names.
- Added initial MapLibre/OpenFreeMap vector background support for live viewing and HTML exports.
- Added PNG export credit/date controls.
- Tightened visible PNG export to Leaflet's active map size and folded PNG credit/date text into the attribution box.
- Made the GeoSketch credit in HTML exports link to the live site.
- Made PNG credit/date visible on the live map when enabled and aligned PNG capture to the refreshed Leaflet viewport.
- Disabled PNG export when only vector backgrounds are visible and documented vector backgrounds as live/HTML-export only.
- Added an attribution/credit/date strip to cropped export-area PNGs.
- Added export-area width, height, and current-view pixel dimensions to the object measurement pane.
- Added a pre-draw export-area format selector and renamed the tool to Define export area.
- Added live export-area PNG dimension labels, more compact attribution, and area-export button text that names the selected crop.
- Finalized the v1.1 export-area flow so multiple export areas require an explicit selection before PNG export.
- Moved export-area creation and format controls into the Files workflow and added a compact export-area selector drawer.
- Started v1.1.1 development with OpenFreeMap reference-layer filters for roads, rail, places, borders, water, chokepoints, airports, and ports.
- Continued v1.1.2 development with stricter reference-layer classification, visible rail/chokepoint/transport highlights, and OpenFreeMap backdrop colour/opacity controls.
- Continued v1.1.3 development by separating the real map backdrop from OpenFreeMap land opacity, dropping ferry routes from chokepoints, and improving rail/airport/port highlight filters.
- Continued v1.1.4 development with reference-layer legends, clearer Vector overlays naming/help, and distinct generated airport/port overlay icons.
- Released v1.2 with Filter Vector Overlays naming, updated help/README text, and roomier airport/port overlay icon rings.
- Started v1.2.1 development with no-fill defaults for new lines and documented vertex deletion in Edit points mode.
- Started v1.2.2 development with untitled-map filename prompts and clearer drawing tooltips for Enter-to-finish tools.
- Started v1.2.3 development with selected-object coordinates and measurements collapsed by default in the right pane.
- Started v1.2.4 development with selectable line/polygon points, coordinate-line highlighting, and Delete Point / Del / Backspace point deletion.
- Started v1.2.5 development with stronger visible vertex handles, explicit line endpoint add controls, and restored "press Enter" drawing tooltips.
- Reset development versioning to v1.2.8 and removed the experimental background image-layer prototype; image objects are the target for v1.3.
- Started v1.2.9 development with editable numeric status controls, configurable horizontal/vertical scale bars, and a reset-settings button for display/export and new-object defaults.
- Started v1.2.10 development with corrected scale-bar segment rendering, cleaner scale-bar labels, and bottom-right default placement for the vertical scale bar.
- Started v1.2.11 development with imported image overlays as drawing-layer objects, including save/load support and mouse edit handles for moving/resizing image bounds.
- Started v1.2.12 development with proportional image resizing and a first 3-point affine image georeferencing workflow.
- Started v1.2.13 development with affine-aware control-point re-picking and georeference markers visible outside image edit mode.
- Started v1.2.14 development with editable georeferencing rows, image-pixel inputs, map-coordinate inputs, per-point deletion, and independent image/map point picking.
- Started v1.2.15 development with crosshair georeference markers, mouse/numeric image rotation, manual affine warp adjustments, and editable opacity percentages beside opacity sliders.
- Started v1.2.16 development with a None vector-overlay preset, Esc cancellation for coordinate/input workflows, and a cleaner image object pane ordering.
- Started v1.2.17 development with clearable map-search markers and north-positive label Y offset controls.
- Started v1.2.18 development with improved snap-to-existing-point drawing, underlay-style selection highlights, and default-off object shadows for point icons, paths, and line endpoints.
- Started v1.2.19 development with separate point icon fill opacity, safer remove-all-buffers confirmation, normal arrow map cursors, and clearer file-load failure messages.
