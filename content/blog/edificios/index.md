---
author: Denis Rodríguez
categories:
- Qgis
date: "2026-03-01"
draft: false
excerpt: En este tutorial se muestra cómo crear un mapa isométrico de edificios utilizando QGIS
  y datos de OpenStreetMap, aplicando técnicas de visualización 2.5D para resaltar la altura
  relativa de las edificaciones con fines exploratorios y comunicacionales.
layout: single
subtitle: Visualización urbana 2.5D a partir de datos de OpenStreetMap
title: Creación de un mapa isométrico de edificios en la avenida Javier Prado Este con QGIS
---

## Creación de un mapa isométrico de edificios con QGIS

En este tutorial se explica paso a paso cómo crear una **visualización isométrica de edificios** utilizando
**QGIS 3** y datos de **OpenStreetMap (OSM)**. Este tipo de representación permite
resaltar la **altura relativa de las edificaciones**, ofreciendo una vista 
tridimensional simplificada del entorno urbano.

La visualización isométrica es especialmente útil para fines **exploratorios, 
comunicacionales y cartográficos**, ya que prioriza la lectura espacial y la 
comparación visual, más que la precisión estructural.

El ejercicio se basa en el uso de expresiones dentro del panel de estilos de QGIS,
aprovechando atributos como el número de pisos y la altura de los edificios
disponibles en OSM. Siguiendo el tutorial de [Ujaval Gandhi](https://www.qgistutorials.com/es/docs/3/isometric_buildings.html)

---

## Datos utilizados

Los datos empleados en este tutorial provienen de **OpenStreetMap**, una base de datos geográfica colaborativa que contiene información detallada sobre:

- Huellas de edificios  
- Número de pisos (`building:levels`)  
- Altura de edificaciones (`height`), cuando está disponible  

Para este ejercicio se utilizan datos descargados mediante el complemento **QuickOSM**, lo que garantiza una estructura de atributos compatible con las expresiones que se aplicarán más adelante.

> **Nota:**  
> Este procedimiento está diseñado para capas obtenidas con QuickOSM. Si se utilizan extractos OSM completos u otras fuentes, será necesario procesar previamente los atributos de altura.


## Descarga de datos con QuickOSM

1. Abra **QGIS** y cargue un mapa base desde:  
   `XYZ Tiles → OpenStreetMap`.
2. Haga zoom sobre el área de interés (en este caso, la avenida Javier Prado Este).
3. Vaya a **Vector → QuickOSM → QuickOSM**.
4. En la pestaña *Quick query* configure:
   - **Key:** `building`
   - **In:** `Canvas Extent`
5. En la pestaña *Advanced*, desactive la descarga de puntos y líneas, dejando solo **polígonos**.
6. Ejecute la consulta y cierre la ventana.

Los edificios se cargarán como una nueva capa vectorial en el proyecto.

## Preparación de los datos

Una vez cargada la capa de edificios, seleccione las entidades de interés y expórtelas como un **GeoPackage**:

- **Formato:** `GeoPackage (.gpkg)`  
- **Sistema de referencia:** `EPSG:3857 – WGS 84 / Pseudo-Mercator`  

El uso de este sistema de coordenadas permite trabajar con **unidades métricas**, necesarias para la correcta representación de alturas en visualizaciones 2.5D.

---

## Estilizado isométrico de los edificios

1. Abra el **Panel de estilos de capa**.
2. Cambie el tipo de renderizador a **2.5 D**.
3. En el campo *Altura*, haga clic en el botón de expresiones e ingrese la siguiente fórmula:

```r
coalesce("building:levels" * 10, "height" * 2, 20)