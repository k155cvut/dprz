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

# Řízená klasifikace, validace land cover produktů

<hr class="l1">

## Cíl cvičení

- Naučit se provádět řízenou klasifikaci na družicových datech
- Porozumnět principu validování land cover produktů

<hr class="l1">

## Základní pojmy

- **Řízená klasifikace** - Jedná se o proces, při kterém se klasifikátor nejprve učí vlastnosti předem zadaných tříd na poskytnutých trénovacích vzorcích a následně tyto "znalosti" aplikuje na celý dataset. V DPZ lze celý proces rozdělit do následujících kroků:

    1. Určení jednotlivých tříd a definování trénovacích vzorků (v DPZ často mluvíme o trénovacích plochách)
    2. Výpočet statistických charakteristik (tzv. **spektrálních příznaků**) pro trénovací plochy popisující jednotlivé třídy, editace trénovacích ploch a výběr vhodných pásem pro vlastní klasifikaci
    3. Volba vhodného rozhodovacího pravidla (tzv. klasifikátoru) pro zařazení všech prvků obrazu (pixelů) do jednotlivých tříd
    4. Zatřídění všech obrazových prvků do stanovených tříd
    5. Úprava, hodnocení a prezentace výsledků klasifikace

![](../assets/cviceni6/01_schema_klasifikace.png){ style="width:60%;"}
{: style="margin-bottom:0px;" align=center }
<figcaption>Schéma řízené klasifikace</figcaption>

- **Trénovací plochy** - Jedná se o reprezentativní pixely nebo prvky ze souboru obrazových dat pro každou definovanou třídu. Pro úspěšnost klasifikace je volba trénovacích množin stěžejní. Je tedy potřeba mít definovám dostatečný počet těchto trénovacích ploch a v ideálním případě je mít i rovnoměrně rozmístěné na obrazových datech (pokud to jednotlivé třídy dovolují). Důležité také je, aby byly trénovací plochy jednotlivých tříd spektrálně co nejvíce homogenní.
- **Klasifikátor** - Algoritmus nebo metoda použitá k provedení klasifikace. V současné době jsou nejpopulárnější metody patřící do strojového či hlubokého učení, případně metody využívající umělých neuronových sítí.
- **Validace** - Kontrola přesnoti výsledku klasifikace s využitím tzv. testovacího datasetu. K určení přesnosti se užívá řada metrik. Patří mezi ně například *Celková přesnost*, *Uživatelská přesnost* či *Zpracovatelská přesnost*.
- **Chybová matice** (Kontingenční tabulka) - Čtvercová matice, kde počet sloupců i řádků odpovídá počtu definovaných tříd. Slouží k přehlednému zobrazení záměny mezi jednotlivými třídami (tj. které třídy se špatně klasifikovaly do jiných tříd). Správně klasifikovaná data se v této matici nachází na hlavní diagonále. V ideálním případě by měla být hlavní diagonála tvořena nejvyššími hodnotami v matici a hodnoty mimo diagonálu by se měly blížit nule.
- **Přetrénování** (Overfitting) - Přetrénování klasifikátoru je jev, kdy se klasifikátor až příliš přizpůsobí trénovacím datům. Důsledkem pak může být špatná generalizace při použití na jiných datech, než na kterých byl klasifikátor natrénován.
- **Land use / land cover** - Jedná se o výsledek klasifikace družicových obrazových dat. Konkrétně se jedná o tematické mapy popisující zemský povrch. *Land use* povrch popisuje z hlediska jeho využití lidmi. *Land cover* pak znázorňuje jednotlivé druhy povrchů (voda, holá půda, les atd.).

<hr class="l1">

## Řízená klasifikace ve SNAP

### Transformace obrazových dat do WGS84

Tento krok budeme považovat za tzv. nultý krok, protože není běžnou součástí řízené klasifikace. Ve SNAP bohužel z nějakého důvodu řízená klasifikace funguje pouze na *Lat/Long* datech v systému WGS84. Je tedy potřeba si naše data přetransformovat. K tomu slouží funkce nacházející se v **Raster** → **Geometric** → **Reprojection**.

![](../assets/cviceni6/02_reprojection_menu.png){ style="width:40%;"}
{: style="margin-bottom:0px;" align=center }

V záložce ***I/O Parameters*** nastavíme produkt, který chceme transformovat, název transformovaného produktu a zda chceme výsledný produkt uložit. V záložce ***Reprojection Parameters*** poté nastavíme, do jakého systému bude produkt transformován. V tomto případě můžeme nechat vše tak, jak je.

![](../assets/cviceni6/03_reprojection_io.png){ style="height:548px;"}
![](../assets/cviceni6/04_reprojection_parameters.png){ style="height:548px;"}
{: .process_container}

Výsledkem je pak v závislosti na poloze více či méně "zdeformovaná" scéna.

![](../assets/cviceni6/05_reprojected_scene.png){ style="width:80%;"}
{: style="margin-bottom:0px;" align=center }

### Tvorba trénovacích ploch

Prvním krokem řízené klasifikace je definování jednotlivých tříd a k nim odpovídajících trénovacích ploch. Pokud trénovací plochy nemáme odněkud přebrané, musíme si je vytvořit sami. Trénovací plochy budeme vkládat do tzv. vektorových kontejnerů. Vektorový kontejner vytvoříme pomocí **Vector** → **New Vector Data Container**, případně pomocí ikony této funkce. Vektorové kontejnery vytvoříme pro všechny klasifikační třídy (buď všechny najednou nebo postupně po vytvoření trénovacích ploch pro danou třídu).

![](../assets/cviceni6/06_container_menu.png)
![](../assets/cviceni6/07_container_icon.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni6/08_new_container.png)
{: .process_container}

Jednotlivé vektorové kontejnery se nám pak zobrazí v **Layer Manager** (spolu s dalšími již existujícími kontejnery) a zároveň i v *Product Explorer* ve složce **Vector Data**.

![](../assets/cviceni6/09_layer_manager.png){ style="height:113px;"}
![](../assets/cviceni6/10_vector_data.png){ style="height:128px;"}
{: .process_container}

Máme-li vytvořený vektorový kontejner, můžeme začít s tvorbou trénovacích ploch. Ty budeme tvořit pomocí funkce **Polygon drawing tool**. Poté, když chceme začít kreslit polygon, se nás SNAP zeptám, do kterého kontejneru chceme polygon vložit. Z nabídky tedy vybereme příslušný název kontejneru a nakreslíme trénovací plochu. Kreslení polygonu ukončíme dvojklikem.

![](../assets/cviceni6/11_polygon_draw.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni6/12_choose_container.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni6/13_training_area.png)
{: .process_container}

Na to, do jakého vektorového kontejneru chceme polygon vkládat, se nás SNAP zeptá dvakrát. Následně už se automaticky volí dříve zvolený kontejner. Pokud bychom ale chtěli poté vkládat polygon do jiného kontejneru, je potřeba příslušný kontejner označit v **Layer Manager**.

![](../assets/cviceni6/14_selected_container.png){ style="height:80px;"}
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni6/15_new_training_area.png){ style="height:534px;"}
{: .process_container}

Tímto způsobem vytvoříme trénovací plochy pro všechny třídy. Trénovacích ploch by mělo být dostatečné množství a je ideální, aby se nacházely rovnoměrně po celé ploše obrazových dat.

### Kontrola homogenity trénovacích ploch