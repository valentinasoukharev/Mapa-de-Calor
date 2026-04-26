# Heatmaps en R

## 📌 Objetivo
Aprender a crear mapas de calor en R usando ggplot2.

## 📦 Librerías necesarias (Instalar primero)
```r
library(sf)
library(purrr)
library(tidyverse)
library(ggplot2)
library(ggrepel)
library(readxl)
```
## Creación del Mapa de Calor
```r
dirmapas <- "C:/Users/Valentina/Desktop/RStudioDocumentos/Bioestadística/departamentos" #La dirección de tu directorio de trabajo donde se encuenta tu shapefile
setwd(dirmapas)
peru_d <- st_read("DEPARTAMENTOS_inei_geogpsperu_suyopomalia.shp") #Este comando permite leer el shapefile y 'transformarlo' en un data frame
```
### Observamos el data frame creado
```r
peru_d
```
