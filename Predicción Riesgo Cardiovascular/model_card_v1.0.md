# 🩺 Model Card — Modelo clínico cardiovascular (v1.0)

Fecha de generación: 2025-10-27 18:36:51  
Fecha de entrenamiento: 2025-10-27  
Responsable: Patricia C. Torrell  
Dataset utilizado: `cardio_train.csv`  
Semilla: 42  
Tipo de modelo: HGB + Mediana

---

## 1. Variables utilizadas
- Numéricas: age, height, weight, BMI, ap_hi, ap_lo
- Ordinales: cholesterol, gluc
- Binarias: gender, smoke, alco, active

## 2. Flags clínicas creadas
- flag_ap_incoherent: PA incoherente (ap_lo ≥ ap_hi)
- flag_HTA: hipertensión clínicamente significativa
- flag_riesgo_clinico: riesgo por presión o IMC
- flag_riesgo_estilo_vida: riesgo por hábitos (alcohol, tabaquismo, inactividad)

## 3. Winsorización aplicada
- was_capped_age_years
- was_capped_height
- was_capped_weight
- was_capped_BMI
- was_capped_ap_hi
- was_capped_ap_lo

## 4. Métricas globales
- ROC-AUC calibrado: 0.79
- PR-AUC calibrado: 0.774
- Brier score: 0.186

## 5. Umbral operativo
- Criterio: Youden J
- Valor sugerido: 0.505

---

## 6. Interpretabilidad

### 6.1 Importancia por permutación (descenso de ROC-AUC)
Se evaluó la sensibilidad del modelo al permutar cada variable (10 repeticiones, scoring = ROC-AUC):

| feature | importance_mean | importance_std |
|---|---|---|
| num__ap_hi | 0.1183 | 0.0016 |
| num__age | 0.0235 | 0.0006 |
| cat__cholesterol_3 | 0.0157 | 0.0004 |
| cat__flag_riesgo_clinico_0 | 0.0140 | 0.0004 |
| num__ap_lo | 0.0064 | 0.0002 |
| cat__cholesterol_1 | 0.0052 | 0.0002 |
| num__BMI | 0.0040 | 0.0003 |
| num__weight | 0.0026 | 0.0002 |
| cat__active_0 | 0.0016 | 0.0001 |
| cat__gluc_3 | 0.0014 | 0.0001 |

### 6.2 PDP (Partial Dependence)
Se calcularon curvas PDP para las 3 variables numéricas más influyentes según permutación:
- `ap_hi`- `age`- `ap_lo`
### 6.3 ALE (Accurate Local Effects)
Se aplicó ALE a variables clínicas con correlación significativa (r > 0.7):
- `ap_hi`- `ap_lo`- `BMI`- `active`
### 6.4 ALE de Interacciones
Se calcularon mapas de interacción ALE para pares clínicos relevantes:
- `ap_hi × ap_lo`: interacción entre presión sistólica y diastólica
- `ap_hi × BMI`: interacción entre presión sistólica y obesidad

## 7. Evaluación por subgrupo
| gender | edad_band | n | TP | FP | FN | TN | sens | espec | PPV | NPV |
|---|---|---|---|---|---|---|---|---|---|---|
| hombre | [0,40) | 133.0000 | 15.0000 | 10.0000 | 16.0000 | 92.0000 | 0.4840 | 0.9020 | 0.6000 | 0.8520 |
| hombre | [40,50) | 1434.0000 | 323.0000 | 89.0000 | 222.0000 | 800.0000 | 0.5930 | 0.9000 | 0.7840 | 0.7830 |
| hombre | [50,60) | 2216.0000 | 740.0000 | 285.0000 | 395.0000 | 796.0000 | 0.6520 | 0.7360 | 0.7220 | 0.6680 |
| hombre | [60,70) | 939.0000 | 522.0000 | 209.0000 | 89.0000 | 119.0000 | 0.8540 | 0.3630 | 0.7140 | 0.5720 |
| mujer | [0,40) | 222.0000 | 19.0000 | 6.0000 | 28.0000 | 169.0000 | 0.4040 | 0.9660 | 0.7600 | 0.8580 |
| mujer | [40,50) | 2418.0000 | 489.0000 | 153.0000 | 389.0000 | 1387.0000 | 0.5570 | 0.9010 | 0.7620 | 0.7810 |
| mujer | [50,60) | 4698.0000 | 1529.0000 | 522.0000 | 890.0000 | 1757.0000 | 0.6320 | 0.7710 | 0.7450 | 0.6640 |
| mujer | [60,70) | 1693.0000 | 1008.0000 | 372.0000 | 130.0000 | 183.0000 | 0.8860 | 0.3300 | 0.7300 | 0.5850 |

## 8. Riesgos y mitigaciones
- Sensibilidad baja en mujeres jóvenes y hombres entre 40 - 50 años
- Especificidad baja en mujeres mayores
- Evaluación por subgrupo
- Umbrales diferenciados

---

## 9. Mantenimiento
- Monitorizar ROC/PR/Brier mensualmente
- Recalibrar si Brier empeora > 0.02
- Reentrenar si ROC-AUC cae > 0.03 sostenido
