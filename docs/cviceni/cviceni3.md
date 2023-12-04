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

Více o spektrálních indexech a jejich konkrétní ukázky nalezneme v následujícím videu:

<div style="text-align: center;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/25OZ1mD8aLc?si=Q-9Ed3AB_btmuCOA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

<hr class="l1">

## Počítání spektrálních indexů

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
      <td>Blue + 2.5·Green - 1.5·(NIR + SWIR<sub>1</sub>) - 0.25·SWIR<sub>2</sub>) = B2 + 2.5·B3 - 1.5·(B8 + B11) - 0.25·B12</td>
    </tr>
  </tbody>
</table>

Pokud chceme spektrální indexy počítat ve SNAP, využijeme nástroj ***Band Maths...***, který nalezneme buď kliknutím pravým tlačítkem na vybraný produkt v ***Product Explorer***, nebo přes menu ***Raster*** → ***Band Maths...***

![](../assets/cviceni3/01_band_maths.png){ style="height:171px;"}
![](../assets/cviceni3/02_band_maths_menu.png){ style="height:115px;"}
{: .process_container}

???+ note "&nbsp;<span style="color:#448aff">Pozn.</span>"
      Stejně jako u barevné syntézy, tak i zde platí, že pro výpočet spektrálních indexů je potřeba, aby všechna vstupující pásma do výpočtu měla stejné prostorové rozlišení. Proto budeme pracovat s převzorkovaným produktem.

V nově otevřeném okně poté zadáme název nově vytvořeného pásma (v našem případě indexu), zvolíme, zda chceme mít pásmo pouze virtuálně a nebo zapsané na disk, a klikneme na ***Edit Expression...*** Zde následně zadáme vzorec pro náš vybraný index. Vzorec můžeme napsat ručně pomocí klávesnice nebo si ho naklikat myší. Vyzkoušíme např. *NDMI = (B8 - B11)/(B8 + B11)*. Poté dáme *OK* a znovu *OK*.

![](../assets/cviceni3/03_band_maths_window.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni3/04_band_maths_expression.png)
{: .process_container}

