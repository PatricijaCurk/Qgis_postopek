---
title: "Kako v QGIS-u primerjam historični zemljevid s sodobnim ortofotom?"
description: "Star zemljevid postavim na današnji ortofoto, preverim, kje se ujemata, in naredim primerjalno karto."
category: "mapping"
difficulty: "beginner"
time: "60 min"
tags: [dediscina, qgis, georeferenciranje, kataster, ortofoto]
author: "Patricija Curk"
status: "student-draft"
---

# Kako v QGIS-u primerjam historični zemljevid s sodobnim ortofotom?

<div class="answer-meta" markdown>
<span>mapping</span>
<span>beginner</span>
<span>60 min</span>
</div>

## Kaj boš naredil

Star zemljevid je samo slika — nima koordinat in ne ve, kje na svetu je. V tem postopku mu koordinate določiš, ga položiš čez današnji posnetek iz zraka in pogledaš, kaj se ujema in kaj ne.

Za primer je uporabljen list franciscejskega katastra za Celje iz leta 1825, lahko pa vzameš kakršenkoli star zemljevid. Rezultat je ena karta, na kateri se vidi staro in novo stanje hkrati.

Traja približno uro. QGIS-a ti ni treba poznati vnaprej.

## Zakaj to sploh počnemo

Star zemljevid ni fotografija preteklosti. Franciscejski kataster je nastal zaradi zemljiškega davka, zato so parcele izmerjene skrbno, stavbe pa poenostavljene ali izpuščene.

Ko tak list položiš na ortofoto, hkrati počneš dvoje: postavljaš vir v prostor in trdiš, da je nekaj takrat stalo na določenem mestu. Druga trditev je vedno šibkejša. Lep prekriv še ne pomeni, da je zemljevid natančen na meter — zato na koncu odstopanje izmeriš in ga zapišeš.

