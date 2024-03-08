# Analyst task
## Popis

Tento projekt slouží k vypracování úkolu na základě [zadání](Zadání/zadani.pdf)
Pro úkol byly poskytnuty dva datové soubory:[containers](Zadání/containers.geojson) a [data-zaplnenost-kontejneru](Zadání/measurements-march.csv)

## Řešení 
- soubor [data_info](data_info.ipynb) slouží jako první pohled na data a má za cíl odhalit problémy dat.
- soubor [data_fce](detection_fun.ipynb)  oobsahuje návrh na řešení některých problémů v datech. Funkce se snaží odhadnout potenciální dobu vyprázdnění kontejneru. Dále obsahuje trendovou funkci vždy pro rostoucí část. To znamená, že pokud je parametr "y" kladný, jedná se pravděpodobně o očekávané chování, kdy kontejner začne znovu plnit po vyprázdnění.

- [MHMP](MHMP.ipynb) je pouze jednoduchý návrh na jednoduchý dashboard s informacemi o kontejnerech, kde  [Container_map](Container_map.html) je mapa rozmístění kontejnerů

  - Rozvaha  [Dashboard](Dashboard.pdf)
  
