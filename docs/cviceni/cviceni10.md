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

# Ukázka práce v Google Earth Engine

<hr class="l1">

## Cíl cvičení

- Seznámit se cloudovým nástrojem Google Earth Engine
- Umět detekovat spáleniště pomocí dat DPZ

<hr class="l1">

## Základní přehled

<a href="https://earthengine.google.com/" target="_blank"> **Google Earth Engine**</a> je cloudová platforma pro geoprostorovou analýzu, která uživatelům umožňuje vizualizovat a analyzovat satelitní snímky naší planety.

- Code Editor: <a href="https://code.earthengine.google.com/" target="_blank"> **https://code.earthengine.google.com/**</a>
- Dokumentace: <a href="https://developers.google.com/earth-engine/" target="_blank"> **https://developers.google.com/earth-engine/**</a>
- Dostupná data: <a href="https://developers.google.com/earth-engine/datasets/" target="_blank"> **https://developers.google.com/earth-engine/datasets/**</a>
- Ukázky: <a href="https://earthengine.google.com/timelapse/" target="_blank"> **https://earthengine.google.com/timelapse/**</a>
- Tutoriály: <a href="https://developers.google.com/earth-engine/guides" target="_blank"> **https://developers.google.com/earth-engine/guides**</a>

### Code Editor

Code Editor je webové vývojové prostředí Google Earth Engine využívající programovací jazyk JavaScript.

![](../assets/cviceni10/01_gee_gui.png)
{: style="margin-bottom:0px;" align=center }

### Ukázky JavaScript kódu

```js
print('Hello world!');
```

Zobrazení dat MODIS:

```js
var dataset = ee.ImageCollection('MODIS/061/MOD09GA')
  // filter by dates
  .filterDate('2023-04-01', '2023-06-01');

// Select the bands you want to visualize
var visParams = {
  bands: ['sur_refl_b01', 'sur_refl_b04', 'sur_refl_b03'],
  min: 0,
  max: 5000,
  gamma: 1.4,
};

// add to the map window
Map.addLayer(dataset, visParams, 'MODIS');
```

Zobrazení dat Landsat 9 s oblačností menší než 10 % a s přeškálováním:

```js
var dataset = ee.ImageCollection('LANDSAT/LC09/C02/T1_L2')
  // filter by dates
  .filterDate('2023-07-01', '2023-08-31')
  // filter by cloud cover
  .filter(ee.Filter.lessThan('CLOUD_COVER', 10));

// Applies scaling factors
function applyScaleFactors(image) {
  var opticalBands = image.select('SR_B.').multiply(0.0000275).add(-0.2);
  var thermalBands = image.select('ST_B.*').multiply(0.00341802).add(149.0);
  return image.addBands(opticalBands, null, true)
              .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);

// Select the bands you want to visualize
var visParams = {
  bands: ['SR_B4', 'SR_B3', 'SR_B2'],
  min: 0.0,
  max: 0.3,
};

// add to the map window
Map.addLayer(dataset, visParams, 'Landsat 9');
```

Zobrazení dat Sentinel-2 s oblačností menší než 10 % a jejich oříznutí na zájmové území:

```js
var sentinel_col = ee.ImageCollection("COPERNICUS/S2_SR_HARMONIZED")
  // filter by dates
  .filterDate('2023-05-01', '2023-06-30')
  // filter for the drawn rectangle
  .filterBounds(geometry)
  // filter by cloud cover
  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 10));
  
// Define a function to clip each image in the collection to the geometry
var clipToGeometry = function(image) {
  return image.clip(geometry);
};

// Use the map function to apply the clip function to each image in the collection
var clipped_col = sentinel_col.map(clipToGeometry);

// Select the bands you want to visualize
var visParams = {
  bands: ['B4', 'B3', 'B2'],
  min: 0.0,
  max: 3000,
};

// add to the map window
Map.addLayer(clipped_col, visParams, 'Sentinel-2');
```