Več o tem, zakaj prostor v humanistiki ni nevtralen okvir, je v poglavju [GIS in prostorska humanistika](https://damjan-popic.github.io/digital-humanities-handbook/sl/chapters/gis-spatial-humanities/).

## Kaj potrebuješ

- **QGIS** — brezplačen, prenesi ga s [qgis.org](https://qgis.org/). Računa ne rabiš.
- **Star zemljevid** kot slika (JPEG ali TIFF). Kje ga dobiš, piše v 1. koraku.
- **Internet** za ortofoto.
- **Uro časa.**

Pomembno je, da ima tvoj zemljevid vsaj **tri objekte, ki stojijo še danes** in so na njem jasno vidni — cerkev, grad, veliko stavbo. Brez tega ga ne moreš umestiti v prostor.

Preden začneš, se odloči za dvoje in to zapiši: v katerem **koordinatnem sistemu** boš delal/a (za Slovenijo EPSG:3794) in **kolikšno odstopanje** boš še štel/a kot ujemanje.

## Vhodni podatki in izvor

Za vsak vir zabeleži, od kod je. Tabelo izpolni s svojimi podatki — spodnja vrstica je primer.

| Enota | Vir ali avtor | Datum | Licenca / dostop | Kaj sem spremenila |
|---|---|---|---|---|
| `moj-zemljevid.jpg` | »Katastrska mapa k. o. Celje, 1825«, neznan avtor; prek portala Kamra, prevzeto z Wikimedia Commons | 1825 | javna domena | nič, izvirnik je nespremenjen |
| ortofoto DOF025 | Geodetska uprava RS, javni WMS | prevzeto LLLL-MM-DD | prosto dostopno, obvezna navedba vira | nič, storitev se ne prenaša |
| `Katastrska_mapa_k.o._Celje,_1825.jpg.points` | lastno delo | 8-9-2026 | CC BY 4.0 | tri kontrolne točke, ročno določene |

Za vsako kontrolno točko si zapiši še tole. Brez zadnjega stolpca postopka ne more ponoviti nihče.

| Polje | Obvezno? | Primer | Pomen |
|---|---:|---|---|
| `id` | da | `P1` | Oznaka, ki jo uporabiš tudi na karti. |
| `opis` | da | `stolnica sv. Danijela` | Kaj je ta stvar v prostoru. |
| `odstopanje_m` | da | `2` | Odstopanje po prilagoditvi, v metrih. |
| `tip` | da | `preverjeno` | Kaj točno si kliknil/a. |

## Orodja in različice

- [QGIS](https://qgis.org/) — nalaganje slojev, georeferenciranje, prikaz in izvoz. Uporabljena različica **4.2.1**.
- Ortofoto: javni WMS Geodetske uprave RS, `https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms`, sloj `DOF025`.
- Neobvezno: vtičnik **AGIS**, ki naloži slovenske podlage in historične karte naenkrat. Na voljo je samo za QGIS 3.x.

Različica je pomembna v eni stvari: v starejših izdajah je bil georeferencer vtičnik in je bil pod menijem `Layer`. Od QGIS 3.16 naprej je vgrajen in je pod **`Raster`**.

---

## Postopek

### 1. Poišči star zemljevid

Na Wikimedia Commons je kategorija [Franciscan cadastral maps of Slovenia](https://commons.wikimedia.org/wiki/Category:Franciscan_cadastral_maps_of_Slovenia) — 99 listov, vsi v javni domeni, torej jih smeš uporabiti.

Odpri list, ki ga hočeš. **Pod sliko klikni na *Original file***, šele potem desni klik in *Shrani sliko kot*. Če preneseš tisto, kar vidiš na strani, dobiš pomanjšan predogled in kasneje ne bo šlo.

Shrani jo v svojo mapo. Naredi si novo mapo samo za to nalogo. **Izvirne slike od zdaj naprej ne popravljaj, ne obrezuj in ne shranjuj znova** — kontrolne točke so vezane na njene piksle.


### 2. Odpri QGIS in nastavi koordinatni sistem

1. Odpri QGIS in na začetni strani klikni **New Project** (ali v meniju `Project ▸ New`).
2. `Project ▸ Properties…`
3. Levo klikni zavihek **CRS**.
4. V polje **Filter** vpiši `3794`.
5. Izberi **EPSG:3794 — Slovenia 1996 / Slovene National Grid**.
6. `OK`

Spodaj desno mora zdaj pisati `EPSG:3794`.

### 3. Dodaj današnji ortofoto

Gre na dva načina. Prvi deluje povsod, drugi samo v QGIS 3.x, prinese pa poleg ortofota še historične podlage.

**Prvi način — povezava na eProstor**

1. `Layer ▸ Add Layer ▸ Add WMS/WMTS Layer…`
2. Klikni `New`.
3. V polje **Name** vpiši karkoli, na primer `GURS`.
4. V polje **URL** vpiši `https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms`
5. `OK`, nato `Connect`.
6. V seznamu izberi **DOF025**.
7. `Add`, nato `Close`.

> Dokler je polje **Name** prazno, je gumb `OK` siv. Tega naslova tudi ne odpiraj v brskalniku — vrne napako, ker ni spletna stran, ampak naslov storitve.

**Drugi način — vtičnik AGIS**

1. `Plugins ▸ Manage and Install Plugins…`
2. V levem stolpcu klikni `All` ali `Not Installed`, poišči **AGIS**, klikni `Install`.
3. Če ga v seznamu ni, ga prenesi kot datoteko ZIP s spleta in ga namesti prek `Plugins ▸ Manage and Install Plugins ▸ Install from ZIP`.

AGIS naloži skupine slojev za Slovenijo, med njimi *Podlage* in *Historicne podlage*. Trenutno je na voljo samo različica za QGIS 3.x, za 4.x še ni posodobljen.

Ko je ortofoto naložen, se pripelji na svoje območje — spodaj v polje **Coordinate** lahko vpišeš koordinato in pritisneš `Enter`, ali pa se preprosto približaš z miško.

### 4. Odpri Georeferencer in naloži star zemljevid

Georeferenciranje pomeni, da sliki poveš, kam na svet spada.

1. `Layer ▸ Georeferencer…`
2. Odpre se novo okno **Georeferencer**.
2. V novem oknu klikni prvo ikono na levi strani `File ▸ Open Raster…` (bližnjica `Ctrl+O`).
3. Izberi svojo sliko starega zemljevida (JPG), klikni `Odpri`.
Stari zemljevid se prikiaže v zgornjem delu okna. 

> Če ne najdeš Georeferencerja pod Layer, ga poišči pod Raster.

### 5. Kontrolne točke 

Kontrolna točka je par: isto mesto na starem zemljevidu in na ortofotu.

Za vsako točko:

1. V oknu Georeferencer klikni `Edit ▸ Add Point` (ikona z rumeno piko).
2. Na **starem zemljevidu** klikni na objekt, ki ga prepoznaš — na primer cerkev.
3. Odpre se okence. Klikni gumb **From Map Canvas**.
4. QGIS te preusmeri v glavno okno. Klikni na **isto cerkev na ortofotu**.
5. `OK`

Ponovi trikrat. Točke naj bodo **razmaknjene** po območju, ne vse skupaj na kupu. Vsaka se sproti pojavi v tabeli **GCP table** na dnu okna.

Dobre točke: cerkev, grad, velika stavba, križišče starih ulic.
Slabe točke: drevesa, njive, rečni bregovi in mostovi — vse to se s časom premakne. So pa odlične za opazovanje, kako se je stvar spremenila čez leta.

> Če imaš dototeko s točkami že pripravljeno, jo naložiš v oknu `Georeferencer` ▸ `File ▸ Load GCP Points…` - izberi datoteko s koordinatnimi točkami ter klikni `Odpri`. Ne delaj obojega hkrati: če točke naložiš in potem še klikaš, jih QGIS sešteje in ista stavba šteje dvakrat. Končnica datoteke s točkami mora bit **.points**.

> Spodaj v tabeli **GCP table** se pojavijo tri vrstice. V stolpcu *Source X* se morajo izpisati številke. Če je tabela prazna, datoteka ni bila prebrana.

Za vsako si na list zapiši npr. »sredina cerkve«, »jugovzhodni vogal gradu«. Ker se rabi na koncu. 

### 6. Nastavi transformacijoin zaženi georeferenciranje

1. V okencu Georeferencer klikni `Settings ▸ Transformation Settings…`.
2. Pri **Transformation type** izberi `Helmert`.
3. Pri **Target CRS** izberi `EPSG:3794` (klikni na spustni meni ali na gumb z globusom in vpiši `3794`).
4. Pri **Output file** klikni gumb `…`, pojdi v mapo kjer bi shranil, v polje **Ime datoteke** vpiši npr.`celje-1825-georef-moj` in klikni `Shrani`.
5. Pri **Resampling method** izberi `Cubic (4x4 Kernel)`.
6. Obkljukaj **Load in project when done**.
7. Obkljukaj še **Save GCP points**.
8. Klikni `OK`.
9. Klikni zeleni gumb za start oz. play (▸)

Spodaj v oknu Georeferencer mora zdaj pisati `Transform: Helmert`. Če piše `None`, vrsta transformacije ni bila izbrana.

> **Zakaj Helmert?** Ker s tremi točkami druge transformacije vedno pokažejo odstopanje nič. To zgleda odlično, pove pa nič. Helmert dovoli samo premik, zasuk in povečavo, zato ostane pravo odstopanje, ki ga lahko izmeriš.

Ko se konča, se sloj sam naloži v projekt.

<details>
<summary>Če javi „Transform Failed: Could not read source image"</summary>

1. Preveri, ali je polje **Output file** res izpolnjeno.
2. Pretvori sliko v TIFF: `Raster ▸ Conversion ▸ Translate (Convert Format)`, izberi svojo sliko, izberi mesto za shranjevanje - `Run`. Potem v georeferencerju odpri ta `.tif` in **znova klikni točke** — ob menjavi slike se zbrišejo. 
3. Zapri okno Georeferencer in ga odpri na novo.

</details>

### 7. Pregledovanje ujemanja

1. V panelu **Layers** klikni z desno tipko na novi sloj.
2. Klikni **Zoom to Layer**.
3. Če sloj ni najvišji, ga z miško povleci nad `DOF025`.
4. Pritisni `F7`. Desno se odpre panel **Layer Styling**.
5. Prepričaj se, da je v spustnem meniju na vrhu panela izbran tvoj kataster.
6. Podrsaj do razdelka **Layer Rendering**.
7. Pri **Blending mode** izberi `Multiply`.
8. Podrsaj do **Resampling** in pri *Zoomed in* izberi `Bilinear`.

Če hočeš namesto tega drsnik prosojnosti: v levem stolpcu ikon panela *Layer Styling* klikni na ikono **Transparency** (ne na čopič) in premikaj **Global opacity**.

### 8. Preverjanje in meritev

Približaj se na merilo `1:2500` in poglej:

| | Kaj pogledaš | Kaj boš opazil/a |
|---|---|---|
| 1 | **Stari trg** | oblika se ujema, hiše stojijo na istem mestu |
| 2 | **Stolnica sv. Danijela** | najboljše ujemanje |
| 3 | **Knežji dvor** | oblika se ujema, stavba stoji na istem mestu |
| 4 | **Vodni stolp** | stoji še danes, a ga list iz 1825 sploh ne prikaže |
| 5 | **Železniška postaja** | leta 1825 so tam njive; proga je bila zgrajena 1846 |
| 6 | **Most čez reko** | most iz 1825 je izginil, danes je na tem mestu jez |

Prvi trije so **preverjanje poravnave** — tam meriš in številko zapišeš. Zadnji trije so **spremembe v prostoru** — tam ni kaj meriti, samo opišeš, kaj se je zgodilo.

**Kako meriš:**

1. Pritisni `Ctrl+Shift+M` (orodje *Measure Line*)
2. V okencu **Measure** pri **Units** izberi `Meters`.
2. Klikni na lego, kot je narisana na starem zemljevidu.
3. V panelu Layers **odkljukaj stari zemljevid** — merilna linija ostane, spodaj je gola fotografija.
4. Klikni na isto stvar, kot je danes.
5. Odčitaj **Total** in številko zapiši - nato klkni `New` za meritev novih točk.
6. Stari zemljevid spet vklopi. Za novo meritev klikni z desno tipko, da prekineš linijo.

> Ne klikaš na dve piki na zaslonu. Obe legi sta na istem mestu, ena čez drugo — isti objekt vidiš dvakrat, narisanega in fotografiranega. Zato med drugim klikom stari zemljevid ugasneš.

### 9. Naredi točkovni sloj z oznakami

1. Klikni `Layer ▸ Create Layer ▸ New GeoPackage Layer…`.
2. Pri **File base** klikni `…`, izberi mapo, kjer to shranjuješ (pri meni je Qgis_postopek/datoteke).
3. Pri **Table name** vpiši ime npr. `Oznake`.
4. Pri **Geometry type** izberi `Point`.
5. Klikni gumb z globusom in nastavi CRS na `EPSG:3794`.
6. V razdelku *New Field* dodaj polja: pri **Name** vpiši `id`, tip `Text`, klikni `Add to Fields List`. Ponovi za `tip`, `opis` in `opomba` (vsi `Text`) ter za `odstopanje_m` (tip `Decimal number`).
7. Klikni `OK`.
8. V panelu Layers klikni na novi sloj, nato klikni ikono s svinčnikom **Toggle Editing**.
9. Klikni ikono **Add Point Feature** (zgoraj, dve ikoni desno od **Toggle Editing**) in klikaj na karto tam, kjer si meril/a. Po vsakem kliku izpolni okence z atributi npr.:
`Id` - `P1`
`Opis` - `Stari trg`
`Opomba` - `Oblika se ujema, hiše stojijo na istem mestu`
`Odstopanje_m` - `št. ki si jo izmeril/a v prejšnem koraku`.
10. Ko končaš, spet klikni svinčnik in potrdi shranjevanje.
11. Oznakam lahko spremeniš simbol, dodaš besede: z dvojnim klikom ali desnim klikom na sloj **Oznake** - `Properties`- odpre se ti novo okno. V levem stolpu izberi `Symbology`, tu lahko spremeniš simbol, barvo, velikost oznak. Pod `Symbology` je zavihtem `Labels` - tu spremeniš iz `No labels` na `Single Labels`. Izbereš lahko barvo, obliko, velikost pisave, pa tudi kaj bo pisalo na karti pri oznaki.

> Če opaziš, da si se na začetku zmotil/a pri id, opisu, opombi ipd.: v slojih označi sloj, ki je nastal v tem koraku **(Oznake)** - desni klik ter poišči `Open Attribute Table`. Ko se ti novo okence odpre stisni svinčnik in popravi podatke. Ko končas s popravljanjem pa pritisni disketo **(Save)** in je to to.

### 10. Naredi karto

1. `Project ▸ Save` — shrani projekt, sicer izgubiš vse nastavitve.
2. `Project ▸ New Print Layout…`, vpiši ime, `OK`.
3. `Add Item ▸ Add Map` in z miško nariši pravokotnik. Pusti prostor spodaj.
4. `Add Item ▸ Add Scale Bar` — merilo.
5. `Add Item ▸ Add North Arrow` — sever.
6. `Add Item ▸ Add Legend` — legenda.
> Če želiš nekatere sloje izbrisati iz legende: Klikni na legendo, desno pri **Legend Items** - V prvem okencu mora pisati `Manual` in z gumbom `−` pod kvadratkom odstrani vnose. Z dvojnim klikom lahko vnos tudi preimenuješ.
8. `Layout ▸ Export as Image…`, izberi `PNG`, pri **Export resolution** vpiši `300` dpi.

To je končni izdelek.

## Izhod

```text

---

## Interpretacija in omejitve

Prednost je da sta oba vira v istem koordinatnem sistemu, zato ju lahko primerjamo z merjenjem in ne le na približno.

Ena izguba	iz risanega dokumenta je nastala slika. Parcelne meje in številke niso podatek, dokler jih ne prepišeš, georeferenciranje pa je list še zavrtelo in prevzorčilo. Odstopanje okoli 10 metrov **ne pomeni**, da se je nekaj v resnici premaknilo. Toliko je natančnost tega postopka pri tem gradivu.

En vir napake so	kontrolne točke na stavbah, katerih tloris se je spremenil, zato del odstopanja lahko pride iz tvoje izbire točke in ne iz zemljevida.

Premočna trditev	bi bila, da se je Savinja premaknila za 12 metrov. Ta številka je manjša od negotovosti.

Nekaterih stavb kot je npr. vodni stolp, na listu iz 1825 ni, ampak to **ne pomeni, da takrat ni stal**. Pomeni samo, da na listu ni izrisan — nekateri listi manjših stavb pogosto ne prikazujejo, zato je treba preveriti znamenitosti.


## Viri in pravice

**Star zemljevid.** List iz 1825 je v javni domeni, ker je avtorsko varstvo poteklo. 

**Ortofoto.** Storitev GURS je prosto dostopna, navedba vira je obvezna.

## Vaja zate

Vzemi drug list iz iste zbirke in ponovi postopek s tremi svojimi kontrolnimi točkami. Zapiši en primer dobrega ujemanja, en primer neujemanja z izmerjeno vrednostjo v metrih in eno omejitev, ki jo je razkril prav tvoj list.
