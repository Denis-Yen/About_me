---
author: Denis Rodríguez
categories:
- SIG
- Mapas
- R
- Shiny
date: "2025-01-31"
draft: false
excerpt:  En este artículo se presenta de manera interactiva el mapa del riesgo a movimientos de masa
  a nivel distrital en el departamento de Piura, elaborado con R mediante Leaflet y una aplicación Shiny,
  a partir de información oficial del CENEPRED.
layout: single
subtitle: Mapa distrital interactivo en shiny
tags:
- style
title: Visualización interactiva del riesgo a movimientos de masa en Piura
---

En este artículo se presenta un mapa interactivo del riesgo a movimientos de masa a nivel distrital en el departamento de Piura, elaborado en R mediante las librerías Leaflet y Shiny, a partir de información oficial del CENEPRED. A diferencia de los mapas estáticos, este visor permite explorar el territorio de forma dinámica, facilitando la lectura espacial del riesgo.

Como se explicó en el artículo anterior, se desarrolló un mapa estático de alta calidad,
ideal para informes técnicos y presentaciones institucionales, el cual puede replicarse 
para otras regiones del país. En esta oportunidad, ese mismo análisis se amplía incorporando 
interactividad, permitiendo al usuario filtrar información, acercarse a zonas específicas y
consultar atributos distritales directamente sobre el mapa.
<iframe 
  src="https://denisyen.shinyapps.io/piura_riesgo_app/"
  width="100%" 
  height="900px"
  style="border:none;">
</iframe>

El resultado es una herramienta visual que mejora la comprensión del riesgo 
a movimientos de masa, al identificar con mayor claridad los distritos expuestos
a un nivel Muy Alto de riesgo, contribuyendo así a la priorización territorial para
la prevención y la gestión del riesgo de desastres.


Puedes encontrar el **código fuente de la aplicación** en mi repositorio de GitHub:  
👉 <https://github.com/Denis-Yen>

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