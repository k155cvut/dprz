---
icon: material/numeric-7-box
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

# Segmentace a objektová klasifikace

<hr class="l1">

## Cíl cvičení

- Naučit se exportovat data ze SNAP
- Porozumnět principu segmentace a objektové klasifikace
- Umět toto téma zpracovat v ArcGIS Pro

<hr class="l1">

## Základní pojmy

- **Segmentace** - Jedná se o proces, při kterém dochází k seskupování sousedních pixelů na samostatné a smysluplné oblasti nebo objekty na základě určitých společných vlastností, jakými jsou například barva, textura či tvarové charakteristiky.
- **Objektová klasifikace** - Jedná se o druh klasifikace, která kombinuje jak spektrální, tak i prostorové informace pro kategorizaci obrazových objektů, což jsou obvykle segmenty nebo oblasti obrazu, nikoli jednotlivé pixely.

<hr class="l1">

## Export dat ze SNAP

Software SNAP nemá nástroje pro objektovou klasifikaci. Tu tedy budeme chtít provést v jiném softwaru, v našem případě v ArcGIS Pro. Proto si náš dříve vytvořený a převzorkovaný subset vyexportujeme do souboru, který budeme moct v ArcGIS Pro otevřít. Export produktu lze provést z menu **File** → **Export**, kde si vybereme jeden z nabízených formátů. Pokud bychom ale takto exportovali rovnou náš převzorkovaný subset, tak by se nám do výsledného souboru propsala veškerá data, který náš produkt obsahuje, tj. všechna pásma, vektorová data, masky, atd. Všechna tato data by poté tvořila jednotlivé vrstvy, čímž by se exportovaný soubor stal dosti nepřehledný a těžko používatelný. Proto si nejprve vytvoříme produkt, který bude obsahovat pouze ta data, která opravdu budeme chtít exportovat. Použijeme tedy funkci **Raster** → **Bands extractor**.

![](../assets/cviceni7/01_band_extractor_menu.png){ style="height:398px;"}
{: style="margin-bottom:0px;" align=center }

V záložce ***I/O Parameters*** nastavíme zdrojový produkt a název exportovaného produktu. A zároveň můžeme rovnou změnit typ souboru z *BEAM-DIMAP* na *GeoTIFF*. V ***Processing Parameters*** poté vybereme pásma, která chceme do exportovaného produktu zapsat. V tomto případě nám postačí jen původní pásma Sentinel-2.

![](../assets/cviceni7/02_bands extractor_io.png){ style="height:313px;"}
![](../assets/cviceni7/03_bands extractor_processing.png){ style="height:313px;"}
{: .process_container}

Data se nám uloží do jednoho ***TIF*** souboru. Jedná se o klasický rastrový formát. V našem případě se navíc jedná ještě o tzv. *Multiband Layer*, protože v sobě obsahuje více než jednu vrstvu. Nyní již tedy můžeme otevřít ArcGIS Pro, vytvořit nový *Map* projekt a vložit exportovaný *TIF* soubor.

<hr class="l1">

## Zobrazení dat DPZ v ArcGIS Pro

Při prvním vložení našeho exportovaného subsetu do ArcGIS Pro se mohou data zobrazit jen v jednotlité černé barvě, jako tomu je v následujícím obrázku.

![](../assets/cviceni7/04_arcgis_pro.png)
{: style="margin-bottom:0px;" align=center }

Jako první by to mohlo evokovat, že je s daty něco v nepořádku. Není tomu ale tak, jen nejsou správně zobrazená. Přepneme se proto do záložky **Raster Layer** (je potřeba mít označenou naší rastrovou vrstvu), zvolíme možnost **Symbology** a vybereme **RGB**. Druhou možností je kliknout pravým tlačítkem na vrstvu v panelu **Contents** a zvolit znovu **Symbology**.

![](../assets/cviceni7/05_symbology_menu.png){ style="height:251px;"}
![](../assets/cviceni7/06_symbology_layer.png){ style="height:487px;"}
{: .process_container}

Zde je poté potřeba změnit parametr ***Stretch type***, který je při prvním otevřením našeho subsetu nastaven na *None*. Vyzkoušíme tedy, co pro naše data sedí nejlépe. Mně se nejlépe osvědčila varianta *Standard Deviation*.

![](../assets/cviceni7/07_change_stretch_type.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni7/08_stdev_stretch.png)
{: .process_container}

Při první změně ***Stretch type*** nás ArcGIS Pro upozorní, že je potřeba spočítat na datech statistiku. Poté již je možné používat funkci ***Stretch type*** i z menu **Raster Layer**.

![](../assets/cviceni7/09_stretch_type_warning.png){ style="height:108px;"}
![](../assets/cviceni7/10_stretch_type.png){ style="height:458px;"}
{: .process_container}

