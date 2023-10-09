<style>
  .md-typeset__scrollwrap {text-align: center;}                                                      /* Zarovnani tabulek na stred */
  /* tbody {width: 100%;display: table;}                                                             /* Roztazeni tabulek na celou sirku */
  h2 {font-weight:700 !important;}                                                                   /* Pokus – zmena formatu nadpisu 2 */
  figcaption {font-size:12px;margin-top:5px !important;text-align:center;line-height:1.2em;}         /* Formatovani Popisku obrazku */
  hr.l1 {background-color:var(--md-primary-fg-color);height:2px;margin-bottom:3em !important;}       /* Formatovani Break Line – LEVEL 1 */
  /* img,iframe {box-shadow: 0 10px 16px 0 rgba(0,0,0,0.2),0 6px 20px 0 rgba(0,0,0,0.2) !important;} /* Stin pod obrazky a videi */
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
  .process_container {display:flex !important; justify-content:center; align-items:center; gap:calc((100vw * 0.03) - 6px) calc((100vw * 0.03) - 6px);} /* Kontejner pro content = FlexBox */
  .process_container div {display:flex;}                                                                                           /* Obsah (obrazky a sipky) */
  .process_container .process_icon {width:/*40px*/calc((100vw * 0.01) + 25px); flex-shrink:0;filter:none !important;}              /* Velikost ikony (bacha na mobily) */
  .process_container img {max-height:400px;}                                                                                       /* Obrazky ve flexboxech maji maximalni vysku */

  /* Grids */
  .grid {display:inline-block !important;border:.05rem solid var(--md-default-fg-color--lightest);border-radius:.1rem;padding:.8rem;transition: all .1s ease-in-out;}
  .grid:hover {transition: all .1s ease-in-out;box-shadow: 0 10px 16px rgba(0,0,0,0.2);}
</style>

# Seznámení se softwarem SNAP, vytvoření subsetu, barevná syntéza, tvorba spektrálních křivek

<hr class="l1">

## Cíl cvičení

- Nahrání dat do **SNAP** a základní orientace v softwaru
- Vytvoření subsetu ze stažených dat Sentinel-2
- Tvorba RGB obrázku pomocí kombinace jednotlivých pásem
- Analýza spektrálních vlastností různých povrchů pomocí spektrálních křivek

<hr class="l1">

## Základní pojmy

- **Resampling** (převzorkování): Změna prostorového rozlišení rastrovách dat (změna velikosti pixelu).
- **Barevná syntéza**: Kombinace tří různých pásem pro vytvoření RGB snímku. Výsledkem může být snímek v přirozených či falešných barvách.
- **Spektrální křivka**: Závislost mezi odrazivostí a vlnovou délkou pro daný objekt či povrch. Tvar těchto křivek bývá pro daný objekt typický.

<hr class="l1">

## Práce v softwaru SNAP

### Nahrání dat do SNAP

Stažená data Sentinel-2 otevřeme pomocí ***File*** → ***Open Product...***, případně kliknutím na ikonu ***Open Product***. Data lze též otevřít jejich přetažením přímo do SNAP, nikoliv však do mapového okna, ale do ***Product Explorer***, který se nachází v levé horní části prostředí SNAP.

![](../assets/cviceni2/01_open_product.png){ style="width:80%;"}
![](../assets/cviceni2/02_open_product_icon.png){ style="width:80%;"}
{: .process_container}

Data Sentinel-2 pro práci ve SNAP nemusíme rozbalovat a otevřeme tak přímo stažený **ZIP** soubor.

![](../assets/cviceni2/03_open_product_window.png){ style="width:80%;"}
{: style="margin-bottom:0px;" align=center }

V levé části prostředí SNAP v ***Product Explorer*** se poté objeví náš produkt. Ten si rozbalíme, v části ***Bands*** si vybereme jakékoliv pásmo a dvojklikem ho zobrazíme v mapovém okně.

![](../assets/cviceni2/04_map_window.png)
{: style="margin-bottom:0px;" align=center }