Spoustu dalších příkladů lze nalézt přímo v prostředí Google Earth Engine v části **Scripts** → **Examples**.

![](../assets/cviceni10/02_gee_examples.png){ style="height:374px;"}
{: style="margin-bottom:0px;" align=center }

<hr class="l1">

## Detekce spálenišť pomocí Google Earth Engine

V rámci tohoto cvičení využijeme Google Earth Engine pro detekování oblastí zasažených požárem. Konkrétní oblastí našeho zájmu bude národní park České Švýcarsko, kde došlo k požáru v období 23. 7. 2022 - 12. 8. 2022 (v německé části požár trval od 25. 7. 2022 - 19. 8. 2022). Práci začneme tím, že si vytvoříme nový soubor, do kterého budeme náš kód psát. Nový soubor vytvoříme v části **Scripts** → **NEW** → **File**. Protože jsme si ale nevytvořili repozitář, do kterého se soubor uloží, Google Earth Engine nás nejprve vyzve k vytvoření právě nového repozitáře. Teprve poté můžeme vytvořit nový soubor.

![](../assets/cviceni10/03_new_file.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni10/04_new_repository.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni10/05_create_file.png)
{: .process_container}

Vytvořený soubor najdeme ve **Scripts** → **Owner**, kde si ho i můžeme otevřít.

![](../assets/cviceni10/06_own_files.png){ style="height:198px;"}
{: style="margin-bottom:0px;" align=center }

### Výběr zájmové oblasti a získání dat

Zájmové území si označíme přidáním bodu do mapy. Bod přidáme pomocí funkce ***Přidat značku***, která se nachází v levé horní části mapového okna. Bod umístíme přibližně mezi obce Hřensko a Mezná, tedy na místo, kde požár zhruba vypukl.

![](../assets/cviceni10/07_add_point.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni10/08_point_location.png)
{: .process_container}

Pomocí funkce ***Edit layer properties*** si můžeme vytvořený bod přejmenovat z *geometry* např. na *point*.

![](../assets/cviceni10/09_edit_point.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni10/10_edit_properties.png)
{: .process_container}

Nyní si můžeme vyhledat potřebná data. Konkrétně budeme pracovat s daty Sentinel-2. Bohužel v roce 2022 nebylo v daném místě mnoho bezoblačných scén, proto nastavíme datum na obdomí mezi 1. 4. 2022 - 31. 10. 2022. Jako limit pro oblačnost nastavíme 30 %.

```js
// Filter the Sentinel-2 image collection
var S2 = ee.ImageCollection('COPERNICUS/S2_SR') // import data
                  .filterBounds(point) // spatial filter
                  .filterDate('2022-04-01', '2022-10-31') // temporal filter
                  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 30)) // metadata filter
                  .sort('CLOUDY_PIXEL_PERCENTAGE'); // sort data

// Print data to the Console
print("Scenes:", S2);

// Add True color RGB composite to the map
Map.addLayer(S2.first(),
            {min:0,max:3000,bands:"B4,B3,B2"}, 
            "Image");
```

Pokud bychom si chtěli zobrazit jiný než první prvek z *ImageCollection*, je potřeba si *ImageCollection* převést na *List*. To můžeme udělat přidáním následujícího kódu.

```js
// Convert ImageCollection to List
var S2List = S2.toList(S2.size());
print(S2List);
// Get second image from the list
Map.addLayer(ee.Image(S2List.get(1)),
            {min:0,max:3000,bands:"B4,B3,B2"}, 
            "Image2");
```

Mezi vrstvami v mapovém okně lze jednoduše přepínat pomocí nástroje **Layers** v pravé části mapového okna.

![](../assets/cviceni10/11_layers.png){ style="height:80px;"}
{: style="margin-bottom:0px;" align=center }

### Maskování oblačnosti, výpočet NBR indexu, analýza časové řady