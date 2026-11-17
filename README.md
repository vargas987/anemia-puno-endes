# Anemia infantil (6–35 meses) en Puno — ¿la predicen los microdatos ENDES?

[![Licencia: MIT](https://img.shields.io/badge/Licencia-MIT-blue.svg)](LICENSE)
[![Datos: ENDES (INEI)](https://img.shields.io/badge/Datos-ENDES%20INEI-green.svg)](https://proyectos.inei.gob.pe/microdatos/)
[![Resultado: negativo reportado](https://img.shields.io/badge/Resultado-negativo%20transparente-orange.svg)](#resultados)

Evaluación reproducible de modelos de **aprendizaje automático con interpretabilidad SHAP** para predecir la anemia infantil en niños de **6 a 35 meses de la región Puno (Perú)**, a partir de los microdatos públicos de la **ENDES (INEI) 2021–2025**.

> **Hallazgo principal (resultado negativo).** Con los predictores de los módulos de **hogar (RECH0)** y **antropometría (RECH6)**, **ningún modelo superó al azar ni a un clasificador trivial** (ROC-AUC = 0,498; IC 95 % 0,41–0,58). El aporte no es un clasificador, sino una **delimitación empírica**: estos datos rutinarios no bastan para predecir la anemia a nivel individual en esta población.

---

## 📌 Resumen del estudio

| | |
|---|---|
| **Población** | Niños de 6 a 35 meses, región Puno (HV024 = 21) |
| **Fuente** | ENDES (INEI), microdatos públicos y anonimizados |
| **Módulos** | RECH0 (hogar) + RECH6 (niño / antropometría / anemia) |
| **Periodo** | 2021, 2022, 2023, 2024, 2025 (integrados) |
| **Muestra** | 1 421 registros (406 sin anemia · 1 015 con anemia; prevalencia 71,4 %) |
| **Predictores** | 33 variables (6 categóricas + 27 numéricas), sin variables con fuga |
| **Modelos** | Regresión Logística · Random Forest · XGBoost · Red neuronal (MLP) |
| **Explicabilidad** | SHAP (condicionada al desempeño) |
| **Guías de reporte** | TRIPOD+AI · REFORMS |

<a name="resultados"></a>
## 📊 Resultados (conjunto de prueba, 214 casos)

| Métrica | Red neuronal (IC 95 %) | Clasificador trivial |
|---|---|---|
| Exactitud | 0,565 (0,498–0,630) | 0,715 |
| ROC-AUC | **0,498** (0,413–0,584) | 0,500 |
| PR-AUC | 0,712 | 0,715 |
| F1 (anemia) | 0,674 | 0,834 |

- **Prueba binomial** frente al trivial: p < 0,001.
- **Prueba de permutación** (10 000 repeticiones): el AUC observado cae en el centro de la distribución nula (no significativo).
- Por ello, **los valores SHAP NO se interpretan como determinantes** (describirían ruido con AUC ≈ 0,5).

---

## 📁 Estructura del repositorio

```
anemia-puno-endes/
├── data/
│   ├── endes_puno_6_35_meses.csv        # Base integrada 2021–2025, anonimizada (anio + 33 predictores + target)
│   ├── diccionario_predictores.csv      # 33 predictores: código, módulo, tipo y % de faltantes
│   ├── DICCIONARIO_DATOS.md             # Descripción de cada variable
│   └── raw_endes_descargar_de_inei/     # (vacío) dónde colocar los módulos crudos del INEI
├── notebooks/
│   ├── 01..05_ENDES_AAAA_Integracion_Puno_6_35_meses.ipynb   # Integración por año
│   └── 06_Preprocesamiento_ENDES_2021_2025.ipynb             # Limpieza, fuga, X/y, modelos
├── figures/                             # Figuras del paper (ROC-IC, permutación, SHAP)
├── paper/                               # Short paper (PDF y Word, 2 columnas IEEE)
├── src/                                 # (opcional) scripts reutilizables
├── requirements.txt
├── CITATION.cff
├── LICENSE
└── README.md
```

---

## 🔁 Cómo reproducir

```bash
# 1. Clonar
git clone https://github.com/<TU_USUARIO>/anemia-puno-endes.git
cd anemia-puno-endes

# 2. Entorno (Python 3.11+)
python -m venv .venv
# Windows:  .venv\Scripts\activate
# Linux/Mac: source .venv/bin/activate
pip install -r requirements.txt

# 3. Ejecutar los notebooks en orden (01 → 06) con Jupyter
jupyter lab
```

El notebook `06_Preprocesamiento_ENDES_2021_2025.ipynb` reproduce: integración, control de calidad, **eliminación de variables con fuga** (HC53, HC56, HC57 y HV040), construcción de `X` (33 predictores) e `y` (`target_anemia`), limpieza de códigos especiales, imputación ajustada solo en entrenamiento, entrenamiento de los 4 modelos, métricas con intervalos de confianza, prueba de permutación y SHAP.

---

## 🧬 Datos y origen (ENDES – INEI)

Los microdatos de la ENDES son **públicos y anonimizados** y se descargan del portal oficial del INEI:
**https://proyectos.inei.gob.pe/microdatos/**

Para reconstruir la base desde cero: descargar, por cada año (2021–2025), los módulos **RECH0** y **RECH6**, filtrar la región **Puno** y las edades **6–35 meses**, y colocarlos en `data/raw_endes_descargar_de_inei/`. La base ya integrada y anonimizada se incluye en `data/endes_puno_6_35_meses.csv` para facilitar la reproducción.

### ⚠️ Ética y disponibilidad
- Se usan **microdatos secundarios, públicos y anonimizados** del INEI; **no** se realiza reidentificación ni se cruzan con otras fuentes.
- Contienen información de salud de menores: se comparte **solo código e identificadores anónimos del INEI**.
- No subir datos personales ni versiones no anonimizadas.

---

## 📝 Cómo citar

Ver [`CITATION.cff`](CITATION.cff). Si usas este repositorio, cítalo junto a la ficha técnica de la ENDES (INEI).

## 👥 Autores
Grupo 11 — E.P. de Ingeniería de Sistemas, Universidad Peruana Unión (Juliaca, Puno, Perú):
Crhistian Chuquitarqui Chura · Jhan Carlos Flores Chaiña · Elroy Hernando Vargas Mamani.

## 📄 Licencia
Código bajo licencia **MIT** (ver [`LICENSE`](LICENSE)). Los microdatos de la ENDES se rigen por los términos de uso del **INEI**.
