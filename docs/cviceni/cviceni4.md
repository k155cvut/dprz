---
icon: material/numeric-4-box
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

# Filtrace obrazu

<hr class="l1">

## Cíl cvičení

- Porozumět principu prostorových filtrů
- Umět prostorové filtry aplikovat

<hr class="l1">

## Základní pojmy

Aplikace prostorových filtrů na obrazová data se řadí mezi metody zvýraznění obrazu. S jinými metodami zvýraznění obrazu jsme se setkali již v minulých cvičeních. Příkladem byla úprava (roztažení) histogramu v *Colour Manipulation*, či vytváření barevných syntéz a počítání spektrálních indexů. Metody zvýraznění obrazu můžeme tedy rozdělit do následujících skupin:

- **Bodové** (radiometrické) zvýraznění - manipulace s odstíny šedi, prahování, hustotní řezy
- **Prostorové** zvýraznění - prostorové **filtrace**, Fourierovy transformace
- **Spektrální** zvýraznění - sestavování barevných syntéz, barevná zvýraznění více pásem (analýza hlavních komponent, aritmetické kombinace, IHS
transformace)

### Prostorová frekvence

Popisuje množství změn v hodnotách pixelů pro dané území v závislosti na vzdálenosti. Nízká prostorová frekvence znamená, že jsou si hodnoty v rastru podobné. Naopak pokud se hodnoty v rastru výrazně liší, mluvíme o vysoké prostorové frekvenci.

![](../assets/cviceni4/01_low_spatial_frequency.png)
![](../assets/cviceni4/02_high_spatial_frequency.png)
{: .process_container}
<figcaption>Nízká prostorová frekvence (vlevo) a vysoká prostorová frekvence (vpravo)</figcaption>

### Konvoluce

- Jedná se o proces, kde je pro výpočet nových hodnot pixelů využito pohyblivé okno (kernel) s předem definovanými hodnotami váh. Toto pohyblivé okno se postupně pohybuje po celém rastru.
- Pohyblivé okno je zpravidla čtvercového tvaru a má většinou rozměry 3×3, 5×5 nebo 7×7 pixelů (je potřeba mít lichý počet sloupců a řádků, protože se počítá hodnota prostředního pixelu, nicméně nemusí to být pravidlem).
- Nové hodnoty rastru se počítají nejčastěji pomocí tzv. konvolučního vzorce, což ale není nic jiného, než vážený průměr hodnot pixelů v pohyblivém okně.

![](../assets/cviceni4/03_convolution.png){ style="height:342px;"}
{: style="margin-bottom:0px;" align=center }

### Dělení filtrů

Prostorové filtry můžeme dělit dvěma způsoby. První způsob dělení je podle druhu informace, kterou dané filtry propouští, a druhou metodou je pak způsob, jakým jsou nové hodnoty rastru určovány.

**Dělení podle propustnosti informací**

- **Vysokofrekvenční** (high-pass) filtry - propouštějí vysokofrekvenční informaci a zesilují tedy rozdíly mezi hodnotami v obraze. Dochází tak ke zvýraznění obrazu. Využívají se například pro detekci hran.
- **Nízkofrekvenční** (low-pass) filtry - propouštějí pouze nízkofrekvenční informaci a potlačují tak rozdíly mezi hodnotami v obraze. Dochází k vyhlazení obrazu. Slouží například k redukci šumu.

**Dělení podle způsobu výpočtu nových hodnot**

- **Lineární** filtry - nová hodnota daného pixelu je vypočtena jako lineární kombinace hodnot v jeho okolí (v pohyblivém okně). Je použito konvolučního vzorce.
- **Nelineární** filtry - nová hodnota daného pixelu není lineární kombinací okolních hodnot a k výpočtu tedy není použita konvoluce. Příkladem může být např. mediánový filtr, či filtry přiřazující danému pixelu maximální případně minimální hodnotu z okolí.

### Využití filtrů a shrnutí

Filtry mají v DPZ řadu využití. Příklady mohou být následující:

- Vylepšení, zvýraznění obrazu
- Analýza či automatické zpracování
- Odstranění šumu
- Vyhlazení klasifikačních výsledků

Princip prostorových filtrů názorně shrnují následující videa:

<div class="process_container">
<iframe width="560" height="315" src="https://www.youtube.com/embed/5_V_iJmtwwg?si=VStQYNNZ062CIMoQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/PDLSvWuhDwI?si=SDYJPBYJWyWw5TCW" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

<hr class="l1">

## Ukázka použití filtrů ve SNAP

Ve SNAP máme dvě možnosti, jak se dostat k prostorovým filtrům. Buď můžeme kliknout pravým tlačítkem myši na některé pásmo v *Product Explorer* a následně zvolit možnost ***Filtered Band...*** nebo můžeme přes menu ***Raster*** → ***Filtered Band...*** Pokud k prostorovým filtrům přistupujeme druhým způsobem, je potřeba mít označeno pásmo buď v *Product Explorer* nebo v mapovém okně.

![](../assets/cviceni4/04_filtered_band.png){ style="height:380px;"}
![](../assets/cviceni4/05_filtered_band_menu.png){ style="height:393px;"}
{: .process_container}

V nově otevřeném okně si pak můžeme vybrat jeden z nabízených filtrů nebo si vytvořit svůj vlastní. Dále můžeme volit počet iterací nebo se podívat na kernel jednotlivých filtrů.

![](../assets/cviceni4/06_Create_Filtered_Band.png){ style="height:490px;"}
![](../assets/cviceni4/07_kernel.png){ style="height:492px;"}
{: .process_container}
<figcaption>Vpravo ukázka kernelu vysokofrekvenčního filtru</figcaption>

Po aplikování filtru se nám filtrovaný obraz přidá do mapového okna.

![](../assets/cviceni4/08_filter_examples.png)
{: style="margin-bottom:0px;" align=center }
<figcaption>Ukázky filtrů. Vlevo nahoře - původní pásmo B8, vpravo nahoře - High-Pass filtr, vlevo dole - Low-Pass filtr, vpravo dole - Standard Deviation filtr</figcaption>

<hr class="l1">

## Úkol - Aplikace vybraných filtrů

- Použít na jedno z původních desetimetrových pásem (tj. B2, B3, B4 nebo B8) čtyři různé filtry (alespoň jeden nízkofrekvenční a jeden vysokofrekvenční). Popsat co se děje s obrazem pro následující oblasti:
    - Vodní plocha a břehové oblasti
    - Město / zástavba
    - Lesní plocha
- Ke každému popisu přidat obrázek jako důkaz
- V závěru zhodnoťte, které filtry jsou výhodné pro zvýraznění změn mezi jednotlivými plochami a které potlačují různorodosti v rámci ploch stejného typu.

### Nízkofrekvenční filtry

- Průměrový (Mean)
- Mediánový
- Low-Pass
- Min/Max
- Morphological

### Vysokofrekvenční filtry

- Line (edge) detection
- Laplace
- High-Pass
- Emboss
- Standard Deviation

Vysokofrekvenční filtr lze získat i tak, když od původního pásma odečteme pásmo s aplikovaným nízkofrekvenčním filtrem.