Jsme tedy již schopni rozeznat, co na naší scéně vidíme, nicméně data se nám zobrazují v jakýchsi falešných barvách. Je tomu tak proto, protože ArcGIS Pro vkládá automaticky do RGB kombinace první tři pásma z *multiband* souboru. My ale již víme, že u dat Sentinel-2 tomu tak není. Červené pásmo zde odpovídá pásmu B4, zelené pásmo pásmu B3 a modré pásmo pásmu B2. Pokud si chceme data zobrazit ve skutečných barvách, musíme kanály vstupující do RGB kompozitu podle toho upravit. To uděláme pomocí znovu v **Raster Layer** → **Symbology** → **RGB** a nastavíme zde správnou RGB kombinaci. Poté se již data zobrazí v přírodních barvách.

![](../assets/cviceni7/11_symbology_settings.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni7/12_true_color.png)
{: .process_container}

<hr class="l1">

## Klasifikace v ArcGIS Pro

V ArcGIS Pro jsou dvě možnosti jak přistupovat ke klasifikování. První možností je, že už víme, jaké dostupné nástroje chceme použít, a vyhledáme si je sami v panelu ***Geoprocessing*** či z nabídky **Classification Tools**. Pokud geoprocesingový panel nemáme již připnutý u mapového okna, najdeme ho v menu **View** → **Geoprocessing**.

