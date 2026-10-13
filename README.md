# 🗺️ Mapatón Regional - Perú 2026

![Status](https://img.shields.io/badge/status-completado-success?style=flat-square)
![Herramienta](https://img.shields.io/badge/Folium-Dashboard_Interactivo-4285F4?style=flat-square&logo=google&logoColor=white)
![Fuente](https://img.shields.io/badge/HDX-Datos_Abiertos-34A853?style=flat-square&logo=googlesheets&logoColor=white)
![Python](https://img.shields.io/badge/Python-Procesamiento_Espacial-3776AB?style=flat-square&logo=python&logoColor=white)

---

## Ver dashboard / View dashboard

> 🔗 **[Descargar y abrir mapa interactivo (mapa_mapaton_peru_2026.html) →](./mapa_mapaton_peru_2026.html)**  

---

## Descripción / Overview

Dashboard interactivo y pipeline de ciencia de datos espaciales que procesa y analiza **475,473 edificaciones** mapeadas colectivamente por voluntarios para el territorio peruano.

Permite visualizar de manera agregada la concentración urbana por cuadrícula y modelar un índice de exposición relativa frente al riesgo climático basándose en la huella constructiva de las estructuras.

---

## Conexión y Flujo de Procesamiento

```
Datos Abiertos HDX (Perú Buildings GeoJSON)
│
├── Descarga automatizada (Peru_buildings.geojson)
├── Reproyección métrica local (UTM Zona 18S - EPSG:32718)
└── Extracción de centroides geográficos y áreas (m²)
          ↓
    Grid Binning (0.05°)
    (Agregación espacial de densidad y área por celdas)
          ↓
    Índice de Exposición
    (Modelamiento ponderado: 50% área + 50% concentración)
          ↓
    Dashboard Interactivo Folium
    (mapa_mapaton_peru_2026.html)
```

---

## Estructura del repositorio

```
mapaton-peru-2026/
│
├── Mapaton_Peru_2026.ipynb    ← Notebook principal: Ejecuta todo el flujo y pipeline
│
├── Peru_buildings.geojson      ← Dataset crudo descargado de HDX (temporal)
│
└── mapa_mapaton_peru_2026.html ← Output definitivo: Mapa dinámico interactivo con panel de control
```

---

## Capas del Dashboard

| Capa / Layer | Descripción | Métrica Visualizada |
|---|---|---|
| 1 · Concentración de Edificaciones | Cantidad absoluta de edificaciones mapeadas por celda | Conteo total (escala de naranjas/cafés) |
| 2 · Índice de Exposición Relativa | Nivel de riesgo combinado basado en aglomeración y huella física | Índice normalizado (escala de rojos) |
| 3 · Ríos Principales | Capa hidrográfica de soporte filtrada para el territorio peruano | Geometría lineal celeste |

---

## KPIs monitoreados

| KPI | Descripción |
|---|---|
| Volumen Procesado | Cantidad total de edificaciones procesadas (475,473 estructuras) |
| % de Huellas Pequeñas | Porcentaje de edificaciones críticas menores a 10 $m^2$ (1.99%) |
| Área Promedio | Superficie de construcción promedio nacional (88.87 $m^2$) |

---

## Fórmulas y Reglas de Negocio en la Preparación de Datos

Lógica analítica configurada en el pipeline espacial mediante Pandas y GeoPandas:

```
score_tamano     = CASE WHEN area_m2 < 10 THEN 80.0
                        WHEN area_m2 < 30 THEN 45.0
                        ELSE 15.0 END

densidad_factor  = (edificaciones_en_celda / max_edificaciones_global) * 100

indice_exposicion = (score_promedio_tamano * 0.5) + (densidad_factor * 0.5)
```

---

## Cómo ejecutar / How to run

```bash
# 1. Clonar el repositorio
git clone https://github.com/evelyncabrera-pe/mapaton-peru-2026.git
cd mapaton-peru-2026

# 2. Instalar dependencias
pip install geopandas folium requests numpy jinja2

# 3. Abrir y ejecutar el notebook
jupyter notebook Mapaton_Peru_2026.ipynb
```

---

## Arquitectura de Datos de la Visualización

```
Variables en Python             Componente de Visualización en el HTML
───────────────────             ───────────────────────────────────────
total_edificios     ──────────→  Tarjeta KPI: Total de Edificaciones

porcentaje_pequeños ──────────→  Tarjeta KPI: % de Huellas Pequeñas (< 10m²)

area_promedio_global ─────────→  Tarjeta KPI: Área Promedio Global

raw_json (Grid)     ──────────→  Capas Dinámicas y Tooltips Interactivos de Leaflet
```

---

## Sobre este proyecto / About

Este repositorio forma parte del portafolio de datos de **Evelyn Cabrera Arias**, Analytics Translator Senior con más de 10 años de experiencia en banca, riesgo crediticio y modelamiento geográfico.

🔗 [linkedin.com/in/evelyn-cabrera](https://linkedin.com/in/evelyn-cabrera) · [github.com/evelyncabrera-pe](https://github.com/evelyncabrera-pe)
"""

with open("README.md", "w", encoding="utf-8") as f:
    f.write(readme_content)

print("¡Archivo README.md generado y formateado con éxito con la nueva estructura de KPIs y badges!")