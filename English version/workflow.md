---
title: "How do I compare a historical map with a current orthophoto in QGIS?"
description: "I place an old map onto today's orthophoto, check where the two agree, and produce a comparison map."
category: "mapping"
difficulty: "beginner"
time: "60 min"
tags: [heritage, qgis, georeferencing, cadastre, orthophoto]
author: "Patricija Curk"
status: "student-draft"
---

# How do I compare a historical map with a current orthophoto in QGIS?

<div class="answer-meta" markdown>
<span>mapping</span>
<span>beginner</span>
<span>60 min</span>
</div>


An old map is just an image with no coordinates attached. In this workflow we give it coordinates, lay it over a current aerial photograph and look at what matches and what does not.

The example uses a sheet of the Franciscean cadastre for Celje from 1825, but any old map will do. The result is a single map on which the old and the current state are visible at once.

The workflow takes about an hour and no prior knowledge of QGIS is needed.

## Why compare the images?

An old map is not a photograph of the past. The Franciscean cadastre, for instance, was made for land taxation, so the parcels are surveyed carefully while buildings are simplified or left out altogether.

When we lay such a sheet over an orthophoto we are doing two things at once: placing a source in space, and claiming that something stood in a particular spot at that time. A neat overlay does not mean the map is accurate to the metre. That is why at the end we measure the offset and write it down.

