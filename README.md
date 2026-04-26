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
## ✍️ Creación del Mapa de Calor
```r
dirmapas <- "C:/Users/Valentina/Desktop/RStudioDocumentos/Bioestadística/departamentos"
#La dirección de tu directorio de trabajo donde se encuenta tu shapefile
setwd(dirmapas)
peru_d <- st_read("DEPARTAMENTOS_inei_geogpsperu_suyopomalia.shp") #Este comando permite leer el shapefile y 'transformarlo' en un data frame
```
### 🗺️ Observamos el data frame creado y el mapa en blanco
```r
peru_d

ggplot(data = peru_d) +
  geom_sf()
```
<img width="448" height="486" alt="image" src="https://github.com/user-attachments/assets/7efcbaa2-ef27-433e-9216-a5850ecc2a68" />
### Lectura y Limpieza de datos
```r
PL <- read_csv("Datos de Lectura 18-64 años.csv")
PLL <- PL %>%
  group_by(NOMBREDD) %>%
  summarise(
    total_encuestados = n(),
    lectores = sum(P401_1 == 1, na.rm = TRUE),
    p_lectores = round((lectores / total_encuestados)*100,2) 
  )
lectores <- PLL %>%
  select(NOMBREDD, p_lectores)
```
La data final contiene en la primera columna los nombres de los departamentos y en la segunda los porcentajes calculados para cada uno

### Impresión del Mapa de Calor
