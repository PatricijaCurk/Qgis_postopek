# Zgodovinski zemljevid in sodobni ortofoto v QGIS-u / Historical map and current orthophoto in QGIS

**Praktični postopek za Priročnik digitalne humanistike / A workflow contribution to the Digital Humanities Handbook**

Georeferenciranje enega lista franciscejskega katastra (k. o. Celje, 1825) na državni ortofoto, preverjanje poravnave na neodvisnih značilnostih in izvoz ene označene primerjalne karte.

Georeferencing one Franciscean cadastre sheet (cadastral municipality of Celje, 1825) onto the national orthophoto, testing the alignment against independent features, and exporting a single annotated comparison map.

---

## Kazalo postopka / Table of Contents

Preden začnete, izberite jezikovno različico, ki vam bolj ustreza. Ne glede na to, katero različico boste izbrali, vas obe vodita skozi popolnoma enak postopek z istim primerom in omejitvami. 

V nadaljevanju vas spodnje kazalo usmerja skozi ključne korake:
1. Poišči star zemljevid (Find an old map)
2. Odpri QGIS in nastavi koordinatni sistem
3. Dodaj današnji ortofoto
4. Odpri Georeferencer in naloži star zemljevid
5. Kontrolne točke 
6. Nastavi transformacijo in zaženi georeferenciranje
7. Pregledovanje ujemanja
8. Preverjanje in meritev
9. Naredi točkovni sloj z oznakami
10. Naredi karto

Before you begin, choose the language that suits you best. Regardless of which version you select, both guide you through the same process using the same examples and consttrains.

The table below guides you through the key steps:

1. Find an old map
2. Open QGIS and set the coordinate system
3. Add the current orthophoto
4. Open Georeferencer and load the old map
5. Control points
6. Set transformation and run georeferencing
7. Review alignment
8. Verification and measurement
9. Create a point layer with labels
10. Create the map layout

---

## Vsebina mape / Contents

```text
Qgis_postopek/
├── README.md                                    opis projekta
├── izjava-o-prispevku.md                        izjava o avtorstvu in uporabi orodij
├── Qgis_postopek.qgz                            projekt QGIS
├── Datoteke/
│   ├── Katastrska_mapa_k.o._Celje,_1825.jpg     izvirni sken, nespremenjen
│   ├── Katastrska_mapa_k.o._Celje,_1825.jpg.points   tri kontrolne točke
│   ├── Katastrska_mapa_k.o._Celje,_1825_modified.tif georeferenciran zemljevid
│   ├── Primerjalna karta_GeopackageLayer.gpkg   točkovni sloj z oznakami
│   └── Primerjalna_karta.png                    KONČNI IZDELEK
├── Slovenska verzija/
│   └── postopek.md                              ta postopek v slovenščini
└── English version/
    └── workflow.md                              ta postopek v angleščini

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
---


## Rezultat na kratko / The result in brief

Rezultat postopka je slika primerjave, na kateri so na sliki označene tri preverjene izmerjene kontrolne točke. Poleg njih sem dodala še tri opazovalne točke, ki niso predmet meritev, temveč služijo zgolj orientaciji v prostoru, lažji umestitvi in vizualnem preverjanju. 

Odstopanje reda **10 m** je pri tem gradivu pričakovano in **ni dokaz o spremembi v prostoru**. Zunaj prikazanega izseka natančnost ni znana.

The result of the procedure is a comparison image featuring three verified and precisely measured control points. In addition to these, three observation points have been added which are not measured, but serve solely for spatial orientation, better contexual placement, and visual verification. 

An offset on the order of **10 m** is expected for this material and is **not evidence of real-world change**. Outside the extent shown, accuracy is unknown.

---

## Pravice / Rights

- Zgodovinski list: **javna domena** (1825). Vir posnetka: portal Kamra, id 14717, prek Wikimedia Commons.
- Ortofoto: **GURS**, prosto dostopno, **https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms**.
- Besedilo tega prispevka: **CC BY 4.0**. Georeferenca in izpeljane datoteke: **CC BY 4.0**.

---

