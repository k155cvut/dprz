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

### Popis oblasti

Poskytnutá data zachycují bývalou vojenskou základnu z 2. světové války s názvem Bluie East Two, na které se mimo jiné nachází tisíce starých barelů. V roce 2019 započalo jejich odstraňování, tudíž jich je na každé scéně jiné množství. Naším cílem práce bude pomocí řízené klasifikace určit, kolik se v oblasti nachází barelů za předpokladu, že na jeden metr čtvereční plochy s barely připadá přibližně **2,12** barelů.

![](../assets/cviceni8/08_barrels.JPG){ style="width:80%;"}
{: style="margin-bottom:0px;" align=center }

<hr class="l1">

## Příprava dat pro klasifikaci

### Pansharpening

### Oříznutí na zájmovou oblast

### Vytvoření spektrálních indexů

### Spojení více pásem do jednoho souboru

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