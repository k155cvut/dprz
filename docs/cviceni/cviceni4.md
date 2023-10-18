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
  .process_container img {max-height:600px; display:flex;}                                                                                       /* Obrazky ve flexboxech maji maximalni vysku */
</style>

# Filtrace obrazu

<hr class="l1">

## Cíl cvičení

- porozumět principu prostorových filtrů
- umět prostorové filtry aplikovat

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
- Pohyblivé okno je zpravidla čtvercového tvaru a má nejčastěji rozměry 3×3, 5×5 nebo 7×7 (je potřeba mít lichý počet sloupců a řádků, protože se počítá hodnota prostředního pixelu).
- Nové hodnoty rastru se počítají pomocí tzv. konvolučního vzorce, což ale není nic jiného, než vážený průměr hodnot pixelů v pohyblivém okně.

![](../assets/cviceni4/03_convolution.png){ style="width:50%;"}
{: style="margin-bottom:0px;" align=center }

<hr class="l1">
