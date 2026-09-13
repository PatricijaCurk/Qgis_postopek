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


Star zemljevid je samo slika, ki nima določenih koordinat. V tem postopku mu bomo koordinate določili, ga položili čez današnji posnetek iz zraka in pogledali, kaj se ujema in kaj ne.

Za primer je uporabljen list franciscejskega katastra za Celje iz leta 1825, lahko pa vzamete kakršenkoli star zemljevid. Rezultat je ena karta, na kateri se vidi staro in novo stanje hkrati.

Postopek traja približno 1 uro in QGIS-a ni potrebno poznati vnaprej.

## Zakaj primerjava slik?

Star zemljevid ni fotografija preteklosti: Franciscejski kataster je npr. nastal zaradi zemljiškega davka, zato so parcele izmerjene skrbno, stavbe pa poenostavljene ali izpuščene.

Ko tak list položimo na ortofoto, hkrati počnemo dvoje: postavljamo vir v prostor in trdimo, da je nekaj takrat stalo na določenem mestu ter, da lep prekriv še ne pomeni, da je zemljevid natančen na meter. Zato na koncu odstopanje izmerimo in ga zapišemo.

Več o tem, zakaj prostor v humanistiki ni nevtralen okvir, je v poglavju [GIS in prostorska humanistika](https://damjan-popic.github.io/digital-humanities-handbook/sl/chapters/gis-spatial-humanities/).

## Kaj potrebuješ za primerjavo historičnega zemljevida s sodobnim ortofotom?

- **QGIS** — brezplačen, prenesi ga s [qgis.org](https://qgis.org/). Računa ne rabiš.
- **Star zemljevid** kot slika (JPG/JPEG ali TIFF). Kje ga dobiš, piše v 1. koraku.
- **Internet** za ortofoto.
- **Uro tvojega časa.**

Pomembno je, da ima tvoj zemljevid vsaj **tri objekte, ki stojijo še danes** in so na njem jasno vidni kot na primer cerkev, grad, obzidje. 


## Vhodni podatki in izvor

| Enota | Vir ali avtor | Datum | Licenca / dostop | Kaj sem spremenila |
|---|---|---|---|---|
| `Katastrska_mapa_k.o._Celje,_1825.jpg` | »Katastrska mapa k. o. Celje, 1825«, neznan avtor; prek portala Kamra, prevzeto z Wikimedia Commons | 1825 | javna domena | nič, izvirnik je nespremenjen |
| ortofoto DOF025 | Geodetska uprava RS, javni WMS | prevzeto 8-9-2026 | prosto dostopno, https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms | nič, storitev se ne prenaša |
| `Katastrska_mapa_k.o._Celje,_1825.jpg.points` | lastno delo | 8-9-2026 | CC BY 4.0 | tri kontrolne točke, ročno določene |

## Orodja in različice

- [QGIS](https://qgis.org/) — nalaganje slojev, georeferenciranje, prikaz in izvoz. V tem postopku je uporabljena različica **4.2.1**, lahko pa tudi starejšo, ampak se postopek mogoče malo spremeni.
- Ortofoto: javni WMS Geodetske uprave RS, `https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms`, sloj `DOF025`.
- Neobvezno: vtičnik **AGIS**, ki naloži slovenske podlage in historične karte naenkrat. Trenutno je na voljo samo za QGIS 3.x.

---

## Postopek

### 1. Poišči star zemljevid

Na Wikimedia Commons je kategorija [Franciscan cadastral maps of Slovenia](https://commons.wikimedia.org/wiki/Category:Franciscan_cadastral_maps_of_Slovenia) — 99 listov, vsi so v javni domeni, torej jih lahko uporabljaš.

Odpri list, po tvojem okusu. **Pod sliko klikni na *Original file***, šele potem desni klik in *Shrani sliko kot*. Če preneseš tisto, kar vidiš na strani, dobiš pomanjšan predogled, kar je lahko težava za kasnejše točke.

Shrani jo v svojo mapo, ki si jo narediš za to nalogo. **Izvirne slike od zdaj naprej ne popravljaj, ne obrezuj in ne shranjuj znova** saj so kontrolne točke vezane na njene piksle.


### 2. Odpri QGIS in nastavi koordinatni sistem

1. Odpri QGIS in na začetni strani klikni **New Project** (ali v meniju `Project ▸ New`).
2. `Project ▸ Properties…`
3. Levo klikni zavihek **CRS**.
4. V polje **Filter** vpiši `3794`.
5. Izberi **EPSG:3794 — Slovenia 1996 / Slovene National Grid**.
6. `OK`

Spodaj desno mora zdaj pisati `EPSG:3794`.

### 3. Dodaj današnji ortofoto

Tu sta dva načina. Prvi deluje povsod, drugi samo v QGIS 3.x, prinese pa poleg ortofota še historične podlage ipd.

**Prvi način: povezava na eProstor**

1. `Layer ▸ Add Layer ▸ Add WMS/WMTS Layer…`
2. Klikni `New`.
3. V polje **Name** vpiši karkoli, na primer `GURS`.
4. V polje **URL** vpiši `https://ipi.eprostor.gov.si/wms-si-gurs-dts/wms`
5. `OK`, nato `Connect`.
6. V seznamu izberi **DOF025**.
7. `Add`, nato `Close`.

> Dokler je polje **Name** prazno, je gumb `OK` siv. 

**Drugi način: vtičnik AGIS**

1. `Plugins ▸ Manage and Install Plugins…`
2. V levem stolpcu klikni `All` ali `Not Installed`, poišči **AGIS**, klikni `Install`.
3. Če ga v seznamu ni, ga prenesi kot datoteko ZIP s spleta in ga namesti prek `Plugins ▸ Manage and Install Plugins ▸ Install from ZIP`.

AGIS naloži skupine slojev za Slovenijo, med njimi *Podlage* in *Historicne podlage*. Trenutno je na voljo samo različica za QGIS 3.x, za 4.x še ni posodobljen.

Ko je ortofoto naložen, se pripelji na svoje območje. Spodaj v polje **Coordinate** lahko vpišeš koordinato in pritisneš `Enter`, ali pa se preprosto približaš z miško.

### 4. Odpri Georeferencer in naloži star zemljevid

Georeferenciranje pomeni, da sliki poveš, "kam na svet spada" torej, da umestiš sliko na pravo mesto.

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
2. Na **starem zemljevidu** klikni na objekt, ki ga prepoznaš kot je npr. cerkev.
3. Odpre se okence. Klikni gumb **From Map Canvas**.
4. QGIS te preusmeri v glavno okno. Klikni na **isto cerkev na ortofotu**.
5. `OK`

Ponovi trikrat. Točke naj bodo **razmaknjene** po območju. Vsaka se sproti pojavi v tabeli **GCP table** na dnu okna.

Dobre točke so cerkev, grad, velika stavba, križišče starih ulic.
Slabe točke so drevesa, njive, rečni bregovi in mostovi. Vse to se s časom spreminja in zato ne spadajo pod kategorijo dobrih točk. So pa odlične za opazovanje spremenitev čez leta.

> Če imaš dototeko s točkami že pripravljeno, jo naložiš v oknu `Georeferencer` ▸ `File ▸ Load GCP Points…`. Izberi datoteko s koordinatnimi točkami ter klikni `Odpri`. Ne delaj obojega hkrati: če točke naložiš in potem še klikaš, jih QGIS sešteje in ista stavba šteje dvakrat. Končnica datoteke s točkami mora bit **.points**.

> Kako lahko sam datoteko s točkami narediš? Potrebuješ Excel, ali pa beležničo. Važno je, da je končnica datoteke prava. X, Y koordinate lahko dobiš na Qgisu s pomikanjem miške, lahko pa tudi na spletu. Najpreprostejši način za uvoz točk v Qgis je uporaba preglednice v formatu CSV (Comma-Seperated Values) ali Excel (.xlsx). Datoteko lahko ustvarite v Excelu, Google preglednicah ali Beležki. Točke za slovenski sistem (ESPG:3794) vpišite vrednosti vzhodne (X/East) in severne (Y/North) koordinate. V Excelu uporabite `.` in ne `,`. Na koncu pa Shrani kot, ter poišči **.cvs**.

> Spodaj v tabeli **GCP table** se pojavijo tri vrstice. V stolpcu *Source X* se morajo izpisati številke. Če je tabela prazna, datoteka ni bila prebrana.

Za vsako si nekam zapiši kaj predstavlja npr. sredina cerkve, jugovzhodni vogal gradu, ker boš potreboval/a v naslednjih korakih, da boš vedel/a kaj pišeš oz. meriš. 

### 6. Nastavi transformacijo in zaženi georeferenciranje

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

Približaj se na merilo, ki ti najbolj ustreza za pregledovanje območja npr. `1:2500`. Pri celjskem katastru izgleda pregled takole:

| | Kaj pogledaš | Kaj boš opazil/a |
|---|---|---|
| 1 | **Stari trg** | oblika se ujema, hiše stojijo na istem mestu |
| 2 | **Stolnica sv. Danijela** | najboljše ujemanje |
| 3 | **Knežji dvor** | oblika se ujema, stavba stoji na istem mestu |
| 4 | **Vodni stolp** | stoji še danes, a ga list iz 1825 sploh ne prikaže |
| 5 | **Železniška postaja** | leta 1825 so tam njive; proga je bila zgrajena 1846 |
| 6 | **Most čez reko** | most iz 1825 je izginil, danes je na tem mestu jez |

Prvi trije so **preverjanje poravnave**, kar je izmerjeno in številko si zapišeš. Zadnji trije so **spremembe v prostoru**, pri katerih ne izmerimo nič, ampak samop opišemo kaj opazimo.

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
`Tip` - `Preverjeno`
`Opomba` - `Oblika se ujema, hiše stojijo na istem mestu`
`Odstopanje_m` - `št. ki si jo izmeril/a v prejšnem koraku`.
10. Ko končaš, spet klikni svinčnik in potrdi shranjevanje.
11. Oznakam lahko spremeniš simbol, dodaš besede: z dvojnim klikom ali desnim klikom na sloj **Oznake** - `Properties`- odpre se ti novo okno. V levem stolpu izberi `Symbology`, tu lahko spremeniš simbol, barvo, velikost oznak. Pod `Symbology` je zavihtem `Labels` - tu spremeniš iz `No labels` na `Single Labels`. Izbereš lahko barvo, obliko, velikost pisave, pa tudi kaj bo pisalo na karti pri oznaki.

> Če opaziš, da si se na začetku zmotil/a pri id, opisu, opombi ipd.: v slojih na levi strani označi sloj, ki je nastal v tem koraku **(Oznake)** - desni klik ter poišči `Open Attribute Table`. Ko se ti novo okence odpre stisni svinčnik in popravi podatke. Ko končas s popravljanjem pa pritisni disketo **(Save)**.

### 10. Naredi karto

1. `Project ▸ Save` — shrani projekt, sicer izgubiš vse nastavitve.
2. `Project ▸ New Print Layout…`, vpiši ime, `OK`. V okencu dobiš bel pravokotnik.
3. `Add Item ▸ Add Map` in z miško nariši pravokotnik. Pusti prostor spodaj.
4. `Add Item ▸ Add Scale Bar` — merilo.
5. `Add Item ▸ Add North Arrow` — puščica za sever.
6. `Add Item ▸ Add Legend` — legenda.
> Če želiš nekatere sloje izbrisati iz legende: Klikni na legendo, desno pri **Legend Items** - V prvem okencu mora pisati `Manual` in z gumbom `−` pod kvadratkom odstrani vnose. Z dvojnim klikom lahko vnos tudi preimenuješ.
8. `Layout ▸ Export as Image…`, izberi `PNG`, pri **Export resolution** vpiši `300` dpi.

In to je tvoj končni izdelek. 

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
```

Končni izdelek je `Primerjalna_karta.png`. Vse ostalo je gradivo, iz katerega je nastala.     

---


## Interpretacija in omejitve

Ker sta oba vira vpeta v isti koordinatni sistem, omogočata neposredno merjenje in primerjavo, kar presega raven zgolj prostih ocen. Kljub te prednosti pa moramo pri tej analizi upoštevati specifično naravo historičnega gradiva in tehnične omejitve postopka.

Prostorska natančnost in georeferenciranje: Prelaganje ročno risanega katastrskega zemljevida v sodoben prostor terja svoj davek. Parcelne meje in signature postanejo podatki šele z vektorizacijo, sam postopek georeferenciranja (rotacija, skaliranje, prevzorčenje) pa list neizogibno popači. Odstopanje okoli 10 metrov še ne pomeni, da se je prostor v resnici zamaknil. To je zgolj meja prostorske natančnosti, ki jo to gradivo omogoča.

Problem kontrolnih točk: Pomemben vir napake izhaja iz izbire točk na stavbah, katerih tlorisi so se skozi desetletja spreminjali zaradi prezidav ali rušitev. Del izmerjenega odstopanja je tako pogosto posledica naše izbire točk in ne dejanske napake pri prvotnem zemljevidu.

Dinamika rečne struge: Trditev, da se je Savinja premaknila za 12 metrov, bi bila metodološko premočna. Ta številka je namreč manjša ali primerljiva z merilno negotovostjo samega postopka prekrivanja.

Odsotnost objektov: Če določenega objekta (npr. vodni stolp) ni na listu iz leta 1825, še ne pomeni, da takrat ni obstajal. Kartografski viri takega tipa so izpuščali manjše objekte ali pa niso bili namenjeni prikazu celotne topografije, zato je za zanesljiv sklep nujno preverjanje še drugih virov in podatkov. 


## Povezava s priročnikom

- [GIS in prostorska humanistika](https://damjan-popic.github.io/digital-humanities-handbook/sl/chapters/gis-spatial-humanities/) — zakaj je umeščanje vira v prostor že samo po sebi trditev; ta postopek je njegov najmanjši izvedljivi primer.
- [Podatki, metapodatki in modeli](https://damjan-popic.github.io/digital-humanities-handbook/sl/chapters/data-metadata-models/) — od tod izhaja zahteva, da je vsaka izpeljana datoteka sledljiva do svojega vira.
- [Etični kontrolni seznam](https://damjan-popic.github.io/digital-humanities-handbook/sl/resources/ethics-checklist/) — preden karto objaviš.

## Viri in pravice

**Star zemljevid.** List iz 1825 je v javni domeni, ker je avtorsko varstvo poteklo. 

**Ortofoto.** Storitev GURS je prosto dostopna, navedba vira je obvezna.

## Vaja zate

Vzemi drug list iz iste zbirke in ponovi postopek s tremi svojimi kontrolnimi točkami. Zapiši en primer dobrega ujemanja, en primer neujemanja z izmerjeno vrednostjo v metrih in eno omejitev, ki jo je razkril prav tvoj list.
