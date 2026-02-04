# Prevención de la migraña: modelo predictivo e inferencia estadística

Este proyecto evalúa si es posible predecir la aparición de un ataque de migraña utilizando el dataset público [*Medicine and Environment Impact on Migraine* (Kaggle)](https://www.kaggle.com/datasets/).

## Estructura del proyecto
- `data/processed` → Datasets creados con nuevas variables.
- `data/raw` → Dataset original.
- `models/` → Modelos entrenados y calibrados.
- `reports/tables/` → Tablas exportadas (CSV).
- `reports/figures/` → Gráficas generadas (PNG).

├── proy_final.ipynb           
├── data/
│   └── raw/
│       └── migraine_data.csv
│   └── processed/
│       └── 28_dataset_limpio.csv
│       └── 210_dataset_final.csv
│       └── dataset_final_yprob.csv
├── models/
│   └── split.pkl
│   └── validation_metrics.pkl
│   └── preprocessor.pkl  
│   └── logistic_regression.pkl 
│   └── logistic_regression_tuned.pkl  
│   └── logistic_regression_calibrator.pkl  
│   └── ogistic_regression_tuned_calibrator.pkl 
│   └── gradient_boosting_tuned_calibrator.pkl 
│   └── xgboost_tuned_calibrator.pkl 
├── reports/
│   ├── figures/ (algunas)
│       └── 22_distribuciones_numericas.png
│       └── 24_distribuciones_categoricas.png
│       └── 39_pr_test_logistic_regression.png
│       └── 39_roc_test_logistic_regression.png
│       └── 61_perm_importance_grouped_prauc.png
│       └── 62_pdp_medication.png
│       └── 65_ale_airq_by_hatype.png
│       └── 412_pr_curve_tuned_isotonic.png
│       └── 412_roc_curve_tuned_isotonic.png
│       └── 612_resumen_perfiles_heatmap.png
│       └── 613_shap_summary_global.png
│   └── tables/ (algunas)
│       └── 12_dictionary_df.csv
│       └── 41_test_metrics_all_calibrations.csv
│       └── 41_test_metrics_best_calibration.csv
│       └── 43_test_metrics_best_tuned_calibration.csv
│       └── 55_resumen_coeficientes_demograficos.csv
│       └── 298_diccionario_variables_derivadas.csv
│       └── 411_test_metrics_rl_tuned_isotonic_bootstrap.csv
│       └── 413_threshold_curve_CFP1_CFN1.csv
│       └── 522_anova_medication.txt
│       └── 562_fairness_metrics_extended.csv
│       └── 613_shap_values_grouped_by_patient.csv
│       └── id_te.csv
└── README.md

## Dependencias

El análisis se realizó en el entorno `bioai` con las siguientes librerías principales:

- **Python**: `pandas`, `scikit-learn` (con todos sus submódulos), `xgboost`, `shap`, `matplotlib`, `seaborn`, `joblib`, `scipy`, `pathlib`, `collections`
- **R**: `tidyverse` (dplyr, tidyr, readr, stringi), `ggplot2`, `corrplot`, `knitr`, `epiR`, `epitools`, `vcd`, `geepack`, `brglm2`, `clubSandwich`, `pROC`, `sandwich`, `lmtest`, `data.table`, `iml`, `pdp`, `DALEX`, `vip`, `yardstick`

## Cómo reproducir los resultados

1. **Coloca el dataset**  
   Descarga el archivo `migraine_data.csv` desde [Kaggle](https://www.kaggle.com/datasets/) y guárdalo en la carpeta `data/raw`.

2. **Abre el notebook**  
   Ejecuta el notebook `proy_final.ipynb` en tu entorno `bioai` (o con todas las dependencias instaladas).

3. **Reproduce el flujo**  
   El notebook contiene, en orden:
   - Exploración y generación de variables derivadas (`fase`, `season_study`, `prev_attacks`, etc.)
   - Preprocesamiento y división de datos
   - Entrenamiento y calibración de modelos (LogisticRegression, XGBoost…)
   - Selección del modelo ganador: **Regresión Logística con calibración isotónica**
   - Cálculo de métricas (con umbral optimizado para sensibilidad ≥ 0.80)
   - Generación de tablas (OR ajustados, ANOVA) y figuras (PDP, ALE, ICE, SHAP)
   - Guardado automático de modelos, tablas y gráficos en sus carpetas respectivas

4. **Resultados clave**  
   - La **medicación continua** reduce significativamente el riesgo de ataque (p < 0.05).  
   - **No se identificó un patrón universal de predicción**: alta heterogeneidad individual (ICE).  
   - **Calidad del aire y estacionalidad** (otoño) actúan como moduladores contextuales del riesgo.  
   - El modelo tiene **alta sensibilidad** pero **señal predictiva global débil**.

## Preguntas respondidas

- **Principal**: ¿Se puede predecir un ataque de migraña? → **Parcialmente, con enfoque personalizado**.  
- **Secundarias**:  
  - ¿La medicación reduce el riesgo? → **Sí, significativamente**.  
  - ¿Influye el subtipo clínico? → **No, no es significativo**.  
  - ¿La calidad del aire importa? → **Tendencia, no significativa**.  
  - ¿Edad/sexo modifican el riesgo? → **Tendencias observadas, no significativas**.

## Notas

- El dataset está desbalanceado (64 % con migraña; 85 % mujeres), lo que limita la evaluación de equidad en subgrupos minoritarios.
- Las figuras y tablas generadas se guardan automáticamente en `figures/` y `tables/` durante la ejecución del notebook.
