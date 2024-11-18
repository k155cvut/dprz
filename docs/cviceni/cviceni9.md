---
icon: material/numeric-9-box
---

<style>
  .md-typeset__scrollwrap {text-align: center ;}
  table th {text-align: center !important;}
  table td {text-align: center !important;}
  h2 {font-weight:700 !important;}                                                                   /* Pokus – zmena formatu nadpisu 2 */
  figcaption {font-size:12px;margin-top:5px !important;text-align:center;line-height:1.2em;}         /* Formatovani Popisku obrazku */
  hr.l1 {background-color:var(--md-primary-fg-color);height:2px;margin-bottom:3em !important;}       /* Formatovani Break Line – LEVEL 1 */
  img,iframe {filter:drop-shadow(0 10px 16px rgba(0,0,0,0.2)) drop-shadow(0 6px 20px rgba(0,0,0,0.2)) !important; object-fit:contain;} /* Stin pod obrazky a videi */

  /* TLACITKA */
  .md-button {text-align:center;transition: all .1s ease-in-out !important;}  /* Button – zarovnani textu */
  .md-button:hover {transform: scale(1.04);opacity:.8;background-color:var(--md-primary-fg-color) !important;border-color:var(--md-primary-fg-color) !important;color:var(--md-primary-bg-color) !important;/*filter: brightness(80%);*/}            /* Button Hover – animace zvetseni a zmeny barvy */
  .md-button:focus {opacity:.8;background-color:var(--md-primary-fg-color) !important;border-color:var(--md-primary-fg-color) !important;color:var(--md-primary-bg-color) !important;}                                                                /* Button Focus – stejny vzhled jako hover */
  .url-name {line-height:1.2;/*padding-top:5px !important;*/}                 /* Button s URL */
  .url-name span:first-child {font-size:.7em; font-weight:300;}               /* Button s URL – format*/
  .url-name span.twemoji {vertical-align:-0px;}                               /* Button s URL – zarovnani ikony*/
  .md-button.button_smaller {font-size:smaller; padding:1px 5px;}             /* Mensi button (bez URL) */

  /* FLEXBOXY */
  .process_container {display:flex !important; justify-content:center; align-items:center; column-gap:calc((100vw * 0.03) - 6px);} /* Kontejner pro content = FlexBox */
  .process_container div {display:flex;}                                                                                           /* Obsah (obrazky a sipky) */
  .process_container .process_icon {width:/*40px*/calc((100vw * 0.01) + 25px); flex-shrink:0;filter:none !important;}              /* Velikost ikony (bacha na mobily) */
  .process_container img {max-height:600px; display:flex;}                                    /* Obrazky ve flexboxech maji maximalni vysku */
</style>

# Tepelná analýza pomocí dat Landsat

<hr class="l1">

## Cíl cvičení

- Seznámit se s daty Landsat
- Umět spočítat povrchovou teplotu na základě dat Landsat

<hr class="l1">

## Základní pojmy

- **LST** - Land Surface Temperature - radiační teplota zemského povrchu měřená ve směru senzoru
- **LSE** - Land Surface Emissivity - bezrozměrná veličina udávající jak účinně povrch vyzařuje tepelné infračervené záření

### Program Landsat

Program Landsat je nejdéle probíhající projekt zaměřený na pozorování Země. Scény zobrazující zemský povrch poskytuje nepřetržitě již od roku 1972. Během více než padesáti let bylo postupně vypuštěno devět družic s názvy Landsat 1 až Landsat 9. V současné době jsou aktivní družice Landsat 8 a Landsat 9 a z části i Landsat 7. Program společně řídí agentury <a href="https://www.nasa.gov/" target="_blank"> **NASA**</a> a <a href="https://www.usgs.gov/" target="_blank"> **U.S. Geological Survey**</a>. Veškerá data jsou volně dostupná. Více o historii, současnosti i budoucnosti programu Landsat se můžete dozvědět na následujích webových stránkách:

