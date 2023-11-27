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

# Ukázka VHR dat, pansharpening, řízená klasifikace v ArcGIS Pro

<hr class="l1">

## Cíl cvičení

- Seznámit se některými daty s vysokým prostorovým rozlišením
- Získat povědomí o možnosti pansharpeningu
- Provést řízenou klasifikaci VHR dat v ArcGIS Pro

<hr class="l1">

## Základní pojmy

- **VHR data** - Very High Resolution data, neboli data s vysokým prostorovým rozlišením
- **Panchromatické pásmo** - Pásmo obsahující veškeré viditelné odražené záření (šířka pásma tedy sahá do oblasti modrého, zeleného a červeného spektra). Takto velká šířka pásma umožňuje udržet vysoký poměr signálu a šumu, což dává možnost mít data ve vysokém prostorovém rozlišení (až čtyřikrát vyšší oproto multispektrálním datům).
- **Pansharpening** (panchromatic sharpening) - Jedná se o proces kombinující panchromatická data s daty multispektrálními, přičemž dochází k navýšení prostorového rozlišení multispektrálních dat se zachováním specifických spektrálních atributů.

<hr class="l1">

## Data pro cvičení

Data pro toto cvičení si můžete stáhnout <a href="https://geo.fsv.cvut.cz/vyuka/155dprz/cv10/cv10_data.zip" target="_blank"> **zde**</a>. Zazipovaný soubor obsahuje tři scény ze stejného území v Grónsku. Vyberte si tedy jednu z nich, se kterou budete v rámci cvičení pracovat. Informace o jednotlivých scénách shrnuje následující tabulka.

<table>
  <thead>
    <tr>
      <th><strong>Datum snímání</strong></th>
      <th><strong>Družice</strong></th>
      <th><strong>Rozlišení multispektrálních pásem</strong></th>
      <th><strong>Rozlišení panchromatického pásma</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>31. 5. 2019</td>
      <td>WorldView-3</td>
      <td>1,2 m</td>
      <td>0,3 m</td>
    </tr>
    <tr>
      <td>1. 8. 2020</td>
      <td>GeoEye-1</td>
      <td>1,6 m</td>
      <td>0,4 m</td>
    </tr>
    <tr>
      <td>30. 8. 2021</td>
      <td>GeoEye-1</td>
      <td>2 m</td>
      <td>0,5 m</td>
    </tr>
  </tbody>
</table>

Názvy složek bohužel o samotných scénách nic neříkají, nicméně jsou řazeny podle roku. První složka tedy obsahuje data pro rok 2019 a poslední složka data pro rok 2021. Každá složka poté obsahuje podsložky s názvem končícím na *MUL* a *PAN*, ve kterých naleznete *TIF* soubor s multispektrálními, resp. panchromatickými daty.

![](../assets/cviceni8/01_folder_names.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni8/02_folder_structure.png)
{: .process_container}

Multispektrální data obsahují celkem čtyři pásma, která jsou řazena v následujícím pořadí: modré, zelené, červené a blízké infračervené (B-G-R-NIR).

![](../assets/cviceni8/03_whole_scene.png){ style="height:554px;"}
{: style="margin-bottom:0px;" align=center }
<figcaption>Celá scéna z roku 2020</figcaption>

### Popis oblasti

Poskytnutá data zachycují bývalou vojenskou základnu z 2. světové války s názvem Bluie East Two, na které se mimo jiné nachází tisíce starých barelů. V roce 2019 započalo jejich odstraňování, tudíž jich je na každé scéně jiné množství. Naším cílem práce bude pomocí řízené klasifikace určit, kolik se v oblasti nachází barelů za předpokladu, že na jeden metr čtvereční plochy s barely připadá přibližně **2,12** barelů.

![](../assets/cviceni8/04_barrels.JPG){ style="width:80%;"}
{: style="margin-bottom:0px;" align=center }

<hr class="l1">

## Příprava dat pro klasifikaci

Cvičení bude znovu probíhat v softwaru ArcGIS Pro. Začneme tedy tím, že si vytvoříme nový *Map* projekt, do kterého si vložíme multispektrální a panchromatická data z jednoho zvoleného roku. U multispektrálních dat zvolíme i nějakou vhodnou RGB kombinaci (a *Stretch type*) s ohledem na pořadí pásem: *B-G-R-NIR*.

![](../assets/cviceni8/05_multi.png)
![](../assets/cviceni8/06_pan.png)
{: .process_container}
<figcaption>Ukázka VHR dat z roku 2019 (vlevo - multispektrální data, vpravo - panchromatická data)</figcaption>

### Pansharpening

