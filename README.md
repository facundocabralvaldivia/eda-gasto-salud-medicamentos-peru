# Gasto de bolsillo en salud y medicamentos en Perú (2019-2025)

Análisis reproducible del gasto de bolsillo en salud y medicamentos en Perú, basado en la **Encuesta Nacional de Hogares (ENAHO)** del INEI.

**Autor:** [Facundo Cabral](https://www.linkedin.com/in/facundocabral/)

---

## Contenido del repositorio

| Archivo / carpeta | Descripción |
|-------------------|-------------|
| `analysis_001.ipynb` | Carga de datos, construcción de variables, tablas nacionales, gráficos y mapas |
| `data/` | **No incluida en Git** — ver sección [Datos](#datos) |

---

## Datos

Los datos para este proyecto no se encuentran subidos al repositorio debido al límite de espacio máximo permitido. A continuación se detalla como obtener los datos y como acoplarlos al proyecto.

### Datos de ENAHO

Toda la data utilizada proviene del [**Protal de Microdatos**](https://proyectos.inei.gob.pe/microdatos/) del INEI. Para obtener la data debes hacer lo siguiente:

1. Ingresar al enlace anterior del Portal de Microdatos
2. Buscar la **ENAHO** por año (2019 a 2025).
3. Descargar, para cada año:
   - el **módulo de salud (400)** — gasto en salud a nivel persona;
   - la **base sumaria** — variables del hogar, incluida **pobreza** (`pobreza`).
4. Descomprimir los `.zip` y copiar los `.dta` a las rutas de este proyecto (ver estructura).

### Datos del shapefile

Para obtener el archivo shapefile que permite hacer la construcción del mapa del Perú para el análisis geoespacial, se puede obtener en [**Geo GPS Perú**](https://www.geogpsperu.com/2014/03/base-de-datos-peru-shapefile-shp-minam.html). Para obtener ese archivo debe hacer lo siguiente:

1. Ingresar al enlace anterior de Geo GPS Perú
2. Buscar el archivo a nivel departamental y descargarlo
3. Descomprimir el `.zip` y copiar los archivos según la ruta de este proyecto (ver estructura).

### Estructura de carpetas requerida

Colocar los archivos de la siguiente manera:

```
data/
├── modulo_400/
│   ├── enaho01a-2019-400.dta
│   ├── enaho01a-2020-400.dta
│   ├── enaho01a-2021-400.dta
│   ├── enaho01a-2022-400.dta
│   ├── enaho01a-2023-400.dta
│   ├── enaho01a-2024-400.dta
│   └── enaho01a-2025-400.dta
├── sumaria/
│   ├── sumaria-2019.dta
│   ├── sumaria-2020.dta
│   ├── sumaria-2021.dta
│   ├── sumaria-2022.dta
│   ├── sumaria-2023.dta
│   ├── sumaria-2024.dta
│   └── sumaria-2025.dta
└── shapefile/
    ├── DEPARTAMENTOS_inei_geogpsperu_suyopomalia.shp
    ├── DEPARTAMENTOS_inei_geogpsperu_suyopomalia.dbf
    ├── DEPARTAMENTOS_inei_geogpsperu_suyopomalia.shx
    └── (demás archivos auxiliares del shapefile: .prj, .cpg, .sbn, .sbx, etc.)
```

## Metodología

Alineada con informes DIGEMID de gasto de bolsillo en salud:

- **Gasto por rubro** `j = 01…16`: `gto_j = i416_j` si `p4151_j == 1`, si no `0`.
- **Gasto total por persona:** suma de `gto01`…`gto16`.
- **Problema de salud:** `p4025 == 0` → submuestra con condición de salud.
- **Factor de expansión (`factor_exp`):**
  - 2019 y 2022–2025: `factor07`
  - 2020: `factor_p` cuando no es nulo; si no, `factor07`
  - 2021: `factor_p`

### Secciones del notebook

1. Carga y merge ENAHO (módulo 400 + sumaria).
2. Gasto nacional total y por personas con problema de salud (2019–2025).
3. Gráficos de evolución y composición por tipo de gasto.
4. Gasto per cápita en medicamentos por departamento (mapas).
5. Medicamentos (`gto02`) por **lugar de compra** (`p417_02`), **pobreza** y **grupo etario** (2019–2025).

---

## Referencias

- INEI — ENAHO: [Microdatos](https://proyectos.inei.gob.pe/microdatos/)
- DIGEMID — informes de gasto de bolsillo en salud.
