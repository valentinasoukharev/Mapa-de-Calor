# Heatmaps en R

## 📌 Objetivo
Aprender a crear mapas de calor en R usando ggplot2.

## ⬇️ Recursos
- Data extraida de la [Plataforma Nacional de Datos Abiertos (PNDA)](https://www.gob.pe/datosabiertos) 
- Shapefile descargado de [GEO GPS PERÚ](https://www.geogpsperu.com/2014/03/base-de-datos-peru-shapefile-shp-minam.html)

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
### Observamos el data frame creado y el mapa en blanco
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
  group_by(NOMBREDD) %>% #agrupamos por departamento
  summarise(
    total_encuestados = n(),
    lectores = sum(P401_1 == 1, na.rm = TRUE),
    p_lectores = round((lectores / total_encuestados)*100,2)  #Calculamos el porcentaje de lectores
  )
lectores <- PLL %>%
  select(NOMBREDD, p_lectores)
```

La data final contiene en la primera columna los nombres de los departamentos y en la segunda los porcentajes calculados para cada uno

### Impresión del Mapa de Calor
Unimos los data frames. En caso que el nombre de las columnas que contienen los nombres de los departamentos no coincidan, se puede utilizar 'by' e igualarlos como se muestra.

```r
peru_datos <- peru_d %>% 
  left_join(lectores, by = c("NOMBDEP" = "NOMBREDD"))
```
Finalmente, hacemos el ggplot: 
```r
ggplot(peru_datos) +
  geom_sf(aes(fill = p_lectores), color = "white", size = 0.2) +
  labs(
  title = "Lectura en adultos en Perú",
  subtitle = "% de personas de 18 a 64 años que leyeron contenido impreso o digital en el último año (2022)",
  caption = "Fuente: Ministerio de Cultura (MINCUL), 2022\nElaboración propia",
  x = NULL,
  y = NULL
) +
  scale_fill_continuous(
    name = "Lectores (%)",
    low = "#FFF7BC",
    high = "#D73027",
    na.value = "gray90",
    limits = c(5, 50)
  ) +
  theme_minimal() +
  theme(
  plot.title = element_text(size = 16, face = "bold", hjust = 0),
  plot.subtitle = element_text(size = 11, hjust = 0),
  plot.caption = element_text(size = 9, hjust = 0),
)
```
<img width="736" height="833" alt="image" src="https://github.com/user-attachments/assets/08da9da5-b32f-44fb-be2f-dded392e7287" />

### Extra
Podemos añadir recuadros donde resaltemos la data exacta relevante, como los porcentajes más bajos y los más altos
<img width="751" height="835" alt="image" src="https://github.com/user-attachments/assets/a253ef32-885e-45ba-a897-cf7c3c7fc857" />

