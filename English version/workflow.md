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

## What you will do

An old map is just an image — it has no coordinates and does not know where in the world it belongs. In this workflow you give it coordinates, lay it over a current aerial photograph, and look at what matches and what does not.

The example uses a sheet of the Franciscean cadastre for Celje from 1825, but you can take any old map. The result is a single map on which the old and the new state are visible at once.

It takes about an hour. You do not need to know QGIS beforehand.

## Why we do this at all

An old map is not a photograph of the past. The Franciscean cadastre was made for land taxation, so the parcels are surveyed carefully while buildings are simplified or left out altogether.

When you lay such a sheet over an orthophoto you are doing two things at once: you are placing a source in space, and you are claiming that something stood in a particular spot at a particular time. The second claim is always the weaker one. A neat overlay does not mean the map is accurate to the metre — which is why at the end you measure the offset and write it down.

For more on why space in the humanities is not a neutral frame, see [GIS and spatial humanities](https://damjan-popic.github.io/digital-humanities-handbook/chapters/gis-spatial-humanities/).

## What you need

- **QGIS** — free, download it from [qgis.org](https://qgis.org/). No account required.
- **An old map** as an image (JPEG or TIFF). Where to get one is in step 1.
- **An internet connection** for the orthophoto.
- **An hour of your time.**

Your map must show at least **three objects that still stand today** and are clearly drawn on it — a church, a castle, a large building. Without them you cannot place it in space.

Before you start, decide two things and write them down: which **coordinate reference system** you will work in (EPSG:3794 for Slovenia) and **how large an offset** you will still count as agreement.

## Input and provenance

Record where each source came from. Fill the table in with your own data — the rows below are examples.

| Item | Source or creator | Date | Licence / access | Changes made |
|---|---|---|---|---|
| `my-map.jpg` | "Katastrska mapa k. o. Celje, 1825", unknown author; via the Kamra portal, retrieved from Wikimedia Commons | 1825 | public domain | none, the original is untouched |
| DOF025 orthophoto | Surveying and Mapping Authority of the Republic of Slovenia, public WMS | retrieved YYYY-MM-DD | freely available, attribution required | none, the service is not downloaded |
| `Katastrska_mapa_k.o._Celje,_1825.jpg.points` | own work | 2026-09-08 | CC BY 4.0 | three control points, picked by hand |

Record this much about every control point as well. Without the last column nobody can repeat the workflow.

| Field | Required? | Example | Meaning |
|---|---:|---|---|
| `id` | yes | `P1` | The label you also use on the map. |
| `opis` | yes | `St Daniel's Cathedral` | What the thing on the ground is. |
| `odstopanje_m` | yes | `2` | The offset after fitting, in metres. |
| `tip` | yes | `checked` | What exactly you clicked on. |

## Tools and versions

- [QGIS](https://qgis.org/) — loading layers, georeferencing, display and export. Version used: **4.2.1**.
- Orthophoto: the public WMS of the Surveying and Mapping Authority of Slovenia, `https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms`, layer `DOF025`.
- Optional: the **AGIS** plugin, which loads Slovenian base layers and historical maps in one go. Available for QGIS 3.x only.

The version matters in one respect: in older releases the georeferencer was a plugin and sat under the `Layer` menu. Since QGIS 3.16 it is built in and sits under **`Raster`**.

---

## Workflow

### 1. Find an old map

Wikimedia Commons has the category [Franciscan cadastral maps of Slovenia](https://commons.wikimedia.org/wiki/Category:Franciscan_cadastral_maps_of_Slovenia) — 99 sheets, all in the public domain, so you are free to use them.

Open the sheet you want. **Under the image, click *Original file*** first, and only then right-click and *Save image as*. If you download what you see on the page you get a scaled-down preview and it will not work later.

Save it in your own folder. Make a new folder just for this task. **From now on do not edit, crop or re-save the original image** — the control points are tied to its pixels.

### 2. Open QGIS and set the coordinate reference system

1. Open QGIS and click **New Project** on the start page (or `Project ▸ New`).
2. `Project ▸ Properties…`
3. Click the **CRS** tab on the left.
4. Type `3794` in the **Filter** box.
5. Select **EPSG:3794 — Slovenia 1996 / Slovene National Grid**.
6. `OK`

The bottom right of the QGIS window must now read `EPSG:3794`.

### 3. Add today's orthophoto

There are two ways. The first works everywhere; the second works only in QGIS 3.x but also brings historical base maps with it.

**First way — connect to eProstor**

1. `Layer ▸ Add Layer ▸ Add WMS/WMTS Layer…`
2. Click `New`.
3. In the **Name** field type anything, for instance `GURS`.
4. In the **URL** field type `https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms`
5. `OK`, then `Connect`.
6. Choose **DOF025** from the list.
7. `Add`, then `Close`.

> While the **Name** field is empty the `OK` button stays greyed out. Do not open this address in a browser either — it returns an error, because it is not a web page but a service endpoint.

**Second way — the AGIS plugin**

1. `Plugins ▸ Manage and Install Plugins…`
2. In the left column click `All` or `Not Installed`, find **AGIS**, click `Install`.
3. If it is not in the list, download it as a ZIP file from the web and install it through `Plugins ▸ Manage and Install Plugins ▸ Install from ZIP`.

AGIS loads groups of layers for Slovenia, among them *Podlage* (base maps) and *Historicne podlage* (historical base maps). Only the QGIS 3.x version is currently available; it has not yet been updated for 4.x.

Once the orthophoto is loaded, navigate to your area — you can type a coordinate into the **Coordinate** box at the bottom and press `Enter`, or simply zoom in with the mouse.

### 4. Open the Georeferencer and load the old map

Georeferencing means telling an image where in the world it belongs.

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
2. On the **old map**, click an object you recognise — a church, for instance.
3. A small dialog opens. Click the **From Map Canvas** button.
4. QGIS takes you to the main window. Click **the same church on the orthophoto**.
5. `OK`

Repeat three times. Keep the points **spread out** across the area rather than clustered in one spot. Each one appears in the **GCP table** at the bottom of the window as you go.

Good points: a church, a castle, a large building, a junction of old streets.
Poor points: trees, fields, riverbanks and bridges — all of these move over time. They are excellent for observing *how* something changed over the years, though.

> If you already have a file of points, load it in the Georeferencer window with `File ▸ Load GCP Points…` — choose the file and click `Open`. Do not do both: if you load points and then click more of your own, QGIS adds them together and the same building counts twice. The file extension must be **.points**.

> Three rows then appear in the **GCP table** below, with numbers in the *Source X* column. If the table stays empty, the file was not read.

Write down for each point what exactly you clicked — "centre of the church", "south-east corner of the castle". You will need this at the end.

### 6. Set the transformation and run the georeferencing

1. In the Georeferencer window click `Settings ▸ Transformation Settings…`.
2. Under **Transformation type** choose `Helmert`.
3. Under **Target CRS** choose `EPSG:3794` (use the drop-down, or click the globe button and type `3794`).
4. Under **Output file** click the `…` button, go to the folder where you want to save, type a name such as `celje-1825-georef-my` in the **File name** field and click `Save`.
5. Under **Resampling method** choose `Cubic (4x4 Kernel)`.
6. Tick **Load in project when done**.
7. Tick **Save GCP points** as well.
8. Click `OK`.
9. Click the green start button (▸).

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

Zoom in to a scale of `1:2500` and look at:

| | What you look at | What you will notice |
|---|---|---|
| 1 | **Stari trg** (the old square) | the shape matches, the houses stand in the same place |
| 2 | **St Daniel's Cathedral** | the best agreement anywhere |
| 3 | **The Counts' Palace** | the shape matches, the building stands in the same place |
| 4 | **The Water Tower** | it still stands today, yet the 1825 sheet does not show it at all |
| 5 | **The railway station** | fields in 1825; the line was built in 1846 |
| 6 | **The bridge over the river** | the 1825 bridge is gone; a weir stands here today |

The first three are **checks on the alignment** — measure these and write the number down. The last three are **changes on the ground** — there is nothing to measure, you simply describe what happened.

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
2. Under **File base** click `…` and choose the folder you are saving to.
3. Under **Table name** type a name, for instance `Oznake`.
4. Under **Geometry type** choose `Point`.
5. Click the globe button and set the CRS to `EPSG:3794`.
6. In the *New Field* section add the fields: type `id` in **Name**, type `Text`, click `Add to Fields List`. Repeat for `tip`, `opis` and `opomba` (all `Text`), and for `odstopanje_m` (type `Decimal number`).
7. Click `OK`.
8. In the Layers panel click the new layer, then click the pencil icon **Toggle Editing**.
9. Click the **Add Point Feature** icon (at the top, two icons to the right of **Toggle Editing**) and click on the map wherever you measured. After each click fill in the attribute form, for example:
   `Id` – `P1`
   `Opis` – `Stari trg`
   `Opomba` – `the shape matches, the houses stand in the same place`
   `Odstopanje_m` – `the figure you measured in the previous step`
10. When you are finished, click the pencil again and confirm the save.
11. You can change how the annotations look and add text: double-click, or right-click the **Oznake** layer and choose `Properties` — a new window opens. Choose `Symbology` in the left column to change the symbol, colour and size. Below `Symbology` is the `Labels` tab, where you switch from `No labels` to `Single Labels`. There you can set the colour, font and size, and also choose what the label on the map will say.

> If you notice you made a mistake early on in the id, description, note and so on: select the layer created in this step (**Oznake**), right-click it and find `Open Attribute Table`. When the new window opens, click the pencil and correct the data. When you are done correcting, press the diskette (**Save**) and that is it.

### 10. Make the map

1. `Project ▸ Save` — save the project, otherwise you lose all the display settings.
2. `Project ▸ New Print Layout…`, type a name, `OK`.
3. `Add Item ▸ Add Map` and draw a rectangle with the mouse. Leave room at the bottom.
4. `Add Item ▸ Add Scale Bar` — the scale bar.
5. `Add Item ▸ Add North Arrow` — north.
6. `Add Item ▸ Add Legend` — the legend.
7. `Layout ▸ Export as Image…`, choose `PNG`, and enter `300` dpi under **Export resolution**.

> To remove layers from the legend: click the legend, and on the right under **Legend Items** make sure the first box reads `Manual`, then remove entries with the `−` button below the panel. Double-clicking an entry also lets you rename it.

That is the finished product.

## Output

```text
Qgis_postopek/
├── Qgis_postopek.qgz                              projekt QGIS
├── Datoteke/
│   ├── Katastrska_mapa_k.o._Celje,_1825.jpg       izvirni sken, nespremenjen
│   ├── Katastrska_mapa_k.o._Celje,_1825.jpg.points   tri kontrolne točke
│   ├── Katastrska_mapa_k.o._Celje,_1825_modified.tif georeferenciran zemljevid
│   ├── Primerjalna karta_GeopackageLayer.gpkg     točkovni sloj z oznakami
│   └── Primerjalna_karta.png                      KONČNI IZDELEK
├── slovenska verzija/
│   └── postopek.md                                postopek v slovenščini
├── English version/
│   └── workflow.md                                postopek v angleščini
```

Look at `map.png` first. Everything else is the material it was built from.

## Interpretation and limits

**The gain.** Both sources are now in the same coordinate reference system, so they can be compared by measurement rather than by eye.

**The loss.** A drawn document has become an image. Parcel boundaries and numbers are not data until you transcribe them, and georeferencing has also rotated and resampled the sheet. An offset of around 10 metres **does not mean** that something has actually moved. That is simply the accuracy of this procedure with this material.

**A likely source of error.** The control points sit on buildings whose footprints have changed, so part of the offset may come from your choice of point rather than from the map.

**A claim that would be too strong.** That the Savinja has shifted by 12 metres. That figure is smaller than the uncertainty.

Some buildings, the Water Tower for instance, do not appear on the 1825 sheet at all, but that **does not mean they were not standing then**. It only means they are not drawn on this sheet — some sheets frequently omit smaller buildings, so landmarks have to be checked separately.

## Sources and rights

**The old map.** The 1825 sheet is in the public domain, because copyright protection has expired.

**The orthophoto.** The GURS service is freely available; crediting the source is obligatory.

## Practice task

Take another sheet from the same collection and repeat the workflow with three control points of your own. Write down one example of good agreement, one example of disagreement with the measured value in metres, and one limitation that your particular sheet revealed.
