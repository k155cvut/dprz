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

# Analýza tepelných ostrovů pomocí dat Landsat

<hr class="l1">

## Cíl cvičení

- Seznámit se s daty Landsat
- Umět analyzovat tepelné ostrovy

<hr class="l1">

## Základní pojmy

- **LST** - Land surface temperature - radiační teplota zemského povrchu měřená ve směru senzoru

### Program Landsat

Program Landsat je nejdéle probíhající projekt zaměřený na pozorování Země. Scény zobrazující zemský povrch poskytuje nepřetržitě již od roku 1972. Během více než padesáti let bylo postupně vypuštěno devět družic s názvy Landsat 1 až Landsat 9. V současné době jsou aktivní družice Landsat 8 a Landsat 9. Program společně řídí agentury <a href="https://www.nasa.gov/" target="_blank"> **NASA**</a> a <a href="https://www.usgs.gov/" target="_blank"> **U.S. Geological Survey**</a>. Veškerá data jsou volně dostupná. Více o historii, současnosti i budoucnosti programu Landsat se můžete dozvědět na následujích webových stránkách:

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

[<span>:material-open-in-new: youtube.com</span><br>Landsat: Landsat 9: Continuing the Legacy)](https://www.youtube.com/playlist?list=PL_8hVmWnP_O3WFxfAa_xBlsmGq87HMhFW){ .md-button .md-button--primary .url-name target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

Jednotlivá pásma Landsat 8 a 9 shrnuje následující tabulka:

![](../assets/cviceni9/02_landsat8_9_bands.png){ style="height:558px;"}
{: style="margin-bottom:0px;" align=center }

<hr class="l1">

## Stažení dat Landsat