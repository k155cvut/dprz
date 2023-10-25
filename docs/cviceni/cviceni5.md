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

# Neřízená klasifikace

<hr class="l1">

## Cíl cvičení

- Naučit se klasifikovat optická družicová data pomocí neřízené klasifikace s cílem vytvořit vrstvu pokrytí území (land cover)
- Porozumět rozdílu mezi neřízenou a řízenou klasifikací
- Pochopit princip neřízené klasifikace

<hr class="l1">

## Základní pojmy

### Klasifikace obecně

- Klasifikaci lze obecně definovat jako seskupování vzájemně si podobných prvků do určitých skupin (tříd, kategorií).
- V dálkovém průzkumu Země klasifikace představuje proces, při kterém se jednotlivé pixely originálního numerického záznamu zařazují do tříd, a vzniká tak klasifikovaný snímek. Originální obrazový záznam se tak stává tematickou mapou (nejčastěji mapa land cover).
- Přiřazování pixelů do tříd probíhá na základě tzv. **příznaků** (u optických dat mluvíme o spektrálních příznacích, tj. hodnoty spektrální odrazivosti různých povrchů).
- Příznaky tvoří tzv. **příznakový prostor**, který je definován všemi pásmy, v nichž lze naměřit nebo vypočíst určitou charakteristiku.

![](../assets/cviceni5/01_schema.png){ style="width:80%;"}
{: style="margin-bottom:0px;" align=center }
<figcaption>Zjednodušené schéma klasifikace, kdy se z multispektrálních dat tvoří tematická mapa</figcaption>

### Dělení klasifikace

Klasifikaci obrazových dat můžeme dělit dvěma způsoby. První způsob klasifikaci dělí podle toho, kdy do ní my jakožto operátor vstupujeme. Mluvíme poté o následujících typech klasifikací:

- **Neřízená** klasifikace - Na začátku zadáváme pouze počet tříd, které chceme klasifikovat. Co ale dané třídy ve skutečnosti představují, musíme určit až dodatečně po samotné klasifikaci.
- **Řízená** klasifikace - Na začátku určujeme konkrétní třídy, které chceme klasifikovat. Klasifikátoru zároveň poskytujeme trénovací množiny, na kterých se klasifikátor jednotlivé třídy "učí".
- **Hybridní** klasifikace - Kombinuje dohromady neřízenou a řízenou klasifikaci.

Druhým způsobem je dělení klasifikace podle toho, zda do tříd přiřazujeme jednotlivé pixely nebo skupiny pixelů. Mluvíme pak o následujících typech klasifikací:

- **Pixelová** (per-pixel) klasifikace - Ke konkrétní třídě jsou postupně přiřazovány jednotlivé pixely.
- **Objektová** klasifikace - Do konkrétních tříd nejsou přiřazovány jednotlivé pixely, ale skupiny pixelů.

### Princip neřízené klasifikace

Většina algoritmů neřízené klasifikace je založena na **shlukové analýze**. Ta iterativním způsobem slučuje pixely do shluků se stejnými či podobnými spektrálními vlastnostmi. Základním předpokladem tedy je, že pixely, které patří do jedné třídy, jsou ve vícerozměrném prostoru přirozeně blízko sebe a naopak pixely odlišných skupin, které představují povrchy lišící se svým spektrálním chováním, jsou dobře separované. Vytvořené shluky se nazívají **spektrální třídy**. Tyto spektrální třídy ale nemají požadovanou informační hodnotu a teprve jejich interpretací a postupným spojováním vzikají **třídy informační**.

V rámci tohoto cvičení si ve SNAP vyzkoušíme neřízenou klasifikaci pomocí metody **K-Means Cluster Analysis**. Ta po uživateli před spuštěním požaduje pouze zadání konkrétního počtu shluků a počet iterací. Postup výpočtu lze pak shrnout do následujících kroků:

1. Definování počtu výsledných shluků a určení počtu iterací
2. Určení počáteční polohy centroidu pro každý shluk (pokud není poloha explicitně zadána, jsou centroidy rozmístěny rovnoměrně po diagonále příznakového prostoru)
3. Postupné přiřazení všech pixelů k tomu shluku, k němuž mají v příznakovém prostoru nejblíže
4. Výpočet nové polohy centroidu pro každý shluk na základě nově přiřazených pixelů
5. Opakování kroků 3. a 4. dokud nenastane jedna z následujících momžností:
    1. Bylo dosaženo jednoho z **kritérií konvergence**, tj. poloha centroidů či počet pixelů zařazených do jednotlivých shluků se již výrazně nemění
    2. Bylo dosaženo maximálního počtu iterací zadaného uživatelem

![](../assets/cviceni5/02_shlukovani.png){ style="width:80%;"}
{: style="margin-bottom:0px;" align=center }
<figcaption>Princip iteračního postupu shlukování</figcaption>

Následně pak uživatel již samostatně provádí následující dva kroky, které jsou stejné pro všechny metody neřízené klasifikace.

1. Přiřazení konkrétního významu každému tzv. stabilnímu shluku (spektrální třídě)
2. Vytvoření informačních tříd spojováním tříd spektrálních

<hr class="l1">

## Neřízená klasifikace ve SNAP

Neřízenou klasifikaci ve SNAP najdeme v menu **Raster** → **Classification** → **Unsupervised Classification**, kde poté zvolíme jednu ze dvou nabízených možností. V našem případě zvolíme **K-Means Cluster Analysis**.

![](../assets/cviceni5/03_unsup_classification_menu.png){ style="width:70%;"}
{: style="margin-bottom:0px;" align=center }

Otevře se nám nové okno, kde nejprve v záložce **I/O Parameters** zvolíme ***Source Product***, což bude náš převzorkovaný subset (do výpočtu musí vstupovat pásma se stejným prostorovým rozlišením), a dále pak název nového produktu, zda ho chceme uložit atd. V záložce **Processing Parameters** poté nastavíme parametry algoritmu, jimiž jsou počet spektrálních tříd (***Number of clusters***) a počet iterací (***Number of iterations***). Parametr ***Random seed*** můžeme nechat tak, jak je. Ten pouze náhodně generuje počáteční shluky. V ***Source band names*** nakonec vybereme jednotlivá pásma, která budou do klasifikace vstupovat. Pokud bychom chtěli klasifikaci provést jen na určité části území, můžeme použit ***ROI-mask*** k vybrání konkrétní masky.

![](../assets/cviceni5/04_kmeans_io.png){ style="height:392px;"}
![](../assets/cviceni5/05_kmeans_processing.png){ style="height:392px;"}
{: .process_container}

???+ note "&nbsp;<span style="color:#448aff">Pozn.</span>"
      Výpočet může být v závislosti na vstupních parametrech a velikosti území časově trochu náročnější.

![](../assets/cviceni5/06_computation_time.png){ style="width:40%;"}
{: style="margin-bottom:0px;" align=center }

Po dokončení výpočtu se do *Product Explorer* přidá nový produkt, kde v *Bands* najdeme naší klasifikovanou vrstvu pod názvem *class_indices*.

![](../assets/cviceni5/07_class_indices_band.png)
![](../assets/arrow.svg){: .off-glb .process_icon}
![](../assets/cviceni5/08_display_class_indices.png)
{: .process_container}

<hr class="l1">

## Vliv počátečních parametrů na výsledek