[:material-open-in-new: Landsat Satellite Missions](https://www.usgs.gov/landsat-missions/landsat-satellite-missions){ .md-button .md-button--primary .button_smaller target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

![](../assets/cviceni9/01_Landsat_timeline.png)
{: style="margin-bottom:0px;" align=center }
<figcaption>Mise Landsat</figcaption>

<div style="text-align: center;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/7XKVSTX1vdE?si=HvOfNrfjRDOqKuzn" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

[<span>:material-open-in-new: youtube.com</span><br>Landsat: Celebrating 50 Years (Extended Edition)](https://www.youtube.com/watch?v=gfUYFPWYwXI){ .md-button .md-button--primary .url-name target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

### Landsat 8/9

V rámci tohoto cvičení budeme pracovat s daty z jedné z těchto dvou družic. Obě družice nesou téměř identické vybavení s tím, že přístroje na Landsatu 9 jsou vylepšenými replikami přístrojů družice Landsat 8. Na obou družicích najdeme tedy dva senzory (OLI, resp. OLI-2 a TIRS, resp. TIRS-2). OLI je zkratkou pro **Operational Land Imager**, který snímá 8 spektrálních pásem a jedno pásmo panchromatické. TIRS je zktratkou pro **Thermal Infrared Sensor**, který měří tepelné záření vyzařované zemským povrchem ve dvou termálních infračervených pásmech.

<div class="process_container">
<iframe width="560" height="315" src="https://www.youtube.com/embed/1DLDjxpPElA?si=5CCyCYFM_ArWyZ9o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/DGE-N8_LQBo?si=Ofntz4jPTBGPvdi8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

[<span>:material-open-in-new: youtube.com</span><br>Landsat 9: Continuing the Legacy](https://www.youtube.com/playlist?list=PL_8hVmWnP_O3WFxfAa_xBlsmGq87HMhFW){ .md-button .md-button--primary .url-name target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

Jednotlivá pásma Landsat 8 a 9 shrnuje následující tabulka:

![](../assets/cviceni9/02_landsat8_9_bands.png){ style="height:558px;"}
{: style="margin-bottom:0px;" align=center }

<hr class="l1">

## Stažení dat Landsat

Data Landsat budeme stahovat z portálu <a href="https://earthexplorer.usgs.gov/" target="_blank"> **USGS EarthExplorer**</a>, kde je potřeba se nejprve zaregistrovat. Registraci provedeme přes tlačítko ***Login*** v pravé horní části webových stránek (vlastní zkušenost ukázala, že registrace zde není zrovna nejjednodušší proces).

![](../assets/cviceni9/03_EarthExplorer.png)
{: style="margin-bottom:0px;" align=center }

Když se nám podařilo se zaregistrovat, můžeme přejít k vybrání oblasti, ze které budeme chtít data stáhnout. Cílem tohoto cvičení je analyzovat tepelné ostrovy, takže si najdeme nějaké větší město kdekoliv na světě. Následně si naše území vyznačíme obyčejným klikáním do mapového okna.

![](../assets/cviceni9/04_location.png)
{: style="margin-bottom:0px;" align=center }

Zároveň si ve spodní části záložky **Search Criteria** nastavíme ***Date Range*** na období letních měsíců a ***Cloud Cover*** na nějakou rozumnou hodnotu (chceme ideálně bezoblačnou scénu).

![](../assets/cviceni9/05_date_range.png){ style="height:350px;"}
![](../assets/cviceni9/06_cloud_cover.png){ style="height:161px;"}
{: .process_container}

Dále se přepneme do záložky **Data Sets**, kde zvolíme, jaká data budeme chtít stahovat. V našem případě zvolíme **Landsat** → **Landsat Collection 2 Level-1** → **Landsat 8-9 OLI/TIRS C2 L1**. Jedná se o data bez atmosferických korekcí, která pro analýzu povrchové teploty budeme potřebovat.

![](../assets/cviceni9/07_select_data.png){ style="height:405px;"}
{: style="margin-bottom:0px;" align=center }

Záložku **Addittional Criteria** můžeme přeskočit a rovnou se přepnout do záložky **Results**. Zde si již můžeme vybrat jednu z nabízených scén. Pomocí funkce ***Show Browse Overlay*** se můžeme podívat, jak dobře se daná scéna s naším uzemím překrývá.

![](../assets/cviceni9/08_search_results.png)
{: style="margin-bottom:0px;" align=center }

Pokud jsme se scénou spokojeni, klikneme na ikonu ***Download Options***. Zde si zvolíme, jaké soubory chceme stahovat. V našem případě si stáhneme všechny soubory. Klikneme tedy na ***Product Options*** a stáhneme si celou Landsat kolekci.

![](../assets/cviceni9/09_download_options.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni9/10_download_files.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni9/11_all_collection.png)
{: .process_container}

K *Level 1* produktu si nicméně stáhneme i *Level 2* produkt (pokud je k dispozici). V záložce **Data Sets** stačí jen změnit data na **Landsat** → **Landsat Collection 2 Level-2** → **Landsat 8-9 OLI/TIRS C2 L2** a poté kliknout zpátky do **Results**, kde si vybereme korespondující scénu s *Level 1* produktem. *Level 2* produkt v sobě má již pásmo odpovídající ***Land Surface Temperature***. K jeho hodnotám se ale došlo trochu jinak, než jak k nim dojdeme my. V závěru cvičení můžeme ale naše výsledky s již předzpracovaným pásmem porovnat.

<hr class="l1">

## Otevření a zobrazení dat

Pokud jsme stahovali všechna data najednou (a ne třeba jen vybraná pásma), tak jsme data obdrželi ve formátu *TAR*. Jedná se o podobný formát jako *ZIP*, ale nedochází zde ke kompresi. Data v tomto formátu lze rozbalit např. pomocí nástroje <a href="https://www.7-zip.org/" target="_blank"> **7-Zip**</a>. Pokud 7-Zip v učebně není nainstalovaný, lze použít i **Total Commander**.

![](../assets/cviceni9/12_7zip.png){ style="height:359px;"}
{: style="margin-bottom:0px;" align=center }

Po rozbalení dat si do ArcGIS Pro nahrajeme pásma B2, B3 a B4 a vytvoříme si z nich RGB kompozit pomocí funkce [:material-open-in-new: Composite Bands](https://pro.arcgis.com/en/pro-app/latest/tool-reference/data-management/composite-bands.htm){ .md-button .md-button--primary .button_smaller target="_blank"}. Připomínám, že pracujeme s *Level 1* daty.

![](../assets/cviceni9/13_rgb_scene.png)
{: style="margin-bottom:0px;" align=center }

<hr class="l1">

## Výpočet povrchové teploty

Výpočet povrchové teploty se bude skládat z celkem šesti kroků, které si postupně projdeme. Základem výpočtu bude termální pásmo **B10**, které si nejprve tedy přidáme do ArcGIS Pro.

### 1) Převod digitálních hodnot na TOA záření

Prvním krokem je, že si převedeme digitální hodnoty pixelů v pásmu **B10** na hodnoty záření na vrcholu atmosféry (*TOA = Top of Atmospheric*). Vzoreček pro tento převod je následující:

**TOA = M<sub>L</sub>·B<sub>10</sub> + A<sub>L</sub>**

kde:

**M<sub>L</sub>** je multiplikativní přeškálovací faktor, jehož hodnotu najdeme v metadatech daného Landsat produktu. Metada nalezneme v textovém souboru končícím na *_MTL.txt*. Konkrétně se pak jedná o hodnotu ***RADIANCE_MULT_BAND_10***

**B<sub>10</sub>** je termální pásmo B10

**A<sub>L</sub>** je aditivní přeškálovací faktor, který rovněž najdeme v metadatech, kde nese název ***RADIANCE_ADD_BAND_10***

K výpočtu použijeme nástroj [:material-open-in-new: Raster Calculator](https://pro.arcgis.com/en/pro-app/latest/tool-reference/image-analyst/raster-calculator.htm){ .md-button .md-button--primary .button_smaller target="_blank"}.

![](../assets/cviceni9/14_raster_calculator.png){ style="height:400px;"}
{: style="margin-bottom:0px;" align=center }

### 2) Převod TOA záření na jasovou teplotu

Pro výpočet jasové teploty (*Brightness Temperature*) použijeme následující vzorec:

**BT = K<sub>2</sub> / ln(K<sub>1</sub>/TOA + 1) − 273.15**

kde:

**K<sub>2</sub>** a **K<sub>1</sub>** jsou konstanty tepelné konverze, které najdeme v metadatech pod názvy ***K1_CONSTANT_BAND_10*** a ***K2_CONSTANT_BAND_10***

![](../assets/cviceni9/15_bt.png){ style="height:400px;"}
{: style="margin-bottom:0px;" align=center }

### 3) Výpočet NDVI

V dalším kroku vypočteme index NDVI. Ten bude následně sloužit k určení emisivity povrchu. Vzoreček pro NDVI již všichi známe. Pokud ne, tak jeho podoba je následující (pozor na jiné pořadí pásem oproti Sentinel-2):

**(NIR - Red)/(NIR + Red) = (B5 - B4)/(B5 + B4)**

A jelikož máme pásma jednotlivě a ne v jedné vrstvě, použijeme znovu *Raster Calculator*.

![](../assets/cviceni9/16_ndvi.png){ style="height:400px;"}
{: style="margin-bottom:0px;" align=center }

### 4) Zjištění podílu vegetace

Vzorec pro určení podílu vegetace má následující tvar:

**P<sub>V</sub> = ((NDVI - NDVI<sub>min</sub>)/(NDVI<sub>max</sub> - NDVI<sub>min</sub>))<sup>2</sup>**

Hodnoty **NDVI<sub>min</sub>** a **NDVI<sub>max</sub>** zjistíme např. přímo z panelu *Contents*.

![](../assets/cviceni9/17_minmax.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni9/18_pv.png){ style="height:400px;"}
{: .process_container}

### 5) Výpočet emisivity povrchu

V předposledním kroku pomocí NDVI vypočteme emisivitu zemského povrchu. Vzoreček je následující:

**e = 0,004 · P<sub>V</sub> + 0,986**

![](../assets/cviceni9/19_e.png){ style="height:400px;"}
{: style="margin-bottom:0px;" align=center }

### 6) Výpočet teploty povrchu

Posledním krokem je již výpočet samotné teploty povrchu.

**LST = BT / (1 + (λ·BT/14388)·ln(e))**

kde:

**λ** je vlnová délka vyzařovaného záření pásma B10. Použijeme průměrnou hodnotu z intervalu 10,6 - 11,19 → tedy **10,895** (hodnota je v mikrometrech, ale nemusíme ji převádět, protože konstanta 14388 je rovněž v mikrometrech).

![](../assets/cviceni9/20_lst.png){ style="height:400px;"}
{: style="margin-bottom:0px;" align=center }

<hr class="l1">

## Zhodnocení výsledků

Takto vypočtenou teplotu povrchu jsme obdrželi rovnou ve °C. Pokud jsme nikde neudělali chybu, tak vidíme, že výsledky jsou realistické. Je ale dobré si uvědomit, že se nejedná o stejnou teplotu jako je teplota vzduchu.

![](../assets/cviceni9/21_lst_values.png)
{: style="margin-bottom:0px;" align=center }

Dále je vhodné si data obarvit nějakým rozumným barevným schématem, které bude pro zobrazení teploty ituitivnější. V mém případě barevnou škálu trochu zkresluje drobná oblačnost v severní části scény a pro lepší vyzualizaci teplot ve městě by stálo za to scénu oříznout.

![](../assets/cviceni9/22_color_schema.png){ style="height:565px;"}
{: style="margin-bottom:0px;" align=center }

Při porovnání s NDVI můžeme vidět, že nižší teploty odpovídají místům se zelení a naopak místa bez zeleně mají teplotu o poznání vyšší (což samozřejmě dává smysl, ale je vždy dobré mít to čím podpořit).

![](../assets/cviceni9/23_rgb.png)
![](../assets/cviceni9/24_lst.png)
![](../assets/cviceni9/25_ndvi.png)
{: .process_container}
<figcaption>Vlevo - RGB scéna, uprostřed - povrchová teplota, vpravo - NDVI</figcaption>

<hr class="l1">

## Porovnání s Level 2 produktem

*Level 2* Landsat produkt nabízí již předzpracované termální pásmo, a pro zjištění povrchové teploty ho lze pouze přeškálovat pomocí správných koeficientů. Přidáme si tedy do ArcGIS Pro pásmo B10 z *Level 2* produktu a pomocí následujícího vzorce ho přeškálujeme:

**LST = M<sub>T</sub>·B<sub>10</sub> + A<sub>T</sub> - 273,15**

kde:

**M<sub>T</sub>** je multiplikativní přeškálovací faktor, který nalezneme v metadatech (textový soubor s koncovkou *_MTL.txt*) pod názvem ***TEMPERATURE_MULT_BAND_ST_B10***

**A<sub>T</sub>** je aditivní přeškálovací faktor, který nalezneme v metadatech pod názvem ***TEMPERATURE_ADD_BAND_ST_B10***

![](../assets/cviceni9/26_lst_lvl2.png){ style="height:400px;"}
{: style="margin-bottom:0px;" align=center }

V mém případě se výsledky trochu liší. V zástavbě byl rozdíl až 10 °C, v oblastech s vegetací se zjištěná teplota lišila zhruba o 5 °C a na vodních plochách byl rozdíl přibližně 3 °C. Otázkou tak zůstává, proč se výsledky takto liší, a který výsledek je přesnější.

![](../assets/cviceni9/27_lst_comparison.png){ style="height:226px;"}
{: style="margin-bottom:0px;" align=center }

Pro zajímavost zde přidávám odstavec z oficiální <a href="https://d9-wret.s3.us-west-2.amazonaws.com/assets/palladium/production/s3fs-public/media/files/LSDS-1619_Landsat8-9-Collection2-Level2-Science-Product-Guide-v5.pdf" target="_blank"> **příručky**</a> *Landsat 8-9*:

*"The Landsat 8-9 Surface Temperature (ST) product is generated from the single channel algorithm version 1.3.0 (derived from June 2017 version of RIT ST code). The Landsat 8-9 Collection 2 ST is derived from the Collection 2 Level 1 Thermal Infrared Sensor (TIRS) band 10 using Top of Atmosphere (TOA) Reflectance, TOA Brightness Temperature (BT), Advanced Spaceborne Thermal Emission and Reflection Radiometer (ASTER) Global Emissivity Dataset (GED) data, ASTER Normalized Difference Vegetation Index (NDVI) data, and atmospheric profiles of geopotential height, specific humidity, and air temperature extracted from reanalysis data."*

<hr class="l1">

## Úkol - Výpočet povrchové teploty

- Stáhněte si data Landsat 8/9 z doby letních měsíců pro nějaké větší město
- Spočtěte povrchovou teplotu jak z Level 1, tak i z Level 2 dat
- Výsledky vizualizujte pomocí vhodné barevné škály