For more on why space in the humanities is not a neutral frame, see [GIS and spatial humanities](https://damjan-popic.github.io/digital-humanities-handbook/chapters/gis-spatial-humanities/).

## What do you need to compare a historical map with a current orthophoto?

- **QGIS** — free, download it from [qgis.org](https://qgis.org/). No account required.
- **An old map** as an image (JPG/JPEG or TIFF). Where to get one is in step 1.
- **An internet connection** for the orthophoto.
- **An hour of your time.**

Your map must show at least **three objects that still stand today** and are clearly drawn on it — a church, a castle, a town wall, for example.


## Input and provenance

| Item | Source or creator | Date | Licence / access | Changes made |
|---|---|---|---|---|
| `Katastrska_mapa_k.o._Celje,_1825.jpg` | "Katastrska mapa k. o. Celje, 1825", unknown author; via the Kamra portal, retrieved from Wikimedia Commons | 1825 | public domain | none, the original is untouched |
| DOF025 orthophoto | Surveying and Mapping Authority of the Republic of Slovenia, public WMS | retrieved 8 Sept 2026 | freely available, https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms | none, the service is not downloaded |
| `Katastrska_mapa_k.o._Celje,_1825.jpg.points` | own work | 8 Sept 2026 | CC BY 4.0 | three control points, picked by hand |

## Tools and versions

- [QGIS](https://qgis.org/) — loading layers, georeferencing, display and export. This workflow uses version **4.2.1**; an older version works too, although the steps may differ slightly.
- Orthophoto: the public WMS of the Surveying and Mapping Authority of Slovenia, `https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms`, layer `DOF025`.
- Optional: the **AGIS** plugin, which loads Slovenian base layers and historical maps in one go. Currently available for QGIS 3.x only.

---

## Workflow

### 1. Find an old map

Wikimedia Commons has the category [Franciscan cadastral maps of Slovenia](https://commons.wikimedia.org/wiki/Category:Franciscan_cadastral_maps_of_Slovenia) — 99 sheets, all in the public domain, so you are free to use them.

Open whichever sheet you like. **Under the image, click *Original file*** first, and only then right-click and *Save image as*. If you download what you see on the page you get a scaled-down preview, which can cause trouble with the later steps of this guide.

Save it in a folder you make just for this task. **From now on do not edit, crop or re-save the original image**, because the control points are tied to its pixels.


### 2. Open QGIS and set the coordinate reference system

1. Open QGIS and click **New Project** on the start page (or `Project ▸ New`).
2. `Project ▸ Properties…`
3. Click the **CRS** tab on the left.
4. Type `3794` in the **Filter** box.
5. Select **EPSG:3794 — Slovenia 1996 / Slovene National Grid**.
6. `OK`

The bottom right of the window must now read `EPSG:3794`.

### 3. Add today's orthophoto

There are two ways. The first works everywhere; the second works only in QGIS 3.x, but also brings historical base maps and more with it.

**First way: connect to eProstor**

1. `Layer ▸ Add Layer ▸ Add WMS/WMTS Layer…`
2. Click `New`.
3. In the **Name** field type anything, for instance `GURS`.
4. In the **URL** field type `https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms`
5. `OK`, then `Connect`.
6. Choose **DOF025** from the list.
7. `Add`, then `Close`.

> While the **Name** field is empty the `OK` button stays greyed out.

**Second way: the AGIS plugin**

1. `Plugins ▸ Manage and Install Plugins…`
2. In the left column click `All` or `Not Installed`, find **AGIS**, click `Install`.
3. If it is not in the list, download it as a ZIP file from the web and install it through `Plugins ▸ Manage and Install Plugins ▸ Install from ZIP`.

AGIS loads groups of layers for Slovenia, among them *Layers* and *Historical Layers*. Only the QGIS 3.x version is currently available; it has not yet been updated for 4.x.

Once the orthophoto is loaded, navigate to your area. You can type a coordinate into the **Coordinate** box at the bottom and press `Enter`, or simply zoom in with the mouse.

### 4. Open the Georeferencer and load the old map

Georeferencing means telling an image "where in the world it belongs" — placing it in its proper spot.

1. `Layer ▸ Georeferencer…`
2. A new **Georeferencer** window opens.
3. In it, click the first icon on the left, `File ▸ Open Raster…` (shortcut `Ctrl+O`).
4. Choose your image of the old map (JPG) and click `Open`.
The old map appears in the upper part of the window.

> If you cannot find the Georeferencer under Layer, look under Raster.

### 5. Control points

A control point is a pair: the same place on the old map and on the orthophoto.

For each point:

1. In the Georeferencer window click `Edit ▸ Add Point` (the icon with the yellow dot).
2. On the **old map**, click an object you recognise, such as a church.
3. A small window pops up. Click the **From Map Canvas** button.
4. QGIS takes you to the main window. Click **the same church on the orthophoto**.
5. `OK`

Repeat three times. Keep the points **spread out** across the area. Each one appears in the **GCP table** at the bottom of the window as you go.

Good points are a church, a castle, a large building, a junction of old streets.
Poor points are trees, fields, riverbanks and bridges. All of these change over time, which is why they do not count as good points. They are excellent for observing change over the years, though.

> If you already have a file of points, load it in the `Georeferencer` window with `File ▸ Load GCP Points…`. Choose the file and click `Open`. Do not do both: if you load points and then click more of your own, QGIS adds them together and the same building counts twice. The extension of a file that the Georeferencer can load is **.points**.

> How do you make a points file yourself? You need a spreadsheet program or a plain text editor; what matters is that the file extension is right. You can read the X and Y coordinates in QGIS by moving the mouse over the map. Online map services usually give latitude and longitude instead, which is a different system. The simplest way to bring a table of points into QGIS is a spreadsheet saved as CSV (Comma-Separated Values). You can make it in Excel, Google Sheets or Notepad. For the Slovenian system (EPSG:3794) enter the easting (X/East) and northing (Y/North) values. In Excel use `.` rather than `,` for decimals. Finally, *Save as* and choose **.csv**.

> Three rows then appear in the **GCP table** below, with numbers in the *Source X* column. If the table stays empty, the file was not read.

Write down somewhere what each point represents — the centre of the church, the south-east corner of the castle — because you will need it in the next steps to know what you are describing and measuring.

### 6. Set the transformation and run the georeferencing

1. In the Georeferencer window click `Settings ▸ Transformation Settings…`.
2. Under **Transformation type** choose `Helmert`.
3. Under **Target CRS** choose `EPSG:3794` (use the drop-down, or click the globe button and type `3794`).
4. Under **Output file** click the `…` button, go to the folder where you want to save, type a name such as `celje-1825-georef-my` in the **File name** field and click `Save`.
5. Under **Resampling method** choose `Cubic (4x4 Kernel)`.
6. Tick **Load in project when done**.
7. Tick **Save GCP points** as well.
8. Click `OK`.
9. Click the green start / play button (▸).

The bottom of the Georeferencer window must now read `Transform: Helmert`. If it says `None`, no transformation type has been selected.

> **Why Helmert?** Because with three points the other transformations always report an offset of zero. That looks excellent and tells you nothing. Helmert allows only a shift, a rotation and a uniform scale, so a real offset is left over — one you can actually measure.

When it finishes, the layer loads into the project by itself.

<details>
<summary>If it reports "Transform Failed: Could not read source image"</summary>

1. Check that the **Output file** field is genuinely filled in.
2. Convert the image to TIFF: `Raster ▸ Conversion ▸ Translate (Convert Format)`, choose your image, choose where to save it, `Run`. Then open that `.tif` in the Georeferencer and **click the points again** — they are cleared when the image is swapped.
3. Close the Georeferencer window and open it again.

</details>

### 7. Inspecting the alignment

1. In the **Layers** panel, right-click the new layer.
2. Click **Zoom to Layer**.
3. If the layer is not on top, drag it above `DOF025` with the mouse.
4. Press `F7`. The **Layer Styling** panel opens on the right.
5. Make sure your cadastre is the layer selected in the drop-down at the top of the panel.
6. Scroll down to **Layer Rendering**.
7. Under **Blending mode** choose `Multiply`.
8. Scroll down to **Resampling** and set *Zoomed in* to `Bilinear`.

If you would rather use a transparency slider: in the left column of icons in the *Layer Styling* panel click the **Transparency** icon (not the brush) and move **Global opacity**.

### 8. Checking and measuring

Zoom in to whatever scale suits you for inspecting the area, for instance `1:2500`. For the Celje cadastre the inspection looks like this:

| | What you look at | What you will notice |
|---|---|---|
| 1 | **Stari trg** (the old square) | the shape matches, the houses stand in the same place |
| 2 | **St Daniel's Cathedral** | the best agreement anywhere |
| 3 | **The Counts' Palace** | the shape matches, the building stands in the same place |
| 4 | **The Water Tower** | it still stands today, yet the 1825 sheet does not show it at all |
| 5 | **The railway station** | fields in 1825; the line was built in 1846 |
| 6 | **The bridge over the river** | the 1825 bridge is gone; a weir stands here today |

The first three are **checks on the alignment**: these are measured and you write the number down. The last three are **changes on the ground**: nothing is measured, you simply describe what you see.

**How to measure:**

1. Press `Ctrl+Shift+M` (the *Measure Line* tool).
2. In the **Measure** dialog set **Units** to `Meters`.
3. Click the position as it is drawn on the old map.
4. In the Layers panel **untick the old map** — the measuring line stays, and the bare photograph is underneath.
5. Click the same thing as it is today.
6. Read **Total**, write the number down, then click `New` to measure the next pair.
7. Switch the old map back on. To start a fresh line, right-click to end the current one.

> You are not clicking two separate dots on the screen. Both positions are in the same place, one on top of the other — you are seeing the same object twice, drawn and photographed. That is why you switch the old map off for the second click.

### 9. Make a point layer with your annotations

1. Click `Layer ▸ Create Layer ▸ New GeoPackage Layer…`.
2. Under **File base** click `…` and choose the folder you are saving to (mine is `Qgis_postopek/Datoteke`).
3. Under **Table name** type a name, for instance `Oznake`.
4. Under **Geometry type** choose `Point`.
5. Click the globe button and set the CRS to `EPSG:3794`.
6. In the *New Field* section add the fields: type `id` in **Name**, type `Text`, click `Add to Fields List`. Repeat for `tip`, `opis` and `opomba` (all `Text`), and for `odstopanje_m` (type `Decimal number`).
7. Click `OK`.
8. In the Layers panel click the new layer, then click the pencil icon **Toggle Editing**.
9. Click the **Add Point Feature** icon (at the top, two icons to the right of **Toggle Editing**) and click on the map wherever you measured. After each click fill in the attribute form, for example:
   `Id` – `P1`
   `Opis` – `Stari trg`
   `Tip` – `Checked`
   `Opomba` – `the shape matches, the houses stand in the same place`
   `Odstopanje_m` – `the figure you measured in the previous step`
10. When you are finished, click the pencil again and confirm the save.
11. You can change how the annotations look and add text: double-click, or right-click the **Oznake** layer and choose `Properties` — a new window opens. Choose `Symbology` in the left column to change the symbol, colour and size. Below `Symbology` is the `Labels` tab, where you switch from `No labels` to `Single Labels`. There you can set the colour, font and size, and also choose what the label on the map will say.

> If you notice you made a mistake early on in the id, description, note and so on: in the Layers panel on the left select the layer created in this step (**Oznake**), right-click it and find `Open Attribute Table`. When the new window opens, click the pencil and correct the data. When you are done correcting, press the diskette (**Save**).

### 10. Make the map

1. `Project ▸ Save` — save the project, otherwise you lose all the display settings.
2. `Project ▸ New Print Layout…`, type a name, `OK`. A white rectangle appears in the window.
3. `Add Item ▸ Add Map` and draw a rectangle with the mouse. Leave room at the bottom.
4. `Add Item ▸ Add Scale Bar` — the scale bar.
5. `Add Item ▸ Add North Arrow` — the north arrow.
6. `Add Item ▸ Add Legend` — the legend.
> To remove layers from the legend: click the legend, and on the right under **Legend Items** make sure the first box reads `Manual`, then remove entries with the `−` button below the panel. Double-clicking an entry also lets you rename it.
7. `Layout ▸ Export as Image…`, choose `PNG`, and enter `300` dpi under **Export resolution**.

And that is your finished product.

## Output

```text
Qgis_postopek/
├── README.md                                    project description
├── izjava-o-prispevku.md                        contribution statement
├── Qgis_postopek.qgz                            QGIS project
├── Datoteke/
│   ├── Katastrska_mapa_k.o._Celje,_1825.jpg     the original scan, unchanged
│   ├── Katastrska_mapa_k.o._Celje,_1825.jpg.points   three control points
│   ├── Katastrska_mapa_k.o._Celje,_1825_modified.tif the georeferenced map
│   ├── Primerjalna karta_GeopackageLayer.gpkg   point layer with annotations
│   └── Primerjalna_karta.png                    THE DELIVERABLE
├── Slovenska verzija/
│   └── postopek.md                              this workflow in Slovenian
└── English version/
    └── workflow.md                              this workflow in English
```

The finished project is `Primerjalna_karta.png`. Everything else is the material it was built from.

---

## Interpretation and limits

Because both sources sit in the same coordinate reference system, they can be measured and compared directly, which goes beyond mere visual estimation. Despite that advantage, the analysis has to take into account the specific nature of historical material and the technical limits of the procedure.

Spatial accuracy and georeferencing: transferring a hand-drawn cadastral map into present-day space takes its toll. Parcel boundaries and symbols only become data once they are vectorised, and the georeferencing itself (rotation, scaling, resampling) inevitably distorts the sheet. An offset of around 10 metres does not yet mean that anything on the ground has actually shifted. It is simply the limit of spatial accuracy that this material allows.

The control-point problem: an important source of error comes from choosing points on buildings whose footprints have changed over the decades through rebuilding or demolition. Part of the measured offset is therefore often a consequence of our choice of points rather than of an actual error in the original map.

River channel dynamics: to claim that the Savinja has shifted by 12 metres would be methodologically too strong. That figure is smaller than, or comparable to, the measurement uncertainty of the overlay itself.

Absence of objects: if a given object (the Water Tower, for instance) is not on the 1825 sheet, that does not mean it did not exist at the time. Cartographic sources of this kind left out smaller structures, or were never meant to show the full topography, so a reliable conclusion requires checking other sources and data as well.


## Connection to the handbook

- [GIS and spatial humanities](https://damjan-popic.github.io/digital-humanities-handbook/chapters/gis-spatial-humanities/) — why placing a source in space is already a claim in itself; this workflow is its smallest workable example.
- [Data, metadata and models](https://damjan-popic.github.io/digital-humanities-handbook/chapters/data-metadata-models/) — the source of the requirement that every derived file be traceable to its origin.
- [Ethics checklist](https://damjan-popic.github.io/digital-humanities-handbook/resources/ethics-checklist/) — before you publish the map.

## Sources and rights

**The old map.** The 1825 sheet is in the public domain, because copyright protection has expired.

**The orthophoto.** The GURS service is freely available; crediting the source is obligatory.

## Practice task

Take another sheet from the same collection and repeat the workflow with three control points of your own. Write down one example of good agreement, one example of disagreement with the measured value in metres, and one limitation that your particular sheet revealed.