???+ note "&nbsp;<span style="color:#448aff">Pozn.</span>"
      Open source software SNAP má občas tendence nedělat zrovna to, co se po něm chce (hlášení podivných chybových zpráv, nereagování na uživatelské vstupy, atd.). V takovém případě doporučujeme celý SNAP zavřít (případně nejdřív uložit provedené změny) a znovu spustit, což ve většině případů problém vyřeší.

### Základní orientace ve SNAP

V levé dolní části prostředí SNAP se nachází čtyři záložky, z nichž tři jsou pro nás zajímavé. Jedná se o ***Navigation***, ***Colour Manipulation*** a ***World View***. V ***Navigation*** se zobrazuje celá scéna nahraná do aktuálního mapového okna. Při zoomování v mapovém okně se zde rovněž zobrazuje aktuální poloha daného výřezu na zobrazené scéně, se kterým můžeme pomocí levého tlačítky myši i pohybovat. Velmi užitečné jsou zde i ikony pro synchronizaci mapových oken a kurzoru v případě prohlížení více pásem najednou. ***Colour Manipulation*** slouží pro barevnou úpravu zobrazení produktů v mapovém okně. Záložka ***World View*** pak ukazuje, kde přesně se daná scéna nachází na Zemi.

![](../assets/cviceni2/05_navigation.png)
![](../assets/cviceni2/06_colour_manipulation.png)
![](../assets/cviceni2/07_world_view.png)
{: .process_container}

V levé horní části vedle záložky ***Product Explorer*** se nachází záložka ***Pixel Info***. Ta se hodí v případě, že chceme znát informaci o konkrétním pixelu, na kterém se zrovna nachází náš kurzor.

![](../assets/cviceni2/08_pixel_info.png){ style="width:45%;"}
{: style="margin-bottom:0px;" align=center }

Do mapového okna lze přidat více než jedno pásmo. Dvojklikem na jednotlivá pásma si je tam postupně můžeme přidávat a následně mezi nimi překlikávat. Pokud si je chceme zobrazit souběžně vedle sebe (například pro porovnání), lze využít jedné z funkcí: ***Tile Horizontally***, ***Tile Vertically*** či ***Tile Evenly***.

![](../assets/cviceni2/10_tilling_icons.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni2/09_tilling.png)
{: .process_container}
<figcaption>Zobrazení různých pásem s rozdílným prostorovým rozlišením vedle sebe.</figcaption>

### Vytvoření RGB snímku

Pro snadnější orientaci je místo prohlížení jednotlivých scén, které jsou v odstínech šedi (pokud si je neobarvíme jinak), vhodné vytvořit pomocí barevné syntézy RGB snímek. To lze udělat kliknutím pravým tlačítkem myši na product v ***Product Explorer*** a následným kliknutím na ***Open RGB Image Window***. V nově otevřeném okně poté nastavíme kombinaci pásem, pomocí kterých bude definována barevná syntéza.

![](../assets/cviceni2/11_open_RGB.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni2/12_rgb_window.png)
{: .process_container}

V případě dat Sentinel-2 je zde již přednastavena kombinace pásem **B4 - B3 - B2**, což odpovídá právě kombinaci červeného, zeleného a modrého pásma, díky čemuž vznikne obraz v přírodních barvách, ve kterých bychom dané území viděli i my pouhým okem. Je ale možné používat i jiné předdefinované kombinace či vytvářet své vlastní. Musí být ale dodržena podmínka, že kombinovat lze pásma pouze se stejným prostorovým rozlišením. V opačném případě nám SNAP zahlásí chybovou hlášku poukazující právě na tento fakt. Zvolenou barevnou syntézu potvrdíme kliknutím na tlačítko ***OK***, čímž se nám vzniklý RGB snímek přidá do mapového okna.

![](../assets/cviceni2/13_rgb_airplane.png){ style="width:80%;"}
{: style="margin-bottom:0px;" align=center }

