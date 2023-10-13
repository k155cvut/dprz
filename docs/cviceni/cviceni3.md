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
  .process_container img {max-height:600px;}                                                                                       /* Obrazky ve flexboxech maji maximalni vysku */
</style>

# Vegetační (spektrální) indexy, maskování, detekce vodních ploch

<hr class="l1">

## Cíl cvičení

- Naučit se, co jsou spektrální indexy zač (prezentace: <a href="https://geo.fsv.cvut.cz/vyuka/155dprz/cv3/Vegeta%C4%8Dn%C3%AD%20(spektr%C3%A1ln%C3%AD)%20indexy.pdf" target="_blank"> **PDF**</a>)
- Umět spektrální indexy počítat
- Tvořit masky v softwaru SNAP
- Detekovat vodní plochy pomocí spektrálních indexů

<hr class="l1">

## Základní pojmy

- **Spektrální index**: Matematická kombinace dvou nebo více pásem s cílem získat informace o specifických vlastnostech zemského povrchu jako např. stav vegetace, kvalita vody, atd.

<hr class="l1">

## Počítání spektrálních indexů ve SNAP

Spektrálních indexů existuje celá řada. Příklady indexů můžeme nalézt např. <a href="https://www.indexdatabase.de/db/i.php" target="_blank"> **zde**</a> nebo <a href="https://custom-scripts.sentinel-hub.com/custom-scripts/sentinel/sentinel-2/" target="_blank"> **zde**</a>. V rámci tohoto cvičení se podíváme na následující indexy:

<table>
  <thead>
    <tr>
      <th><strong>Index</strong></th>
      <th><strong>Název</strong></th>
      <th><strong>Vzorec (Sentinel-2)</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>NDMI</strong></td>
      <td>Normalized Difference Moisture Index</td>
      <td>(NIR - SWIR<sub>1</sub>)/(NIR + SWIR<sub>1</sub>) = (B8 - B11)/(B8 + B11)</td>
    </tr>
    <tr>
      <td><strong>NDVI</strong></td>
      <td>Normalized Difference Vegetation Index</td>
      <td>(NIR - Red)/(NIR + Red) = (B8 - B4)/(B8 + B4)</td>
    </tr>
    <tr>
      <td><strong>NDWI</strong></td>
      <td>Normalized Difference Water Index</td>
      <td>(Green - NIR)/(Green + NIR) = (B3 - B8)/(B3 + B8)</td>
    </tr>
    <tr>
      <td><strong>AWEI<sub>sh</sub></strong></td>
      <td>Automated Water Extraction Index</td>
      <td>Blue + 2.5×Green - 1.5×(NIR + SWIR<sub>1</sub>) - 0.25×SWIR<sub>2</sub>) = B2 + 2.5×B3 - 1.5×(B8 + B11) - 0.25×B12</td>
    </tr>
  </tbody>
</table>

Pokud chceme spektrální indexy počítat ve SNAP, využijeme nástroj ***Band Maths...***, který nalezneme buď kliknutím pravým tlačítkem na vybraný produkt v ***Product Explorer***, nebo přes menu ***Raster*** → ***Band Maths...***

![](../assets/cviceni3/01_band_maths.png){ style="width:80%;"}
![](../assets/cviceni3/02_band_maths_menu.png){ style="width:80%;"}
{: .process_container}

???+ note "&nbsp;<span style="color:#448aff">Pozn.</span>"
      Stejně jako u barevné syntézy, tak i zde platí, že pro výpočet spektrálních indexů je potřeba, aby všechna vstupující pásma do výpočtu měla stejné prostorové rozlišení. Proto budeme pracovat s převzorkovaným produktem.

V nově otevřeném okně poté zadáme název nově vytvořeného pásma (v našem případě indexu), zvolíme, zda chceme mít pásmo pouze virtuálně a nebo zapsané na disk, a klikneme na ***Edit Expression...*** Zde následně zadáme vzorec pro náš vybraný index. Vzorec můžeme napsat ručně pomocí klávesnice nebo si ho naklikat myší. Vyzkoušíme např. *NDMI = (B8 - B11)/(B8 + B11)*. Poté dáme *OK* a znovu *OK*.

![](../assets/cviceni3/03_band_maths_window.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni3/04_band_maths_expression.png)
{: .process_container}

Vypočtené pásmo se nám přidá do mapového okna a také do *Bands*. Pokud jsme v předchozím kroku nechali zaškrtnuto *Virtual (save expression only, don't store data)*, zobrazuje se u ikony pásma písmeno *V* značící, že pásmo je pouze virtuální. Pokud bychom ho chtěli zapsat na disk, klikneme na pásmo pravým talčítkem myši a dáme ***Convert Band***.

![](../assets/cviceni3/05_new_band.png){ style="width:80%;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni3/06_convert_band.png){ style="width:80%;"}
{: .process_container}