Vypočtené pásmo se nám přidá do mapového okna a také do *Bands*. Pokud jsme v předchozím kroku nechali zaškrtnuto *Virtual (save expression only, don't store data)*, zobrazuje se u ikony pásma písmeno *V* značící, že pásmo je pouze virtuální. Pokud bychom ho chtěli zapsat na disk, klikneme na pásmo pravým talčítkem myši a dáme ***Convert Band***.

![](../assets/cviceni3/05_new_band.png){ style="height:309px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni3/06_convert_band.png){ style="height:350px;"}
{: .process_container}

[:material-open-in-new: NDMI](https://eos.com/make-an-analysis/ndmi/){ .md-button .md-button--primary .button_smaller .external_link_icon target="_blank"} neboli *Normalized Difference Moisture Index* popisuje úroveň vlhkosti ve vegetaci a slouží tedy jako indikátor vodního stresu vegetace. Nabývá hodnot od -1 do 1. Čím vyšší hodnota, tím vyšší úroveň vlhkosti ve vegetaci. Pro lepší názornost přiřadíme pásmu s indexem NDMI některou z nabízených barevných palet ve SNAP. Palety najdeme v záložce ***Colour Manipulation*** → ***Palette***, kde si nějakou vybereme (např. *gradient_red_white_blue*).

![](../assets/cviceni3/07_palette.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni3/08_ndmi.png)
{: .process_container}

V tomto případě červená barva znázorňuje místa s nízkou hodnotou NDMI, a tudíž místa s žádnou či velmi nízkou úrovní vlhkosti ve vegetaci, a naopak modrá místa znázorňují místa s větším obsahem vlkosti. Při porovnání s RGB snímkem tak můžeme vidět, že červeně znázorněná místa odpovídají holé půde a zástavbě a modrá místa především zemědělským plodinám.

![](../assets/cviceni3/09_ndmi_rgb.png)
{: style="margin-bottom:0px;" align=center }
<figcaption>Porovnání NDMI s RGB syntézou v přírodních barvách</figcaption>

<hr class="l1">

## Maskování

Příklad maskování si ukážeme na dalším indexu, který si spočítáme. Cílem bude zamaskovat místa porostlá vegetací. K tomu je ideální použít index [:material-open-in-new: NDVI](https://eos.com/make-an-analysis/ndvi/){ .md-button .md-button--primary .button_smaller .external_link_icon target="_blank"} neboli *Normalized Difference Vegetation Index*, který slouží právě k detekci vegetace a ke zkoumání jejího stavu. Vzoreček pro výpočet je následující: *NDVI = (B8 - B4)/(B8 + B4)*, a postup výpočtu je stejný, jako tomu bylo u *NDMI*. K vytvoření masky použijeme nástroj ***Mask Manager*** nacházející se v právě části prostředí SNAP. Pokud tam tento nástroj nevidíme, lze jej spustit z menu ***View*** → ***Tool Windows*** → ***Mask Manager***. V našem případě se zde již nachází některé předpočítané masky, které byly součástí produktu Sentinel-2.

![](../assets/cviceni3/10_mask_manager.png)
![](../assets/cviceni3/11_mask_manager_menu.png)
{: .process_container}

V ***Mask Manager*** jsou tři možnosti, jak masky tvořit. Jsou jimi: ***Creates a new mask based on a logical band maths expression***, ***Creates a new mask based on a value range*** a ***Creates a new mask based on a new geometry container (lines and polygons)***. Vytvořené mask je pak možné dále kombinovat pomocí logických operátorů. My nyní použijeme druhou možnost a vytvoříme masku pomocí rozsahu hodnot. Nejprve je ale potřeba znát, co hodnoty NDVI představují. To zhruba znázorňuje následující tabulka.

<table>
  <thead>
    <tr>
      <th><strong>Povrch</strong></th>
      <th><strong>Rozsah hodnot</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Vodní plocha</strong></td>
      <td>(-1; 0)</td>
    </tr>
    <tr>
      <td><strong>Zástavba a holá půda</strong></td>
      <td>(0; 0.2)</td>
    </tr>
    <tr>
      <td><strong>Řídká vegetace</strong></td>
      <td>(0.2; 0.3)</td>
    </tr>
    <tr>
      <td><strong>Středně hustá vegetace</strong></td>
      <td>(0.3; 0.5)</td>
    </tr>
    <tr>
      <td><strong>Hustá vegetace</strong></td>
      <td>(0.5; 1)</td>
    </tr>
  </tbody>
</table>

Hovořit se ale rovněž dá i o zdravé a nezdravé vegataci. Chlorofyl totiž pohlcuje červené záření a buněčná struktura rostlin naopak odráží záření infračervené, což ve výsledku způsobuje vysoké honoty NDVI. Když ale není vegetace v dobrém stavu a chlorofylu ubývá, dochází k vyššímu odrazu červeného záření, což se projevuje nižšími hodnotami NDVI.

Řekněme tedy, že chceme vytvořit masku s hustou a středně hustou vegetací. V ***Mask Manager*** zvolíme možnost ***Creates a new mask based on a value range*** a zadáme požadovaný rozsah hodnot NDVI.

![](../assets/cviceni3/12_mask_value_range.png){ style="height:90px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni3/13_vegetation_mask.png){ style="height:113px;"}
{: .process_container}

V ***Mask Manager*** se nám poté objeví nová maska, kterou si můžeme pojmenovat či jí změnit barvu.

![](../assets/cviceni3/14_new_mask.png){ style="height:41px;"}
{: style="margin-bottom:0px;" align=center }

Následně si masku zobrazíme přes nějaké pásmo (ideálně přes RGB kompozit) a přesvědčíme se, zda opravdu maskuje to, co jsme chtěli.

![](../assets/cviceni3/15_show_mask.png)
{: style="margin-bottom:0px;" align=center }

<hr class="l1">

## Úkol - Tvorba NDVI mapy a detekce vodní ploch

### Zadání

- Vytvořit NDVI mapu s diskrétními barvami pro různé druhy povrchů detekovatelných pomocí NDVI, popsat co jaká barva znázorňuje
- Detekovat vodní plochy pomocí vybraných indexů
- Zhodnotit který z indexů detekoval vodní plochy nejlépe, aniž by docházelo i k zásadnímu detekování zástavby
- Zjistit celkovou detekovanou plochu vody pro jednotlivé indexy

### Postup

První část úkolu je snadná. NDVI již máme spočítané, takže jen stačí přidělit jednotlivým hodnotám NDVI barvy dle tabulky výše. Zobrazíme si tedy pásmo NDVI a přejdeme do ***Colour Manipulation***. Zvolíme možnost ***Sliders*** a klikneme na ***More Options***, kde zaškrtneme checkbox ***Discrete colours***.

![](../assets/cviceni3/16_discrete_colors.png){ style="height:310px;"}
{: style="margin-bottom:0px;" align=center }

Slidery poté upravíme dle našich potřeb. Kliknutím na hodnotu slider můžeme tuto hodnotu měnit. Kliknutím na samotný slider můžeme měnit barvu daného slideru (při zaškrtnutém *Discrete colours* bude vybraná barva platit pro hodnoty od daného slideru až po slider následující). Kliknutím pravým talčítkem myši můžeme slidery buď přidávan nebo odstraňovat. Není potřeba zadávat slider pro nejvyšší možnou hodnotu. Stačí mít slider pro počáteční hodnotu posledního intervalu.

![](../assets/cviceni3/17_left_slider.png)
![](../assets/cviceni3/18_add_new_slider.png)
![](../assets/cviceni3/19_final_sliders.png)
{: .process_container}

Výsledná mapa pak může vypadat nějak takto:

![](../assets/cviceni3/20_ndvi_map.png){ style="height:693px;"}
{: style="margin-bottom:0px;" align=center }

Druhá část není o nic složitější. Pro detekci vody použijeme následující indexy: NDVI, NDWI a AWEI<sub>sh</sub>. Jen je potřeba znát, které hodnoty zhruba odpovídají vodní hladině. Tuto informaci shrnuje následující tabulka.

<table>
  <thead>
    <tr>
      <th><strong>Index</strong></th>
      <th><strong>Hodnoty pro vodní plochu</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>NDVI</strong></td>
      <td>(-1; 0)</td>
    </tr>
    <tr>
      <td><strong>NDWI</strong></td>
      <td>(0; 1)</td>
    </tr>
    <tr>
      <td><strong>AWEI<sub>sh</sub></strong></td>
      <td>> 0</td>
    </tr>
  </tbody>
</table>

Postup pro NDVI a NDWI je stejný jako při maskování vegetace. U AWEI<sub>sh</sub> je postup také identický. Jen při tvorbě samotné masky použijeme místo ***Creates a new mask based on a value range*** funkci ***Creates a new mask based on a logical band maths expression***.

![](../assets/cviceni3/21_Logical_Expression.png){ style="height:334px;"}
{: style="margin-bottom:0px;" align=center }

Výsledky detekce vodních ploch poté porovnáme. Hodnoty indexů definující vodní plochy můžeme rovněž trochu upravovat, dokud není dosaženo optimálních výsledků. Nejhůře pro detekci vody by měl vyjít index NDVI (na obrázku níže vlevo), který pro tento účel není primárně určen.

![](../assets/cviceni3/22_ndvi_water.png)
![](../assets/cviceni3/23_ndwi_water.png)
![](../assets/cviceni3/24_awei_water.png)
{: .process_container}

Celkovou plochu, které naše masky pokrývají, zjistíme pomocí ***Analysis*** → ***Statistics***.

![](../assets/cviceni3/25_statistics_menu.png){ style="height:233px;"}
{: style="margin-bottom:0px;" align=center }

Po otevření nového okna je nejprve nutno znovu kliknout do mapového okna, aby nástroj věděl, odkud se bude statistika počítat. V právě části poté zaškrtneme možnost ***Use ROI mask(s):*** a zvolíme konkrétní masku. Následně klikneme na dvě modré šipky v pravé horní části, čímž se statistika spočítá. V lévé části pak najdeme hodnotu ***#Pixels total***, která v tomto případě udává počet pixelů pokrytých maskou.

![](../assets/cviceni3/26_statistics.png)
{: style="margin-bottom:0px;" align=center }