---
author: Denis Rodríguez
categories:
- Población y Desarrollo
- Demografía
- R
date: "2025-12-27"
draft: false
layout: single
title: "Cómo visualizar la dependencia demográfica en R: explorando su punto de inflexión"
subtitle: "Un gráfico interactivo para analizar la dependencia juvenil y por vejez"
excerpt: "A través de un gráfico interactivo en R, aprende y explorar la evolución de la dependencia demográfica y descubrir el momento en que la dependencia por vejez superará a la juvenil. Una visualización interactiva que invita a comprender, con datos, un cambio clave en la estructura poblacional."
---

### ¿Qué es la dependencia demográfica?
<iframe 
  src="/01-dependencia-demografica/grafico_final.html"
  width="100%" 
  height="500px"
  frameborder="0">
</iframe>

La **dependencia demográfica** es un indicador fundamental para analizar la estructura por 
edades de una población. Permite estimar la **presión potencial** que ejercen los grupos 
considerados dependientes sobre la población en edad de trabajar.

Tradicionalmente, se distinguen dos grandes componentes:

- **Dependencia juvenil**: población menor de 15 años  
- **Dependencia por vejez**: población de 65 años y más  

Ambos grupos se expresan como porcentaje de la población en edad activa (15–64 años).

El análisis de su evolución resulta clave para la **planificación de políticas públicas**,
especialmente en áreas como:

- Sistemas de pensiones  
- Salud y cuidados de largo plazo  
- Educación  
- Mercado laboral y productividad  

En este artículo se presenta un **gráfico interactivo** que muestra la evolución de la **dependencia demográfica en el Perú** entre 1950 y 2100,
desarrollado en **R** con la librería **highcharter**. Para conocer en detalle las opciones de personalización y funcionalidades disponibles, 
puedes consultar la **[documentación oficial de la API de Highcharts](https://api.highcharts.com/highcharts/)**.
Es importante tener en cuenta que **highcharter es gratuito únicamente para fines no comerciales**, por lo que su uso en proyectos
comerciales requiere una licencia.

### Fuente de los datos

Los datos utilizados provienen de:

> **[World Population Prospects 2024 – Naciones Unidas](https://population.un.org/wpp/)** 

A partir de esta fuente, se ha elaborado previamente un **archivo en Excel**, en el cual se calcula la **dependencia demográfica juvenil y por vejez** como porcentaje respecto a la población en edad de trabajar (15–64 años).

El archivo contiene información histórica y proyecciones de la población mundial, y **puede descargarse directamente en el siguiente [enlace](https://raw.githubusercontent.com/Denis-Yen/Blog/main/data/dependencia_demografica.xlsx)**

### Librerías utilizadas en R

Para la visualización y manipulación de datos se emplearon las siguientes librerías:

```r
library(highcharter)
library(tidyverse)
library(openxlsx)
```
### Descargar y leer los datos
El archivo en formato Excel se encuentra alojado en mi repositorio de **GitHub**.  
Para utilizarlo, basta con ejecutar el siguiente código en tu ordenador, el cual descargará el archivo y lo leerá directamente en **R**:
```r
url <- "https://raw.githubusercontent.com/Denis-Yen/Blog/main/data/dependencia_demografica.xlsx"

temp_file <- tempfile(fileext = ".xlsx")
download.file(url, temp_file, mode = "wb")

datos_dep <- read.xlsx(temp_file)

```
### Preparar los datos
En este paso se renombran las variables y se transforma la base de datos del formato **wide** al formato **long**.  
Esta transformación es fundamental, ya que permite construir una **serie de tiempo interactiva** y visualizar múltiples categorías dentro de un mismo gráfico.
```r
colnames(datos_dep) <- c(
  "Año",
  "Dependencia_juvenil",
  "Dependencia_vejez"
)

datos_dep_lng <- datos_dep |>
  pivot_longer(
    cols = c("Dependencia_juvenil", "Dependencia_vejez"),
    names_to = "Categoria",
    values_to = "Porcentaje"
  ) |>
  mutate(
    Porcentaje = round(Porcentaje, 1)
  )

```

### Construir el gráfico
En este bloque se genera el **gráfico interactivo de líneas** utilizando la librería **highcharter**.  
El gráfico permite visualizar la evolución de la **dependencia demográfica juvenil** y la **dependencia por vejez** 
en el Perú entre 1950 y 2100, incorporando títulos, subtítulos, ejes, etiquetas y una leyenda clara para facilitar su interpretación.
```r
grafico <- highchart() |>
  hc_chart(type = "line") |>
  hc_title(
    text = "Perú. Evolución de la dependencia demográfica (1950–2100)"
  ) |>
  hc_subtitle(
    text = "(Población de 0–14 años + población de 65 años y más) / población de 15–64 años"
  ) |>
  hc_xAxis(
    title = list(text = "Año"),
    categories = unique(datos_dep_lng$Año)
  ) |>
  hc_yAxis(
    title = list(text = "Porcentaje"),
    labels = list(format = "{value:.1f}")
  ) |>
  hc_tooltip(
    shared = TRUE,
    valueDecimals = 1,
    valueSuffix = " %"
  ) |>
  hc_add_series(
    data = datos_dep_lng |>
      filter(Categoria == "Dependencia_juvenil") |>
      pull(Porcentaje),
    name = "Dependencia juvenil (0–14 años)",
    color = "#1D3557"
  ) |>
  hc_add_series(
    data = datos_dep_lng |>
      filter(Categoria == "Dependencia_vejez") |>
      pull(Porcentaje),
    name = "Dependencia por vejez (65 años y más)",
    color = "#E63946"
  ) |>
  hc_caption(
    text = "Fuente: World Population Prospects 2024 | Elaboración: Denis Rodríguez"
  )

```
Finalmente, el gráfico evidencia que hacia el año 2050 la dependencia por vejez alcanzará
aproximadamente **27 personas mayores por cada 100 personas en edad de trabajar (15 a 64 años),
superando a la dependencia juvenil**. Este comportamiento marca un punto de inflexión en la estructura
demográfica del Perú y refleja el avance sostenido del proceso de envejecimiento poblacional.

Este cambio demográfico plantea desafíos estratégicos para las políticas públicas en el país, 
particularmente en lo referido a la sostenibilidad del sistema de pensiones, la creciente demanda
de servicios de salud, el desarrollo e implementación de sistemas de cuidados de largo plazo,
así como la adaptación del mercado laboral frente a una población progresivamente más envejecida.

<iframe 
  src="/01-dependencia-demografica/grafico_final.html"
  width="100%" 
  height="500px"
  frameborder="0">
</iframe>

----

<div style="text-align:center; margin: 2rem 0;">
  <a href="https://www.buymeacoffee.com/denisyenrc7">
    <img src="https://img.buymeacoffee.com/button-api/?text=Cómprame%20un%20café&emoji=☕&slug=denisyenrc7&button_colour=6fc7da&font_colour=000000&font_family=Cookie&outline_colour=000000&coffee_colour=FFDD00" />
  </a>
</div>

<div style="text-align:center; color:#6fc7da; font-weight:600;">
Si este contenido te fue útil,
</div>
<div style="text-align:center; color:#6fc7da;">
puedes apoyarme con un café ☕.  
Es una forma sencilla de ayudarme a seguir creando y compartiendo conocimiento.
</div>


