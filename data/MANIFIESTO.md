# Manifiesto de datos

| Campo | Valor |
|---|---|
| Nombre del archivo en `data/raw/` | `sentrybox_enaho2025.csv` |
| Fuente (institución) | Instituto Nacional de Estadística e Informática (INEI) – Encuesta Nacional de Hogares (ENAHO) 2025, Módulo 500 «Empleo e Ingresos» |
| URL exacta de descarga | https://proyectos.inei.gob.pe/microdatos/ (archivo base: `Enaho01a-2025-500.csv`) |
| Fecha de descarga | 2026-09-15 |
| Licencia o condiciones de uso | Datos abiertos del Estado peruano publicados por el INEI para uso estadístico y académico con cita obligatoria de la fuente |
| ¿Se permite redistribuir? | Sí (subconjunto filtrado para fines académicos citando al INEI como fuente oficial) |
| Filas × columnas (raw) | 4765 filas × 18 columnas |
| MD5 del archivo raw | `79cb9c9fcccd760795c3a28fdc8b35e3` |
| MD5 del archivo processed | `5caa212fdfd858bb9a4f218d43c94d8d` |

## Diccionario de las variables usadas

| Variable | Tipo | Descripción | Unidad / categorías |
|---|---|---|---|
| `ganancia_neta_mes` | Objetivo | Ganancia neta de la ocupación principal en el mes anterior (P530A) | Soles (S/) |
| `edad` | Numérica | Edad del comerciante en años cumplidos (P208A) | Años |
| `horas_semana` | Numérica | Horas trabajadas la semana pasada en la ocupación principal (P513T) | Horas/semana |
| `anios_en_ocupacion` | Numérica | Tiempo de antigüedad en la ocupación principal (P513A1 + P513A2/12) | Años |
| `personas_en_negocio` | Numérica | Cantidad de personas que laboran en el negocio incluido el encuestado (P512B) | Personas |
| `sexo_mujer` | Binaria / Numérica | Sexo del encuestado (P207) | 1 = mujer, 0 = hombre |
| `es_empleador` | Binaria / Numérica | Categoría de ocupación (P507) | 1 = empleador, 0 = independiente |
| `ayuda_familiar_no_remunerado` | Binaria / Numérica | Cuenta con familiares que ayudan sin pago fijo (P508) | 1 = sí, 0 = no |
| `lleva_libros_contables` | Binaria / Numérica | Lleva libros o sistema de contabilidad (P510B) | 1 = sí, 0 = no |
| `tiene_ocupacion_secundaria` | Binaria / Numérica | Tuvo otro trabajo secundario la semana pasada (P514) | 1 = sí, 0 = no |
| `estado_civil` | Categórica | Estado conyugal del encuestado (P209) | Códigos 1 a 6 (conviviente, casado, viudo, divorciado, separado, soltero) |
| `nivel_educativo` | Categórica | Último nivel de estudios aprobado (P301A) | Códigos 1 a 12 |
| `registro_sunat` | Categórica | Situación de registro tributario RUC en SUNAT (P510A1) | 1 = persona jurídica, 2 = persona natural con RUC, 3 = sin RUC |
| `subsector_ciiu` | Categórica | Subsector de comercio minorista según CIIU Rev. 4 (P506R4) | Códigos 4711 a 4799 |
| `dominio` | Categórica | Dominio geográfico del INEI | Códigos 1 a 8 (8 = Lima Metropolitana) |
| `estrato` | Categórica | Estrato geográfico socioeconómico del INEI | Códigos 1 a 8 |
| `mes_encuesta` | Categórica | Mes de ejecución de la encuesta | 1 a 12 |

*(Nota: La columna `tiene_trabajadores_pagados` presente en el archivo raw se excluye de las variables predictoras del modelo por presentar colinealidad exacta `r = 1.0` con `es_empleador`).*

## Transformaciones de raw a processed
1. Se quitaron espacios en blanco al inicio y final de los nombres de las columnas (`df.columns.str.strip()`).
2. Se verificaron y eliminaron filas duplicadas (`df.drop_duplicates()`), manteniéndose las 4765 filas.
3. No se reemplazaron códigos adicionales en `prepare_data.py` (`CODIGOS_FALTANTES = []`), dejando el tratamiento de imputación dentro del Pipeline de entrenamiento.
4. Se verificó la ausencia de valores nulos en la variable objetivo `ganancia_neta_mes` (`df.dropna(subset=[TARGET])`).
5. No se aplicaron recortes manuales de filas para preservar la muestra íntegra de 4765 microcomerciantes minoristas.