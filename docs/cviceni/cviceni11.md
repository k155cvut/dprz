---
icon: custom/vc-numeric-11-box
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

# Radarová interferometrie, tvorba DEM

<hr class="l1">

## Cíl cvičení

- Seznámit se s radarovými daty
- Vyzkoušet si vytvoření DMT pomocí radarových dat

<hr class="l1">

## Základní pojmy

- **Radarová interferometrie (InSAR - Interferometric Synthetic Aperture Radar)** - jedná se o radarovou metodu, která využívá dvou nebo více radarových snímků ke sledování deformací povrchu nebo pro tvorbu digitálního výškového modelu pomocí rozdílů ve fázi vln vracejících se zpět do senzoru.

Výhodou radarových dat je možnost snímání zemského povrchu i za oblačného počasí bez ohledu na denní dobu. Princip aktivního radarového snímání je založen na vysílání mikrovlnného záření k povrchu Země, kde se záření odráží zpět k senzoru, který následně měří fyzikální charakteristiky zpětného odrazu záření. Různé změny na zemském povrchu se projeví v charakteru odraženého záření, proto je možné radarové snímání využít např. při povodních či sesuvech půdy téměř v reálném čase. V oboru zemědělství a lesnictví je vhodný pro monitoring změn lesních porostů a detekce některých zemědělských operací [<a href="https://www.szif.cz/cs/ams-sentinel" target="_blank">https://www.szif.cz/cs/ams-sentinel</a>]. V rámci tohoto cvičení budeme pracovat s daty z družice Sentinel-1. Více o radarových datech se můžete dočíst například v <a href="https://geo.fsv.cvut.cz/vyuka/155dprz/Handbook_Precourse_Sentinel-1.pdf" target="_blank"> **této příručce**</a>.

## Tvorba DEM z radarových dat

Asi největší slabinou pro interferometická data z družice Sentinel-1 představuje vegetace – čím vyšší, tím vzniká větší dekorelace a následný šum v interferogramu. Pokud se zájmová lokalita nachází v oblasti s výraznými změnami v jednotlivých obdobích – tedy léto-zima, je rozumné využít data ze zimního období, tedy z období, kdy alespoň část porostu je zbavena vegetativních částí. InSARu vadí i travnatý porost, nicméně největší problémy dělají tropické deštné pralesy, a touto metodou je nemožné pro tuto oblast vytvořit digitální výškový model. Pro vytvoření výškového modelu je za potřebí dvou snímků.

Během cvičení můžete zpracovávat buď data z oblasti, kterou si sami vyberete (v takovém případě je nutné začít od prvního z následujících kroků, kde se dozvíte, jak vybrat vhodná data), nebo můžete pracovat s již vyhledanými a staženými daty, které jsou k dispozici <a href="https://geo.fsv.cvut.cz/vyuka/155dprz/cv7/data_cv7.zip" target="_blank"> **Zde**</a>. V případě druhé varianty je možné první dva kroky přeskočit.

Pro začátek je dobré znát následující nástroj v prostředí webového prohlížeče od organizace <a href="https://asf.alaska.edu/" target="_blank"> **Alaska Satellite Facility**</a>. Konkrétně se jedná o nástroj <a href="https://search.asf.alaska.edu/#/" target="_blank"> **Vertex**</a>.

![](../assets/cviceni11/01_vertex.png)
{: style="margin-bottom:0px;" align=center }

### 1) Vyhledání dat