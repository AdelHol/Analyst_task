# Analyst_task
## Popis

Tento projekt slouží k vypracování ukolu na základě [zadání](Zadání/zadani.pdf)
Pro úkol byly poskytnuty dva datové soubory [containers](Zadání/containers.geojson) a [data-zaplnenost-kontejneru](Zadání/measurements-march.csv)

## Řešení 
- soubor [data_info](data_info.ipynb) je jen první pohled na data, odhalení problému dat
- soubor [data_info](detection_fun.ipynb)  obsahuje návrh na řešení některých problému řady. Funkce se snaží odchadnout potencionálná dobu vypráznění kontejneru, dále pak trend funkce vždy pro rostoucí část, tzn pokud je paramter a kladý jedná se pravděpodobně o očekávané chování, kontejner se po vyprázdnění opět začně plnit
  
- MHMP je jen jednoduchý návrh na jednoduchý dashboard
  