![](../assets/cviceni7/13_geoprocessing_menu.png){ style="height:232px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni7/14_geoprocessing_pane.png){ style="height:497px;"}
{: .process_container}

**Classification Tools** najdeme v menu pod záložkou **Imagery**, která je určená právě pro práci s daty DPZ.

![](../assets/cviceni7/15_classification_tools.png){ style="height:360px;"}
{: style="margin-bottom:0px;" align=center }

Druhou možností je využít tzv. [:material-open-in-new: Image Classification Wizard](https://pro.arcgis.com/en/pro-app/latest/help/analysis/image-analyst/the-image-classification-wizard.htm){ .md-button .md-button--primary .button_smaller target="_blank"}, který najdeme rovněž v záložce **Imagery**. Tento nástroj uživatele postupně a přehledně provede všemi kroky klasifikace.

![](../assets/cviceni7/16_classification_wizard_menu.png){ style="height:195px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni7/17_classification_wizard_pane.png){ style="height:494px;"}
{: .process_container}

<hr class="l1">

## Postup pro objektovou klasifikaci

V rámci cvičení využijeme první možnost a jednotlivé kroky si budeme spouštět postupně.

### Segmentace

Prvním krokem objektové klasifikace je segmentace obrazových dat. Jak název napovídá, výsledkem tohoto kroku bude segmentovaný obraz. Jednotlivé segmenty jsou tvořeny pixely, které jsou si barevně podobné. Na rozdíl od pixelové klasifikace, kde se postupně klasifikují jednotlivé pixely, tak v objektové klasifikaci se budou klasifikovat právě tyto segmenty. Segmentaci najdeme v menu **Imagery** → **Classification Tools** → **Segmentation** (alternativou může být funkce ***Segment Mean Shift***, kterou najdeme v geoprocesingovém panelu, a která se liší pouze větším množstvím zadávaných parametrů). Funkce **Segmentation** má zde na vstupu tři parametry. Parametr ***Spectral detail*** udává, jak moc důležité budou spektrální rozdíly jednotlivých prvků v obrazových datech. Čím vyšší je hodnota, tím větší je separabilita spektrálně podobných prvků do různých segmetů. Druhým parametrem je ***Spatial detail***, který nastavuje důležitost blízkosti jednotlivých objektů na obrazových datech mezi sebou. Vyšší hodnoty jsou vhodné pro menší seskupené objekty. Naopak nižší hodnoty vytvářejí více vyhlazené výstupy. Posledním parametrem je ***Minimum segment size in pixels***, který určuje, z jakého minimálního množství pixelů musí být daný segment tvořen. Nicméně neexistuje asi žádný obecný návod, jak jednotlivé parametry nastavit, a je potřeba si pro zpracovávaná data vyzkoušet, jaká kombinace nám vyhovuje nejvíce. Pro začátek můžeme zkusit segmentaci spustit s defaultním nastavením.

![](../assets/cviceni7/18_segmentation_menu.png){ style="height:271px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni7/19_segmentation_pane.png){ style="height:493px;"}
{: .process_container}

[:material-open-in-new: Segmentation](https://pro.arcgis.com/en/pro-app/latest/help/analysis/image-analyst/segmentation.htm){ .md-button .md-button--primary .button_smaller target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

Po dokončení segmentace je vhodné výsledek porovnat s realitou. Můžeme buď jednoduše zapínat a vypínat vrstvy nebo můžeme použít funkci ***Swipe*** nacházející se v menu v záložce ***Raster Layer***.

![](../assets/cviceni7/20_swipe_menu.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni7/21_swiping.png)
{: .process_container}

Z mého výsledku je vidět, že především v zástavbě dochází k rozdělení obrazu na až zbytečně moc segmentů, a není tedy od věci zkusit parametry pozměnit.

### Tvorba trénovacích ploch

Když jsme s výsledkem segmentace spokojení, můžeme přejít k druhému kroku, čímž je tvorba trénovacích ploch. Trénovací plochy budeme tvořit pomocí nástroje nacházejícího se v **Imagery** → **Classification Tools** → **Training Samples Manager**.

![](../assets/cviceni7/22_training_samples_menu.png){ style="height:203px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni7/23_training_samples_pane.png){ style="height:494px;"}
{: .process_container}

**Training Samples Manager** má v sobě již předdefinované klasifikační třídy. Ty ale můžeme smazat a vytvořit si své vlastní. Stejně tak můžeme změnit název celého klasifikačního schématu z defaultního *NLCD2011*. Případně můžeme vytvořit schéma úplně nové pomocí funkce ***Create New Schema***. Po vytvoření nového klasifikačního schématu doporučuji toto schéma uložit, kdybychom se pak chtěli k tvorbě trénovacích ploch vrátit (proto je dobré mít panel co nejvíce roztažený, aby byla vidět všechna funkcionalita). Trénovací plochy zde poté můžeme tvořit buď pomocí jednotlivých bodů či kreslením různých geometrických obrazců. Nicméně asi jako nejjednodušší možnost se nabízí využít jednotlivých segmentů jako trénovacích ploch. Jednotlivé segmenty můžeme vybírat pomocí nástroje **Segment Picker**. Následně si tedy označíme třídu, pro kterou chceme trénovací plochy sbírat, a můžeme začít klikat na jednotlivé segmenty.

![](../assets/cviceni7/24_segment_picker.png){ style="height:344px;"}
{: style="margin-bottom:0px;" align=center }

Při vybírání segmentů je ale důležité, abychom je vybírali v dostatečném *zoom levelu*. Pokud bychom byli oddáleni moc, jednotlivé segmenty mohou být zjednodušené, což může vést k nesprávnému označení. Porovnání zjednodušeného a správě označeného segmentu znázorňuje následující obrázek.

![](../assets/cviceni7/25_oversimplified_segment.png)
![](../assets/cviceni7/26_correct_segment.png)
{: .process_container}

Ve spodní části **Training Samples Manager** můžeme poté jednotlivé plochy spravovat. Trénovací plochy rovněž doporučuji si uložit (v tomto případě nejlépe do geodatabáze vytvořené spolu s novým projektem).

![](../assets/cviceni7/27_training_samples_info.png){ style="height:322px;"}
{: style="margin-bottom:0px;" align=center }

[:material-open-in-new: Use Training Samples Manager](https://pro.arcgis.com/en/pro-app/latest/help/analysis/image-analyst/training-samples-manager.htm){ .md-button .md-button--primary .button_smaller target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

### Klasifikace

Klasifikaci provedeme pomocí nástroje **Imagery** → **Classification Tools** → **Classify**. Bohužel nástroj není tak intuitivní jako **Classification Wizard**, který se nás nejprve ptá, jaký typ klasifikace chceme provést. Zde již musíme vědět, k čemu se jaký z nabízených klasifikátorů používá, podle čehož se pak i mění nabídka vstupních parametrů. My chceme provést řízenou objektovou klasifikaci. K tomu je tedy potřeba vybrat jakýkoliv z nabízených klasifikátorů kromě *ISO Cluster*, který se používá při neřízené klasifikaci. My použijeme klasifikátor *Random Trees*, který by měl být snad to samé, co *Random Forest*. Dále zadáme naše trénovací plochy, maximální počet stromů, maximální hloubku stromů (což je parametr, který určuje, jak moc se mohou stromy větvit) a maximální počet trénovacích vzorků v jednotlivých klasifikačních třídách. Dále je pak důležité vložit do nástroje segmentovaný obraz, jinak by se jednalo o klasickou pixelovou klasifikaci. Při vložení segmentovaného obrazu se nám zobrazí nabídka, které všechny atributy budeme chtít pro jednotlivé segmenty použít (můžeme zaškrtnou vše). Co je dále podstatné je to, abychom celou klasifikaci prováděli nad našimi **multispektrálními daty** (náš subset). Klasifikaci lze samozřejmě provést i nad segmentovaným obrazem, výsledky by ale byly dosti neuspokojivé.

![](../assets/cviceni7/28_classify_menu.png){ style="height:271px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni7/29_classify_pane.png){ style="height:494px;"}
{: .process_container}

Výsledkem je pak klasifikovaný obraz. Výsledek závisí především na tom, jak jsme původní data segmentovali. Pokud segmentace byla velmi podrobná, ale některé segmenty se nezařadily do správné třídy, je potřeba přidat více trénovacích ploch, případně pozměnit parametry klasifikace.

![](../assets/cviceni7/30_classification_result.png)
{: style="margin-bottom:0px;" align=center }

[:material-open-in-new: Classify](https://pro.arcgis.com/en/pro-app/latest/help/analysis/image-analyst/classify.htm){ .md-button .md-button--primary .button_smaller target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

<hr class="l1">

## Úkol - Objektová klasifikace

- Provést objektovou klasifikaci na svém území
- Zhodnotit výsledky (jaký byl postup objektové klasifikace, jaké jste nastavili parametry, jaké třídy jste klasifikovali, s čím byly problémy atd.)