Když už máme k dispozici panchromatické pásmo, které má v našem případě 4× vyšší rozlišení než multispektrální data, tak ho využijeme pro tzv. ***Pansharpening***. Jedná se o proces, který pomocí panchromatického pásma navýší prostorové rozlišení multispektrálních dat na stejnou úroveň jako má právě panchromatické pásmo. Na rozdíl od obyčejného převzorkování (*resampling*) zde multispektrální data převezmou i prostorové detaily panchromatického pásma. Pro pansharpening použijeme nástroj [:material-open-in-new: Create Pansharpened Raster Dataset](https://pro.arcgis.com/en/pro-app/latest/tool-reference/data-management/create-pansharpened-raster-dataset.htm){ .md-button .md-button--primary .button_smaller target="_blank"}, který najdeme přes panel ***Geoprocessing***. Zde poté zadáme multispektrální a panchromatická data a také jeden z nabízených algoritmů. V našem případě bych doporučil zvolit algoritmus *Gram-Schmidt*, kde můžete i nastavit, o jaký typ senzoru se jedná. Případně můžete i vyzkoušet, jak se budou výsledky s různými algoritmy pro pansharpening lišit.

![](../assets/cviceni8/07_pansharpen_tool.png){ style="height:494px;"}
{: style="margin-bottom:0px;" align=center }

Výsledkem budou pansharpovaná data, která můžeme porovnat s daty původními. Také je dobré všimnout si, že ArcGIS Pro změnil pořadí pásem v pansharpovaných datech na *R-G-B-NIR*.

![](../assets/cviceni8/08_prepan.png)
![](../assets/cviceni8/09_postpan.png)
{: .process_container}

### Oříznutí na zájmovou oblast

Jelikož se barely nacházejí jen na části území, je zbytečné pracovat s celou scénou. Proto si scénu ořízneme jen na naše zájmové území. <a href="https://geo.fsv.cvut.cz/vyuka/155dprz/cv10/aoi.zip" target="_blank"> **Zde**</a> si tedy stáhněte shapefile, které toto zájmové území ohraničuje, a vložte si ho do vašeho projektu.

![](../assets/cviceni8/10_aoi.png){ style="height:554px;"}
{: style="margin-bottom:0px;" align=center }
<figcaption>Vyznačené zájmové území na scéně z roku 2019</figcaption>

Pansharpovanou scénu si ořízneme pomocí funkce [:material-open-in-new: Clip Raster](https://pro.arcgis.com/en/pro-app/3.1/tool-reference/data-management/clip.htm){ .md-button .md-button--primary .button_smaller target="_blank"}.

![](../assets/cviceni8/11_clip_raster.png){ style="height:494px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni8/12_clipped_data.png){ style="height:417px;"}
{: .process_container}

### Vytvoření spektrálních indexů

Pro klasifikování máme k dispozici zatím jen 4 pásma a není tedy od věci si nějaké kanály dopočítat. Spočítáme si proto dva spektrální indexy; NDVI a NDWI. Nástroj pro výpočet NDVI najdeme v menu **Imagery** → **Indices** → **NDVI**. Zde poté stačí zadat, které pásmo odpovídá blizkému infračervenému pásmu (4), a které odpovídá červenému pásmu (1).

![](../assets/cviceni8/13_indices.png){ style="height:525px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni8/14_ndvi.png){ style="height:203px;"}
{: .process_container}

Index NDWI zde bohužel předdefinován není, takže budeme muset použít jiný nástroj. Konkrétně tedy nástroj **Imagery** → **Raster Functions** → **Band Arithmetic**. Zde poté zvolíme, jakou metodu chceme použít (vybereme tedy NDWI), a zadáme pořadí potřebných pásem NIR a Green (4 2).

![](../assets/cviceni8/16_band_arithmetic.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni8/16_band_arithmetic.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni8/17_ndwi.png)
{: .process_container}

Takto vypočtené NDVI a NDWI kanály bohužel nejsou uložené v naší geodatabázi, a jedná se zřejmě jen o dočasné soubory. Pro jistotu si je tedy do geodatabáze uložíme. Klikneme pravým tlačítkem myši na daný raster v panelu *Contents* a zvolíme možnost **Data** → **Export Raster**. Zde poté nastavíme parametr ***Output Raster Dataset*** tak, aby se spočítaný spektrální index uložil právě do naší geodatabáze. Stejný postup provedeme pro oba indexy.

![](../assets/cviceni8/18_export_raster.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni8/19_export_setting.png)
{: .process_container}

### Spojení více pásem do jednoho souboru

Posledním krokem před klasifikací je spojení pansharpovaných multispektrálních dat a vypočtených spektrálních indexů do jednoho multiband rastru. K tomu použijeme nástroj [:material-open-in-new: Composite Bands](https://pro.arcgis.com/en/pro-app/latest/tool-reference/data-management/composite-bands.htm){ .md-button .md-button--primary .button_smaller target="_blank"}. Zde pouze nastavíme pořadí jednotlivých pásem a výstupní rastr, který nejlépe znovu uložíme do geodatabáze.

![](../assets/cviceni8/20_composite_bands.png){ style="height:271px;"}
{: style="margin-bottom:0px;" align=center }

Nyní můžeme všechny ostatní vrsty zavřít, protože dále již budeme pracovat pouze s tímto multiband rastrem.

<hr class="l1">

## Řízená klasifikace

### Klasifikační třídy a trénovací plochy

### Klasifikace

<hr class="l1">

## Postklasifikační úpravy

- Sieve filter

<hr class="l1">

## Určení počtu barelů

- Zamaskování hangáru a náklaďáků
- Summarize Categorical Raster

<hr class="l1">