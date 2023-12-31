<style>
  .md-button {text-align:center;transition: all .1s ease-in-out !important;}  /* Button – zarovnani textu */
  .md-button:hover {transform: scale(1.04);opacity:.8;background-color:var(--md-primary-fg-color) !important;border-color:var(--md-primary-fg-color) !important;color:var(--md-primary-bg-color) !important;/*filter: brightness(80%);*/}            /* Button Hover – animace zvetseni a zmeny barvy */
  .md-button:focus {opacity:.8;background-color:var(--md-primary-fg-color) !important;border-color:var(--md-primary-fg-color) !important;color:var(--md-primary-bg-color) !important;}                                                                /* Button Focus – stejny vzhled jako hover */
  .url-name {line-height:1.2;}                                                /* Button s URL */
  .url-name span:first-child {font-size:.7em; font-weight:300;}               /* Button s URL – format*/
  .process_container {display:flex !important; justify-content:center; align-items:center; gap:calc((100vw * 0.06) - 6px) calc((100vw * 0.06) - 6px)} 
  .process_container img {max-height:90px;}
</style>

# Charakteristika předmětu

Cvičení z předmětu 155DPRZ vás seznámí ze základy dálkového průzkumu Země. 

Naučíte se:

- **zpracovávat** a **analyzovat** družicová rastrová data
- porozumět **spektrálním křivkám**
- **filtrovat** rastrová data pomocí předdefinovaných filtrů
- počítat a využívat **vegetační indexy**
- **klasifikovat** obrazová data pomocí řízené i neřízené klasifikace
- analyzovat **tepelné ostrovy** 
- používat **cloudové nástroje** pro zpracování dat DPZ

Během výuky bude používán převážně open source software [**SNAP**](https://step.esa.int/main/download/snap-download/). Část cvičení bude probíhat v softwaru [**ArcGIS Pro**](https://www.arcdata.cz/cs-cz/produkty/arcgis/arcgis-pro/prehled). Na konci kurzu je pak jedno cvičení věnováno práci v cloudovém prostředí [**Google Earth Engine**](https://earthengine.google.com/).

![](assets/SNAP_icon.jpg){ .off-glb style="filter:none !important;"}
![](assets/agp_logo.png#only-light){ .off-glb style="filter:none !important;"}
![](assets/agp_logo2.png#only-dark){ .off-glb style="filter:none !important;"}
![](assets/earth-engine-logo.png){ .off-glb style="filter:none !important;"}
{: .process_container}

## SNAP tutoriály

[<span>:material-open-in-new: step.esa.int</span><br>Tutorials – STEP](https://step.esa.int/main/doc/tutorials/){ .md-button .md-button--primary .url-name target="_blank"}
[<span>:material-open-in-new: youtube.com</span><br>ESA SNAP Tutorials](https://www.youtube.com/playlist?list=PLjD2jvOGfddcGUuN4or7J6VfAaIR5uH7u){ .md-button .md-button--primary .url-name target="_blank"}
[<span>:material-open-in-new: youtube.com</span><br>Introductory Remote Sensing](https://www.youtube.com/playlist?list=PLf6lu3bePWHCOUjTDZRNx5N07otDM8iUq){ .md-button .md-button--primary .url-name target="_blank"}
[<span>:material-open-in-new: youtube.com</span><br>Sentinel-1 Complete Tutorials in SNAP](https://www.youtube.com/playlist?list=PLJeebDESAF94gWsdm63tabJMh3D4LrxzV){ .md-button .md-button--primary .url-name target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

## DPZ v ArcGIS Pro

[<span>:material-open-in-new: pro.arcgis.com</span><br>Imagery and remote sensing in ArcGIS Pro](https://pro.arcgis.com/en/pro-app/latest/help/data/imagery/imagery-and-remote-sensing-in-arcgis.htm){ .md-button .md-button--primary .url-name target="_blank"}
[<span>:material-open-in-new: youtube.com</span><br>Remote Sensing in ArcGIS Pro](https://www.youtube.com/playlist?list=PL3TjE07UZwaLOxwY0hDXAvRnozPNJfj0h){ .md-button .md-button--primary .url-name target="_blank"}
[<span>:material-open-in-new: youtube.com</span><br>ArcGIS Pro Image Processing](https://www.youtube.com/playlist?list=PLW6kSC5GYhLVJhJtbXSfqyye8Ag4Ka8HW){ .md-button .md-button--primary .url-name target="_blank"}
[<span>:material-open-in-new: youtube.com</span><br>Imagery and Remote Sensing](https://www.youtube.com/playlist?list=PLGZUzt4E4O2I64qHyVPAxtDPJj0LtJIpQ){ .md-button .md-button--primary .url-name target="_blank"}
[<span>:material-open-in-new: youtube.com</span><br>Remote Sensing with ArcGIS Pro](https://www.youtube.com/playlist?list=PLkV8CNVuB_rLK-WQbDHOMMCTJOylWSNfC){ .md-button .md-button--primary .url-name target="_blank"}
{: align=center style="display:flex; justify-content:center; align-items:center; column-gap:20px; row-gap:10px; flex-wrap:wrap;"}

## Doporučená literatura

1. Halounová L., Pavelka K.: Dálkový průzkum Země. Vydavatelství ČVUT, Praha 2007. ISBN: 80-01-03124-1
2. Dobrovolný, P.: Dálkový průzkum Země. Brno 1998. ISBN: 80-210-1812-7
3. Halounová, L.: Zpracování obrazových dat. ČVUT v Praze, 2008. ISBN: 978-80-01-04253-3
4. Lillesand, T.M., Kiefer, R.W., Chipman, J.W.: Remote Sensing and Image Interpretation, 7th Ed., Wiley, 2007. ISBN: 978-1-118-34328-9
5. Canty, M.J.: Image Analysis, Clasification and Change Detection in Remote Sensing. CRC Taylor & Francis. 2007. ISBN: 0-8493-7251-8

## Přednášky
Účast doporučená.
{: style="opacity:50%;margin-top:0;"}

Přednášející: Prof. Dr. Ing. Karel Pavelka

1. Co je DPZ, historie DPZ a jeho kořenů
2. Elektromagnetické záření - vznik a popis
3. Veličiny používané v DPZ a jejich jednotky, druhy záření
4. Zdroje záření, interakce záření
5. Odrazivost, propustnost, absorpce
6. Atmosféra, interakce atmosféry s elektromagnetickým zářením
7. Interakce atmosféry s elektromagnetickým zářením - rozptyl, absorpce, propustnost
8. Přenosová funkce atmosféry
9. Zářivé vlastnosti krajinných objektů - obecný rozbor
10. Zářivé vlastnosti krajinných objektů - dle jednotlivých druhů povrchů
11. Pořizování dat - analogová a digitální data
12. Družicové nosiče
13. Úvod do zpracování obrazu