Pokud máte "štěstí", můžete na své scéně najít podobný úkaz jako na obrázku výše. Nejedná se o chybu senzoru, ale o zachycené letadlo prolétávající v době snímání pod družicí. O štěstí lze ale mluvit opravdu jen v uvozovkách, protože nám to zanáší do dat nechtěný šum.

### Tvorba subsetu

V rámci cvičení není praktické především z výpočetních důvodů pracovat s celou scénou Sentinel-2, která má rozměry 110 km × 110 km. Proto si vytvoříme subset o rozměrech zhruba 30 km × 30 km. Subset lze vytvořit kliknutím v menu na ***Raster*** → ***Subset...***.

![](../assets/cviceni2/14_subset_menu.png){ style="width:40%;"}
{: style="margin-bottom:0px;" align=center }

Takto by se ale složitě odhadoval subset o konkrétních rozměrech, které by se musely dopočítávat ze souřadnic, což by bylo poměrně pracné. Doporučujeme proto následující postup.

Nejprve si upravíme mapové okno do zhruba čtvercového tvaru a zazoomujeme do naší oblasti zájmu.

![](../assets/cviceni2/15_square_map_window.png)
{: style="margin-bottom:0px;" align=center }

Poté pomocí funkce ***Determines the distance between two points*** změříme přibližně velikost mapového okna. Měření se provede kliknutím na jedno místo a následně dvojklikem na místo druhé.

![](../assets/cviceni2/16_measure_distance_tool.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni2/17_distance.png)
{: .process_container}

Pokud vzdálenost odpovídá zhruba 30 km, přepneme kurzor na ***Selection tool***, a poté klikneme pravým tlačítkem myši do mapového okna a zvolíme možnost ***Spatial Subset from View...***. 

![](../assets/cviceni2/17b_selection_tool.png){ style="width:70%;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni2/18_subset_from_view.png){ style="width:70%;"}
{: .process_container}

Otevře se nám nové okno, ve kterém můžeme pozměnit parametry subsetu. V záložce ***Spatial Subset*** můžeme vše ponechat tak, jak je. Pokud ale chceme subset o rozměrech přesbě 30 km × 30 km, můžeme upravit hodnoty **Scene start X**, **Scene start Y** a **Scene end X**, **Scene end Y** tak, aby výsledné hodnoty **Subset scene width** a **Subset scene height** byly rovny 500. To ale platí pouze v případě, že je jako referenční pásmo použito pásmo o prostorovém rozlišení 60 m (např. B1). Důvod je ten, že 500 × 60 m = 30 000 m. Pokud by bylo použito jako referenční pásmo pásmo o prostorovém rozlišení 20 m či 10 m, je potřeba hodnoty **Subset scene width** a **Subset scene height** náležitě upravit. V záložce ***Band Subset*** mohu zvolit, jaká všechna pásma budou součástí subsetu. I zde ponecháme všechno tak, jak je (tj. ponecháme zaškrtnuto ***Select all***).

![](../assets/cviceni2/19_spatial_subset.png)
![](../assets/cviceni2/20_band_subset.png)
{: .process_container}

Po stisknutí tlačítka ***OK*** se v ***Product Explorer*** objeví nově vzniklý subset.

![](../assets/cviceni2/21_new_product.png){ style="width:50%;"}
{: style="margin-bottom:0px;" align=center }

Nově vytvořený produkt můžeme poté uložit na disk. To lze udělat kliknutím pravým tlačítkem myši na daný produkt a zvolit možnost ***Save Product*** či ***Save Product As...***. SNAP nás poté informuje, že produkt bude uložen do formátu **BEAM-DIMAP**, což je nativní formát softwaru SNAP, a zeptá se nás, jestli chceme pokračovat v konverzi produktu. Dotaz potvrdíme tlačítkem ***Yes***, zvolíme místo, kam chceme produkt uložit a dáme ***Save***, čímž se na disku vytvoří **DIM** soubor s naším subsetem.

![](../assets/cviceni2/22_save_product.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni2/23_save_product_window.png)
{: .process_container}

### Resampling