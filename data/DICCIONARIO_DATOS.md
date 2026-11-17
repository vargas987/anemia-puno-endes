# Diccionario de datos — `endes_puno_6_35_meses.csv`

Base integrada **ENDES 2021–2025**, región Puno, niños de 6 a 35 meses.
**1 421 registros** (406 sin anemia · 1 015 con anemia; prevalencia 71,4 %). Módulos de origen: **RECH0** (hogar, prefijo `HV`) y **RECH6** (niño, prefijo `HC`).

> **Versión anonimizada.** El archivo publicado contiene únicamente la columna `anio`, los **33 predictores** y la variable objetivo `target_anemia` (35 columnas en total). **No** incluye identificadores ni variables de geolocalización (HHID, NCONGLOME, UBIGEO, CODCCPP, NOMCCPP, latitud/longitud), ni las variables de hemoglobina: se retiran por privacidad y para evitar la fuga de información.
> El detalle por predictor (código, módulo, tipo y % de faltantes) está en **`diccionario_predictores.csv`**.

## Columna de año
| Variable | Descripción |
|---|---|
| `anio` | Año de la ronda ENDES (2021–2025) de la que proviene el registro |

## Variable objetivo
| Variable | Descripción | Valores |
|---|---|---|
| `target_anemia` | Presencia de anemia (derivada de HC57, hemoglobina ajustada por altitud) | 0 = sin anemia · 1 = con anemia |

## Predictores usados en el modelo (33)

### Hogar (RECH0 · HV)
| Código | Descripción |
|---|---|
| HV009 | N.º de miembros del hogar |
| HV010 | N.º de mujeres de 15–49 años |
| HV012 | N.º de residentes habituales |
| HV013 | N.º de miembros de facto |
| HV014 | N.º de niños ≤ 5 años |
| HV025 | Área de residencia (1 = urbano · 2 = rural) |
| HV026 | Tipo de lugar de residencia |
| HV035 | Niños elegibles para antropometría/anemia en el hogar |
| HV041 | N.º de niños elegibles (otra definición) |

### Niño / antropometría (RECH6 · HC)
| Código | Descripción |
|---|---|
| HC1 | Edad del niño en meses |
| HC2 | Peso (kg × 10) |
| HC3 | Talla (cm × 10) |
| HC4–HC12 | Indicadores antropométricos (talla/edad, peso/edad, peso/talla en percentil, DE y % respecto a la mediana) |
| HC15 | Posición de medición (acostado/de pie) |
| HC16 | Día de nacimiento / medición |
| HC27 | Sexo del niño (1 = hombre · 2 = mujer) |
| HC61 | Nivel educativo de la madre |
| HC62–HC64 | Indicadores educativos/relacionados de la madre |
| HC68 | Nivel educativo de la madre (agrupado) |
| HC70–HC73 | Puntajes Z antropométricos (OMS): talla/edad, peso/edad, peso/talla, IMC/edad |

## Variables EXCLUIDAS por fuga de información (no son predictores)
| Código | Motivo |
|---|---|
| HC53 | Nivel de hemoglobina (define el desenlace) |
| HC56 | Hemoglobina ajustada por altitud |
| HC57 | Nivel de anemia (fuente del target) |
| HV040 | Altitud del conglomerado (fuga indirecta: entra en el ajuste de hemoglobina) |

## Identificadores y variables geográficas (NO se publican)
Por tratarse de datos de salud de menores, **se retiraron del archivo publicado** los identificadores y variables de geolocalización que traen los módulos crudos del INEI: `HHID`, `NCONGLOME`, `UBIGEO`, `CODCCPP`, `NOMCCPP`, `latitudy`, `longitudx`, los ponderadores del diseño muestral, fechas y números de orden. Para reconstruir la base completa, descargar los módulos RECH0/RECH6 del INEI y ejecutar los notebooks `01`–`06`.

> Los significados exactos deben contrastarse con el **diccionario oficial de la ENDES (INEI)** de cada año.
